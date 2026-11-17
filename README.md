# Claude con Gemini y GPT

Claude Code usa tus suscripciones de ChatGPT y Gemini: imágenes, tareas y cuota, sin gastar API ni tokens de Claude.

Ya pagas ChatGPT o Gemini. Con esto, Claude Code los usa como ayudantes **gastando de esas suscripciones**, no de una API que se cobra aparte. Es para quien usa Claude Code y tiene al menos una de las dos; no necesitas saber programar, Claude lo instala.

- **`/imagen`**: Claude te hace y edita imágenes con GPT, y lo paga tu plan de ChatGPT.
- **`/gemini`** y **`/gpt`**: Claude les pasa una tarea (leer un repo entero, revisar código, resumir un PDF enorme) y te trae la conclusión ya revisada.
- **`/cuota-gemini`** y **`/cuota-gpt`**: ves cuánto te queda de cada suscripción, en barras y con la hora en que se recarga.

```
/cuota-gpt

GPT en Codex
  cada 5 horas  ██████████ 100%  bien   se recarga mañana 03:14
  semanal       ████████░░  80%  bien   se recarga dom 22 22:07
```

## Instalar: pega esto en Claude Code

```
Lee https://raw.githubusercontent.com/davidiriza-lab/claude-con-gemini-y-gpt/main/INSTALAR.md
y sigue sus instrucciones al pie de la letra.
```

Claude te pregunta qué suscripciones tienes, instala solo lo que te sirve, te guía para iniciar sesión (eso lo haces tú, con tu cuenta, en el navegador) y prueba que todo funcione. Si ya tienes un archivo con el mismo nombre, te pregunta antes de tocarlo. Al terminar, abre Claude Code de nuevo para que cargue los comandos.

## Qué necesitas

- **Claude Code** en Mac o Linux. En Windows no está probado.
- **Al menos una** de estas suscripciones (con una basta; la otra parte simplemente no se instala):
  - **ChatGPT** (Plus o superior), para `/imagen`, `/gpt` y `/cuota-gpt`.
  - **Google AI Pro o Ultra**, para `/gemini` y `/cuota-gemini`.
- **Python 3.9 o más nuevo.** En Mac ya viene; si es la primera vez que se usa, la Mac ofrece instalar sus herramientas de línea de comandos y hay que aceptar.

Opcional:

- **VS Code con la extensión de Codex (OpenAI)**: es la forma más fácil de tener Codex. Sin VS Code, se instala con `npm i -g @openai/codex` (necesita Node.js).
- **ImageMagick**, solo en Linux, para convertir a JPG o achicar imágenes. En Mac no hace falta.

## Cómo se usa

- `/imagen póster vertical con el texto "Gran apertura", fondo terracota, estilo editorial`
- `/imagen edita ~/Desktop/foto.jpg: quita el fondo y ponlo blanco`
- `/gemini lee toda la carpeta src y dime dónde se manda el correo de bienvenida`
- `/gpt revisa los cambios de esta rama y dime qué se podría romper`
- `/cuota-gpt` o `/cuota-gemini`

También funcionan en la terminal: `imagen-gpt`, `cuota-gpt`, `cuota-gemini`.

Si quieres armar tu propio panel o script, `cuota-gpt --json` y `cuota-gemini --json` dan los mismos datos en JSON (cuánto queda de cada tanque y cuándo se recarga).

## Qué trae

| Archivo | Qué es |
|---|---|
| `bin/codex-sub` | Encuentra Codex (en tu terminal o dentro de la extensión de VS Code, Cursor o Windsurf). |
| `bin/imagen-gpt` | Genera y edita imágenes con tu suscripción de ChatGPT. |
| `bin/cuota-gpt` | Cuánto te queda de ChatGPT en Codex. |
| `bin/cuota-gemini` | Cuánto te queda de Gemini en Antigravity. |
| `commands/*.md` | Los comandos `/imagen`, `/gpt`, `/gemini`, `/cuota-gpt` y `/cuota-gemini` para Claude Code. |
| `INSTALAR.md` | Lo que sigue Claude cuando le pides instalarlo. |

Los programas quedan en `~/.local/bin/` y los comandos en `~/.claude/commands/`.

### Cómo funciona (y por qué no te cobran aparte)

| Suscripción | Herramienta oficial | Qué hace |
|---|---|---|
| ChatGPT | **Codex** (la extensión de OpenAI en VS Code o su programa de terminal), con la sesión de tu cuenta de ChatGPT | Trae una herramienta para hacer imágenes y un modo para recibir tareas sin abrir ventanas |
| Google AI Pro / Ultra | **Antigravity CLI** (`agy`), el reemplazo oficial del Gemini CLI | Recibe tareas sin abrir ventanas y te dice cuánta cuota te queda |

- **Las imágenes nunca pasan por la API.** `imagen-gpt` le quita a Codex tu `OPENAI_API_KEY` (si tienes una), así que no puede cobrarte por ahí aunque quiera.
- **Gemini y GPT solo leen.** Cuando Claude les pasa una tarea, no pueden modificar tus archivos ni correr comandos que cambien algo. Claude revisa lo que dicen antes de dártelo, porque pueden equivocarse.
- **Cada suscripción tiene sus límites**, normalmente un tanque cada 5 horas y otro semanal. Los ves con `/cuota-gpt` y `/cuota-gemini`. Como referencia, en una prueba tres imágenes gastaron alrededor del 1% del tanque de 5 horas de ChatGPT Plus.

## Si algo falla

- **No aparecen los comandos.** Abre Claude Code de nuevo; los comandos se cargan al arrancar.
- **`/imagen` o `/cuota-gpt` dicen que es problema de sesión.** En tu terminal: `~/.local/bin/codex-sub login` y elige entrar con tu cuenta de ChatGPT (no con una API key).
- **"Escribo `codex` y dice que no existe, pero tengo la extensión."** La extensión trae Codex escondido en su carpeta. `codex-sub` lo encuentra solo.
- **`/imagen` falla porque Codex no acepta el modelo.** `imagen-gpt` usa el primer modelo que Codex lista para tu cuenta. Para forzar otro: `IMAGEN_GPT_MODELO=<modelo> imagen-gpt …`.
- **"El Gemini CLI no me deja entrar" (`UNSUPPORTED_CLIENT`).** Google lo dejó de servir para cuentas personales el 18 de junio de 2026. Usa Antigravity CLI (`agy`); el instalador lo pone por ti.
- **"El login de `agy` no me deja pegar el código."** Pasa si `agy` se abrió desde el instalador (`curl … | bash`): ahí no recibe teclado. Ciérralo con `Ctrl+C`, abre otra terminal y escribe `~/.local/bin/agy`.
- **`/cuota-gemini` dice que `agy` no devolvió la cuota.** Casi siempre es la sesión: abre `agy` en una terminal, entra con tu cuenta de Google y sal con `/quit`.
- **Algunas cuentas de Google AI Pro reciben un error de "not eligible" en Antigravity.** Es un problema de Google sin arreglo publicado; si te pasa, sigue con la parte de ChatGPT.
- **Actualizar o agregar la otra suscripción.** Pega otra vez el texto de instalación. Lo que ya está al día no se toca.
- **Quitarlo.** Borra `codex-sub`, `imagen-gpt`, `cuota-gpt` y `cuota-gemini` de `~/.local/bin/`, y `imagen.md`, `gpt.md`, `gemini.md`, `cuota-gpt.md` y `cuota-gemini.md` de `~/.claude/commands/`.

## Guía completa

Paso a paso: **https://davidiriza.com/lab/claude-con-chatgpt-y-gemini**

## Licencia

MIT. Hecho por [David Iriza](https://www.davidiriza.com/lab).
