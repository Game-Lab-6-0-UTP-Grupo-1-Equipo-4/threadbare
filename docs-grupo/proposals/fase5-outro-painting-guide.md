# Guía de Pintado — Fase 5: 4_outro (Violet Dawn / Ruinas en el Vacío)

Escena objetivo: `scenes/quests/story_quests/los_fragmentos_del_s/4_outro/los_fragmentos_del_s_outro.tscn`

---

## Concepto visual

El outro representa el amanecer violeta sobre las ruinas que se disuelven en el vacío.
El jugador llega al claro donde los fragmentos han sido recolectados.
Los bordes del mapa se desintegran en el vacío animado (chromakey),
mientras el centro conserva el camino de tierra y los árboles intactos.
La iluminación cálida-violácea (DreamLight Color 0.95/0.84/0.93) pulsa suavemente a lo largo de la escena.

---

## Capas a pintar

### 1. VoidChromakey (capa de vacío — bordes del mapa)

**Tileset:** `los_fragmentos_del_s_void_chromakey.tres`

**Concepto:** Las ruinas se disuelven hacia el vacío en los bordes exteriores.
Pintar el material de vacío a lo largo de los bordes norte, este y oeste del mapa,
dejando el corredor central (el camino Ground + Path) completamente libre.

**Límites del mapa:** 960 × 540 px (límites de cámara). Tile size: 64 × 64 px.

| Zona | Columnas (tile) | Filas (tile) | Descripción |
|------|-----------------|--------------|-------------|
| Borde norte | 0–14 | 0–1 | Franja superior — ruinas cayendo al vacío |
| Borde oeste | 0–1 | 0–8 | Columna izquierda |
| Borde este | 13–14 | 0–8 | Columna derecha |
| Esquina SO | 0–2 | 6–8 | Extensión del borde oeste en la franja inferior |
| Esquina SE | 12–14 | 6–8 | Extensión del borde este en la franja inferior |

> Dejar libre la zona central (columnas 2–12, filas 2–6) donde se ubican el camino, el personaje y los árboles.

---

### 2. Elevation (capa de piedra — restos de arquitectura)

**Tileset:** `los_fragmentos_del_s_elevation_2.tres`
**Modulación aplicada en escena:** `Color(0.80, 0.72, 0.94, 1)` — tono piedra lavanda.

**Concepto:** Bloques de piedra dispersos que representan los restos de una estructura
que se desmorona. Se pintan en grupos pequeños, separados, entre los árboles y el borde del vacío.

**Posiciones sugeridas (coordenadas mundo, px):**

| Grupo | Centro aproximado | Descripción |
|-------|-------------------|-------------|
| Ruinas NO | (80, 180) | Arco de piedra caído junto al borde del vacío |
| Ruinas NE | (860, 200) | Escalón fragmentado |
| Ruinas SO | (110, 440) | Bloque hundido cerca del borde inferior-izquierdo |
| Ruinas SE | (840, 460) | Fragmento de muro parcialmente en el vacío |
| Ruinas centro-norte | (480, 120) | Plataforma baja al fondo del claro |

> Usar el terreno de piedra exterior (exterior_floors / Stone terrain) disponible en el tileset.
> Mantener los grupos pequeños (3–5 tiles cada uno) para evitar bloquear el paso del jugador.
> Los tiles de piedra en los bordes pueden superponerse parcialmente con la zona de VoidChromakey
> para lograr el efecto de "ruina disolviéndose en el vacío".

---

## Mapa conceptual de zonas (coordenadas tile, origen 0,0 = esquina superior izquierda)

```
[0  ][1  ][2  ][3  ][4  ][5  ][6  ][7  ][8  ][9  ][10 ][11 ][12 ][13][14]
[VOID][VOID][...][...][...][...][...][...][...][...][...][...][...][VOID][VOID]  fila 0
[VOID][VOID][...][...][...][...][...][...][...][...][...][...][...][VOID][VOID]  fila 1
[VOID][VOID][ camino libre — Ground + Path — árboles ]          [VOID][VOID]  fila 2
[VOID][VOID][        personaje y props de decoración           ][VOID][VOID]  fila 3–5
[VOID][VOID][...][...][...][...][...][...][...][...][...][...][...][VOID][VOID]  fila 6
[VOID][VOID][...][...][...][...][...][...][...][...][...][...][...][VOID][VOID]  fila 7–8
```

---

## Props de decoración ya colocados (texto de escena)

Los siguientes nodos ya están en la escena bajo `OnTheGround` (sin pintar — solo instancias):

| Nodo | Posición mundo (px) | Tipo |
|------|---------------------|------|
| Rock | (200, 400) | Roca estática |
| Rock2 | (680, 410) | Roca estática |
| Bush | (140, 360) | Arbusto animado |
| Bush2 | (740, 370) | Arbusto animado |
| Flower | (300, 430) | Flor |
| Flower2 | (560, 420) | Flor |
| ShineParticles | (480, 320) | Partículas violáceas (punto focal) |

No se requiere pintar estos nodos. Se asignarán unique_id automáticamente al abrir y re-guardar la escena en el editor.

---

## Checklist del editor

### Paso 1 — Abrir y re-guardar
- [ ] Abrir `los_fragmentos_del_s_outro.tscn` en el editor de Godot.
- [ ] Verificar que no hay dependencias rotas (panel inferior sin errores en rojo).
- [ ] Guardar la escena sin cambios (`Ctrl+S`) para que el editor asigne `unique_id` a los nodos nuevos:
  `DreamLight`, `AmbientAnimation`, `VoidChromakey`, `Elevation`, `BackgroundMusic`,
  `Rock`, `Rock2`, `Bush`, `Bush2`, `Flower`, `Flower2`, `ShineParticles`.

### Paso 2 — Pintar VoidChromakey
- [ ] Seleccionar la capa `VoidChromakey` en el panel Scene.
- [ ] Pintar los bordes del mapa según la tabla de zonas de arriba.
- [ ] Verificar que el corredor central queda libre (sin tiles de vacío).

### Paso 3 — Pintar Elevation
- [ ] Seleccionar la capa `Elevation` en el panel Scene.
- [ ] Pintar los 5 grupos de ruinas de piedra según las posiciones sugeridas.
- [ ] Confirmar que la modulación violácea (`Color(0.80, 0.72, 0.94, 1)`) es visible en el editor.

### Paso 4 — Re-guardar la escena
- [ ] Guardar la escena después del pintado.

### Paso 5 — Playtest
- [ ] Ejecutar la escena desde el editor.
- [ ] **Diálogo/cinemática:** confirmar que la secuencia de diálogo y la cinemática de salida se disparan correctamente y completan sin errores.
- [ ] **Árboles:** confirmar que el Forest/AreaFiller y los 87 árboles existentes siguen visibles y sin z-order roto.
- [ ] **DreamLight:** confirmar que la tinta violeta-rosada (`Color(0.95, 0.84, 0.93)`) tiñe el mapa sin bloquear la HUD.
- [ ] **Pulso AmbientAnimation:** confirmar que el color oscila suavemente entre el valor base y `Color(1, 0.89, 0.98, 1)` en un ciclo de 7 s.
- [ ] **Música:** confirmar que `The_Void_Bloom.ogg` comienza a reproducirse al cargar la escena.
- [ ] **Props:** confirmar que rocas, arbustos y flores son visibles sin problemas de z-order.
- [ ] **ShineParticles:** confirmar que las partículas violáceas emiten de forma continua en (480, 320).
- [ ] **VoidChromakey:** confirmar que los tiles de vacío en los bordes se ven animados (material chromakey activo).
- [ ] **Elevation:** confirmar que los grupos de ruinas de piedra aparecen con el tono lavanda esperado.
- [ ] **Colisiones:** confirmar que el jugador no queda atrapado en los tiles de piedra (el personaje en la escena de outro es decorativo, pero verificar que la cinemática se completa).

---

## Notas técnicas

- La capa `VoidChromakey` tiene un material chromakey ya configurado en el tileset `.tres`. No es necesario modificar el tileset.
- La capa `Elevation` usa `los_fragmentos_del_s_elevation_2.tres` (tileset de piedra/acantilados). Los terrenos de física ya están configurados.
- `BackgroundMusic` usa el script `background_music.gd` que inicia la reproducción automáticamente en `_ready()`.
- `AmbientAnimation` tiene `autoplay = "ambient"` — el pulso arranca sin intervención adicional.
- Los nodos sin `unique_id` en el texto de escena recibirán su ID al guardar en el editor. Esto es comportamiento normal de Godot; no indica un error.
