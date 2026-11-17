# Instrucciones para Claude: instalar claude-con-gemini-y-gpt

Sigue estos pasos en orden. Habla con el usuario en su idioma y sin tecnicismos. Solo espera al usuario donde se indica.

Si leíste este archivo con una herramienta que resume páginas, bájalo completo antes de seguir, porque los comandos van tal cual:
`curl -fsSL https://raw.githubusercontent.com/davidiriza-lab/claude-con-gemini-y-gpt/main/INSTALAR.md`

Reglas para toda la instalación:

- **Nunca reemplaces un archivo que ya existe sin preguntar.** El paso 3 te dice cómo revisarlo.
- No pidas contraseñas, llaves ni tokens. Los inicios de sesión los hace el usuario en su navegador.
- No instales nada que no esté en estos pasos.

## 1. Revisar el sistema

- `uname -s` debe ser `Darwin` (Mac) o `Linux`. En Windows (fuera de WSL) dile que no está probado y detente.
- `python3 --version` debe ser 3.9 o más nuevo. Si no hay Python 3, dile cómo instalarlo y detente. En una Mac nueva, `python3` puede abrir una ventana que ofrece instalar las "herramientas de línea de comandos": que acepte, espere a que termine y vuelve a probar.
- `curl --version` debe responder (viene en Mac y en casi todo Linux).

## 2. Preguntar qué suscripciones tiene

Pregúntale al usuario, en una sola pregunta, cuáles tiene:

- **ChatGPT** (Plus, Pro, Business…) → instala la parte de GPT (pasos 4 y 6). Le da `/imagen`, `/gpt` y `/cuota-gpt`.
- **Google AI Pro o Ultra** → instala la parte de Gemini (pasos 5 y 7). Le da `/gemini` y `/cuota-gemini`.

Con una sola basta: instala solo esa parte y sáltate los pasos de la otra. Las dos partes no dependen una de la otra, y la que falte se puede agregar después repitiendo esta instalación.

Si no tiene ninguna, explícale que esto sirve para usar esas suscripciones y detente.

## 3. Descargar los archivos

Qué archivos van con cada suscripción:

| Suscripción | Programas en `~/.local/bin/` | Comandos en `~/.claude/commands/` |
|---|---|---|
| ChatGPT | `codex-sub`, `imagen-gpt`, `cuota-gpt` | `imagen.md`, `gpt.md`, `cuota-gpt.md` |
| Google | `cuota-gemini` | `gemini.md`, `cuota-gemini.md` |

**3.1 Bájalos a una carpeta temporal** (todavía no se instala nada):

```bash
R=https://raw.githubusercontent.com/davidiriza-lab/claude-con-gemini-y-gpt/main
T="${TMPDIR:-/tmp}/claude-con-gemini-y-gpt"
mkdir -p "$T/bin" "$T/commands"
for f in codex-sub imagen-gpt cuota-gpt cuota-gemini; do
  curl -fsSL "$R/bin/$f" -o "$T/bin/$f" || echo "FALLÓ bin/$f"
done
for f in imagen gpt gemini cuota-gpt cuota-gemini; do
  curl -fsSL "$R/commands/$f.md" -o "$T/commands/$f.md" || echo "FALLÓ commands/$f.md"
done
ls -la "$T/bin" "$T/commands"
```

Si algo dice `FALLÓ`, revisa la conexión y repite. No sigas con archivos incompletos.

**3.2 Revisa qué hay ya en la computadora:**

```bash
T="${TMPDIR:-/tmp}/claude-con-gemini-y-gpt"
revisar() {
  if [ ! -e "$2" ]; then echo "NUEVO     $2"
  elif cmp -s "$1" "$2"; then echo "IGUAL     $2"
  else echo "DISTINTO  $2"; fi
}
for f in codex-sub imagen-gpt cuota-gpt cuota-gemini; do revisar "$T/bin/$f" "$HOME/.local/bin/$f"; done
for f in imagen gpt gemini cuota-gpt cuota-gemini; do revisar "$T/commands/$f.md" "$HOME/.claude/commands/$f.md"; done
ls -d "$HOME"/.claude/skills/imagen "$HOME"/.claude/skills/gpt "$HOME"/.claude/skills/gemini \
  "$HOME"/.claude/skills/cuota-gpt "$HOME"/.claude/skills/cuota-gemini 2>/dev/null || true
```

Fíjate solo en los archivos de las suscripciones que tiene:

- `NUEVO`: se instala.
- `IGUAL`: ya lo tiene al día; no hagas nada.
- `DISTINTO`: ya tiene un archivo con ese nombre y no es este. Puede ser una versión anterior de esto mismo o algo suyo. Dile en una línea cuál es y pregúntale: reemplazar o conservar. Si reemplaza, guarda antes una copia como `<archivo>.bak`. Si conserva, ese comando o programa se queda como estaba y esa parte puede no funcionar: dilo en el reporte final.
- Si el último `ls` muestra alguna carpeta, el usuario ya tiene una skill con el mismo nombre que uno de estos comandos, y en Claude Code la skill le gana al comando. Avísale y pregúntale qué prefiere antes de instalar ese comando.

**3.3 Instala** solo los que quedaron como `NUEVO` o que el usuario aceptó reemplazar. Para cada uno:

```bash
T="${TMPDIR:-/tmp}/claude-con-gemini-y-gpt"
mkdir -p ~/.local/bin ~/.claude/commands
# programa (ejemplo con imagen-gpt):
cp "$T/bin/imagen-gpt" ~/.local/bin/imagen-gpt && chmod +x ~/.local/bin/imagen-gpt
# comando (ejemplo con imagen.md):
cp "$T/commands/imagen.md" ~/.claude/commands/imagen.md
```

Al terminar, lista lo instalado con `ls -la` y borra la carpeta temporal (`rm -r "$T"`).

Revisa que `~/.local/bin` esté en el PATH (`echo $PATH`). Si no está, dile qué línea agregar a su `~/.zshrc` o `~/.bashrc`: `export PATH="$HOME/.local/bin:$PATH"`. No la agregues tú. Los comandos de Claude usan la ruta completa, así que funcionan igual.

## 4. GPT: encontrar Codex y revisar la sesión

1. `~/.local/bin/codex-sub --version`. Si dice que no encontró Codex, dale dos opciones:
   - instalar en VS Code la extensión **Codex – OpenAI's coding agent** (la más fácil si ya usa VS Code), o
   - en la terminal: `npm i -g @openai/codex` (necesita Node.js).

   Espera a que lo haga y vuelve a probar.
2. `~/.local/bin/codex-sub login status`.
   - Si dice **"Logged in using ChatGPT"**, sigue.
   - Si dice que usa una **API key**, explícale que así se cobra por API y que hay que entrar con su cuenta de ChatGPT.
   - Si no hay sesión, o usa API key: pídele que corra en su terminal `~/.local/bin/codex-sub login` y elija **Sign in with ChatGPT** en el navegador. **Espera** a que te diga que ya entró y vuelve a revisar `login status`.

## 5. Gemini: instalar Antigravity CLI e iniciar sesión

No uses el viejo `gemini` CLI ni una `GEMINI_API_KEY`: Google cerró ese CLI para cuentas personales el 18-jun-2026.

1. Revisa si ya existe: `command -v agy || ls ~/.local/bin/agy`.
2. Si no existe, instálalo con el instalador oficial. Si no tienes permiso para correrlo tú, dale el comando al usuario:
   ```bash
   curl -fsSL https://antigravity.google/cli/install.sh | bash
   ```
   Si el instalador abre solo la pantalla de inicio de sesión, ahí **no se puede escribir**. Dile que la cierre con `Ctrl+C`.
3. Iniciar sesión lo hace el usuario, en una terminal **nueva**:
   ```bash
   ~/.local/bin/agy
   ```
   Explícale los pasos: se abre el navegador → entra con la cuenta de Google que tiene la suscripción → copia el código que le muestra la página → lo pega en la terminal. Si la terminal es chica y no ve dónde pegar, que la agrande o baje con `Shift+↓`. Después elige un color (Enter), deja las opciones como vienen y sale con `/quit`. **Espera** a que te diga que ya entró.
4. Si ve un error de **"not eligible"**, es un problema conocido de Google con algunas cuentas de AI Pro. Dile que por ahora siga solo con la parte de GPT.

## 6. Probar GPT

1. `~/.local/bin/cuota-gpt` debe mostrar el plan y las barras.
2. Avísale que vas a hacer una imagen de prueba chiquita (gasta muy poco de su cuota; si prefiere no gastarla, sáltate este punto y dilo en el reporte) y córrela:
   ```bash
   ~/.local/bin/imagen-gpt "un círculo naranja sobre fondo blanco, plano y simple" -o /tmp/prueba-imagen-gpt.jpg --max 256
   ```
   Debe imprimir la ruta del archivo. Ábrelo y mira que sea un círculo naranja.

## 7. Probar Gemini

1. `~/.local/bin/cuota-gemini` debe mostrar las barras.
2. `~/.local/bin/agy -p "Responde solo: OK" --print-timeout 2m < /dev/null` debe contestar algo como "OK".

## 8. Opcional: que Claude haga siempre las imágenes así

Pregúntale si quiere que, en todos sus proyectos, Claude haga las imágenes con su suscripción en vez de la API. Si dice que sí, agrega a `~/.claude/CLAUDE.md` las secciones de abajo que apliquen a lo que instaló (la de imágenes solo si tiene ChatGPT). Créalo si no existe; si ya existe, agrégalas al final sin borrar nada, y si ya las tiene no las dupliques:

```markdown
## Imágenes: siempre con mi suscripción de ChatGPT
Toda imagen que se genere o edite con GPT va por `~/.local/bin/imagen-gpt` (comando `/imagen`), que gasta de mi suscripción y no de la API. La API de OpenAI para imágenes solo si yo lo autorizo en ese momento.

## Gemini y GPT: solo cuando yo lo pida
Usa `/gemini` y `/gpt` solo cuando yo lo diga. Puedes sugerirlo, pero no delegues por tu cuenta.
```

## 9. Reportar

Dile al usuario en pocas líneas:

- qué quedó instalado y qué parte no (y por qué),
- qué archivos suyos conservaste o reemplazaste (y dónde quedó la copia `.bak`),
- que abra Claude Code de nuevo para que carguen los comandos,
- y ejemplos para probar, solo de lo que instaló: `/imagen …`, `/gpt …`, `/cuota-gpt`, `/gemini …`, `/cuota-gemini`.
