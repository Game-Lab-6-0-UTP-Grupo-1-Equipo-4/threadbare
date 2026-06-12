# Guía de Pintado — Fase 4: Escena del Puzzle de Secuencia

**Escena:** `scenes/quests/story_quests/los_fragmentos_del_s/3_sequence_puzzle/los_fragmentos_del_s_sequence_puzzle.tscn`

---

## 1. Layout de Islas — Posiciones de Nodos

El diseño "islas + puentes" ubica 6 islas perimetrales con una campana cada una, una isla central con el jugador y los carteles de pistas, y un área de agua central-sur con el ítem coleccionable.

### Coordenadas mundiales de referencia

| Elemento | Posición (px) | Tile coords (16px/tile) |
|---|---|---|
| **Isla Central** — jugador + 3 carteles | centro ~(480, 320) | tile ~(30, 20) |
| **Isla NW** — campana Blue | (220, 140) | tile ~(14, 9) |
| **Isla NE** — campana Pink | (680, 140) | tile ~(42, 9) |
| **Isla W** — campana Yellow | (150, 320) | tile ~(9, 20) |
| **Isla E** — campana Green | (760, 320) | tile ~(47, 20) |
| **Isla SW** — campana Purple | (220, 500) | tile ~(14, 31) |
| **Isla SE** — campana Red | (680, 500) | tile ~(42, 31) |
| **Área CollectibleItem** | (480, 580) | tile ~(30, 36) |
| **Cartel HintSign1** | (440, 300) | tile ~(27, 19) |
| **Cartel HintSign2** | (480, 295) | tile ~(30, 18) |
| **Cartel HintSign3** | (520, 300) | tile ~(32, 19) |
| **Sign (texto eco)** | (480, 350) | tile ~(30, 22) |

Los límites de cámara son: izquierda=-64, arriba=-64, derecha=1024, abajo=704. Todo el layout cabe dentro.

---

## 2. Guía de Pintado por Capa

Abrir la escena en el editor Godot y pintar **en este orden** (de abajo hacia arriba en el árbol `TileMapLayers`):

### 2.1 VoidChromakey (primera capa — fondo animado)

- **Tileset:** `los_fragmentos_del_s_void_chromakey.tres`
- **Qué pintar:** relleno completo del área de la escena, cubriendo todo el espacio fuera de las islas. Actúa como fondo de abismo animado.
- **Área sugerida:** desde tile (-4, -4) hasta tile (64, 44), cubriendo toda la cámara con margen.
- **Terreno a usar:** void animado (chromakey). Usar `Select` en el panel TileMap para elegir la tile del void animado.

### 2.2 Water (segunda capa)

- **Tileset:** `los_fragmentos_del_s_water.tres`
- **Qué pintar:** pool de agua central-sur, alrededor del CollectibleItem.
- **Área sugerida:** rectángulo de aprox. tiles (24, 32) a (36, 40), centrado en tile ~(30, 36).
- **Nota:** el `tile_map_data` actual ya está pintado — revisar si cubre el área central-sur. Si no, agregar agua ahí.

### 2.3 Foam

- **Tileset:** `los_fragmentos_del_s_foam_2.tres`
- **Qué pintar:** borde de espuma alrededor del agua pintada en la capa Water.

### 2.4 Bridges (nueva capa)

- **Tileset:** `los_fragmentos_del_s_bridges.tres`
- **Qué pintar:** puentes que conectan cada isla perimetral con la isla central, y entre islas adyacentes si se desea.
- **Rutas de puentes sugeridas:**
  - Isla NW → Central: diagonal NW-SE, aprox. tiles (16,11) a (28,18)
  - Isla NE → Central: diagonal NE-SW, aprox. tiles (40,11) a (32,18)
  - Isla W → Central: horizontal, aprox. tiles (11,20) a (28,20)
  - Isla E → Central: horizontal, aprox. tiles (33,20) a (46,20)
  - Isla SW → Central: diagonal SW-NE, aprox. tiles (16,29) a (28,22)
  - Isla SE → Central: diagonal SE-NW, aprox. tiles (40,29) a (32,22)
  - Central → CollectibleItem: vertical, aprox. tiles (30,22) a (30,34)
- **Herramienta:** usar el modo "Line" del editor TileMap para trazar los puentes.

### 2.5 Grass (capa existente — islas)

- **Tileset:** `los_fragmentos_del_s_exterior_floors.tres`
- **Qué pintar:** superficie de cada isla (piedra exterior). Usar terreno `Stone` del exterior_floors.
- **Tamaño por isla:** aprox. 3×3 tiles centrados en la posición de cada campana/jugador.
  - Isla Central: tiles ~(28,18) a (32,22)
  - Isla NW: tiles ~(12,7) a (16,11)
  - Isla NE: tiles ~(40,7) a (44,11)
  - Isla W: tiles ~(7,18) a (11,22)
  - Isla E: tiles ~(45,18) a (49,22)
  - Isla SW: tiles ~(12,29) a (16,33)
  - Isla SE: tiles ~(40,29) a (44,33)
  - Área CollectibleItem: tiles ~(28,34) a (32,38)

---

## 3. Notas Técnicas

- **unique_id de nodos nuevos:** los nodos `VoidChromakey`, `Bridges`, `Fog`, `AmbientAnimation`, y los 7 `ShineParticles` fueron agregados manualmente al `.tscn`. El editor Godot asigna `unique_id` automáticamente al re-guardar. **Paso obligatorio: abrir la escena y re-guardar** antes de hacer playtest.

- **tile_map_data en VoidChromakey y Bridges:** estas capas no tienen datos pintados todavía (`tile_map_data` vacío). El pintado se hace completamente en el editor.

- **Capas con tile_map_data existente:** `Water`, `Foam`, `Grass`, `Sand` ya tienen datos. No borrar esos datos; pintá encima o agregá en la misma capa según sea necesario.

---

## 4. Checklist de Verificación

Después de pintar, verificar:

- [ ] **VoidChromakey** cubre todo el fondo fuera de las islas sin huecos visibles
- [ ] **Agua** aparece en el área central-sur, alrededor del CollectibleItem
- [ ] **Puentes** conectan visualmente la isla central con cada una de las 6 islas perimetrales
- [ ] **Islas** (Grass/Stone) tienen superficie caminable bajo cada campana y bajo el jugador
- [ ] Re-guardar la escena en Godot (File → Save Scene) para asignar unique_id a nodos nuevos
- [ ] Playtest: el jugador spawna en la isla central
- [ ] Playtest: el jugador puede caminar a cada isla por los puentes
- [ ] Playtest: cada campana se puede activar (llegando a ella)
- [ ] Playtest: la secuencia del puzzle sigue funcionando (Yellow → Green → Blue, etc.)
- [ ] Playtest: el CollectibleItem aparece en el área sur al resolver el puzzle
- [ ] Playtest: `AmbientAnimation` corre (luz DreamLight pulsa suavemente en lavanda)
- [ ] Playtest: niebla (`Fog`) visible con tono violeta
- [ ] Playtest: partículas `ShineParticles` visibles en cada campana y el CollectibleItem

---

## 5. Valores de Color de Referencia

| Elemento | Color |
|---|---|
| DreamLight (CanvasModulate) | `Color(0.86, 0.80, 1.0, 1)` — lavanda etéreo |
| DreamLight pulso mínimo | `Color(0.81, 0.75, 0.95, 1)` |
| DreamLight pulso máximo | `Color(0.91, 0.85, 1.0, 1)` |
| Water modulate | `Color(0.52, 0.45, 0.85, 1)` — violeta-azul profundo |
| Foam modulate | `Color(0.82, 0.75, 0.96, 1)` — lavanda suave |
| Fog modulate | `Color(0.75, 0.65, 0.95, 1)` — violeta niebla |
| ShineParticles modulate | `Color(0.7, 0.5, 1, 1)` — violeta brillante |
