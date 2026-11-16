---
description: "Le pasa una tarea a Gemini (Antigravity CLI, con tu suscripción de Google) y trae el resultado. Para leer cosas enormes, segunda opinión o trabajo mecánico."
argument-hint: "[tarea para Gemini]"
---

# `/gemini` — mandarle una tarea a Gemini

El usuario quiere que Gemini haga esta tarea: **$ARGUMENTS**

Gemini corre con `agy` (Antigravity CLI), con la sesión de la suscripción de Google del
usuario (Google AI Pro o Ultra). **No gasta API**: no uses `GEMINI_API_KEY` ni el viejo
`gemini` CLI (Google lo dejó de servir a cuentas personales el 18-jun-2026).

## Cuándo conviene (y cuándo no)

- **Sí:** leer mucho de una vez (carpetas o repos enteros, PDFs, transcripciones largas),
  una segunda opinión con ojos frescos, tareas mecánicas en lote.
- **No:** decisiones que dependen del criterio del usuario, ni nada que toque producción.

## Cómo llamarlo

Desde la carpeta del proyecto:

```bash
cd "<carpeta del proyecto>" && agy -p "<instrucción>" --print-timeout 10m < /dev/null
```

Si `agy` no está en el PATH, usa `~/.local/bin/agy`.

Reglas probadas:

1. **Siempre solo lectura.** Termina la instrucción con "No modifiques ningún archivo ni
   ejecutes comandos de terminal; solo lee y responde." En modo sin pantalla los comandos
   de terminal se niegan solos; leer archivos sí funciona.
2. **No uses `--mode plan`:** en ese modo Gemini ignora lo que le pides y responde sobre
   sus propias instrucciones.
3. **El contexto va dentro de la instrucción.** Gemini no lee `CLAUDE.md`. Si la tarea
   depende de reglas del proyecto, pégalas en la instrucción. Para pasarle un texto:
   `T="$(cat archivo)"` y mételo en el `-p`.
4. **Carpetas fuera del proyecto:** `--add-dir <ruta>` (se puede repetir).
5. **Modelo:** hay uno por defecto. Para algo pesado o rápido, cámbialo con `--model <id>`;
   la lista sale con `agy models`.
6. Una llamada a la vez. Si va a tardar, en segundo plano.

## Al terminar

- No pegues la salida cruda: **verifica** lo que Gemini afirma contra los archivos (puede
  equivocarse) y entrega la conclusión, diciendo qué vino de Gemini.
- Si `agy` dice que no hay sesión, pídele al usuario que abra `agy` en una terminal, entre
  con su cuenta de Google y salga con `/quit`.
- Cuota: `/cuota-gemini`.
