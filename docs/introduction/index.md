<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Capsaicin · TRPV1 — 3D Binding</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Cormorant+Garamond:ital,wght@0,300;0,600;1,300&display=swap" rel="stylesheet">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: #04080f;
    font-family: 'Space Mono', monospace;
    color: #e8dcc8;
    overflow: hidden;
    height: 100vh;
    width: 100vw;
  }

  #canvas-container {
    position: fixed;
    inset: 0;
  }

  /* ── TOP HEADER ── */
  #header {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 10;
    padding: 16px 24px 14px;
    background: linear-gradient(to bottom, rgba(4,8,15,0.95) 70%, transparent);
    display: flex;
    align-items: baseline;
    gap: 18px;
    pointer-events: none;
  }
  #header h1 {
    font-family: 'Cormorant Garamond', serif;
    font-size: 22px;
    font-weight: 600;
    color: #f0e6d0;
    letter-spacing: 0.02em;
  }
  #header h1 em { color: #e8612a; font-style: italic; }
  #header .sep { color: #444; font-size: 18px; }
  #header .sub {
    font-size: 9px;
    color: #7a6e5e;
    letter-spacing: 0.18em;
    text-transform: uppercase;
  }

  /* ── LEGEND ── */
  #legend {
    position: fixed;
    bottom: 24px;
    left: 24px;
    z-index: 10;
    background: rgba(4,8,15,0.82);
    border: 1px solid #1e2a1e;
    border-radius: 4px;
    padding: 14px 16px;
    font-size: 10px;
    line-height: 2;
    backdrop-filter: blur(6px);
  }
  #legend .title {
    font-size: 8px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: #5a6a5a;
    margin-bottom: 6px;
  }
  .leg-row { display: flex; align-items: center; gap: 8px; }
  .leg-dot {
    width: 10px; height: 10px; border-radius: 50%; flex-shrink: 0;
  }
  .leg-line {
    width: 22px; height: 2px; flex-shrink: 0;
  }

  /* ── CONTROLS HINT ── */
  #controls {
    position: fixed;
    bottom: 24px;
    right: 24px;
    z-index: 10;
    background: rgba(4,8,15,0.82);
    border: 1px solid #1e2a1e;
    border-radius: 4px;
    padding: 12px 16px;
    font-size: 9px;
    color: #5a6a5a;
    line-height: 2;
    backdrop-filter: blur(6px);
    letter-spacing: 0.08em;
  }
  #controls .title {
    font-size: 8px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    margin-bottom: 4px;
    color: #4a5a4a;
  }
  kbd {
    background: #12181f;
    border: 1px solid #2a3a2a;
    border-radius: 2px;
    padding: 1px 5px;
    font-family: inherit;
    font-size: 8px;
    color: #a0b090;
  }

  /* ── STEP BUTTONS ── */
  #steps {
    position: fixed;
    top: 50%;
    right: 24px;
    transform: translateY(-50%);
    z-index: 10;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  .step-btn {
    background: rgba(4,8,15,0.82);
    border: 1px solid #2a3a2a;
    color: #7a8a7a;
    font-family: 'Space Mono', monospace;
    font-size: 9px;
    letter-spacing: 0.1em;
    padding: 10px 14px;
    cursor: pointer;
    border-radius: 3px;
    transition: all 0.2s;
    text-align: left;
    backdrop-filter: blur(6px);
    white-space: nowrap;
  }
  .step-btn:hover, .step-btn.active {
    background: rgba(232,97,42,0.15);
    border-color: #e8612a;
    color: #e8a070;
  }
  .step-num {
    color: #e8612a;
    margin-right: 6px;
  }

  /* ── INFO PANEL ── */
  #info-panel {
    position: fixed;
    top: 70px;
    left: 24px;
    z-index: 10;
    background: rgba(4,8,15,0.88);
    border: 1px solid #2a3a2a;
    border-radius: 4px;
    padding: 16px 18px;
    width: 260px;
    backdrop-filter: blur(8px);
    transition: opacity 0.4s;
    font-size: 11px;
    line-height: 1.7;
  }
  #info-panel .info-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: 16px;
    font-weight: 600;
    color: #f0e6d0;
    margin-bottom: 8px;
    border-bottom: 1px solid #2a3a2a;
    padding-bottom: 6px;
  }
  #info-panel .info-tag {
    display: inline-block;
    font-size: 8px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    padding: 2px 7px;
    border-radius: 2px;
    margin-bottom: 8px;
  }
  .tag-hbond { background: rgba(45,180,100,0.2); color: #6ddb96; border: 1px solid #2d6a4a; }
  .tag-hydro { background: rgba(60,130,220,0.2); color: #7ab0e8; border: 1px solid #2a4a8a; }
  .tag-struct { background: rgba(232,97,42,0.2); color: #e8a070; border: 1px solid #8a3a1a; }
  #info-panel p { color: #a09080; font-size: 10.5px; }
  #info-panel strong { color: #d0c0a8; }
</style>
</head>
<body>

<div id="canvas-container"></div>

<div id="header">
  <h1><em>Capsaicin</em> — TRPV1</h1>
  <span class="sep">·</span>
  <span class="sub">3D Vanilloid Binding Pocket</span>
</div>

<div id="legend">
  <div class="title">Atom Colours</div>
  <div class="leg-row"><div class="leg-dot" style="background:#e8612a"></div> Oxygen</div>
  <div class="leg-row"><div class="leg-dot" style="background:#4a90e8"></div> Nitrogen</div>
  <div class="leg-row"><div class="leg-dot" style="background:#c8c8c8"></div> Carbon (capsaicin)</div>
  <div class="leg-row"><div class="leg-dot" style="background:#3a7acc"></div> TRPV1 helices</div>
  <div class="leg-row"><div class="leg-dot" style="background:#2dcc7a"></div> Binding residues</div>
  <div class="leg-row"><div class="leg-dot" style="background:#e8e840"></div> Binding pocket</div>
  <br>
  <div class="title">Interactions</div>
  <div class="leg-row"><div class="leg-line" style="background:#2dcc7a"></div> H-bond</div>
  <div class="leg-row"><div class="leg-line" style="background:#4a90e8; opacity:0.6"></div> van der Waals</div>
  <div class="leg-row"><div class="leg-line" style="background:#f0e060; opacity:0.5"></div> Ion flow (Ca²⁺)</div>
</div>

<div id="controls">
  <div class="title">Navigation</div>
  <div><kbd>drag</kbd> Rotate</div>
  <div><kbd>scroll</kbd> Zoom</div>
  <div><kbd>right-drag</kbd> Pan</div>
</div>

<div id="steps">
  <button class="step-btn active" onclick="setStep(0)"><span class="step-num">01</span>Overview</button>
  <button class="step-btn" onclick="setStep(1)"><span class="step-num">02</span>Receptor</button>
  <button class="step-btn" onclick="setStep(2)"><span class="step-num">03</span>Capsaicin</button>
  <button class="step-btn" onclick="setStep(3)"><span class="step-num">04</span>Binding</button>
  <button class="step-btn" onclick="setStep(4)"><span class="step-num">05</span>Ion Flow</button>
</div>

<div id="info-panel">
  <div class="info-title">Overview</div>
  <span class="info-tag tag-struct">TRPV1 · Vanilloid Receptor</span>
  <p>Capsaicin (🌶️ chili pepper) binds the intracellular <strong>vanilloid binding domain</strong> of TRPV1 — a non-selective cation channel on nociceptors. Binding opens the pore, letting Ca²⁺ and Na⁺ flood in, triggering a pain signal.</p>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
// ─────────────────────────────────────────────
//  SCENE SETUP
// ─────────────────────────────────────────────
const container = document.getElementById('canvas-container');
const W = window.innerWidth, H = window.innerHeight;

const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false });
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
renderer.setSize(W, H);
renderer.shadowMap.enabled = true;
renderer.shadowMap.type = THREE.PCFSoftShadowMap;
container.appendChild(renderer.domElement);

const scene = new THREE.Scene();
scene.background = new THREE.Color(0x04080f);
scene.fog = new THREE.FogExp2(0x04080f, 0.035);

const camera = new THREE.PerspectiveCamera(45, W / H, 0.1, 200);
camera.position.set(0, 4, 22);

// ─────────────────────────────────────────────
//  LIGHTS
// ─────────────────────────────────────────────
const ambientLight = new THREE.AmbientLight(0x0a1020, 1.2);
scene.add(ambientLight);

const keyLight = new THREE.DirectionalLight(0xfff5e0, 1.8);
keyLight.position.set(10, 15, 10);
keyLight.castShadow = true;
scene.add(keyLight);

const fillLight = new THREE.PointLight(0x2050a0, 2.0, 40);
fillLight.position.set(-8, 0, 8);
scene.add(fillLight);

const rimLight = new THREE.PointLight(0xe8612a, 1.5, 30);
rimLight.position.set(0, -8, -8);
scene.add(rimLight);

const topLight = new THREE.PointLight(0x40c080, 1.0, 25);
topLight.position.set(0, 12, 0);
scene.add(topLight);

// ─────────────────────────────────────────────
//  HELPERS
// ─────────────────────────────────────────────
function sphere(r, color, pos, opacity=1) {
  const mat = new THREE.MeshPhongMaterial({
    color, shininess: 80,
    transparent: opacity < 1,
    opacity,
    emissive: new THREE.Color(color).multiplyScalar(0.12)
  });
  const mesh = new THREE.Mesh(new THREE.SphereGeometry(r, 24, 24), mat);
  mesh.position.set(...pos);
  return mesh;
}

function cylinder(from, to, r, color, opacity=1) {
  const dir = new THREE.Vector3(...to).sub(new THREE.Vector3(...from));
  const len = dir.length();
  const mid = new THREE.Vector3(...from).add(new THREE.Vector3(...to)).multiplyScalar(0.5);
  const mat = new THREE.MeshPhongMaterial({
    color, shininess: 40,
    transparent: opacity < 1, opacity,
    emissive: new THREE.Color(color).multiplyScalar(0.08)
  });
  const mesh = new THREE.Mesh(new THREE.CylinderGeometry(r, r, len, 16), mat);
  mesh.position.copy(mid);
  mesh.quaternion.setFromUnitVectors(new THREE.Vector3(0,1,0), dir.normalize());
  return mesh;
}

function helix(cx, cz, radius, height, nTurns, color, opacity=0.82) {
  const group = new THREE.Group();
  const points = [];
  const steps = nTurns * 32;
  for (let i = 0; i <= steps; i++) {
    const t = i / steps;
    const angle = t * nTurns * Math.PI * 2;
    points.push(new THREE.Vector3(
      cx + Math.cos(angle) * radius,
      -height/2 + t * height,
      cz + Math.sin(angle) * radius
    ));
  }
  // Draw as tube segments
  const mat = new THREE.MeshPhongMaterial({
    color, shininess: 60,
    transparent: true, opacity,
    emissive: new THREE.Color(color).multiplyScalar(0.15)
  });
  for (let i = 0; i < points.length - 1; i++) {
    const a = points[i], b = points[i+1];
    const dir = b.clone().sub(a);
    const len = dir.length();
    const mid = a.clone().add(b).multiplyScalar(0.5);
    const m = new THREE.Mesh(new THREE.CylinderGeometry(0.22, 0.22, len, 8), mat);
    m.position.copy(mid);
    m.quaternion.setFromUnitVectors(new THREE.Vector3(0,1,0), dir.normalize());
    group.add(m);
  }
  return group;
}

function dashedLine(from, to, color, dashLen=0.35, gapLen=0.2) {
  const group = new THREE.Group();
  const start = new THREE.Vector3(...from);
  const end = new THREE.Vector3(...to);
  const dir = end.clone().sub(start);
  const total = dir.length();
  const unit = dir.clone().normalize();
  const segLen = dashLen + gapLen;
  let dist = 0;
  const mat = new THREE.MeshBasicMaterial({ color, transparent: true, opacity: 0.8 });
  while (dist < total) {
    const dLen = Math.min(dashLen, total - dist);
    const mid = start.clone().add(unit.clone().multiplyScalar(dist + dLen/2));
    const m = new THREE.Mesh(new THREE.CylinderGeometry(0.04, 0.04, dLen, 6), mat);
    m.position.copy(mid);
    m.quaternion.setFromUnitVectors(new THREE.Vector3(0,1,0), unit);
    group.add(m);
    dist += segLen;
  }
  return group;
}

function glowSphere(r, color, pos) {
  const group = new THREE.Group();
  // Core
  const core = sphere(r, color, [0,0,0], 1);
  group.add(core);
  // Glow shell
  const glow = new THREE.Mesh(
    new THREE.SphereGeometry(r * 1.6, 16, 16),
    new THREE.MeshBasicMaterial({ color, transparent: true, opacity: 0.12, side: THREE.BackSide })
  );
  group.add(glow);
  group.position.set(...pos);
  return group;
}

// ─────────────────────────────────────────────
//  GROUPS
// ─────────────────────────────────────────────
const receptorGroup = new THREE.Group();
const capsaicinGroup = new THREE.Group();
const interactionGroup = new THREE.Group();
const ionGroup = new THREE.Group();
scene.add(receptorGroup, capsaicinGroup, interactionGroup, ionGroup);

// ─────────────────────────────────────────────
//  TRPV1 RECEPTOR
// ─────────────────────────────────────────────
const helixColor = 0x3a7acc;
const poreColor  = 0x1a4a8a;

// 4 outer helices (S1-S4 per subunit, simplified as 2 pairs)
const helixPositions = [
  [-3.2, -3.2], [3.2, -3.2], [-3.2, 3.2], [3.2, 3.2]
];
helixPositions.forEach(([x, z]) => {
  const h = helix(x, z, 0.55, 9, 3.5, helixColor, 0.78);
  receptorGroup.add(h);
});

// 4 inner pore-lining helices (S5-S6)
const innerHelixPos = [
  [-1.4, -1.4], [1.4, -1.4], [-1.4, 1.4], [1.4, 1.4]
];
innerHelixPos.forEach(([x, z]) => {
  const h = helix(x, z, 0.35, 8.5, 3.0, 0x5a9ae0, 0.72);
  receptorGroup.add(h);
});

// Central pore cylinder (translucent)
const poreMesh = new THREE.Mesh(
  new THREE.CylinderGeometry(0.9, 0.9, 9, 32, 1, true),
  new THREE.MeshPhongMaterial({
    color: 0x1a3a6a, transparent: true, opacity: 0.18,
    side: THREE.DoubleSide
  })
);
receptorGroup.add(poreMesh);

// Lipid bilayer planes
[-4.0, 4.0].forEach(y => {
  const plane = new THREE.Mesh(
    new THREE.BoxGeometry(14, 0.25, 14),
    new THREE.MeshPhongMaterial({
      color: 0x4a3a18, transparent: true, opacity: 0.35,
      emissive: 0x2a1a08
    })
  );
  plane.position.y = y;
  receptorGroup.add(plane);
});

// Vanilloid binding pocket highlight (glowing torus)
const pocketTorus = new THREE.Mesh(
  new THREE.TorusGeometry(2.1, 0.25, 16, 60),
  new THREE.MeshPhongMaterial({
    color: 0xe8e820, emissive: 0x606010,
    transparent: true, opacity: 0.55
  })
);
pocketTorus.rotation.x = Math.PI / 2;
pocketTorus.position.y = -1.8;
receptorGroup.add(pocketTorus);

// Binding residue spheres (Tyr511, Ser512, Thr550, Leu515, Met547)
const residues = [
  { name: 'Tyr511', pos: [-2.2, -1.6, -1.8], color: 0x2dcc7a, type: 'hbond' },
  { name: 'Ser512', pos: [-2.4, -2.4, -1.4], color: 0x2dcc7a, type: 'hbond' },
  { name: 'Thr550', pos: [-1.8, -0.8, -2.2], color: 0x2dcc7a, type: 'hbond' },
  { name: 'Leu515', pos: [2.0, -1.8, -1.6], color: 0x4a90e8, type: 'hydro' },
  { name: 'Met547', pos: [2.2, -1.2, -2.0], color: 0x4a90e8, type: 'hydro' },
  { name: 'Ile569', pos: [1.8, -2.6, 1.8],  color: 0x4a90e8, type: 'hydro' },
  { name: 'Leu669', pos: [-1.6, -3.0, 2.0], color: 0x4a90e8, type: 'hydro' },
];
residues.forEach(res => {
  const g = glowSphere(0.45, res.color, res.pos);
  g.userData = res;
  receptorGroup.add(g);
});

// ─────────────────────────────────────────────
//  CAPSAICIN MOLECULE
// ─────────────────────────────────────────────
// Aromatic ring (benzene, flat in XZ plane)
const ringRadius = 1.05;
const ringAtoms = [];
for (let i = 0; i < 6; i++) {
  const angle = (i / 6) * Math.PI * 2 - Math.PI/6;
  const x = Math.cos(angle) * ringRadius - 1.2;
  const z = Math.sin(angle) * ringRadius - 1.2;
  const y = -1.8;
  const atom = sphere(0.22, 0x888888, [x, y, z]);
  capsaicinGroup.add(atom);
  ringAtoms.push([x, y, z]);
}
// Ring bonds
for (let i = 0; i < 6; i++) {
  const bond = cylinder(ringAtoms[i], ringAtoms[(i+1)%6], 0.09, 0x666666);
  capsaicinGroup.add(bond);
}

// OH substituent (red oxygen, top-right of ring)
const ohPos = [ringAtoms[1][0] + 0.7, -1.0, ringAtoms[1][2] - 0.5];
capsaicinGroup.add(sphere(0.28, 0xe8400a, ohPos)); // O
capsaicinGroup.add(sphere(0.14, 0xffffff, [ohPos[0]+0.3, ohPos[1]+0.2, ohPos[2]-0.1])); // H
capsaicinGroup.add(cylinder(ringAtoms[1], ohPos, 0.08, 0x999999));

// OCH3 substituent (left of ring)
const omePos = [ringAtoms[4][0] - 0.7, -1.2, ringAtoms[4][2] + 0.4];
capsaicinGroup.add(sphere(0.28, 0xe8400a, omePos));
capsaicinGroup.add(sphere(0.22, 0x777777, [omePos[0]-0.5, omePos[1]+0.1, omePos[2]+0.3])); // CH3
capsaicinGroup.add(cylinder(ringAtoms[4], omePos, 0.08, 0x999999));

// Linker CH2 from ring to N
const linkerStart = [ringAtoms[2][0] + 0.4, ringAtoms[2][1], ringAtoms[2][2] - 0.7];
const nPos = [1.4, -1.8, -1.0];
capsaicinGroup.add(sphere(0.19, 0x777777, linkerStart));
capsaicinGroup.add(cylinder(ringAtoms[2], linkerStart, 0.09, 0x666666));
capsaicinGroup.add(cylinder(linkerStart, nPos, 0.09, 0x666666));

// Amide N (blue)
capsaicinGroup.add(sphere(0.32, 0x4a90e8, nPos));

// C=O (up from N)
const coPos = [nPos[0] - 0.4, nPos[1] + 1.0, nPos[2] - 0.2];
capsaicinGroup.add(sphere(0.22, 0x555555, [coPos[0], coPos[1]-0.35, coPos[2]]));
capsaicinGroup.add(sphere(0.28, 0xe8400a, coPos));
capsaicinGroup.add(cylinder([coPos[0], coPos[1]-0.35, coPos[2]], coPos, 0.09, 0x888888));

// Acyl chain (zig-zag 8 carbons) extending into hydrophobic pocket
const chainAtoms = [nPos];
for (let i = 1; i <= 8; i++) {
  const prev = chainAtoms[i-1];
  const x = prev[0] + 1.1;
  const y = prev[1] + (i % 2 === 0 ? 0.35 : -0.35);
  const z = prev[2] + 0.1;
  chainAtoms.push([x, y, z]);
}
for (let i = 0; i < chainAtoms.length - 1; i++) {
  const col = i < 2 ? 0x888888 : 0x555555;
  capsaicinGroup.add(sphere(0.18, col, chainAtoms[i+1]));
  capsaicinGroup.add(cylinder(chainAtoms[i], chainAtoms[i+1], 0.09, 0x666666));
}
// Terminal methyl
capsaicinGroup.add(sphere(0.22, 0x444444, chainAtoms[chainAtoms.length-1]));

// ─────────────────────────────────────────────
//  INTERACTION LINES
// ─────────────────────────────────────────────
// H-bonds (green dashed)
[[ohPos, residues[0].pos], [ohPos, residues[1].pos], [coPos, residues[2].pos]].forEach(([a, b]) => {
  interactionGroup.add(dashedLine(a, b, 0x2dcc7a, 0.3, 0.15));
});
// VdW contacts (blue dashed)
[[chainAtoms[3], residues[3].pos], [chainAtoms[5], residues[4].pos],
 [chainAtoms[7], residues[5].pos], [chainAtoms[8], residues[6].pos]].forEach(([a, b]) => {
  interactionGroup.add(dashedLine(a, b, 0x4a90e8, 0.2, 0.2));
});

// ─────────────────────────────────────────────
//  ION FLOW (Ca²⁺ particles)
// ─────────────────────────────────────────────
const ions = [];
for (let i = 0; i < 5; i++) {
  const g = glowSphere(0.22, 0x40e860, [0,0,0]);
  g.userData.phase = i / 5;
  ionGroup.add(g);
  ions.push(g);
}

// ─────────────────────────────────────────────
//  ORBIT CONTROLS (manual)
// ─────────────────────────────────────────────
let isDragging = false, isRightDrag = false;
let prevMouse = { x: 0, y: 0 };
let sphericalTheta = 0, sphericalPhi = Math.PI / 3;
let sphericalRadius = 22;
let panX = 0, panY = 0;
let targetTheta = 0, targetPhi = Math.PI / 3;
let targetRadius = 22, targetPanX = 0, targetPanY = 0;

renderer.domElement.addEventListener('mousedown', e => {
  isDragging = true;
  isRightDrag = e.button === 2;
  prevMouse = { x: e.clientX, y: e.clientY };
});
renderer.domElement.addEventListener('contextmenu', e => e.preventDefault());
window.addEventListener('mousemove', e => {
  if (!isDragging) return;
  const dx = e.clientX - prevMouse.x;
  const dy = e.clientY - prevMouse.y;
  if (isRightDrag) {
    targetPanX -= dx * 0.03;
    targetPanY += dy * 0.03;
  } else {
    targetTheta -= dx * 0.008;
    targetPhi = Math.max(0.2, Math.min(Math.PI - 0.2, targetPhi + dy * 0.008));
  }
  prevMouse = { x: e.clientX, y: e.clientY };
});
window.addEventListener('mouseup', () => { isDragging = false; });
renderer.domElement.addEventListener('wheel', e => {
  targetRadius = Math.max(5, Math.min(45, targetRadius + e.deltaY * 0.05));
});

// ─────────────────────────────────────────────
//  STEP CAMERAS
// ─────────────────────────────────────────────
const stepData = [
  {
    theta: 0.3, phi: 1.0, radius: 22, px: 0, py: 0,
    title: 'Overview',
    tag: 'tag-struct', tagText: 'TRPV1 · Full Complex',
    text: 'Capsaicin docked inside the <strong>vanilloid binding pocket</strong> of TRPV1. The receptor\'s 4 helical subunits surround a central ion pore. The glowing yellow ring marks the binding domain.'
  },
  {
    theta: 0.8, phi: 1.2, radius: 16, px: 0, py: 1,
    title: 'TRPV1 Receptor',
    tag: 'tag-struct', tagText: 'Ion Channel · Homotetramer',
    text: '<strong>TRPV1</strong> is a non-selective cation channel (PDB: 3J5P). Four subunits each contribute 6 TM helices (S1–S6). The inner S5–S6 helices line the pore. Lipid bilayers (brown) mark the membrane.'
  },
  {
    theta: -0.5, phi: 0.9, radius: 14, px: -1, py: -1,
    title: 'Capsaicin',
    tag: 'tag-struct', tagText: 'C₁₈H₂₇NO₃ · MW 305 Da',
    text: '<strong>Three pharmacophore regions:</strong> [A] Vanillyl head (aromatic ring, –OH in red, –OCH₃), [B] Amide linker (N in blue, C=O in red), [C] Hydrophobic acyl tail (C8 chain, grey).'
  },
  {
    theta: 0.2, phi: 1.3, radius: 12, px: 0.5, py: -1.5,
    title: 'Binding Interactions',
    tag: 'tag-hbond', tagText: 'H-bonds + van der Waals',
    text: '<strong>Green dashes:</strong> H-bonds from –OH→Tyr511/Ser512 and C=O→Thr550.<br><strong>Blue dashes:</strong> VdW contacts between acyl tail and Leu515, Met547, Ile569, Leu669.'
  },
  {
    theta: 0.0, phi: 0.4, radius: 18, px: 0, py: 2,
    title: 'Ion Flow',
    tag: 'tag-hbond', tagText: 'Channel Open State',
    text: 'Capsaicin binding triggers a conformational change → <strong>pore gate opens</strong>. Ca²⁺ and Na⁺ flood into the neuron (green spheres), depolarising the membrane and generating a pain action potential.'
  }
];

let currentStep = 0;
function setStep(i) {
  currentStep = i;
  document.querySelectorAll('.step-btn').forEach((b, j) => {
    b.classList.toggle('active', j === i);
  });
  const d = stepData[i];
  targetTheta = d.theta;
  targetPhi = d.phi;
  targetRadius = d.radius;
  targetPanX = d.px;
  targetPanY = d.py;
  // Update info panel
  document.querySelector('#info-panel .info-title').textContent = d.title;
  document.querySelector('#info-panel .info-tag').className = 'info-tag ' + d.tag;
  document.querySelector('#info-panel .info-tag').textContent = d.tagText;
  document.querySelector('#info-panel p').innerHTML = d.text;
}

// ─────────────────────────────────────────────
//  STAR FIELD BACKGROUND
// ─────────────────────────────────────────────
const starGeo = new THREE.BufferGeometry();
const starVerts = [];
for (let i = 0; i < 1800; i++) {
  starVerts.push((Math.random()-0.5)*160, (Math.random()-0.5)*160, (Math.random()-0.5)*160);
}
starGeo.setAttribute('position', new THREE.Float32BufferAttribute(starVerts, 3));
const starMat = new THREE.PointsMaterial({ color: 0x8899aa, size: 0.08, transparent: true, opacity: 0.5 });
scene.add(new THREE.Points(starGeo, starMat));

// ─────────────────────────────────────────────
//  ANIMATE
// ─────────────────────────────────────────────
let t = 0;
function animate() {
  requestAnimationFrame(animate);
  t += 0.016;

  // Smooth camera
  const ease = 0.06;
  sphericalTheta += (targetTheta - sphericalTheta) * ease;
  sphericalPhi   += (targetPhi   - sphericalPhi)   * ease;
  sphericalRadius+= (targetRadius- sphericalRadius) * ease;
  panX += (targetPanX - panX) * ease;
  panY += (targetPanY - panY) * ease;

  camera.position.set(
    panX + sphericalRadius * Math.sin(sphericalPhi) * Math.sin(sphericalTheta),
    panY + sphericalRadius * Math.cos(sphericalPhi),
    sphericalRadius * Math.sin(sphericalPhi) * Math.cos(sphericalTheta)
  );
  camera.lookAt(panX, panY, 0);

  // Gentle receptor sway
  receptorGroup.rotation.y = Math.sin(t * 0.12) * 0.04;

  // Pocket torus pulse
  pocketTorus.material.opacity = 0.3 + 0.35 * Math.sin(t * 2.2);
  pocketTorus.scale.setScalar(1 + 0.04 * Math.sin(t * 2.2));

  // Capsaicin slow rotation
  capsaicinGroup.rotation.y += 0.004;

  // Interaction lines visibility
  interactionGroup.visible = currentStep >= 3;

  // Ion animation
  const showIons = currentStep === 4;
  ionGroup.visible = showIons;
  if (showIons) {
    ions.forEach((ion, i) => {
      const phase = (t * 0.7 + ion.userData.phase) % 1;
      ion.position.set(
        (Math.random() - 0.5) * 0.3,
        4.5 - phase * 10,
        (Math.random() - 0.5) * 0.3
      );
      ion.children[0].material.opacity = phase < 0.1 ? phase * 10 : phase > 0.9 ? (1-phase)*10 : 0.95;
    });
  }

  // Point light animation
  fillLight.position.x = -8 + Math.sin(t * 0.3) * 3;
  fillLight.position.z = 8 + Math.cos(t * 0.3) * 3;

  renderer.render(scene, camera);
}
animate();

// ─────────────────────────────────────────────
//  RESIZE
// ─────────────────────────────────────────────
window.addEventListener('resize', () => {
  const W = window.innerWidth, H = window.innerHeight;
  camera.aspect = W / H;
  camera.updateProjectionMatrix();
  renderer.setSize(W, H);
});
</script>
</body>
</html>
