# Imersão Vibe Coding — Landing Promocional

Três versões de landing page de vendas para a **Imersão Vibe Coding** (Do Zero ao SaaS com IA em 3 dias, presencial em Itaguaí — RJ).

🔗 **Online (GitHub Pages):** https://inematds.github.io/imersaovc/

## Versões

| Arquivo | Estilo | Quando usar |
|---|---|---|
| [`versao-1.html`](versao-1.html) | **Conversão direta** — oferta, escassez, FAQ de objeções | tráfego quente / anúncio |
| [`versao-2.html`](versao-2.html) | **Storytelling** — jornada antes→depois, editorial, depoimentos | desejo e confiança |
| [`versao-3.html`](versao-3.html) | **Visual ousado** — hero animado, glass, contadores, motion | impacto imediato |

`index.html` é um seletor que mostra as 3. Quando decidir a versão final, **renomeie o arquivo dela para `index.html`**.

## Oferta (já configurada nas páginas)

- De **R$ 4.800** por **R$ 3.000** (oferta de lançamento)
- **20 vagas** por imersão
- **Dupla ou mais (2+):** R$ 2.700 por pessoa

## Pagamento via Asaas

Mesmo padrão usado no **inema.vip**: o botão de CTA aponta direto para um **link de checkout do Asaas** (`https://www.asaas.com/c/XXXX`).

### Como ativar (obrigatório antes de vender)

1. No painel do Asaas, crie um **Link de Pagamento**:
   - **Individual:** valor R$ 3.000.
   - **Dupla:** valor R$ 2.700 (por pessoa). Pode ser um link separado ou usar o campo de **desconto** do próprio Asaas.
2. Copie a URL gerada (formato `https://www.asaas.com/c/...`).
3. Em cada `versao-*.html`, substitua os placeholders:
   - `https://www.asaas.com/c/SEU_LINK_INDIVIDUAL` → link de R$ 3.000
   - `https://www.asaas.com/c/SEU_LINK_DUPLA` → link de R$ 2.700/pessoa
4. Commit + push. O GitHub Pages atualiza sozinho.

> Procure por `SEU_LINK_INDIVIDUAL` e `SEU_LINK_DUPLA` nos arquivos.

### Cobrança dinâmica (opcional)

Se quiser gerar cobranças via API (customer + PIX/cartão/boleto, desconto automático para 2+, webhook de confirmação), o cliente de referência já existe no projeto `inemaonline` (`worker/src/asaas.ts`): cria customer, cria payment com campo `discount` (`FIXED`/`PERCENTAGE`), gera QR Code PIX e processa webhook `PAYMENT_CONFIRMED`/`PAYMENT_RECEIVED`.

## Editar datas/local

As páginas usam o local **Itaguaí — RJ, Rua Prefeito José Maria de Brito, 251** e o texto _"Próxima turma — datas a confirmar"_. Procure pelos comentários `<!-- EDITAR DATAS -->` para colocar as datas reais.

## Stack

HTML estático + Tailwind (CDN) + Google Fonts. Sem build. Abre direto no navegador.

---
INEMA.CLUB · 2026
