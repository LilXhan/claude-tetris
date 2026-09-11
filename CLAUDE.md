# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Tetris clásico en JavaScript vanilla (sin frameworks, sin build, sin dependencias). Tres archivos: `index.html` (DOM/canvas), `style.css` (tema dark), `game.js` (toda la lógica).

## Running

No hay build ni tests. Abrir `index.html` directo en el navegador, o servir estático:

```bash
python3 -m http.server 8000   # luego abrir http://localhost:8000
```

Cambios en `game.js`/`style.css` se ven con solo recargar la página.

## Architecture (game.js)

Todo el estado vive en variables globales top-level (`board`, `current`, `next`, `score`, `lines`, `level`, `paused`, `gameOver`, `dropInterval`, etc.), sin clases ni módulos.

- **Tablero**: matriz `ROWS × COLS`, cada celda es `0` (vacía) o índice de color 1–7.
- **Piezas** (`PIECES`): matrices cuadradas fijas; rotar es `rotateCW` (transponer + invertir filas), no hay tabla SRS.
- **Wall kicks** (`tryRotate`): tras rotar, prueba desplazamientos `[0,-1,1,-2,2]` en x hasta encontrar uno sin colisión.
- **Loop de juego** (`loop`): un único `requestAnimationFrame`, acumula `dt` y baja la pieza cuando supera `dropInterval`; `draw()` redibuja todo el canvas cada frame (no hay dirty-rect).
- **Fin de pieza** (`lockPiece`): `merge()` → `clearLines()` → `spawn()`. `spawn()` promueve `next` a `current` y genera nueva `next`; si la nueva pieza colisiona al aparecer, dispara `endGame()`.
- **Progresión**: nivel = `floor(lines/10)+1`; `dropInterval = max(100, 1000 - (level-1)*90)`.
- Todo el manejo de input es un único listener `keydown` con `switch` sobre `e.code`.

Si se cambia `COLS`, `ROWS` o `BLOCK`, hay que ajustar a mano `width`/`height` del `<canvas id="board">` en `index.html`.
