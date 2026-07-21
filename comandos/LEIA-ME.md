# JARVIS Orba - Ponte de comandos por voz (Claude iOS)

O operador fala uma frase pelo Claude iOS. O Claude escreve essa frase aqui no
repo; o PC (poller + JARVIS router) interpreta, executa a automacao certa e
devolve a resposta. Nada do PC fica exposto a internet.

## Como o Claude iOS executa QUALQUER comando

Escreva `comandos/comando.json` com a **frase exata** do operador em `texto`:

```json
{ "acao": "jarvis", "texto": "<frase exata do operador>", "id": "<AAAA-MM-DD-HHMMSS unico>" }
```

- `texto`: exatamente o que o operador falou. O JARVIS no PC decide o que fazer.
- `id`: SEMPRE unico (data-hora). O PC so executa quando o id muda.
- Se o operador disser um numero de programado, pode manda-lo tambem em
  `"programado": "48.000"` (formato BR), mas nao e obrigatorio.

Depois: ler `comandos/resultado.json`, conferir que `resultado.id` == o `id`
enviado, e devolver o campo `texto` ao operador (por voz).

## O que o JARVIS entende (fast-path, instantaneo)

| Frase do operador                         | O que faz                                   |
|-------------------------------------------|---------------------------------------------|
| "apontamento" / "como esta a producao"    | ultimo resultado da producao (instantaneo)  |
| "rodar apontamento [numero]"              | roda a automacao de apontamento (~3 min)    |
| "faltas" / "quem faltou hoje"             | relatorio consolidado de faltas (email RH)  |
| "dashboard" / "link"                      | link do painel de producao                  |
| "ajuda"                                   | lista de comandos                           |

Qualquer OUTRA pergunta ("quanto de frasco preciso comprar essa semana?") cai
no **cerebro** (Claude headless no projeto KPA). Se o CLI nao estiver logado, o
JARVIS avisa e sugere fazer login uma vez.

## Retrocompatibilidade

O formato antigo continua funcionando:
`{ "acao": "status|rodar", "programado": "<opcional>", "id": "..." }`
- "status" -> ultimo resultado.  "rodar" -> roda apontamento.

## Resposta (resultado.json)

```json
{ "id":"...", "acao":"...", "intent":"...", "estado":"ok|erro|...", "texto":"...", "horario":"..." }
```

`intent` mostra o que o JARVIS entendeu (apont_status, apont_rodar, faltas,
dashboard, ajuda, cerebro). O PC le `comando.json` a cada 30s.
