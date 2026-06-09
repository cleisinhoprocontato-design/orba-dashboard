# Ponte de comandos - Apontamento Orba (voz via Claude iOS)

Para disparar a partir do Claude, edite `comandos/comando.json` com:

{ "acao": "rodar", "programado": "48.000", "id": "2026-06-09-1130" }

- acao: "rodar" (fluxo completo) ou "status" (so o ultimo resultado).
- programado: opcional, formato BR com ponto de milhar (ex "48.000"). Se omitir, reusa o ultimo salvo.
- id: OBRIGATORIO e UNICO por comando (use data-hora). So executa quando o id muda.

O PC le este arquivo a cada 30s, executa, e escreve a resposta em `comandos/resultado.json`.
Apos mandar "rodar", espere ~3 min e leia `comandos/resultado.json`.