# 🏰 The Chronicles of Naradak — Juego Completo + Mapas 2.5D (tilemap/)

Tu RPG táctico medieval refactorizado en módulos + un mapa isométrico interactivo como sorpresa.

## 📦 Contenido

```
naradak/
├── index.html                 # Juego principal (RPG táctico)
├── tilemap/                   # TODOS los mapas (HTML + JS + sprites)
│   ├── mapa.html              # Pueblo
│   ├── *_mapa.html            # Interiores: taberna, gremio, herreria, hospital, alquimia, tienda, castillo, entrenamiento, muelle
│   ├── js/                    # Motores *_mapa.js, *_assets.js (base64), assets.js, chars.js
│   └── sprites/               # Sprites sueltos (PNG/JPG) de los mapas
├── css/
│   └── styles.css            # Estilos globales
└── js/
    ├── data/                 # Datos estáticos (clases, items, mundo, loot, gremio, recetas)
    │   ├── clases.js         # Clases, atributos, habilidades
    │   ├── items.js          # 105 items (armaduras, armas, pociones)
    │   ├── mundo.js          # Biomas, enemigos, jefes, maldiciones
    │   ├── loot.js           # Tablas de loot + loot por bioma
    │   ├── gremio.js         # Rangos y 175 misiones
    │   ├── forja.js          # Recetas de herrería (Raro a Legendario)
    │   └── alquimia.js       # Recetas de alquimia
    ├── core/                 # Motor del juego
    │   ├── estado.js         # Estado global G, guardado (5 slots), hooks
    │   ├── combate.js        # Oleadas, botín, maldiciones y pociones
    │   ├── danos.js          # Tipos de daño y ataques especiales
    │   └── combate_velocidad.js # Velocidades base (el combate es en tiempo real: js/demo/combate_demo.js)
    ├── sistemas/             # Sistemas de juego
    │   ├── profesiones.js    # Crafteo y profesiones (en desarrollo)
    │   ├── taberna.js        # Tiempo, taberna, rumores, encargos
    │   ├── equipamiento.js   # Equipo, venta, reemplazo
    │   └── talentos.js       # Ramas de talentos + almas (Nv50+)
    ├── ui/                   # Renderizado de pantallas
    │   ├── render.js         # UI de todas las pantallas
    │   └── personaje.js      # Inventario y talentos
    └── main.js               # Arranque del juego
```

## 🎮 Cómo usar

1. **Abrir el juego**: Abre `index.html` en tu navegador
   - Crea un personaje, elige clase, combate, explora misiones

2. **Ciudad = Mapa**: en el campamento pulsa **"Ir a la Ciudad"** y se abre el mapa del pueblo (el botón flotante se eliminó)
   - Cada local abre su interfaz del juego: Tienda, Gremio, Herrería, Alquimista, Hospital, Taberna y Castillo (Modo Aventura)
   - Clic en el edificio (camina y entra) o **E** cerca de él; "← Campamento" vuelve al hub
   - Los botones "Volver" de cada local regresan al mapa
   - El reloj del mapa se sincroniza con el del juego

3. **Personaje**: pestañas Inventario · Materiales · Talentos (Materiales lista todo lo disponible con cantidad y precio)

## 🎨 Características del mapa

- **Isométrico 2.5D**: Proyección isométrica con profundidad visual
- **Sprites extraídos**: Árboles, arbustos, rocas, flores, edificios (de tus hojas)
- **Física simple**: Colisiones, pathfinding A* para movimiento
- **Dinámico**: 
  - Día/noche con ciclo real: 1 día = ~4.3 min en tiempo real
  - Iluminación nocturna (lucirnagas, luces en edificios)
  - Recolección de minerales
- **Interacción**: Haz click en edificios, se abre un panel con opciones
- **Personaje**: Renderizado en isométrico con arma visible

## 📋 Diferencias vs. original

El código se dividió en **19 archivos modulares**:
- Datos en `js/data/` (7 archivos)
- Motor en `js/core/` (4 archivos)
- Sistemas en `js/sistemas/` (4 archivos)
- UI en `js/ui/` (2 archivos)
- Mapas en `tilemap/` (HTML + `js/` + `sprites/`)

**Orden de carga**: datos → core → sistemas → ui → mapa → main

**Verificado**: Cada archivo pasa revisión de sintaxis; carga sin errores.

## 🛠 Desarrollo

Para editar el código:
1. Cada módulo es independiente (léel comentario de cabecera)
2. El estado global está en `js/core/estado.js` (objeto `G`)
3. Render en `js/ui/render.js` llama a todas las pantallas
4. Los mapas viven aislados en `tilemap/` (motor del pueblo: `tilemap/js/mapa.js`)

**Tomar nota**: El sistema de profesiones está **inhibido** (`PROF_ACTIVO=false`); para reactivarlo editá `js/sistemas/profesiones.js` línea 1.

## 📸 Sprites usados

Extraídos y escalados de tus hojas:
- **Árboles** (16 tipos), arbustos (5), rocas (7)
- **Minerales** (4 tipos de cristales con colores)
- **Edificios** (tienda, herrería, alquimista, gremio, hospital, taberna, forja, puestos)
- **Decoración** (farol, mesa, puesto de vendedor)
- **Banner y pergaminos** para el UI

Total: **67 assets** en `assets.js` (incrustados en base64, ~470 KB)

## 🚀 Próximas mejoras

- [ ] Conectar mapa con el juego (ir a Taberna desde el mapa abre la misma Taberna)
- [ ] NPCs caminando
- [ ] Combate aleatorio en el mapa
- [ ] Quest markers en el mapa
- [ ] Guardado de progreso persistente entre mapa y juego

---

**Hecho con**: vanilla JS + canvas isométrico

Abrí los archivos y disfrutá explorar Naradak desde dos perspectivas: el RPG táctico y el mapa vivo del pueblo. 🎭

## 🧑\u200d🌾 Aldeanos (nuevo)
- Jugador y 14 aldeanos usan tus 3 hojas de personajes (`tilemap/js/chars.js`, 150 sprites: 82 humanos jugables/aldeanos, 60 monstruos y 8 animales; ver `CHARS_INFO`).
- Elige tu personaje en la pantalla de inicio (◀ ▶). Los aldeanos caminan solos; **E** o clic para hablar.

## 🍺 Interior de la Taberna (nuevo)
- Al entrar a la Taberna desde el mapa se abre `tilemap/taberna_mapa.html` (interior 2.5D). Sprites recortados de tus hojas en `tilemap/js/taberna_assets.js`.
- Personajes fijos; clic (o **E** cerca) abre la interfaz correspondiente:
  - **Gorm** (tabernero, tras la barra) → Barra · Escuadrón
  - **Ayla** (mapa de expediciones) → Expediciones
  - **Pim** (tablón de encargos) → Tablón
  - **Encapuchado** (mesa del rincón) → Rumores
- Avisos sobre los NPC: ✔ expedición lista, nº de rumores y de encargos.
- La puerta (pared izquierda), **Esc** o "← Ciudad" vuelven al mapa del pueblo; "Volver" de la Taberna regresa al interior.

## 🏰 Interior del Castillo (nuevo)
- Al entrar al Castillo desde el mapa se abre `tilemap/castillo_mapa.html` (sala 24×18, ~4× el área de la Taberna, con cámara que sigue al jugador). Sprites recortados de tus hojas en `tilemap/js/castillo_assets.js`.
- Generales de Guerra (sprites grandes 48×72), posiciones fijas:
  - **General Aldric** (mesa de guerra) → abre el **Modo Aventura**
  - **General Valka**, **General Brom**, **Mariscal Ysolde** y el **Rey Eadric** → diálogo
- La puerta (pared izquierda), **Esc** o "← Ciudad" vuelven al mapa del pueblo; "Volver" del Modo Aventura regresa al interior del Castillo (`G.screen="castint"`).

## 🎯 Área de Entrenamiento (nuevo)
- Edificio junto al Castillo (celdas 24–26 × 2–4, botón **Entrenar**). Se entra con clic / **E**; abre `tilemap/entrenamiento_mapa.html` (pantalla `entint`, ver `irLocal` en `js/sistemas/taberna.js`).
- Interior 2.5D de 30×22 celdas con cámara. Sprites recortados de tus hojas en `tilemap/js/entrenamiento_assets.js`; motor en `tilemap/js/entrenamiento_mapa.js`.
- **Zona de Combate** (izquierda): 15 maniquíes (madera, armadura y espantapájaros), armeros, bloques de práctica, banco de trabajo y yunque. **General Roderic** en la arena de duelo.
- **Zona de Arquería** (derecha): 6 carriles con 12 dianas (lejanas y medias), línea de tiro, bancos, mesa y puesto. **General Thalia** tras la línea de tiro.
- Divisoria de vallas con portón central entre ambas zonas. Puerta (pared izquierda), **Esc** o "← Ciudad" vuelven al pueblo.

## ⚓ Muelle del lago (nuevo)
- En el mapa del pueblo: pasarela de tablones desde el camino sur hasta la casa del embarcadero sobre el lago, con bote, farol-pez, cajas y barril (sprites de tu hoja 263187).
- Al entrar se abre `tilemap/muelle_mapa.html` (`G.screen="muint"`): sala 13×10 con puerta a la izquierda, redes, grúa, bote y mesa.
- Personajes fijos (solo conversación, sin oferta todavía): Capitán Marek, Viejo Orlan, Tessa, Brugo y Nilda.
- Salida: puerta, Esc o "← Ciudad" → mapa del pueblo.

## 🗺 Carpeta `tilemap/`
- Contiene todos los mapas: `tilemap/mapa.html` (pueblo) y los interiores `tilemap/*_mapa.html`, sus motores y sprites en `tilemap/js/` y `tilemap/sprites/`.
- Los HTML de `tilemap/` cargan `../css/styles.css` y `../js/sistemas/sonido.js`; el juego los abre por iframe desde `MAPA_URL` en `js/sistemas/taberna.js`.
- `js/sistemas/sonido.js` calcula la ruta de `assets/audio/` a partir de su propia URL, por lo que el audio funciona desde `index.html` y desde `tilemap/`.
- Para agregar un mapa nuevo: copiar un `*_mapa.html`, crear `tilemap/js/<nombre>_mapa.js` y `<nombre>_assets.js`, y registrarlo en `MAPA_URL` e `irLocal`.

## 🕳 Explorar Mazmorra — combate en tiempo real con mapas (CoN_v2_039)
- **Explorar Mazmorra** (hub) abre el combate 2.5D del demo (`js/demo/combate_demo.js`, función `mazmorraExplorar`) con tu personaje real: stats, armas, armaduras, accesorios, atributos, talentos, maldiciones, vida/recurso actuales y pociones del inventario (Q = vida, E = recurso).
- Cada enemigo caído da XP, botín, almas y progreso de misión. Victoria → avanza de piso (`finCombateVictoria`) · Derrota → maldición y hub (`finCombateMuerte`) · **Huir** → −10 min y vuelves al hub.
- Oleadas: piso normal = `MZ_OLEADAS` (3) · pisos 5/10 = 1 oleada con jefe · Extremo = 5 oleadas (equivale a las 5 etapas).
- Mapas (`mazMapaDe(piso)`): **Cripta de las Antorchas** (pisos 1, 3) · **Galería de las Celdas** (2, 4) · **Osario Olvidado** (6, 8) · **Cámara de los Tormentos** (7, 9) · **Santuario del Guardián** (5 y, por ahora, 10). Las decenas impares (11-19, 31-39, 51-59) usan otro juego de mapas: **Biblioteca Maldita** (pisos impares) y **Mina Colapsada** (pisos pares); las decenas pares (1-9, 21-29, 41-49, 61-69) usan Cripta/Galería/Osario/Tormentos. El mapa de jefe del piso 10 será distinto.
- Nuevos mapas: crear `mapaMazX()` con `mazBase(...)` y registrarlo en `MAPAS` + `mazMapaDe()`.
- **Trampas** (todos los mapas de mazmorra, `M.tr(tipo,x,y,ancho,alto,{ph})` en celdas; datos y balance en `TRD`, global `TUNE.trampas`, 0 = sin daño; se desactivan en modo Muñeco):
  - **Pinchos**: ciclo de aviso rojo (0.8 s) → pinchos fuera (0.7 s) · 9 % de tu vida máx. (14 % a un enemigo normal).
  - **Llamas**: conducto de fuego con aviso naranja (1 s) y 1.5 s de llamas, daño por pulsos (3.5 % por pulso).
  - **Glifo arcano**: se activa al pisarlo —tú o un enemigo—, explota a los 0.6 s (15 % + silencio 1.5 s; a enemigos 22 % + aturdimiento) y se recarga 7 s. Útil para atraer enemigos.
  - **Veneno**: charco que daña por pulsos y ralentiza (jugador −35 %, enemigos −30 %).
  - Dañan también a los enemigos (jefes ×0.25, mini-jefes ×0.5); el daño al jugador respeta RD, escudo e inmunidad (esquivar/inmune). Los enemigos no aparecen sobre una trampa y las zonas de entrada quedan libres.
  - Colocación: Cripta 7 · Galería 9 · Osario 9 · Tormentos 9 · Biblioteca 9 · Mina 10 · Santuario 6. Los mapas también aparecen en el selector del Demo de Combate.
- El Modo Aventura del Castillo también usa el combate en tiempo real. El combate clásico por turnos (pantalla `combat`) se eliminó del juego.

## 🌲 Mapas exteriores por bioma + corrección de monstruos
- Hoja 259908 (`tilemap/js/chars.js`, índices 112-119) re-cortada a cuadro 48×72 (monstruos ya no salen cortados/corridos). Reglas `SPR_REGLAS`/`SPR_ESC`/`SPR_TOP` en `js/demo/combate_demo.js`.
- 12 mapas nuevos (bosques, mina, lago, nieve, ceniza, desierto, marchito) con `extBase`; selección por bioma en `MZ_BIOMAS` (mezclados con mazA-F).

## 🌐 Servidor local + Modo Online (en desarrollo)
- `server/servidor.js` (Node, sin dependencias): sirve el juego y un WebSocket `/ws` en la red local, sin internet. Guía: `server/LEEME.md`. Lanzadores: `server/iniciar.bat` / `server/iniciar.sh`.
- Protocolo JSON: `hola`, `lugar`, `mover`, `chat`, `info` → `bienvenido`, `entra`, `sale`, `mueve`, `lugar`, `chat`. Mensajes `grupo_*`/`mision_*` responden `en_desarrollo` (flag `MISIONES_GRUPO_ACTIVO`).
- Cliente: `js/online/online.js` (flag `ONLINE_ACTIVO=true`). Hub: botón "Modo Online" y Gremio: pestaña "Misiones en grupo", ambos marcados "en desarrollo".
- Se quitó Tailwind CDN (sin uso) y las fuentes de Google ahora cargan sin bloquear.

## 🏙 Oakhaven — ciudad enorme (nuevo)
- `tilemap/oakhaven_mapa.html` + `tilemap/js/oakhaven_mapa.js`: mapa 2.5D de 144×104 celdas con plaza mayor (Gremio, Hospital, Tienda, Taberna, Herrería, Alquimista), castillo y área de entrenamiento al norte, ~100 comercios (entrables), ~650 casas, mercados Norte/Sur con ~200 puestos, jardines, puerto con muelle y 1200 habitantes que caminan por las calles. Pocos árboles (solo borde y jardines).
- Generación determinista (semilla fija): todos los jugadores ven la misma ciudad. Minimapa (clic para ir), zoom (rueda, +/−), Shift para correr, ciclo día/noche.
- Online: abrir con `?online=1` desde el servidor local; muestra a los demás jugadores (lugar `oakhaven` en `/ws`), nombres, burbujas de chat (Enter). Con `?embed=1` emite `enter`/`exit`/`ready` por `postMessage` como los demás mapas (integración al juego pendiente: registrar en `MAPA_URL`/`irLocal`).

## 🎖 General de la plaza de Oakhaven (nuevo)
- **General Varek** (sprite de General 48×72, `GEN` en `tilemap/js/oakhaven_mapa.js`) está fijo en la Plaza Mayor, a la derecha de la espada central (celda 74,44), con una "!" dorada, nombre visible, marcador amarillo en el minimapa y un claro libre de aldeanos a su alrededor.
- Hablar: acercarse y **E**, o clic sobre él (camina hasta él). El diálogo ofrece dos opciones (también con las teclas **1** y **2**):
  1. 🧭 **Salir a explorar**
  2. 🕳 **Entrar en las Mazmorras del Reino de Artaros**
- **Salir a explorar** abre el Mundo Abierto (pantalla `mundoint`); **Mazmorras** abre el Laberinto de Artaros. Ambas opciones las gestiona el juego (`Oakhaven.onGeneral` devuelve `true`).
- Para conectar los mapas: en el modo embebido el mapa envía al juego `{naradak:"general",op:"explorar"|"mazmorras"}` (ver `Oakhaven.onGeneral` en `tilemap/oakhaven_mapa.html`). Cuando el juego gestione la opción, `Oakhaven.onGeneral` debe devolver `true` (cierra el diálogo) y `js/sistemas/taberna.js` (bloque `G.screen==="oakint"`) debe recibir `d.naradak==="general"`.
- El sprite (`c:80`) debe ser `k:"humano"` en `chars.js`; los índices 84 y 86 son "monstruo" y Oakhaven no los carga.

## 🕳 Laberinto de Artaros — mazmorra inmensa (nuevo)
- **Entrada única**: General Varek (Oakhaven) → "Entrar en las Mazmorras del Reino de Artaros" (el botón del HUB se eliminó). Abre la pantalla `laberinto` (`renderLaberinto`, en `js/sistemas/equipamiento.js`) → `mazmorraLaberinto()` (`js/demo/combate_demo.js`, bloque "LABERINTO").
- **Mapa**: 105×77 celdas generado por semilla (`labGenerar`): pasillos de 3 celdas sinuosos con lazos, ~40 salas (cripta, osario, cámara de tormentos, biblioteca, almacén, sala de guardia, cámaras del mecanismo), **bloques de calabozos** con celdas enrejadas, **cámaras del tesoro** selladas y el **Santuario del Guardián** (jefe). Se valida que siempre sea resoluble (`labResoluble`). Estética de las criptas/celdas del juego (props `pilar`, `sarcofago`, `altar`, `mesa`, `brasero`, `huesos`, `e_librero`…).
- **Palancas y compuertas** (color = pareja palanca↔compuerta): 1 palanca (roja, azul), **varias palancas** a la vez (verde 2, dorada 3 → puerta del jefe), **temporizadas** (cámaras del tesoro, 24 s), **palanca de celdas** (abre todas las celdas: emboscada o cofre), y **palancas plateadas que desactivan las trampas** de un tramo. Al accionar, la cámara muestra la compuerta y el minimapa la marca. Ajustes en `LB`.
- **Trampas**: las del juego (`TRD`: pinchos, llamas, glifo, veneno) en pasillos y salas.
- **Enemigos**: duermen y despiertan por distancia de camino (no a través de muros) o al ser atacados; las manadas se avisan. Más fuertes cuanto más profundo. Mini-jefes guardan palancas/tesoros; el jefe usa el jefe del bioma del piso actual.
- **Minimapa con niebla**: isométrico (misma orientación que la vista), solo muestra lo explorado (línea de visión; las salas se revelan al entrar). **Tab / N / clic** = mapa grande (pausa). **F** (o tocar el objeto) = interactuar con palancas, cofres y la escalera de salida.
- **Progreso** (`j.lab`): semilla, explorado, palancas, cofres y bajas se guardan al subir la escalera de la entrada (o con eventos clave). Al morir: maldición como en el resto del juego, reapareces en la entrada conservando el progreso. Al vencer al Guardián: oro, materiales y `j.labHechos++`; la siguiente expedición es un laberinto nuevo.
- Archivos tocados: `js/demo/combate_demo.js`, `js/ui/render.js`, `js/sistemas/equipamiento.js`, `js/sistemas/taberna.js`, `js/sistemas/sonido.js`, `tilemap/oakhaven_mapa.html`.

## ✚ Clérigo — clase de apoyo (nuevo)
- **Clase `Clérigo`** (recurso **Fe**, daño mágico a distancia, color turquesa): vida 190 · daño 10-15 · def 14 · crítico 8%. Atributos primarios ENF/INT/EVA (ENF = potencia de buffs, reducción mágica y crítico). Se trata como clase mágica vía `esMagica()` (`js/data/clases.js`).
- **Habilidades base**: **Pulso Vital** (onda en área: 110% daño sagrado + cura 25% vida máx) · **Égida Radiante** (+40% DEF y escudo 20% vida máx, 3 turnos) · **Plegaria de Pureza** (purga aturdimiento/silencio, inmune a control y +25% daño, 3 turnos).
- **Ramas (Nv 50)**, definidas en `RAMAS["Clérigo"]` (`js/sistemas/talentos.js`):
  - **Hierofante** (apoyo): Santuario de Luz (Nv60: −35% daño recibido, regeneración, escudo y purga) · Resurgir Divino (Nv80: cura 50%, +30% DEF, +20% daño, inmune a control). Talento nuevo `cura` (+% curación y escudos).
  - **Heraldo Solar** (DPS): Nova Solar (Nv60: 2 explosiones en área que además te curan) · Ascensión Radiante (Nv80: +35% daño y 10% robo de vida, purga el control).
- Campos nuevos de habilidades de rama: `hl` (cura instantánea), `df` (DEF %), `pur` (purga control), `lh` (cura por enemigo golpeado), `rb` (robo de vida del buff). Funcionan en el combate en tiempo real (`js/demo/combate_demo.js`) y en el clásico (`combate.js`, `talentos.js`).
- Equipo: 35 ítems de tienda (`items.js`) y 18 recetas de forja (`forja.js`); recetas de Taberna vía `TAB_BASES`. Héroes del demo: Seraphine y Elara. 7 íconos nuevos en `assets/hab/`.

## 👤 Cuentas y guardado en el servidor (nuevo)
- `login.html` (registro/entrada), `js/online/cuenta.js` (primer script de `index.html` y de todos los `tilemap/*.html`) y las rutas `/api/registro|login|datos|yo` de `server/servidor.js`.
- El juego sigue usando `localStorage` (`naradak_*`) como copia de trabajo; `cuenta.js` la carga desde el servidor al abrir y envía cada cambio al servidor. Detalles y administración en `server/LEEME.md`.
- `js/online/online.js` ahora se carga antes de `js/ui/personaje.js` (corrige `renderConectando is not defined`).

## 🌲 Mundo Abierto — bosques y ruinas (en construcción por sectores)
- **Entrada**: General Varek → "Salir a explorar" → pantalla `mundoint` (`tilemap/mundo_mapa.html` + `tilemap/js/mundo_mapa.js`, iframe como Oakhaven). Siempre se aparece en **Home** (Claro del Campamento): fogata animada, 5 carpas, faroles, estandartes, 4 soldados con diálogo y cartel de regreso a Oakhaven (**E**).
- **Plano**: cuadrícula 5×5 de sectores de 88×88 celdas (mundo de 484×484), con cinturones de bosque/roca infranqueables entre sectores. Home está en la esquina superior izquierda:

  | | | | | |
  |---|---|---|---|---|
  | **Home** | 2 | 3 | 9 | 10 |
  | 1 | 4 | 8 | 11 | 18 |
  | 5 | 7 | 12 | 17 | 19 |
  | 6 | 13 | 16 | 20 | 23 |
  | 14 | 15 | 21 | 22 | 24 |

- **Construidos**: **Home**, **Sector 1 · Bosque del Camino Viejo**, **Sector 2 · Aldea Abandonada de los Vigías** (aldea en ruinas con cabañas, capilla y molino; puesto de vigilancia en ruinas al suroeste, círculo de menhires al noreste y pequeño cementerio al sureste) y **Sector 3 · Necrópolis de los Olvidados** (suelo oscuro, gran cementerio central, templo en ruinas al noroeste y círculo de menhires al sureste; se llega desde Home → Sector 2 → Sector 3) y **Sector 4 · Ciudadela Quebrada** (fortaleza central amurallada con torres, anillo de muralla en ruinas con huecos junto a los caminos, templo en ruinas al noroeste, círculo de menhires al noreste, cementerio al sureste y pequeño fuerte al suroeste; se llega desde el Sector 1 (este) o el Sector 2 (sur)). Antes de esto sólo existían Home y Sector 1. (justo al sur de Home: bosque con claros, ruinas sueltas, ~7 senderos con ruinas menores y un puesto de vigilancia en ruinas en el centro). El resto de sectores aparecen como vacío sin explorar (números atenuados en el minimapa).
- **Unión Home ↔ Sector 1**: el camino del campamento termina en una **Puerta de piedra** (4 pilares + portal con runa) al sur de Home; al cruzarla (caminando sobre la runa o con **E**) se llega a la puerta norte del Sector 1, y viceversa. Fundido de 0,7 s y 2,5 s de enfriamiento.
- **Caminos sellados**: cada sector construido genera un camino hacia cada vecino de la cuadrícula; si el vecino aún no existe, el camino termina en una puerta sellada por un muro (E para examinar). Sector 1: este → Sector 4 y sur → Sector 5 (abiertos). Sector 5: norte → Sector 1 (abierto); este → Sector 7 y sur → Sector 6 (sellados). Sector 2: sur → Sector 4 (abierto). Sector 3: este → Sector 9 y sur → Sector 8 (sellados). Sector 4: norte → Sector 2 y oeste → Sector 1 (abiertos); este → Sector 8 y sur → Sector 7 (sellados).
- **Añadir un sector**: en `mundo_mapa.js`, `defSec(columna,fila,{n:"Nombre",dn:densidad,dark:suelo_oscuro,core:"fortress|fortress_t|temple|temple_s|graveyard|village|circle|circle_big",ru:ruinas,col:"#color",tr:[árboles]})`. Las puertas, caminos y el enlace con los vecinos construidos se generan solos (pares recíprocos `g<k><N|E|S|W>`), y el sello desaparece de ambos lados.
- **Interfaz**: minimapa con niebla de exploración y la cuadrícula numerada (**M** = pequeño/grande/oculto, **Tab/N** = grande), clic en suelo o minimapa para caminar, WASD/flechas, Shift correr, zoom con rueda o +/−, ciclo día/noche sincronizado.
- **Online**: lugar `mundo` del servidor (conexión persistente `OnlineHub`); se ven otros jugadores, chat, grupo, comercio y chat privado.
- **Sector 5 · Santuario de los Peregrinos Caídos** (suelo oscuro, templo central, círculos de menhires al noreste y noroeste, cementerio al suroeste y puesto en ruinas al sureste; se llega desde Sector 1 por el sur). Dificultad: vida ×2,6 · daño ×2,2 · cantidad ×1,85 · recompensa ×2,5.
- Pendiente: sectores 6–24, jefes de sector, recursos, misiones. (Los monstruos y el botín de los sectores construidos están en la sección siguiente.)

## 👹 Monstruos del Mundo Abierto y multiplicadores por sector
- **Generación** (`tilemap/js/mundo_mapa.js`, bloque "MONSTRUOS"): al entrar se generan manadas (1-3) en cada sector construido, solo en celdas alcanzables y fuera de las zonas seguras (**campamento**, radio 12, y 5 celdas alrededor de cada puerta de piedra). Reaparecen solos 28-48 s después de morir, lejos del jugador. El 6 % son **Alfa** (×2 vida, ×1,4 daño, ×2,5 XP/botín, aura roja).
- **Monstruos**: los del bioma 1 (Slime Verde, Goblin Explorador, Lobo Salvaje, Araña Gigante, Duende Tramposo, Ent Joven, Bandido del Bosque, Champiñón Venenoso). Stats base = `_normalEnemigo` del juego (igual que Mazmorra/Laberinto) × multiplicadores del sector. Duermen/pasean cerca de su punto, te detectan por distancia (más de noche), la manada se avisa, te persiguen y vuelven a casa curándose si te alejas o entras al campamento.
- **Multiplicadores por sector** (`dif` en `defSec`, también visibles en el minimapa y en el HUD):

  | Sector | ♥ vida | ⚔ daño | 👹 cantidad | 💰 XP/botín | agro | vel | piso eq. |
  |---|---|---|---|---|---|---|---|
  | Home | ×1 | ×1 | ×0,5 | ×1 | ×0,8 | ×1 | 1 |
  | Sector 1 | ×1,6 | ×1,4 | ×1 | ×1,5 | ×1 | ×1,05 | 3 |
  | Sector 2 | ×1,85 | ×1,6 | ×1,3 | ×1,75 | ×1,05 | ×1,04 | 5 |
  | Sector 3 | ×2,1 | ×1,8 | ×1,5 | ×2 | ×1,1 | ×1,08 | 7 |
  | Sector 4 | ×2,35 | ×2 | ×1,7 | ×2,25 | ×1,15 | ×1,1 | 9 |

  La defensa sube con la vida del sector. Un sector nuevo sin `dif` usa `difAuto(id)` (crece con el número de sector); basta con pasar `dif:{...}` en `defSec` para ajustarlo. Cantidad base por sector: `MON_BASE` (42, antes 24; tope por sector `MON_MAX_SEC` = 110, antes 60; reaparición `MON_RESP` = 22-38 s). Población resultante: Home 21, Sector 1 42, Sector 2 55, Sector 3 63; ajustes globales: `MON_BASE`, `MON_SEGURA`, `MON_RESP`, `MON_ALFA`.
- **Minimapa**: cada sector construido muestra sus multiplicadores (♥ ⚔; en el mapa grande también 👹 💰), un tinte verde→rojo según peligro, el círculo verde de la zona segura y puntos rojos (dorados = Alfa) de los monstruos en la zona explorada. El HUD superior muestra los multiplicadores del sector actual.
- **Combate**: barra de vida abajo (stats reales de `calcStats`). **Clic en un monstruo** = ir y atacar; **Espacio / J** = atacar al más cercano; botón ⚔ en pantalla táctil (mantener = auto-ataque). Alcance 1,9 celdas (6 Mago/Arquero, 4 Clérigo). Daño con `danoMitigado` + defensas del enemigo (esquivar/absorber/bloquear). Mover con WASD cancela el objetivo.
- **Recompensas**: XP (`ganarXP`) y botín del juego (`dropearLoot`, oro multiplicado por el ×💰 del sector); cuenta para misiones/profesiones (`registrarBaja`, `cosechar`). La vida se guarda en la partida.
- **Regeneración**: ~1,2 %/s de vida y 2 %/s de recurso fuera de combate; ~6 %/s de vida en el campamento. **Estado «En combate»**: si te atacan, te persiguen o atacas a un enemigo se detiene la regeneración natural de vida y recurso (insignia roja «⚔ En combate · Ns» en tu panel); se reanuda 10 s después de que deje de cumplirse cualquiera de esos factores. No afecta a pociones, curaciones de habilidades ni regeneración por talentos. Al morir: fundido y reaparición en el campamento con 50 % de vida (sin penalización).
- Con el mapa grande abierto el mundo se pausa (salvo en online). Corregido de paso: `dt` negativo en el primer fotograma podía romper el bucle del mapa.
- Limitación: los monstruos se simulan en cada cliente (no se sincronizan entre jugadores online todavía).

## 👥 Grupos, Raid y Laberinto sincronizado (nuevo)
- **Conexión única y persistente**: `OnlineHub` (`js/online/online.js`) mantiene el WebSocket mientras estás en Oakhaven, Mundo Abierto o Laberinto (se cierra solo ~6 s después de salir de esas pantallas). `oakhaven_mapa.js` y `mundo_mapa.js` (iframes) y `combate_demo.js` se suscriben a él; sin el juego principal (mapa suelto) siguen usando su WebSocket propio.
- **Hablar con otro jugador** (E cerca o clic en su personaje, en Oakhaven y en el Mundo Abierto): 👥 Invitar a grupo · 🤝 Comerciar · 💬 Chat privado.
- **Grupo** (hasta 5) y **Raid** (el líder lo activa desde el panel; hasta `MAX_JUGADORES`). Panel a la izquierda con nivel, vida, recurso y estado (activo/inactivo); tocar tu personaje → **Abandonar**; el líder también puede expulsar y dar el liderazgo. Mensajes del servidor: `g_invitar`, `g_resp`, `g_salir`, `g_expulsar`, `g_raid`, `g_lider`, `info`/`g_info`.
- **Comercio**: oro + objetos del inventario (no equipo) con confirmación de ambos (`c_pedir`, `c_resp`, `c_oferta`, `c_conf`, `c_fin`). **Chat privado**: `susurro`.
- **Mundo Abierto y Oakhaven**: todos los jugadores comparten el mismo mundo (los NPC se sincronizan con el reloj del servidor; el mapa es determinista).
- **Laberinto de Artaros en grupo**: solo el grupo comparte instancia (semilla, enemigos, palancas, cofres); quien juega solo tiene la suya. El anfitrión simula la IA y difunde el estado; el resto ve marionetas y envía su daño. Muertes, palancas y cofres se difunden; cada uno recibe su botín. En grupo no se puede pausar. Mensajes `lab_unir`, `lab_salir`, `lab_ev`, `lab_host`, `lab_pedir`.
- Se eliminó la espada (`sword`) de la Plaza Mayor de Oakhaven.
- Archivos: `server/servidor.js`, `js/online/online.js`, `js/demo/combate_demo.js`, `js/sistemas/equipamiento.js`, `tilemap/js/oakhaven_mapa.js`, `tilemap/js/mundo_mapa.js`.

## 🎮 Interfaz de combate del Mundo Abierto (módulos)
- **Módulo 1** (arriba-izquierda): nombre, nivel, vida y recurso. Tocarlo abre el menú del grupo (abandonar / convertir en raid).
- **Módulo 2** (bajo el 1): hasta 4 compañeros del grupo (nombre, Nv, vida, recurso); solo salen los que hay. Tocar = chat privado, comerciar, dar liderazgo, expulsar. El panel de grupo del juego principal se oculta en el Mundo Abierto (`OnlineHub.ocultarPanel`).
- **Módulo 3** (abajo-izquierda): **Atacar** (tocar = ataca/acerca al más cercano, mantener = auto-ataque), **habilidades** (3 de clase + 2 de rama si están desbloqueadas; teclas 1-5; recarga y coste visibles) y **Sprint** (botón alternable; Shift también).
- **Módulo 4** (sobre el minimapa): cinturón de pociones (vida, recurso, élfica) con su cantidad. Teclas **Q** (vida) y **R** (recurso).
- **Módulo 5**: el chat sube sobre el módulo 3. Se eliminó la barra de vida inferior y el botón ⚔ antiguo.
- **Combate**: al combatir aparece el arma de la clase, tajos/proyectiles, habilidades con los mismos efectos que el combate en tiempo real (FX, aturdimientos, escudos, pilares, marcas…), números flotantes, sacudida de pantalla, telegrafía del golpe enemigo (0,42 s) y recurso (se guarda en la partida).
- Archivos: `tilemap/mundo_mapa.html`, `tilemap/js/mundo_mapa.js`, `js/online/online.js`.

- **Sprint con enfriamiento** (`SPR_DUR`=4 s, `SPR_CD`=12 s, `SPR_MULT`=×1,6 en `mundo_mapa.js`): ya no es permanente. Botón » o **Shift** activa el sprint 4 s; al acabar enfría 12 s (barra oscura y segundos en el botón). Pulsar de nuevo mientras corre lo cancela y deja 7,2 s de enfriamiento.
- **Botones de habilidades y sprint más grandes**: `--b` en `tilemap/mundo_mapa.html` pasa de 34-54 px a 40-80 px (iconos, teclas, coste y cifras de recarga escalan con él; el chat sube solo).

## 🔐 Cuentas: acceso y registro
- El **cliente (APK)** solo sirve para entrar: envía usuario y contraseña a `/api/login` y únicamente carga el juego si el servidor los valida.
- **Crear cuenta** se hace en el navegador: `http://IP:8080/registro` (`registro.html`); el cliente abre esa página. Contraseña mínima de 4 caracteres (`MIN_CLAVE` en `server/servidor.js`). `REGISTRO_ABIERTO=false` cierra el alta.
- `login.html` (acceso desde el navegador) redirige su pestaña "Crear cuenta" a `/registro`.
- `/api/estado` informa de `registroUrl` y `minClave`.

## 🌐 Monstruos sincronizados: el servidor es el anfitrión (nuevo)
- **Simulación en el servidor** (`server/mundo_sim.js`, cargado por `servidor.js`): el servidor crea, mueve, hace atacar, cura y reaparece a todos los monstruos del Mundo Abierto (20 Hz) y reparte su estado a los jugadores del lugar `mundo` (10 Hz, solo los que están a ≤ 50 celdas; minimapa a 1 Hz). Los clientes ya no simulan nada: son "títeres" que interpolan (`monStepSrv`), así todos ven a cada monstruo en la **misma posición, vida y estado**, aunque ataque a otro jugador.
- **Mismo mundo**: el servidor carga `tilemap/js/mundo_mapa.js` hasta el marcador `/*@SIM_FIN*/` (mapa, sectores, puertas, dificultad y constantes `MON_*`), sin duplicar datos. Las stats salen de la misma fórmula que el cliente (`js/data/mundo.js` + multiplicadores del sector); el servidor es quien manda en vida, daño y defensa.
- **Golpes**: el cliente calcula su daño (con su personaje) y lo envía (`mon_hit`: daño, empuje, aturdimiento); aturdimientos, bajadas de defensa y absorciones van en `mon_fx`. El servidor valida (distancia ≤ 50, máx. 60 mensajes/s, daño ≤ vida máxima) y lo aplica a todos. Quien da el golpe mortal recibe XP y botín (`mon_muere`); los demás ven la muerte.
- **Ataques de monstruos**: el servidor elige objetivo (el jugador más cercano que no esté en zona segura ni muerto), telegrafía 0,42 s, resuelve el cono contra TODOS los jugadores y envía `mon_atq {id,a,h:[jugadores alcanzados]}`; cada cliente alcanzado aplica su propia defensa/esquiva (`golpeMon`).
- **Cuando no hay jugadores** en el mundo la simulación se pausa y los monstruos vuelven a su sitio con la vida completa. Los monstruos a > 64 celdas de todos los jugadores se congelan (ahorra CPU).
- **Sin servidor/sin conexión**: si el servidor no tiene la simulación (o se cae la conexión) el cliente vuelve a simular en local como antes.
- **Reloj online**: al entrar en modo online, Oakhaven y el Mundo Abierto usan la hora del servidor (misma hora y mismo día/noche para todos; 08:00 al arrancar el servidor, 1 s = 5,6 min de juego) y el **reloj flotante** del juego principal desaparece mientras tanto (el mapa avisa cada 4 s; el HUD del mapa muestra 🌐). Al salir del modo online vuelve el reloj flotante.
- Archivos: `server/servidor.js`, `server/mundo_sim.js` (nuevo), `tilemap/js/mundo_mapa.js`, `tilemap/js/oakhaven_mapa.js`, `js/sistemas/taberna.js`.

## ♛ Jefes de sector (Sector 1: General Torvald)
- Definidos en `JEFES` (`tilemap/js/mundo_mapa.js`, antes de `SIM_FIN`); simulados en `server/mundo_sim.js` (solo online). Héroe humano con aspecto de General (sprite 80) y las 3 habilidades base de Guerrero: **Golpe Brutal** (150 % + aturde), **Postura de Acero** (+20 % DEF, 5 s) y **Ejecutar** (robo de vida 50 %), todas telegrafiadas.
- Stats = monstruo normal del sector × (vida ×9, defensa ×5, daño ×0.8). Zona remota elegida por el servidor (celda alcanzable más lejana de las puertas). No ataca hasta ser atacado; si salen de su área (15 celdas) vuelve y se cura del todo. Reaparece a los 20 min.
- Botín (una tirada): 5 % legendario · 15 % épico · 30 % raro (objeto de forja de una clase al azar → al líder del equipo, que lo reparte por **comercio**, ahora con equipo de la bolsa) · 50 % oro (20 000 repartidos entre el equipo). Todos los participantes reciben XP.
- Archivos: `server/mundo_sim.js`, `server/servidor.js`, `tilemap/js/mundo_mapa.js`, `js/online/online.js`.
- **Sector 2: Capitana Valka** (Centinela de los Vigías, sprite 81, habilidades de Paladín): Golpe Sagrado (140 % en área), Sentencia de Luz (+25 % DEF, 5 s) y Bendición de Poderío (se cura 5 % de su vida). Mismos multiplicadores y botín.
- **Sector 3: Archimago Morvek** (Custodio de la Necrópolis, sprite 82, habilidades de Mago, daño mágico, ataca a distancia): Bola de Fuego (250 % en línea), Congelar (70 % en área + aturde 1,5 s) y Barrera Arcana (escudo del 5 % de su vida, 6 s). Mismos multiplicadores y botín.

## 🗺 Mapa abierto (sin portales ni bordes)
- Se eliminaron las puertas de piedra y los caminos sellados. El cinturón entre sectores construidos (y el cruce de cuatro con ≥3 construidos) es ahora terreno transitable con bosque, caminos continuos entre sectores vecinos y pertenece al sector más cercano (dificultad incluida). Siguen infranqueables el borde del mundo y el lado de los sectores sin construir.
- Los jefes eligen su zona remota midiendo el camino desde la celda del sector más cercana al campamento (`arenaJefe` en `server/mundo_sim.js`). El mundo cambia de aspecto respecto a versiones anteriores (se regenera por semilla).
- Archivos: `tilemap/js/mundo_mapa.js`, `server/mundo_sim.js`.

- Integrado en Server_071: los jefes aparecen en el centro de la zona de jefe de su sector (`TM[k].boss`, arena despejada de la 071); si un sector no la tiene, el servidor elige la zona más remota. Los cinturones abiertos usan también el bosque ralo y `conectarBosque()`.
- **IA de jefes mejorada**: elige la mejor habilidad lista (cura si está bajo, refuerzo al empezar o con el golpe, ataques según a cuántos alcanzan), enfurece por debajo del 50 % y 25 % de vida (más velocidad, menos espera y telegrafías más cortas), carga si el objetivo está lejos, cambia de objetivo al azar cada 8-12 s y los jefes a distancia se apartan. Corregido: la zona de jefe vetada a monstruos (NOMON) bloqueaba al propio jefe.
- **Buffs de jefes moderados**: solo cambian los refuerzos (DEF, escudo, curación) en `JEFES` y `RAMA_JEFE` de `tilemap/js/mundo_mapa.js`: menos potencia, 5-6 s de duración y 18-40 s de espera (antes casi permanentes; Guardián/Hierofante curaban 18-20 % cada 4-6 s). Los ataques y tipos de daño de las clases no se tocan.

- **Grupo: XP, oro y botín compartidos (mundo abierto)**: al morir un monstruo, el servidor (`matar` en `server/mundo_sim.js`) manda `mon_muere` con `eq` = el que lo mata + los miembros de su grupo en el mundo, vivos y a ≤45 celdas; cada uno recibe la recompensa completa (XP, oro y su propia tirada de botín, con sus talentos y maldiciones) y cuenta la baja para misiones. En el Laberinto ya funcionaba así.
- **Refuerzos de Paladín/Clérigo**: solo los aliados del grupo dentro del área (175 u ≈ 5,5 celdas, la del Pulso Vital) los reciben; el anillo del área se dibuja al lanzar. Solo viaja la parte de refuerzo (vida, defensa, escudo, purga, inmunidad, daño/reducción), nunca el daño. Aplicado también al Laberinto.

## 🤖 Bots aventureros (nuevo)
- Seis bots controlados por el servidor (uno por clase) viven en Oakhaven y el Mundo Abierto como jugadores reales: empiezan en nivel 1, suben de nivel cazando, compran/venden en la Tienda, usan pociones y habilidades, aceptan grupo y te ayudan a pelear con su rol de clase. Usan las mismas fórmulas del juego (`calcStats`, `ganarXP`, `dropearLoot`, talentos…) cargadas en el servidor.
- Archivos: `server/bots.js` (nuevo), `server/servidor.js` (hooks: `crearJugadorBot`, `/api/bots`, los bots no ocupan plaza), `server/mundo_sim.js` (expone el mundo a los bots). Progreso en `server/datos/bots.json`. Ajustes y órdenes de chat: ver `server/LEEME.md`.
