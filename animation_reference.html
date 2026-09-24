<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TUI AI Asistan</title>
<style>
  *{margin:0;padding:0;box-sizing:border-box}
  body{background:#050508;display:flex;justify-content:center;align-items:center;height:100vh;overflow:hidden;font-family:monospace}
  #tui-screen{background:#0a0a12;border:1px solid #1a1a3e;border-radius:6px;padding:14px 18px;box-shadow:0 0 40px rgba(30,60,180,0.12),inset 0 0 80px rgba(0,0,0,0.6);position:relative;overflow:hidden}
  #tui-screen::after{content:'';position:absolute;top:0;left:0;right:0;bottom:0;background:repeating-linear-gradient(0deg,transparent,transparent 2px,rgba(0,0,0,0.08) 2px,rgba(0,0,0,0.08) 4px);pointer-events:none;z-index:10}
  canvas{display:block;image-rendering:pixelated;image-rendering:crisp-edges}
</style>
</head>
<body>
<div id="tui-screen">
<canvas id="c"></canvas>
</div>
<script>
const cvs = document.getElementById('c');
const ctx = cvs.getContext('2d');

const PX = 7;
const COLS = 72;
const ROWS = 30;
cvs.width = COLS * PX;
cvs.height = ROWS * PX;

const STAR_R = 9.5;
const STAR_CX = 36;
const STAR_CY = 11;

const C_BG       = '#0a0a12';
const C_BORDER   = '#16163a';
const C_CORNER   = '#2a2a6a';
const C_STAR     = '#ffffff';
const C_STAR_DIM = '#8888bb';
const C_GLOW     = '#4466ff';
const C_TEXT     = '#7777aa';
const C_ACCENT   = '#aaaaff';
const C_CURSOR   = '#ffffff';
const C_TITLE    = '#5555cc';

let grid = [];
function initGrid() {
  grid = [];
  for (let r = 0; r < ROWS; r++) {
    grid[r] = [];
    for (let c = 0; c < COLS; c++) {
      grid[r][c] = { ch: ' ', fg: C_BG, bg: null };
    }
  }
}

function setCell(c, r, ch, fg, bg) {
  if (r >= 0 && r < ROWS && c >= 0 && c < COLS) {
    grid[r][c] = { ch, fg: fg || C_TEXT, bg: bg || null };
  }
}

function drawBox(x, y, w, h, color) {
  setCell(x, y, '╔', color || C_BORDER);
  setCell(x + w - 1, y, '╗', color || C_BORDER);
  setCell(x, y + h - 1, '╚', color || C_BORDER);
  setCell(x + w - 1, y + h - 1, '╝', color || C_BORDER);
  for (let i = 1; i < w - 1; i++) {
    setCell(x + i, y, '═', color || C_BORDER);
    setCell(x + i, y + h - 1, '═', color || C_BORDER);
  }
  for (let j = 1; j < h - 1; j++) {
    setCell(x, y + j, '║', color || C_BORDER);
    setCell(x + w - 1, y + j, '║', color || C_BORDER);
  }
}

// ─── STAR OF DAVID LOGIC ───
function pointInTriangle(px, py, ax, ay, bx, by, cx, cy) {
  const d1 = (px - bx) * (ay - by) - (ax - bx) * (py - by);
  const d2 = (px - cx) * (by - cy) - (bx - cx) * (py - cy);
  const d3 = (px - ax) * (cy - ay) - (cx - ax) * (py - ay);
  const hasNeg = (d1 < 0) || (d2 < 0) || (d3 < 0);
  const hasPos = (d1 > 0) || (d2 > 0) || (d3 > 0);
  return !(hasNeg && hasPos);
}

function getStarPixels(angle, scale) {
  const pixels = new Map();
  const cosA = Math.cos(angle);
  const sinA = Math.sin(angle);
  const R = STAR_R * scale;

  // Upward triangle vertices (before rotation)
  const uT = [
    { x: 0, y: -R },
    { x: R * Math.cos(Math.PI / 6), y: R * Math.sin(Math.PI / 6) },
    { x: -R * Math.cos(Math.PI / 6), y: R * Math.sin(Math.PI / 6) }
  ];
  // Downward triangle vertices
  const dT = [
    { x: 0, y: R },
    { x: R * Math.cos(Math.PI / 6), y: -R * Math.sin(Math.PI / 6) },
    { x: -R * Math.cos(Math.PI / 6), y: -R * Math.sin(Math.PI / 6) }
  ];

  // Rotate triangles
  function rotateTri(tri) {
    return tri.map(p => ({
      x: p.x * cosA - p.y * sinA,
      y: p.x * sinA + p.y * cosA
    }));
  }
  const ruT = rotateTri(uT);
  const rdT = rotateTri(dT);

  const searchR = Math.ceil(R) + 2;
  for (let dy = -searchR; dy <= searchR; dy++) {
    for (let dx = -searchR; dx <= searchR; dx++) {
      const inUp = pointInTriangle(dx, dy, ruT[0].x, ruT[0].y, ruT[1].x, ruT[1].y, ruT[2].x, ruT[2].y);
      const inDown = pointInTriangle(dx, dy, rdT[0].x, rdT[0].y, rdT[1].x, rdT[1].y, rdT[2].x, rdT[2].y);

      if (inUp || inDown) {
        // Outline detection: check if any neighbor is outside both triangles
        let isOutline = false;
        for (let ny = -1; ny <= 1; ny++) {
          for (let nx = -1; nx <= 1; nx++) {
            if (nx === 0 && ny === 0) continue;
            const nInUp = pointInTriangle(dx + nx, dy + ny, ruT[0].x, ruT[0].y, ruT[1].x, ruT[1].y, ruT[2].x, ruT[2].y);
            const nInDown = pointInTriangle(dx + nx, dy + ny, rdT[0].x, rdT[0].y, rdT[1].x, rdT[1].y, rdT[2].x, rdT[2].y);
            if (!nInUp && !nInDown) {
              isOutline = true;
            }
          }
        }
        const key = `${dx},${dy}`;
        if (!pixels.has(key)) {
          pixels.set(key, { dx, dy, outline: isOutline });
        } else if (isOutline) {
          pixels.get(key).outline = true;
        }
      }
    }
  }
  return pixels;
}

// ─── PENNY COIN FLIP LOGIC ───
// We use a compound oscillation to avoid "artificial stops"
// Primary: cos-based squeeze (like a spinning coin)
// Secondary: slight wobble so it never fully flattens
// Tertiary: slow phase drift for organic feel
function getPennyScale(t) {
  const primaryFreq = 1.1;
  const wobbleFreq = 2.7;
  const driftFreq = 0.13;

  const primary = Math.cos(t * primaryFreq);
  const wobble = Math.sin(t * wobbleFreq) * 0.08;
  const drift = Math.sin(t * driftFreq) * 0.05;

  let scale = primary + wobble + drift;

  // Clamp minimum so it NEVER fully disappears (penny edge thickness)
  const minScale = 0.06;
  if (Math.abs(scale) < minScale) {
    scale = scale >= 0 ? minScale : -minScale;
  }
  return scale;
}

// Rotation speed varies slightly for organic feel
function getAngle(t) {
  const baseSpeed = 0.7;
  const variation = Math.sin(t * 0.3) * 0.15;
  return t * (baseSpeed + variation);
}

// ─── SPARKLE PARTICLES ───
const sparkles = [];
for (let i = 0; i < 12; i++) {
  sparkles.push({
    angle: Math.random() * Math.PI * 2,
    dist: STAR_R + 2 + Math.random() * 5,
    speed: 0.3 + Math.random() * 0.6,
    phase: Math.random() * Math.PI * 2,
    char: ['·', '∙', '✦', '⊹', '⋆'][Math.floor(Math.random() * 5)]
  });
}

// ─── TUI TEXT CONTENT ───
const titleText = '◆ N E U R A L   A S I S T A N ◆';
const statusLines = [
  'MODEL  : transformer-v4.2.1-quantized',
  'DURUM  : aktif ◈ bekleniyor...',
  'BELLEK : 14.2 GB / 24.0 GB VRAM',
  'TOKEN  : akış hazır │ gecikme: 12ms',
];
const promptLine = '> _';

// ─── MAIN RENDER LOOP ───
let startTime = performance.now();

function render(timestamp) {
  const t = (timestamp - startTime) / 1000;
  initGrid();

  // Draw outer border
  drawBox(0, 0, COLS, ROWS, C_BORDER);
  setCell(0, 0, '╔', C_CORNER);
  setCell(COLS - 1, 0, '╗', C_CORNER);
  setCell(0, ROWS - 1, '╚', C_CORNER);
  setCell(COLS - 1, ROWS - 1, '╝', C_CORNER);

  // Title bar
  const titleStart = Math.floor((COLS - titleText.length) / 2);
  for (let i = 0; i < titleText.length; i++) {
    setCell(titleStart + i, 0, titleText[i], C_TITLE);
  }

  // Separator line under star area
  for (let i = 1; i < COLS - 1; i++) {
    setCell(i, 21, '─', C_BORDER);
  }
  setCell(0, 21, '├', C_BORDER);
  setCell(COLS - 1, 21, '┤', C_BORDER);

  // ─── STAR ANIMATION ───
  const angle = getAngle(t);
  const scale = getPennyScale(t);
  const absScale = Math.abs(scale);
  const starPixels = getStarPixels(angle, absScale);

  // Brightness based on how "face-on" the coin is
  const brightness = Math.min(1, absScale * 1.5);

  // Glow halo behind star
  const glowR = Math.ceil(STAR_R * absScale) + 3;
  for (let dy = -glowR; dy <= glowR; dy++) {
    for (let dx = -glowR; dx <= glowR; dx++) {
      const dist = Math.sqrt(dx * dx + dy * dy);
      if (dist <= glowR && dist > STAR_R * absScale * 0.5) {
        const intensity = 1 - (dist / glowR);
        if (intensity > 0.3) {
          const gc = Math.floor(STAR_CX + dx);
          const gr = Math.floor(STAR_CY + dy);
          if (gr > 1 && gr < 21 && gc > 0 && gc < COLS - 1) {
            if (!starPixels.has(`${dx},${dy}`)) {
              setCell(gc, gr, '░', `rgba(60,80,200,${(intensity * 0.3 * brightness).toFixed(2)})`);
            }
          }
        }
      }
    }
  }

  // Render star pixels
  starPixels.forEach((info, key) => {
    const gc = Math.floor(STAR_CX + info.dx);
    const gr = Math.floor(STAR_CY + info.dy);
    if (gr > 1 && gr < 21 && gc > 0 && gc < COLS - 1) {
      // Use block characters for pixel art density
      let ch, fg;
      if (info.outline) {
        ch = '█';
        fg = C_STAR;
      } else {
        // Inner fill with slight pattern for texture
        const pattern = (Math.abs(info.dx) + Math.abs(info.dy)) % 3;
        if (pattern === 0) {
          ch = '▓';
          fg = C_STAR_DIM;
        } else if (pattern === 1) {
          ch = '█';
          fg = brightness > 0.7 ? C_STAR : C_STAR_DIM;
        } else {
          ch = '▒';
          fg = C_STAR_DIM;
        }
      }

      // Edge shimmer effect
      const shimmer = Math.sin(t * 4 + info.dx * 0.8 + info.dy * 0.5);
      if (info.outline && shimmer > 0.7) {
        fg = C_GLOW;
        ch = '█';
      }

      setCell(gc, gr, ch, fg);
    }
  });

  // Sparkle particles orbiting
  sparkles.forEach(sp => {
    const sa = sp.angle + t * sp.speed;
    const sd = sp.dist + Math.sin(t * 1.5 + sp.phase) * 2;
    const sx = Math.floor(STAR_CX + Math.cos(sa) * sd);
    const sy = Math.floor(STAR_CY + Math.sin(sa) * sd * 0.5); // elliptical
    if (sy > 1 && sy < 21 && sx > 0 && sx < COLS - 1) {
      const vis = (Math.sin(t * 3 + sp.phase) + 1) / 2;
      if (vis > 0.4) {
        setCell(sx, sy, sp.char, C_GLOW);
      }
    }
  });

  // ─── STATUS TEXT AREA ───
  for (let i = 0; i < statusLines.length; i++) {
    const line = statusLines[i];
    for (let j = 0; j < line.length; j++) {
      setCell(3 + j, 23 + i, line[j], C_TEXT);
    }
  }

  // Animated loading dots
  const dotCount = Math.floor(t * 2) % 4;
  const dotStr = '.'.repeat(dotCount);
  const dotX = 3 + statusLines[1].length - 3;
  for (let d = 0; d < 3; d++) {
    setCell(dotX + d, 24, d < dotCount ? '●' : '○', d < dotCount ? C_ACCENT : C_BORDER);
  }

  // Prompt line with blinking cursor
  const blinkOn = Math.sin(t * 4) > 0;
  for (let j = 0; j < promptLine.length - 1; j++) {
    setCell(3 + j, 28, promptLine[j], C_ACCENT);
  }
  if (blinkOn) {
    setCell(3 + promptLine.length - 1, 28, '█', C_CURSOR);
  } else {
    setCell(3 + promptLine.length - 1, 28, '_', C_ACCENT);
  }

  // Decorative corners inside
  setCell(2, 2, '┌', C_BORDER);
  setCell(COLS - 3, 2, '┐', C_BORDER);
  setCell(2, 20, '└', C_BORDER);
  setCell(COLS - 3, 20, '┘', C_BORDER);
  for (let i = 3; i < COLS - 3; i++) {
    setCell(i, 2, '─', C_BORDER);
    setCell(i, 20, '─', C_BORDER);
  }
  for (let j = 3; j < 20; j++) {
    setCell(2, j, '│', C_BORDER);
    setCell(COLS - 3, j, '│', C_BORDER);
  }

  // Label under star
  const label = '[ ✡ HEXGRAM ]';
  const lx = Math.floor((COLS - label.length) / 2);
  for (let i = 0; i < label.length; i++) {
    setCell(lx + i, 19, label[i], C_BORDER);
  }

  // ─── DRAW TO CANVAS ───
  ctx.fillStyle = C_BG;
  ctx.fillRect(0, 0, cvs.width, cvs.height);

  ctx.font = `${PX + 1}px "Courier New", "Consolas", monospace`;
  ctx.textBaseline = 'top';

  for (let r = 0; r < ROWS; r++) {
    for (let c = 0; c < COLS; c++) {
      const cell = grid[r][c];
      if (cell.bg) {
        ctx.fillStyle = cell.bg;
        ctx.fillRect(c * PX, r * PX, PX, PX + 1);
      }
      if (cell.ch !== ' ') {
        ctx.fillStyle = cell.fg;
        ctx.fillText(cell.ch, c * PX, r * PX - 1);
      }
    }
  }

  requestAnimationFrame(render);
}

requestAnimationFrame(render);
</script>
</body>
</html>
