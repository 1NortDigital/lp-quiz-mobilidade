# lp-quiz-mobilidade

Quiz de diagnóstico da 1Nort para **lojas de mobilidade elétrica**, no padrão visual e de rastreamento do quiz.1nort.com.br.

Site estático: um único `index.html`, sem build. Publica em qualquer hospedagem estática (Cloudflare Pages, Hostinger).

## Fluxo

1. Quantos veículos elétricos a loja vende por mês
2. Função na loja (vendedor ou "outra função" encerram como desqualificado)
3. Já contou com agência de marketing
4. @ do Instagram da loja
5. Nome e WhatsApp (envia lead parcial)
6. E-mail (finaliza, tela "Analisando" e redirect pro WhatsApp)

## O que muda por funil

Tudo fica no topo do `<script>` em `index.html`:

- `BRAND`: logo, fundo, hero, links. As imagens são carregadas de `https://quiz.1nort.com.br`.
- `CONFIG`: `pixelId`, `webhookUrl`, `sheetsUrl`, `pagina`, eventos, valores, WhatsApp.
- `WELCOME`, `STEPS`, `ENDS`, `SUCESSO`, `LGPD`: textos e perguntas.

## Eventos (Meta Pixel)

| Momento | Evento | Quem |
|---|---|---|
| Envia nome + WhatsApp | `Contact` + `RespondiConversion` (R$25) | só qualificado |
| Fim do quiz | `Lead` (R$100 se top, R$25 se não) | só qualificado |
| Escolhe vendedor/outro | `LeadDesqualificado` | desqualificado |

Todos usam o mesmo `event_id`, que também vai no payload (`eventos`) para dedupe com a CAPI.

## Payload (Sheets + webhook)

Contém `pagina`, `nome`, `telefone`, `email`, `instagram`, `papel`, `qualificado`, `score`, `top_tier`, `tier` (`parcial` / `qualificado` / `top` / `desqualificado`), `respostas`, `fbc`, `fbp`, `eventos` e UTMs.

`pagina` = `lp-quiz-mobilidade-eletrica`, para o n8n/Apps Script separar dos leads do quiz solar.
