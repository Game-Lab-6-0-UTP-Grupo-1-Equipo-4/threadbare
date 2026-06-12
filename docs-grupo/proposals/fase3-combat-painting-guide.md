# Guia de Pintado — Fase 3: Arena de Combate (los_fragmentos_del_s)

Escena objetivo: `scenes/quests/story_quests/los_fragmentos_del_s/2_combat/los_fragmentos_del_s_combat.tscn`

## Concepto visual

Arena de piedra en ruinas suspendida sobre el vacío. El piso central de piedra está rodeado de void, con plataformas elevadas en las esquinas y pasarelas de puente que conectan el centro con los bordes. Los 6 objetivos se ubican en plataformas elevadas para que el jugador deba navegar los puentes.

---

## Mapa de la arena (coordenadas mundo, tile 32 px)

La escena tiene límites de cámara: 960 × 640 px (30 × 20 tiles).

```
Tile (col, row) — origen (0,0) en esquina superior izquierda

Col:   0    4    8   12   16   20   24   28   30
Row 0  [VOID][VOID][VOID][VOID][VOID][VOID][VOID][VOID][VOID]
Row 2  [VOID][PLAT-IZQ-T ]    [   ][PLAT-CTR-T ][PLAT-DER-T][VOID]
Row 4  [VOID][VOID][PUENT][ARENA CENTRAL      ][PUENT][VOID]
Row 8  [VOID][    ][ARENA CENTRAL (centro)     ][    ][VOID]
Row 12 [VOID][VOID][PUENT][ARENA CENTRAL      ][PUENT][VOID]
Row 14 [VOID][PLAT-IZQ-B ]   [            ][PLAT-DER-B][VOID]
Row 16 [VOID][VOID][VOID][VOID][VOID][VOID][VOID][VOID][VOID]
Row 20 ------------------------------------------------
```

### Zonas y coordenadas clave (mundo px → tile col,row)

| Zona | Mundo (px) | Tile (col, row) | Capas |
|------|-----------|-----------------|-------|
| Arena central | x:256–704, y:192–448 | col 8–22, row 6–14 | Grass + Shadows + Stones |
| Plataforma IZQ superior | x:160–352, y:128–224 | col 5–11, row 4–7 | Stones (elevation) |
| Plataforma DER superior | x:608–800, y:128–224 | col 19–25, row 4–7 | Stones (elevation) |
| Plataforma CTR superior | x:352–608, y:96–192 | col 11–19, row 3–6 | Stones (elevation) |
| Plataforma IZQ inferior | x:160–352, y:416–544 | col 5–11, row 13–17 | Stones (elevation) |
| Plataforma DER inferior | x:608–800, y:416–544 | col 19–25, row 13–17 | Stones (elevation) |
| Puente N-IZQ | x:192–320, y:192–256 | col 6–10, row 6–8 | Bridges |
| Puente N-DER | x:640–768, y:192–256 | col 20–24, row 6–8 | Bridges |
| Puente S-IZQ | x:192–320, y:384–448 | col 6–10, row 12–14 | Bridges |
| Puente S-DER | x:640–768, y:384–448 | col 20–24, row 12–14 | Bridges |
| Void (resto) | fuera de zonas anteriores | — | Void (full-bleed) |

---

## Posiciones de nodos repositionados

| Nodo | Posición (px) | Zona |
|------|--------------|------|
| Player (spawn) | (348, 335) | Arena central |
| ThrowingNPC | (700, 300) | Borde derecho arena |
| CollectibleItem | (780, 300) | Detrás del NPC (accesible después del combate) |
| RepelPowerup | (160, 200) | Plataforma IZQ superior |
| Target 1 | (280, 160) | Plataforma IZQ superior |
| Target 2 | (680, 160) | Plataforma DER superior |
| Target 3 | (720, 480) | Plataforma DER inferior |
| Target 4 | (640, 480) | Plataforma DER inferior |
| Target 5 | (400, 160) | Plataforma CTR superior |
| Target 6 | (280, 480) | Plataforma IZQ inferior |

---

## Instrucciones de pintado por capa

### 1. Void (capa inferior, sin tile_map_data — pintar completa)
- **Tileset**: `los_fragmentos_del_s_void_chromakey.tres`
- **Qué pintar**: toda el área de la escena (30 × 20 tiles = 600 tiles)
- **Cómo**: usar la herramienta de fill (balde) para cubrir todo el canvas con el tile void
- **Resultado**: fondo oscuro de vacío bajo toda la escena

### 2. Grass (piso del arena central — tile_map_data existente, no tocar)
- Ya está pintado con el piso de la escena original
- Verificar que el área del arena central (col 8–22, row 6–14) tenga piso de pasto/piedra exterior

### 3. Shadows (sombras — tile_map_data existente, no tocar)
- Ya tiene sombras; verificar que queden coherentes con la nueva forma del arena

### 4. Stones / Elevation (plataformas elevadas)
- **Tileset**: `los_fragmentos_del_s_elevation_2.tres` (ya en la escena)
- **Qué pintar**: las 5 zonas de plataforma (ver tabla arriba)
  - Plataforma IZQ superior: col 5–11, row 4–7
  - Plataforma DER superior: col 19–25, row 4–7
  - Plataforma CTR superior: col 11–19, row 3–6
  - Plataforma IZQ inferior: col 5–11, row 13–17
  - Plataforma DER inferior: col 19–25, row 13–17
- **Terreno recomendado**: stone arena floor del tileset elevation_2
- **Nota**: estas plataformas deben tener física de piso (walkable) — ya configurado en el .tres

### 5. Bridges (pasarelas)
- **Tileset**: `los_fragmentos_del_s_bridges.tres`
- **Qué pintar**: 4 pasarelas (ver tabla arriba)
  - Puente N-IZQ: col 6–10, row 6–8 (conecta arena central con plataforma IZQ superior)
  - Puente N-DER: col 20–24, row 6–8 (conecta arena central con plataforma DER superior)
  - Puente S-IZQ: col 6–10, row 12–14 (conecta arena central con plataforma IZQ inferior)
  - Puente S-DER: col 20–24, row 12–14 (conecta arena central con plataforma DER inferior)
- **Física**: los puentes deben ser transitables (walkable bridge tiles)
- **Nota**: las pasarelas son el único camino hacia las plataformas elevadas donde están los objetivos

### 6. Decoration (capa superior — ruinas y dressing)
- **Tileset**: `los_fragmentos_del_s_decoration.tres`
- **Modulate**: Color(0.85, 0.78, 0.98, 1) — ya asignado en la escena
- **Qué pintar** (opcional, mejora visual):
  - Bordes rotos de las plataformas (2–3 tiles de ruinas en los bordes)
  - Columnas caídas en esquinas del arena central
  - Escombros decorativos en los extremos del void (col 0–3 y col 27–30)
- **Regla**: nada que bloquee el paso del jugador ni que tape los objetivos

---

## Shader AtmosphereLayer — valores aplicados (solo referencia)

La capa `AtmosphereLayer` ya está en la escena con intensidad baja para no obstaculizar el combate:

| Uniforme | Valor | Razón |
|----------|-------|-------|
| `filter_color` | Color(0.45, 0.30, 0.70, 1) | Tinte púrpura oscuro |
| `filter_intensity` | 0.25 | Bajo (mitad del default) — proyectiles visibles |
| `darkness` | 0.12 | Oscurecimiento mínimo |
| `vignette_strength` | 0.8 | Sutil |
| `enable_noise` | false | Desactivado — no interferir con proyectiles |

---

## Lista de verificación del editor

Abrir `los_fragmentos_del_s_combat.tscn` en Godot y seguir estos pasos en orden:

### Paso 1 — Re-guardar la escena
- [ ] File → Save (o Ctrl+S)
- [ ] Verificar que no hay dependencias rotas en el panel Scene
- [ ] El editor asignará `unique_id` a los nodos nuevos: DreamLight, AtmosphereLayer, AtmosphereColorRect, Void, Bridges, Decoration, Fog, BackgroundMusic

### Paso 2 — Pintar capa Void
- [ ] Seleccionar el nodo `Void` en la jerarquía
- [ ] En el editor de TileMap, usar fill completo (todo el canvas)
- [ ] Confirmar que el void cubre toda el área fuera de plataformas

### Paso 3 — Pintar Stones/Elevation (plataformas)
- [ ] Seleccionar el nodo `Stones`
- [ ] Pintar las 5 plataformas (ver coordenadas arriba)
- [ ] Verificar que las plataformas tienen física (Collision preview en el editor)

### Paso 4 — Pintar Bridges (pasarelas)
- [ ] Seleccionar el nodo `Bridges`
- [ ] Pintar las 4 pasarelas de conexión (ver coordenadas arriba)
- [ ] Verificar que los tiles de bridge tienen física walkable

### Paso 5 — Pintar Decoration (opcional)
- [ ] Seleccionar el nodo `Decoration`
- [ ] Agregar ruinas decorativas en bordes de plataformas y esquinas void
- [ ] No bloquear caminos ni objetivos

### Paso 6 — Verificar posiciones de nodos
- [ ] Target 1–6: verificar que cada uno está sobre una plataforma válida
- [ ] ThrowingNPC (700, 300): debe estar sobre piso del arena central
- [ ] RepelPowerup (160, 200): debe estar sobre plataforma IZQ superior
- [ ] Player spawn (348, 335): arena central
- [ ] CollectibleItem (780, 300): borde derecho del arena

### Paso 7 — Playtest
- [ ] La escena carga sin errores ni dependencias rotas
- [ ] Los 6 objetivos son alcanzables (el jugador puede llegar a todas las plataformas via puentes)
- [ ] Los proyectiles del ThrowingNPC son visibles a través del shader (no bloqueados por AtmosphereLayer)
- [ ] Los puentes son transitables (sin bloqueo de colisión)
- [ ] El fog se renderiza con tinte púrpura
- [ ] `Threadbare_Combat.ogg` comienza a reproducirse al cargar la escena
- [ ] El HUD no está afectado por el shader (ScreenOverlay y HUD están por encima de AtmosphereLayer)
- [ ] `DreamLight` tinta la escena con Color(0.70, 0.58, 0.86, 1) — storm purple
- [ ] El combate funciona normalmente: disparos, física, colisión de proyectiles, revelación del CollectibleItem

---

## Notas técnicas

- `AtmosphereLayer` tiene `layer = 0`, antes del `ScreenOverlay` en la jerarquía → el HUD no queda afectado
- `Fog` es una `Parallax2D` con `top_level = true` → se auto-gestiona, no requiere ajuste de posición
- Los nodos nuevos no tienen `unique_id` hasta el primer guardado en el editor — es comportamiento normal de Godot 4
- El tile_map_data de Grass y Shadows es el original — no modificar esos blobs existentes
