# Instrucciones para Claude: instalar claude-con-gemini-y-gpt

Sigue estos pasos en orden. Habla con el usuario en su idioma y sin tecnicismos. Solo espera al usuario donde se indica.

## 1. Revisar el sistema

- `uname -s` debe ser `Darwin` (Mac) o `Linux`. En Windows (fuera de WSL) dile que no está probado y detente.
- `python3 --version` debe ser 3.9 o más nuevo. Si no hay Python 3, dile cómo instalarlo y detente.

## 2. Preguntar qué suscripciones tiene

Pregúntale al usuario, en una sola pregunta, cuáles tiene:

- **ChatGPT** (Plus, Pro, Business…) → instala la parte de GPT (pasos 4 y 6).
- **Google AI Pro o Ultra** → instala la parte de Gemini (pasos 5 y 7).

Si no tiene ninguna, explícale que esto sirve para usar esas suscripciones y detente.

## 3. Descargar los archivos

```bash
R=https://raw.githubusercontent.com/davidiriza-lab/claude-con-gemini-y-gpt/main
mkdir -p ~/.local/bin ~/.claude/commands
for f in codex-sub imagen-gpt cuota-gpt cuota-gemini; do
  curl -fsSL "$R/bin/$f" -o ~/.local/bin/$f && chmod +x ~/.local/bin/$f
done
for f in imagen gpt gemini cuota-gpt cuota-gemini; do
  curl -fsSL "$R/commands/$f.md" -o ~/.claude/commands/$f.md
done
ls -la ~/.local/bin/{codex-sub,imagen-gpt,cuota-gpt,cuota-gemini} ~/.claude/commands/{imagen,gpt,gemini,cuota-gpt,cuota-gemini}.md
```

Antes de descargar, revisa si ya existe alguno de esos archivos en `~/.claude/commands/`. Si existe y no viene de este repo, pregúntale al usuario si lo reemplazas. Si solo tiene una de las dos suscripciones, puedes saltarte los comandos de la otra.

Revisa que `~/.local/bin` esté en el PATH (`echo $PATH`). Si no está, dile qué línea agregar a su `~/.zshrc` o `~/.bashrc`: `export PATH="$HOME/.local/bin:$PATH"`. Los comandos de Claude usan la ruta completa, así que funcionan igual.

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
2. Avísale que vas a hacer una imagen de prueba chiquita (gasta muy poco de su cuota) y córrela:
   ```bash
   ~/.local/bin/imagen-gpt "un círculo naranja sobre fondo blanco, plano y simple" -o /tmp/prueba-imagen-gpt.jpg --max 256
   ```
   Debe imprimir la ruta del archivo. Ábrelo y mira que sea un círculo naranja.

## 7. Probar Gemini

1. `~/.local/bin/cuota-gemini` debe mostrar las barras.
2. `~/.local/bin/agy -p "Responde solo: OK" --print-timeout 2m < /dev/null` debe contestar algo como "OK".

## 8. Opcional: que Claude haga siempre las imágenes así

Pregúntale si quiere que, en todos sus proyectos, Claude haga las imágenes con su suscripción en vez de la API. Si dice que sí, agrega esto a `~/.claude/CLAUDE.md` (créalo si no existe; si ya existe, agrégalo al final sin borrar nada):

```markdown
## Imágenes: siempre con mi suscripción de ChatGPT
Toda imagen que se genere o edite con GPT va por `~/.local/bin/imagen-gpt` (comando `/imagen`), que gasta de mi suscripción y no de la API. La API de OpenAI para imágenes solo si yo lo autorizo en ese momento.

## Gemini y GPT: solo cuando yo lo pida
Usa `/gemini` y `/gpt` solo cuando yo lo diga. Puedes sugerirlo, pero no delegues por tu cuenta.
```

## 9. Reportar

Dile al usuario en pocas líneas:

- qué quedó instalado y qué parte no (y por qué),
- que abra Claude Code de nuevo para que carguen los comandos,
- y ejemplos para probar: `/imagen …`, `/gpt …`, `/gemini …`, `/cuota-gpt`, `/cuota-gemini`.
