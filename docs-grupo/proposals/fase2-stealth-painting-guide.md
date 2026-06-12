# Guía de Pintura — Fase 2: Stealth (los_fragmentos_del_s)

Escena: `scenes/quests/story_quests/los_fragmentos_del_s/1_stealth/los_fragmentos_del_s_stealth.tscn`

---

## Límites de cámara

- Izquierda: 0 px — Derecha: 2816 px
- Arriba: 0 px — Abajo: 1280 px
- Área útil de juego: 2816 × 1280 píxeles
- Tamaño de tile: 64 × 64 px → grilla de 44 × 20 tiles

---

## Mapa de corredores (diseño de layout)

El layout usa un patrón de corredores en zigzag (switchback) para obligar al jugador a pasar cerca de las rutas de las guardias.

```
[0,0]─────────────────────────────────────────────[44,0]
 │                                                     │
 │  ZONA A (spawn)     ZONA B (corredor alto)          │
 │  cols 0-11          cols 11-22                      │
 │  rows 5-11          rows 2-11                       │
 │                          │                          │
 │         checkpoint (col 26, row 7)                  │
 │                          │                          │
 │  ZONA C (corredor bajo)  ZONA D (cámara coleccionable)│
 │  cols 22-33              cols 33-44                 │
 │  rows 9-17               rows 10-18                 │
 │                                                     │
[0,20]────────────────────────────────────────────[44,20]
```

### Coordenadas de referencia (mundo → tile)

| Punto                     | Mundo (px)  | Tile (col, row) |
|---------------------------|-------------|-----------------|
| Spawn del jugador         | (131, 463)  | (2, 7)          |
| Checkpoint (medio)        | (1704, 451) | (26, 7)         |
| Coleccionable (meta)      | (2335, 955) | (36, 15)        |
| Guard1 inicio patrulla    | (200, 960)  | (3, 15)         |
| Guard1 fin patrulla       | (2600, 960) | (40, 15)        |
| Guard2 inicio patrulla    | (640, 640)  | (10, 10)        |
| Guard2 fin patrulla       | (2700, 640) | (42, 10)        |
| Guard3 punto A            | (2100, 900) | (32, 14)        |
| Guard3 punto B            | (2500, 850) | (39, 13)        |
| Guard3 punto C            | (2600, 1050)| (40, 16)        |
| Guard3 punto D            | (2200, 1100)| (34, 17)        |

---

## Rutas de patrulla de guardias

### Guard1 — Patrulla horizontal (sweep bajo)
- Camina entre `(200, 960)` y `(2600, 960)` — fila de tiles ≈ row 15
- Cubre toda la extensión horizontal en la mitad inferior
- **Rol en diseño**: bloquea la ruta baja directa; el jugador debe cruzar en los huecos de tiempo

### Guard2 — Patrulla horizontal (sweep medio)
- Camina entre `(640, 640)` y `(2700, 640)` — fila de tiles ≈ row 10
- Cubre el corredor intermedio
- **Rol**: bloquea el acceso al checkpoint y al corredor superior-derecho

### Guard3 — Patrulla circular (cámara del coleccionable)
- Recorre en bucle: A(2100,900) → B(2500,850) → C(2600,1050) → D(2200,1100) → A
- Perímetro alrededor del coleccionable en `(2335, 955)`
- **Rol**: obliga al jugador a esperar el hueco en la ronda para recoger el fragmento

---

## Capas de TileMap y qué pintar

### Orden de árbol (bottom → top en escena)
1. `Grass` — piso base (ya pintado, no modificar)
2. `Shadows` — sombras de geometría (ya pintado, no modificar)
3. `Stone` — plataformas y muros de piedra (ya pintado, no modificar)
4. **`Fence`** — lineas de valla/reja a lo largo de los corredores ← **pintar**
5. **`Decoration`** — props decorativos como cobertura ← **pintar**

### Tileset: Fence (`los_fragmentos_del_s_fence.tres`)
- Usa el terrain de valla para trazar líneas de separación entre corredores
- Pintar a lo largo de:
  - Separación Zona A / Zona B: cols 10-11, rows 4-11 (muro vertical con hueco en row 8 para paso)
  - Separación Zona B / Zona C: diagonal tiles cols 21-22, rows 9-14 (hueco en row 12)
  - Separación Zona C / Zona D: cols 32-33, rows 9-18 (hueco en row 15 para acceso al coleccionable)
- **Aspecto visual preferido**: aspecto de Cliff_Mines (oscuro, herrumbroso) para reforzar el mood nocturno
- La capa actúa como obstáculo visual; la colisión viene del tileset (physics ya configurado)

### Tileset: Decoration (`los_fragmentos_del_s_decoration.tres`)
- Pintar props como cobertura táctica para el jugador:
  - Zona A: 2-3 props en cols 3-8, rows 6-10 (arbustos o rocas)
  - Zona B: 2 props en cols 14-18, rows 4-9
  - Zona C: 2-3 props en cols 24-30, rows 11-16
  - Zona D (entrada): 1-2 props en cols 34-36, rows 11-14
- Modulate de la capa: `Color(0.85, 0.78, 0.98, 1)` — lavanda suave
- No colocar props que bloqueen completamente el paso; son coberturas, no muros

---

## Recordatorio: límites de cámara en el editor

Al pintar, revisar que ningún tile quede fuera de la región `(0,0)-(2816,1280)`. La cámara (`Camera2D` con `limit_right=2816`, `limit_bottom=1280`) nunca mostrará lo que esté fuera de esos límites.

---

## Checklist del editor (antes del pull request)

- [ ] Abrir `los_fragmentos_del_s_stealth.tscn` en el editor de Godot
- [ ] Re-guardar la escena para que el editor asigne `unique_id` a los nodos nuevos: `AtmosphereLayer`, `AtmosphereColorRect`, `Fog`, `BackgroundMusic`, `Guard3-CircleGuard`, `Guard3-CirclePath`
- [ ] Pintar la capa `Fence` con líneas de separación de corredores (ver tabla de coordenadas)
- [ ] Pintar la capa `Decoration` con props de cobertura en Zonas A-D
- [ ] Verificar en el panel de escena que `AtmosphereLayer` aparece **antes** de `ScreenOverlay` en el árbol
- [ ] Verificar que `AtmosphereColorRect` cubre toda la pantalla (anchors = full rect)
- [ ] Playtest: las guardias patrullan sus rutas nuevas ✓
- [ ] Playtest: la detección funciona (acercarse a una guardia activa el estado detected) ✓
- [ ] Playtest: el checkpoint en `(1704, 451)` activa el diálogo al tocarlo ✓
- [ ] Playtest: el coleccionable en `(2335, 955)` es alcanzable esperando el hueco en la ronda de Guard3 ✓
- [ ] Playtest: el shader `purple_night` aplica el tinte oscuro sin bloquear el HUD ✓
- [ ] Playtest: la música `Threadbare_Stealth.ogg` inicia al cargar la escena ✓
- [ ] Playtest: el fog tiene tinte morado y no interfiere con la jugabilidad ✓
