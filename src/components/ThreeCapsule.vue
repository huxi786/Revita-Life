<script setup>
import { onMounted, ref, onUnmounted } from 'vue';
import * as THREE from 'three';
import gsap from 'gsap';
import ScrollTrigger from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

const canvasRef = ref(null);
const containerRef = ref(null);

let renderer, scene, camera;
let jar, jarFloatWrapper;
let leftCapsule, rightCapsule, leftFloatWrapper, rightFloatWrapper;
let animationFrameId;
const mouse = { x: 0, y: 0 };
const isMobile = window.innerWidth < 768;

onMounted(async () => {
  await document.fonts.ready;
  initThree();
  createStudioEnvironment();
  addJar();
  addCapsules();
  initScrollTimeline();
  animate();

  window.addEventListener('resize', onWindowResize);
  window.addEventListener('mousemove', onMouseMove);
  window.addEventListener('touchmove', onTouchMove, { passive: true });
});

onUnmounted(() => {
  window.removeEventListener('resize', onWindowResize);
  window.removeEventListener('mousemove', onMouseMove);
  window.removeEventListener('touchmove', onTouchMove);
  cancelAnimationFrame(animationFrameId);
  if (renderer) renderer.dispose();
  ScrollTrigger.getAll().forEach(st => st.kill());
});

function initThree() {
  scene = new THREE.Scene();

  camera = new THREE.PerspectiveCamera(32, window.innerWidth / window.innerHeight, 0.1, 500);
  camera.position.set(0, 0.4, 16);

  renderer = new THREE.WebGLRenderer({
    canvas: canvasRef.value,
    alpha: true,
    antialias: true
  });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  renderer.setClearColor(0x000000, 0);
  renderer.outputColorSpace = THREE.SRGBColorSpace;
  renderer.toneMapping = THREE.ACESFilmicToneMapping;
  renderer.toneMappingExposure = 1.3;

  // Studio Lighting
  const ambient = new THREE.AmbientLight(0xfff8f0, 0.6);
  scene.add(ambient);

  const keyLight = new THREE.SpotLight(0xfff5eb, 4);
  keyLight.position.set(8, 14, 10);
  keyLight.angle = 0.45;
  keyLight.penumbra = 0.7;
  keyLight.decay = 1.8;
  scene.add(keyLight);

  const rimLight = new THREE.PointLight(0xe8e0d8, 2.5);
  rimLight.position.set(-12, 6, -6);
  rimLight.decay = 2;
  scene.add(rimLight);

  const fillLight = new THREE.PointLight(0xf0e4d4, 1.8);
  fillLight.position.set(0, -4, 10);
  fillLight.decay = 2;
  scene.add(fillLight);

  const backLight = new THREE.PointLight(0xffffff, 1.2);
  backLight.position.set(0, 10, -12);
  backLight.decay = 2;
  scene.add(backLight);
}

function createStudioEnvironment() {
  const pmrem = new THREE.PMREMGenerator(renderer);
  const envScene = new THREE.Scene();

  // Warm white enclosing sphere
  const shell = new THREE.Mesh(
    new THREE.SphereGeometry(60, 32, 32),
    new THREE.MeshBasicMaterial({ color: 0xf5f0ea, side: THREE.BackSide })
  );
  envScene.add(shell);

  // Overhead softbox
  const topBox = new THREE.Mesh(
    new THREE.PlaneGeometry(30, 30),
    new THREE.MeshBasicMaterial({ color: 0xffffff, side: THREE.DoubleSide })
  );
  topBox.position.y = 22;
  topBox.rotation.x = Math.PI / 2;
  envScene.add(topBox);

  // Right key panel
  const rightPanel = new THREE.Mesh(
    new THREE.PlaneGeometry(18, 24),
    new THREE.MeshBasicMaterial({ color: 0xfff6ee, side: THREE.DoubleSide })
  );
  rightPanel.position.set(20, 6, 8);
  rightPanel.lookAt(0, 0, 0);
  envScene.add(rightPanel);

  // Left fill panel
  const leftPanel = new THREE.Mesh(
    new THREE.PlaneGeometry(14, 18),
    new THREE.MeshBasicMaterial({ color: 0xf0e8dd, side: THREE.DoubleSide })
  );
  leftPanel.position.set(-18, 4, 6);
  leftPanel.lookAt(0, 0, 0);
  envScene.add(leftPanel);

  // Ground
  const ground = new THREE.Mesh(
    new THREE.PlaneGeometry(50, 50),
    new THREE.MeshBasicMaterial({ color: 0xe5ddd4, side: THREE.DoubleSide })
  );
  ground.position.y = -18;
  ground.rotation.x = -Math.PI / 2;
  envScene.add(ground);

  const envMap = pmrem.fromScene(envScene, 0, 0.1, 100).texture;
  scene.environment = envMap;
  pmrem.dispose();
}

function createLabelTexture() {
  const canvas = document.createElement('canvas');
  canvas.width = 1024;
  canvas.height = 512;
  const ctx = canvas.getContext('2d');

  // White background
  ctx.fillStyle = '#ffffff';
  ctx.fillRect(0, 0, 1024, 512);

  // Double border frame
  ctx.strokeStyle = '#c4b5a3';
  ctx.lineWidth = 2;
  ctx.strokeRect(28, 28, 968, 456);
  ctx.strokeStyle = '#ddd3c6';
  ctx.lineWidth = 1;
  ctx.strokeRect(36, 36, 952, 440);

  // Brand name
  ctx.fillStyle = '#3d352c';
  ctx.textAlign = 'center';
  ctx.font = '700 100px "Cormorant Garamond", serif';
  ctx.fillText('Revitalife', 512, 195);

  // Registered trademark
  ctx.font = '500 30px "Inter", sans-serif';
  ctx.fillStyle = '#8a7d6f';
  ctx.fillText('\u00AE', 688, 148);

  // Decorative divider line
  ctx.strokeStyle = '#c4b5a3';
  ctx.lineWidth = 1;
  ctx.beginPath();
  ctx.moveTo(340, 228);
  ctx.lineTo(684, 228);
  ctx.stroke();

  // Diamond at center of divider
  ctx.fillStyle = '#c4b5a3';
  ctx.beginPath();
  ctx.moveTo(512, 220);
  ctx.lineTo(518, 228);
  ctx.lineTo(512, 236);
  ctx.lineTo(506, 228);
  ctx.closePath();
  ctx.fill();

  // Tagline
  ctx.font = '400 30px "Inter", sans-serif';
  ctx.fillStyle = '#a8927d';
  ctx.fillText('Customized. Compounded. Care.', 512, 285);

  // Company line
  ctx.font = '300 20px "Inter", sans-serif';
  ctx.fillStyle = '#bfb1a2';
  ctx.fillText('Revitalife Inc.', 512, 430);

  const texture = new THREE.CanvasTexture(canvas);
  texture.anisotropy = 16;
  texture.needsUpdate = true;
  return texture;
}

function createShadowTexture() {
  const canvas = document.createElement('canvas');
  canvas.width = 256;
  canvas.height = 256;
  const ctx = canvas.getContext('2d');
  const grad = ctx.createRadialGradient(128, 128, 10, 128, 128, 128);
  grad.addColorStop(0, 'rgba(60,45,30,0.10)');
  grad.addColorStop(0.6, 'rgba(60,45,30,0.04)');
  grad.addColorStop(1, 'rgba(60,45,30,0)');
  ctx.fillStyle = grad;
  ctx.fillRect(0, 0, 256, 256);
  const texture = new THREE.CanvasTexture(canvas);
  texture.needsUpdate = true;
  return texture;
}

function addJar() {
  // Glass material
  const glassMat = new THREE.MeshPhysicalMaterial({
    color: 0xffffff,
    metalness: 0,
    roughness: 0.02,
    transmission: isMobile ? 0.85 : 0.96,
    thickness: 1.5,
    ior: 1.52,
    transparent: true,
    opacity: 1,
    clearcoat: 1,
    clearcoatRoughness: 0.02,
    envMapIntensity: 1.3,
    specularIntensity: 1,
    specularColor: new THREE.Color(0xffffff),
    side: THREE.DoubleSide
  });

  // Beige material for lid
  const beigeMat = new THREE.MeshPhysicalMaterial({
    color: 0xa8927d,
    roughness: 0.28,
    metalness: 0.02,
    clearcoat: 0.8,
    clearcoatRoughness: 0.12,
    envMapIntensity: 0.9
  });

  jar = new THREE.Group();

  // --- Realistic bottle profile via LatheGeometry ---
  const pts = [];
  // Closed rounded bottom
  pts.push(new THREE.Vector2(0, -2.3));
  pts.push(new THREE.Vector2(1.6, -2.3));
  pts.push(new THREE.Vector2(1.88, -2.2));
  pts.push(new THREE.Vector2(2.0, -2.0));

  // Body with subtle bulge
  const bodySteps = 18;
  for (let i = 0; i <= bodySteps; i++) {
    const t = i / bodySteps;
    const y = -2.0 + t * 3.6;
    const bulge = Math.sin(t * Math.PI) * 0.08;
    pts.push(new THREE.Vector2(2.05 + bulge, y));
  }

  // Shoulder transition
  pts.push(new THREE.Vector2(1.95, 1.9));
  pts.push(new THREE.Vector2(1.78, 2.15));
  pts.push(new THREE.Vector2(1.68, 2.3));

  // Neck
  pts.push(new THREE.Vector2(1.63, 2.35));
  pts.push(new THREE.Vector2(1.63, 2.6));

  // Rim lip
  pts.push(new THREE.Vector2(1.72, 2.6));
  pts.push(new THREE.Vector2(1.72, 2.5));

  const bodyGeo = new THREE.LatheGeometry(pts, 64);
  const body = new THREE.Mesh(bodyGeo, glassMat);
  jar.add(body);

  // --- Lid with ridges ---
  const lidGroup = new THREE.Group();

  const lidGeo = new THREE.CylinderGeometry(1.82, 1.86, 0.75, 64);
  const lidMain = new THREE.Mesh(lidGeo, beigeMat);
  lidGroup.add(lidMain);

  // Top cap of lid
  const lidTopGeo = new THREE.CylinderGeometry(1.82, 1.82, 0.08, 64);
  const lidTop = new THREE.Mesh(lidTopGeo, beigeMat);
  lidTop.position.y = 0.415;
  lidGroup.add(lidTop);

  // 40 ridges around lid
  const ridgeGeo = new THREE.BoxGeometry(0.055, 0.55, 0.12);
  for (let i = 0; i < 40; i++) {
    const ridge = new THREE.Mesh(ridgeGeo, beigeMat);
    const angle = (i / 40) * Math.PI * 2;
    ridge.position.x = Math.cos(angle) * 1.84;
    ridge.position.z = Math.sin(angle) * 1.84;
    ridge.rotation.y = -angle;
    lidGroup.add(ridge);
  }

  lidGroup.position.y = 2.98;
  jar.add(lidGroup);

  // --- Label ---
  const labelGeo = new THREE.CylinderGeometry(
    2.08, 2.08, 2.6, 64, 1, true,
    -Math.PI / 3.5, Math.PI / 1.75
  );
  const labelTexture = createLabelTexture();
  const labelMat = new THREE.MeshBasicMaterial({
    map: labelTexture,
    transparent: true,
    side: THREE.DoubleSide
  });
  const labelMesh = new THREE.Mesh(labelGeo, labelMat);
  labelMesh.position.y = -0.15;
  jar.add(labelMesh);

  // --- Ground shadow ---
  const shadowTex = createShadowTexture();
  const shadowGeo = new THREE.PlaneGeometry(7, 7);
  const shadowMat = new THREE.MeshBasicMaterial({
    map: shadowTex,
    transparent: true,
    depthWrite: false
  });
  const shadow = new THREE.Mesh(shadowGeo, shadowMat);
  shadow.rotation.x = -Math.PI / 2;
  shadow.position.y = -2.35;
  jar.add(shadow);

  // Float wrapper — separates micro-float from scroll animation
  jarFloatWrapper = new THREE.Group();
  jarFloatWrapper.add(jar);
  scene.add(jarFloatWrapper);
}

function addCapsules() {
  // Beige for capsule tops
  const beigeMat = new THREE.MeshPhysicalMaterial({
    color: 0xa8927d,
    roughness: 0.28,
    metalness: 0.02,
    clearcoat: 0.8,
    clearcoatRoughness: 0.12,
    envMapIntensity: 0.9
  });

  // Off-white for capsule bottoms
  const offWhiteMat = new THREE.MeshPhysicalMaterial({
    color: 0xf2ece4,
    roughness: 0.15,
    metalness: 0,
    clearcoat: 1,
    clearcoatRoughness: 0.05,
    envMapIntensity: 0.7
  });

  // Seam ring material
  const seamMat = new THREE.MeshBasicMaterial({ color: 0xc0b0a0 });

  const createCapsule = () => {
    const group = new THREE.Group();
    const r = 0.35;
    const halfBody = 0.55;

    // Top half — beige
    const topCapGeo = new THREE.SphereGeometry(r, 32, 16, 0, Math.PI * 2, 0, Math.PI / 2);
    const topCap = new THREE.Mesh(topCapGeo, beigeMat);
    topCap.position.y = halfBody;

    const topCylGeo = new THREE.CylinderGeometry(r, r, halfBody, 32);
    const topCyl = new THREE.Mesh(topCylGeo, beigeMat);
    topCyl.position.y = halfBody / 2;

    // Bottom half — off-white
    const bottomCapGeo = new THREE.SphereGeometry(r, 32, 16, 0, Math.PI * 2, Math.PI / 2, Math.PI / 2);
    const bottomCap = new THREE.Mesh(bottomCapGeo, offWhiteMat);
    bottomCap.position.y = -halfBody;

    const bottomCylGeo = new THREE.CylinderGeometry(r, r, halfBody, 32);
    const bottomCyl = new THREE.Mesh(bottomCylGeo, offWhiteMat);
    bottomCyl.position.y = -halfBody / 2;

    // Seam ring between two halves
    const ringGeo = new THREE.TorusGeometry(r + 0.004, 0.009, 8, 48);
    const ring = new THREE.Mesh(ringGeo, seamMat);
    ring.rotation.x = Math.PI / 2;

    group.add(topCap, topCyl, bottomCap, bottomCyl, ring);
    return group;
  };

  // Left capsule — hidden behind jar
  leftCapsule = createCapsule();
  leftCapsule.position.set(0.3, 0.2, -1.5);
  leftCapsule.rotation.z = Math.PI * 0.18;

  leftFloatWrapper = new THREE.Group();
  leftFloatWrapper.add(leftCapsule);
  scene.add(leftFloatWrapper);

  // Right capsule — hidden behind jar
  rightCapsule = createCapsule();
  rightCapsule.position.set(-0.3, 0.2, -1.5);
  rightCapsule.rotation.z = -Math.PI * 0.18;

  rightFloatWrapper = new THREE.Group();
  rightFloatWrapper.add(rightCapsule);
  scene.add(rightFloatWrapper);
}

function initScrollTimeline() {
  const tl = gsap.timeline({
    scrollTrigger: {
      trigger: containerRef.value,
      start: 'top bottom',
      end: 'bottom top',
      scrub: 1.8,
    }
  });

  // Phase 1: Capsules emerge from behind jar
  tl.to(leftCapsule.position, {
    x: -5, z: 2.8, y: 0.4,
    duration: 3,
    ease: 'power2.inOut'
  }, 0);
  tl.to(rightCapsule.position, {
    x: 5, z: 2.8, y: 0.4,
    duration: 3,
    ease: 'power2.inOut'
  }, 0);

  // Phase 2: Jar scale focus + capsule rotation
  tl.to(jar.scale, {
    x: 1.1, y: 1.1, z: 1.1,
    duration: 2,
    ease: 'power1.inOut'
  }, 1.2);
  tl.to(leftCapsule.rotation, {
    x: Math.PI * 0.35, y: Math.PI * 0.12,
    duration: 2.5
  }, 1.5);
  tl.to(rightCapsule.rotation, {
    x: -Math.PI * 0.35, y: -Math.PI * 0.12,
    duration: 2.5
  }, 1.5);

  // Phase 3: Exit — capsules float up, jar recedes
  tl.to(leftCapsule.position, {
    y: 8,
    duration: 3,
    ease: 'power1.in'
  }, 3.5);
  tl.to(rightCapsule.position, {
    y: 8,
    duration: 3,
    ease: 'power1.in'
  }, 3.5);
  tl.to(jar.position, {
    z: -7, y: -2.5,
    duration: 3.5,
    ease: 'power1.in'
  }, 3.5);
  tl.to(jar.scale, {
    x: 0.65, y: 0.65, z: 0.65,
    duration: 3.5
  }, 3.5);
}

function onMouseMove(event) {
  mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
  mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
}

function onTouchMove(event) {
  if (event.touches.length > 0) {
    mouse.x = (event.touches[0].clientX / window.innerWidth) * 2 - 1;
    mouse.y = -(event.touches[0].clientY / window.innerHeight) * 2 + 1;
  }
}

function animate() {
  animationFrameId = requestAnimationFrame(animate);

  const time = Date.now() * 0.0007;

  // Smooth mouse parallax
  const targetRY = mouse.x * 0.06;
  const targetRX = -mouse.y * 0.04;
  scene.rotation.y += (targetRY - scene.rotation.y) * 0.035;
  scene.rotation.x += (targetRX - scene.rotation.x) * 0.035;

  // Micro-float on wrappers (independent of GSAP scroll positions)
  jarFloatWrapper.position.y = Math.sin(time) * 0.12;
  leftFloatWrapper.position.y = Math.cos(time * 1.15) * 0.09;
  rightFloatWrapper.position.y = Math.sin(time * 1.08) * 0.09;

  renderer.render(scene, camera);
}

function onWindowResize() {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
}
</script>

<template>
  <section ref="containerRef" class="revitalife-3d-section">
    <div class="three-canvas-wrapper">
      <canvas ref="canvasRef"></canvas>
    </div>

    <div class="revitalife-3d-content">
      <div class="container">
        <div class="scroll-hint">
          <div class="mouse-icon">
            <div class="wheel"></div>
          </div>
          <span>Scroll Down to Experience</span>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.revitalife-3d-section {
  position: relative;
  height: 400vh;
  background: radial-gradient(ellipse at 50% 40%, #ffffff 0%, #f8f5f1 40%, #efe9e1 100%);
  overflow: visible;
}

.three-canvas-wrapper {
  position: sticky;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  z-index: 1;
}

canvas {
  display: block;
  width: 100% !important;
  height: 100% !important;
}

.revitalife-3d-content {
  position: sticky;
  top: 0;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  padding-bottom: 8vh;
  z-index: 2;
  pointer-events: none;
}

.scroll-hint {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.2rem;
  color: #a8927d;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  opacity: 0.5;
}

.mouse-icon {
  width: 24px;
  height: 38px;
  border: 2px solid #a8927d;
  border-radius: 12px;
  position: relative;
}

.wheel {
  width: 3px;
  height: 6px;
  background: #a8927d;
  position: absolute;
  top: 6px;
  left: 50%;
  transform: translateX(-50%);
  border-radius: 2px;
  animation: scrollWheel 2s infinite;
}

@keyframes scrollWheel {
  0% { transform: translate(-50%, 0); opacity: 0; }
  20% { opacity: 1; }
  100% { transform: translate(-50%, 12px); opacity: 0; }
}

@media (prefers-reduced-motion: reduce) {
  .wheel {
    animation: none;
  }
}
</style>