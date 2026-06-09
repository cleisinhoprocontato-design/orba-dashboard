# Ponte de comandos - Apontamento Orba (gatilho de voz pelo Claude iOS)

## Gatilhos (frases que o operador fala)
- "apontamento pa"            -> STATUS: NAO roda; so devolve o ultimo resultado (instantaneo).
- "rodar apontamento"         -> RODAR o fluxo completo (~3 min) e devolver o resultado.
- "rodar apontamento, programado 48.000" -> rodar usando esse programado (formato BR, ponto de milhar).

## Como o Claude executa o gatilho
1. Editar `comandos/comando.json` neste repo com:
   { "acao": "status|rodar", "programado": "<opcional>", "id": "<AAAA-MM-DD-HHMMSS unico>" }
   - "apontamento pa" -> acao "status".
   - "rodar apontamento" -> acao "rodar". programado SO se o operador disser um numero; senao omitir (reusa o ultimo salvo).
   - id: SEMPRE unico (data-hora). O PC so executa quando o id muda.
2. status e quase instantaneo (~30s); "rodar" avisa "rodando, leva ~3 min".
3. Ler `comandos/resultado.json`, conferir que `resultado.id` == o `id` enviado, e devolver o campo `texto`.

## Resposta (resultado.json)
{ "id":..., "acao":..., "estado":"ok|erro|faltou_programado", "texto":"...", "horario":"..." }

O PC le `comando.json` a cada 30s. Tudo roda no servidor da producao; nada do PC fica exposto.