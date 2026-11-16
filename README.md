# Claude con Gemini y GPT

Ya pagas ChatGPT y Gemini. Con esto, Claude Code los usa como ayudantes **gastando de esas suscripciones**, no de tus tokens de Claude ni de una API que se cobra aparte.

- **`/imagen`**: Claude te hace y edita imágenes con GPT, y lo paga tu plan de ChatGPT.
- **`/gemini`** y **`/gpt`**: Claude les pasa una tarea (leer un repo entero, revisar código, resumir un PDF enorme) y te trae la conclusión ya revisada.
- **`/cuota-gemini`** y **`/cuota-gpt`**: ves cuánto te queda de cada suscripción, en barras y con la hora en que se recarga.

```
/cuota-gpt

GPT en Codex
  cada 5 horas  ██████████ 100%  bien   se recarga mañana 03:14
  semanal       ████████░░  80%  bien   se recarga dom 22 22:07
```

## Lo que necesitas

- **Claude Code** en Mac o Linux. En Windows no está probado.
- **Al menos una** de estas suscripciones:
  - **ChatGPT** (Plus o superior), para `/imagen`, `/gpt` y `/cuota-gpt`.
  - **Google AI Pro o Ultra**, para `/gemini` y `/cuota-gemini`.
- Python 3.9 o más nuevo (ya viene en Mac).

## Instalar: pídeselo a Claude

Pega esto en Claude Code:

```
Lee https://raw.githubusercontent.com/davidiriza-lab/claude-con-gemini-y-gpt/main/INSTALAR.md
y sigue sus instrucciones al pie de la letra.
```

Claude te pregunta qué suscripciones tienes, instala lo necesario, te guía para iniciar sesión (eso lo haces tú, con tu cuenta, en el navegador) y prueba que todo funcione. Al terminar, abre Claude Code de nuevo para que cargue los comandos.

## Usarlo

- `/imagen póster vertical con el texto "Gran apertura", fondo terracota, estilo editorial`
- `/imagen edita ~/Desktop/foto.jpg: quita el fondo y ponlo blanco`
- `/gemini lee toda la carpeta src y dime dónde se manda el correo de bienvenida`
- `/gpt revisa los cambios de esta rama y dime qué se podría romper`
- `/cuota-gpt` o `/cuota-gemini`

También funcionan en la terminal: `imagen-gpt`, `cuota-gpt`, `cuota-gemini`.

## Cómo funciona (y por qué no te cobran aparte)

| Suscripción | Herramienta oficial | Qué hace |
|---|---|---|
| ChatGPT | **Codex** (la extensión de OpenAI en VS Code o su programa de terminal), con la sesión de tu cuenta de ChatGPT | Trae una herramienta para hacer imágenes y un modo para recibir tareas sin abrir ventanas |
| Google AI Pro / Ultra | **Antigravity CLI** (`agy`), el reemplazo oficial del Gemini CLI | Recibe tareas sin abrir ventanas y te dice cuánta cuota te queda |

- **Las imágenes nunca pasan por la API.** `imagen-gpt` le quita a Codex tu `OPENAI_API_KEY` (si tienes una), así que no puede cobrarte por ahí aunque quiera.
- **Gemini y GPT solo leen.** Cuando Claude les pasa una tarea, no pueden modificar tus archivos ni correr comandos que cambien algo. Claude revisa lo que dicen antes de dártelo, porque pueden equivocarse.
- **Cada suscripción tiene sus límites**, normalmente un tanque cada 5 horas y otro semanal. Los ves con `/cuota-gpt` y `/cuota-gemini`. Como referencia, en una prueba tres imágenes gastaron alrededor del 1% del tanque de 5 horas de ChatGPT Plus.

## Cosas que ya nos pasaron

- **"El Gemini CLI no me deja entrar" (`UNSUPPORTED_CLIENT`).** Google lo dejó de servir para cuentas personales el 18 de junio de 2026. Usa Antigravity CLI (`agy`); el instalador lo pone por ti.
- **"El login de `agy` no me deja pegar el código."** Pasa si `agy` se abrió desde el instalador (`curl … | bash`): ahí no recibe teclado. Ciérralo con `Ctrl+C`, abre otra terminal y escribe `~/.local/bin/agy`.
- **"Escribo `codex` y dice que no existe, pero tengo la extensión."** La extensión trae Codex escondido en su carpeta. `codex-sub` lo encuentra solo.
- **Algunas cuentas de Google AI Pro reciben un error de "not eligible" en Antigravity.** Es un problema de Google sin arreglo publicado; si te pasa, sigue con la parte de ChatGPT.

## Qué hay en este repo

| Archivo | Qué es |
|---|---|
| `bin/codex-sub` | Encuentra Codex (en tu terminal o dentro de la extensión de VS Code, Cursor o Windsurf). |
| `bin/imagen-gpt` | Genera y edita imágenes con tu suscripción de ChatGPT. |
| `bin/cuota-gpt` | Cuánto te queda de ChatGPT en Codex. |
| `bin/cuota-gemini` | Cuánto te queda de Gemini en Antigravity. |
| `commands/*.md` | Los comandos `/imagen`, `/gpt`, `/gemini`, `/cuota-gpt` y `/cuota-gemini` para Claude Code. |
| `INSTALAR.md` | Lo que sigue Claude cuando le pides instalarlo. |

Licencia MIT. Hecho por [David Iriza](https://www.davidiriza.com/lab).
