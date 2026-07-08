[English](README.md) | [한국어](README.ko.md) | [中文](README.zh.md) | [日本語](README.ja.md) | Español

# godmode

<div align="center">
  <img src="assets/godmode-hero-01.png" width="860" alt="Mythic key-art: a lightning bolt strikes a Greek marble temple while a plasma loop orbits its columns; a lone developer with a laptop watches from the stairs below the carved GODMODE wordmark and a phosphor-green '> iddqd' cheat code">
</div>

```
> iddqd

GOD MODE ON.
Modo dios activado.
Tu idea ya no puede morir antes de lanzarse.
```

> **El código de trucos para terminar las cosas.** Actívalo, suelta una idea, y el bucle se niega a parar hasta que los propios documentos digan que está terminado.

Todos los proyectos mueren igual: 80% terminados, 0% lanzados. godmode elimina la opción de morir. Encadena entrevista → PRD → configuración de goal → ejecución de `/goal`, y luego martilla ciclos de **verificar → revisar → mejorar** hasta cumplir la Definición de Hecho (DoD): **todos los ítems de VALIDATION pasan Y cero supuestos bloqueantes**. Si se estanca, una escalera de recuperación cambia el enfoque en vez de rendirse. Las salidas anticipadas las atrapa el guardián y las devuelve al bucle.

No te pregunta si quieres continuar. Ese es precisamente el punto.

## Inicio rápido

### 1. Añade el marketplace

```
/plugin marketplace add fivetaku/godmode
```

### 2. Instala

```
/plugin install godmode@godmode
```

### 3. Introduce el código de trucos

```
/godmode hazme un temporizador pomodoro que me avergüence cuando abandono antes de tiempo
```

Eso es todo. Te avisará cuando esté terminado. (Lo deciden los documentos, no las sensaciones.)

## Las reglas del modo dios

1. **"Hecho" se define, no se siente.** DoD = todos los ítems de VALIDATION pasan + 0 supuestos bloqueantes. Escrito en documentos, verificado por el bucle.
2. **Nunca para antes de tiempo.** Estancamiento → reintento → cambio de enfoque con un revisor más profundo → compuerta de replanificación. Rendirse no está en la escalera.
3. **Microbucles antes que perfección de un solo tiro.** Cada etapa pasa con "suficiente para empezar" y el bucle aprieta la calidad sin descanso.
4. **Revisores intercambiables.** self-review (por defecto) / insane-review / codex / gjc — tú eliges al juez.

## Requisitos

- [Claude Code](https://claude.com/claude-code)
- Funciona mejor con el pipeline de [gptaku-plugins](https://github.com/fivetaku/gptaku_plugins) instalado (`show-me-the-prd`, `goaljaby`) — godmode los orquesta cuando están presentes.

## Linaje

godmode es la edición meme de [insane-loop](https://github.com/fivetaku/insane-loop) — el mismo motor, más actitud.

## Licencia

MIT — ver [LICENSE](LICENSE) y [DISCLAIMER](DISCLAIMER.md). El modo dios es una metáfora. Sigues siendo mortal y responsable de lo que lanzas.
