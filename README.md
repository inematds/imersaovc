# Imersão Vibe Coding — Landing Promocional

Quatro versões de landing page de vendas para a **Imersão Vibe Coding** (Do Zero ao SaaS com IA em 3 dias, presencial em Itaguaí — RJ).

🔗 **Online (GitHub Pages):** https://inematds.github.io/imersaovc/

## Versões

| Arquivo | Estilo | Quando usar |
|---|---|---|
| [`versao-1.html`](versao-1.html) | **Conversão direta** — oferta, escassez, FAQ de objeções | tráfego quente / anúncio |
| [`versao-2.html`](versao-2.html) | **Storytelling** — jornada antes→depois, editorial, depoimentos | desejo e confiança |
| [`versao-3.html`](versao-3.html) | **Visual ousado** — hero animado, glass, contadores, motion | impacto imediato |
| [`versao-4.html`](versao-4.html) | **Imersão presencial (formato completo, recomendada)** — hero com vídeo, datas/local, "Acesso à Imersão Presencial", jornada dia a dia, barra de vagas | página de vendas pronta |

`index.html` é um seletor que mostra as 4. Quando decidir a versão final, **renomeie o arquivo dela para `index.html`**.

## Oferta (já configurada nas páginas)

- De **R$ 4.800** por **R$ 3.000** (oferta de lançamento)
- **Pagamento:** cartão de crédito em **até 6x**
- **20 vagas** por imersão

## Pagamento via Asaas

Mesmo padrão usado no **inema.vip**: o botão de CTA aponta direto para o **link de checkout do Asaas** (`https://www.asaas.com/c/XXXX`).

**Já configurado.** O botão "Garantir vaga" das 4 versões aponta para o link real:

- R$ 3.000 (cartão, até 6x) → `https://www.asaas.com/c/hkzrb361iks95c50`

Para trocar o link, basta substituir essa URL nos `versao-*.html`.

> 📄 **Como gerenciar os links pela API** (criar, mudar forma de pagamento, parcelas, valor, excluir): [`docs/asaas-links-pagamento.md`](docs/asaas-links-pagamento.md).

### Cobrança dinâmica (opcional)

Para gerar cobranças via API (customer + payment, webhook de confirmação, controle automático de vagas), o cliente de referência já existe em `inemaonline` (`worker/src/asaas.ts`): cria customer, cria payment, processa webhook `PAYMENT_CONFIRMED`/`PAYMENT_RECEIVED`.

## Editar datas/local

As páginas usam o local **Itaguaí — RJ, Rua Prefeito José Maria de Brito, 251** e o texto _"Próxima turma — datas a confirmar"_. Procure pelos comentários `EDITAR-DATAS` para colocar as datas reais.

## Stack

HTML estático + Tailwind (CDN) + Google Fonts. Sem build. Abre direto no navegador.

---
INEMA.CLUB · 2026
