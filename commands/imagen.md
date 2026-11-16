---
description: "Genera o edita imágenes con GPT usando tu suscripción de ChatGPT (no la API de OpenAI)."
argument-hint: "[qué imagen quieres | edita <archivo>: qué cambiar]"
---

# `/imagen` — imágenes con tu suscripción de ChatGPT

El usuario quiere: **$ARGUMENTS**

Usa `~/.local/bin/imagen-gpt`. Corre la herramienta de imágenes que trae Codex con la sesión
de ChatGPT del usuario y le quita `OPENAI_API_KEY` a Codex, así que **no gasta API**.

```bash
~/.local/bin/imagen-gpt "<prompt>" -o <ruta/salida.png|.jpg> \
  [--formato cuadrado|vertical|horizontal] [--ref foto.jpg ...] [-n 1-4] [--max 1024]
```

- **Generar:** prompt + `-o`. Escribe un prompt detallado (sujeto, estilo, luz, encuadre).
  El texto que va dentro de la imagen va entre comillas y en el idioma final.
- **Editar o usar referencia:** `--ref <archivo>` (se puede repetir). Di qué cambia y qué se
  queda igual.
- **Variantes:** `-n 3` → `nombre-1`, `nombre-2`, `nombre-3`.
- **Para web:** salida `.jpg` y `--max 1024`.
- **Dónde guardar:** en la carpeta del proyecto donde se va a usar; si es una prueba, en una
  carpeta temporal.
- Tarda de 20 a 60 s por imagen. Se pueden lanzar varias en paralelo (no se cruzan), pero
  en lotes prueba 1 antes de lanzar las demás.

Después de generar, **mira la imagen** antes de entregarla y revisa que cumpla lo pedido. Si
falla, ajusta el prompt y repite.

Si falla por cuota (`/cuota-gpt`) o por sesión, avísale al usuario. No caigas a la API de
OpenAI sin que él lo autorice.
