# Asaas — Gerenciar links de pagamento pela API

Como criar, alterar, listar e excluir os **links de pagamento** (`paymentLinks`) da Imersão Vibe Coding direto pela API do Asaas. Foi assim que os links em produção foram gerados e ajustados.

## 1. Autenticação e ambiente

| Item | Valor |
|---|---|
| Header de auth | `access_token: <SUA_CHAVE>` |
| Base produção | `https://api.asaas.com/v3` |
| Base sandbox | `https://api-sandbox.asaas.com/v3` |
| Onde está a chave (prod) | `inemabk/inemaonline/worker/.dev.vars` → `ASAAS_API_KEY` |

> A chave de produção começa com `$aact_prod...`. **Nunca** comitar a chave. Teste em sandbox antes de mexer em produção.

Convenção do prefixo (vista em `inemaonline/worker/src/asaas.ts`): prod = `$aact_M` / `$aact_prod`, sandbox = `$aact_YT`.

## 2. Campos que controlam o comportamento

| Campo | O que faz | Valores |
|---|---|---|
| `name` | Nome interno do link | texto |
| `value` | Valor em **reais** (não centavos) | ex. `3000` |
| `billingType` | **Forma de pagamento aceita** | `UNDEFINED` (cliente escolhe) · `CREDIT_CARD` · `PIX` · `BOLETO` |
| `chargeType` | Tipo de cobrança | `DETACHED` (avulsa) · `INSTALLMENT` (parcelado) · `RECURRENT` (assinatura) |
| `maxInstallmentCount` | **Nº máximo de parcelas** (só com `INSTALLMENT`) | ex. `6` |
| `dueDateLimitDays` | Dias p/ pagar boleto | ex. `3` |
| `subscriptionCycle` | Periodicidade (só com `RECURRENT`) | `MONTHLY`, `YEARLY`, ... |
| `notificationEnabled` | Notificações ao cliente | `true`/`false` |

**Regra de ouro:** parcelamento (`INSTALLMENT` + `maxInstallmentCount`) é **cartão de crédito**. Para travar só cartão, use `billingType: "CREDIT_CARD"`.

## 3. Endpoints

| Ação | Método + rota |
|---|---|
| Criar | `POST /v3/paymentLinks` |
| Atualizar | `PUT /v3/paymentLinks/{id}` |
| Listar | `GET /v3/paymentLinks` |
| Recuperar 1 | `GET /v3/paymentLinks/{id}` |
| Excluir | `DELETE /v3/paymentLinks/{id}` |
| Restaurar | `POST /v3/paymentLinks/{id}/restore` |

A resposta de criação/consulta traz `id` e `url` (`https://www.asaas.com/c/{id}`) — é a `url` que vai nos botões da landing.

## 4. Receitas — "como mudar cada coisa"

Em todos os exemplos: `KEY` = sua chave; produção. (`-s` = silencioso)

### Criar um link (parcelado, só cartão)
```bash
curl -s -X POST https://api.asaas.com/v3/paymentLinks \
  -H "access_token: $KEY" -H "Content-Type: application/json" \
  -d '{
    "name": "Imersão Vibe Coding — R$ 3.000 (6x)",
    "billingType": "CREDIT_CARD",
    "chargeType": "INSTALLMENT",
    "value": 3000,
    "maxInstallmentCount": 6,
    "notificationEnabled": true
  }'
```

### Mudar a FORMA DE PAGAMENTO (ex.: travar em só cartão)
```bash
curl -s -X PUT https://api.asaas.com/v3/paymentLinks/{id} \
  -H "access_token: $KEY" -H "Content-Type: application/json" \
  -d '{ "billingType": "CREDIT_CARD", "chargeType": "INSTALLMENT", "value": 3000, "maxInstallmentCount": 6 }'
```
- Só Pix → `"billingType": "PIX"`
- Cliente escolhe tudo → `"billingType": "UNDEFINED"`

### Mudar o NÚMERO DE PARCELAS
```bash
curl -s -X PUT https://api.asaas.com/v3/paymentLinks/{id} \
  -H "access_token: $KEY" -H "Content-Type: application/json" \
  -d '{ "chargeType": "INSTALLMENT", "value": 3000, "maxInstallmentCount": 10 }'
```

### Mudar o VALOR
```bash
curl -s -X PUT https://api.asaas.com/v3/paymentLinks/{id} \
  -H "access_token: $KEY" -H "Content-Type: application/json" \
  -d '{ "value": 2500 }'
```

### Desativar / excluir
```bash
curl -s -X DELETE https://api.asaas.com/v3/paymentLinks/{id} -H "access_token: $KEY"
# desfazer:
curl -s -X POST https://api.asaas.com/v3/paymentLinks/{id}/restore -H "access_token: $KEY"
```

### Listar todos / ver um
```bash
curl -s https://api.asaas.com/v3/paymentLinks -H "access_token: $KEY"
curl -s https://api.asaas.com/v3/paymentLinks/{id} -H "access_token: $KEY"
```

## 5. O que NÃO se muda pela API

- **Cupom / voucher de código** (cliente digita e abate): **não existe** no Asaas. Alternativas: link separado com valor menor, ou checkout próprio + API criando a cobrança com `discount`.
- **6x sem juros para o cliente**: é config da **conta** (painel → Configurações → Parcelamento — quem assume os juros). Não é um campo do link.

## 6. Links atuais (produção)

Todos `CREDIT_CARD` + `INSTALLMENT`, ativos:

| Valor | Máx. parcelas | id | URL |
|---|---|---|---|
| R$ 1.500 | 3x | `ycay1a0dk38loy3x` | https://www.asaas.com/c/ycay1a0dk38loy3x |
| R$ 2.000 | 4x | `u3kenc0dep8lnxlb` | https://www.asaas.com/c/u3kenc0dep8lnxlb |
| R$ 2.500 | 5x | `hzz4io2te275dqtk` | https://www.asaas.com/c/hzz4io2te275dqtk |
| R$ 3.000 | 6x | `hkzrb361iks95c50` | https://www.asaas.com/c/hkzrb361iks95c50 |

Nas landings (`versao-*.html`), o botão usa o link de **R$ 3.000**.

## 7. Snippet Node (lê a chave do .dev.vars, não imprime a chave)

```js
const fs = require('fs');
const env = fs.readFileSync('inemabk/inemaonline/worker/.dev.vars', 'utf8');
const key = env.match(/^ASAAS_API_KEY=(.*)$/m)[1].trim().replace(/^["']|["']$/g, '');
const base = 'https://api.asaas.com/v3';
const H = { access_token: key, 'Content-Type': 'application/json' };

// exemplo: travar um link em só cartão
const id = 'hkzrb361iks95c50';
const r = await fetch(`${base}/paymentLinks/${id}`, {
  method: 'PUT', headers: H,
  body: JSON.stringify({ billingType: 'CREDIT_CARD', chargeType: 'INSTALLMENT', value: 3000, maxInstallmentCount: 6 }),
});
console.log(r.status, (await r.json()).billingType);
```

---
Ref. oficial: https://docs.asaas.com/reference/criar-um-link-de-pagamentos · INEMA.CLUB · 2026
