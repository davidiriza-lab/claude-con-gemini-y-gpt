---
description: "Le pasa una tarea a GPT (Codex, con tu suscripción de ChatGPT) y trae el resultado. Para leer cosas enormes, segunda opinión de código o trabajo mecánico."
argument-hint: "[tarea para GPT]"
---

# `/gpt` — mandarle una tarea a GPT

El usuario quiere que GPT haga esta tarea: **$ARGUMENTS**

GPT corre con `~/.local/bin/codex-sub`, un atajo a Codex con la sesión de la suscripción
de ChatGPT del usuario. **No gasta API**: no uses `OPENAI_API_KEY`.

## Cuándo conviene (y cuándo no)

- **Sí:** leer mucho de una vez, una segunda opinión con ojos frescos (sobre todo de código),
  tareas mecánicas en lote.
- **No:** decisiones que dependen del criterio del usuario, ni nada que toque producción.

## Cómo llamarlo

```bash
~/.local/bin/codex-sub exec -C "<carpeta del proyecto>" --sandbox read-only --ephemeral \
  --skip-git-repo-check "<instrucción>" < /dev/null
```

Reglas probadas:

1. **Siempre `--sandbox read-only`:** puede leer y correr comandos que solo leen, pero no
   escribir nada.
2. **Siempre `< /dev/null`:** si no, se queda esperando texto.
3. **El contexto va dentro de la instrucción.** Codex lee `AGENTS.md`, no `CLAUDE.md`. Si la
   tarea depende de reglas del proyecto, pégalas en la instrucción.
4. **Carpetas extra:** `--add-dir <ruta>`.
5. **Modelo:** usa el de `~/.codex/config.toml`. Para cambiarlo: `-m <modelo>` o
   `-c model_reasoning_effort="high"`.
6. Para quedarte solo con la respuesta final: `-o <archivo>` y lee ese archivo.
7. Una llamada a la vez. Si va a tardar, en segundo plano.

## Al terminar

- No pegues la salida cruda: **verifica** lo que GPT afirma (puede equivocarse) y entrega la
  conclusión, diciendo qué vino de GPT.
- Si dice que no hay sesión: `~/.local/bin/codex-sub login` y entrar con la cuenta de ChatGPT.
- Cuota: `/cuota-gpt`.
