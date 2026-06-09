# Ponte de comandos - Apontamento Orba (gatilho de voz pelo Claude iOS)

## Gatilhos (frases que o operador fala)
- "apontamento pa"            -> RODAR o fluxo completo e devolver o resultado.
- "apontamento pa, programado 48.000" -> rodar usando esse programado (formato BR, ponto de milhar).
- "status apontamento"        -> NAO roda; devolve o ultimo resultado em cache (instantaneo).

## Como o Claude executa o gatilho
1. Editar `comandos/comando.json` neste repo com:
   { "acao": "rodar", "programado": "<opcional>", "id": "<AAAA-MM-DD-HHMMSS unico>" }
   - acao "rodar" para "apontamento pa"; acao "status" para "status apontamento".
   - programado: incluir SO se o operador disser um numero; senao omitir (reusa o ultimo salvo).
   - id: SEMPRE unico (data-hora). O PC so executa quando o id muda.
2. Avisar o operador: "rodando, leva ~3 min" (para "rodar"); status e quase instantaneo.
3. Depois de ~3 min (ou ~30s no status), ler `comandos/resultado.json`.
   - Conferir que `resultado.id` == o `id` que foi enviado (garante que e a resposta certa).
   - Devolver o campo `texto` ao operador.

## Resposta (resultado.json)
{ "id":..., "acao":..., "estado":"ok|erro|faltou_programado", "texto":"...", "horario":"..." }

O PC le `comando.json` a cada 30s. Tudo roda no servidor da producao; nada do PC fica exposto.