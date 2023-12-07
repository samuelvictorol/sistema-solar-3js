<template>
 <div id="canvas-container" class="animate__animated animate__fadeIn ">
    <div id="start-container" class="animate__animated animate__fadeIn">
      <button id="btn-start">START TRIP</button>
      <div class="by">
        <p>By: <a target="_blank" href="https://samuelvictorol.github.io/portfolio/">Samuel Victor</a></p>
      </div>
    </div>
  </div>
</template>

<script setup>
import * as THREE from 'three';
import { onMounted } from 'vue';

onMounted(() => {
// Configuração da cena, câmera e renderizador
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
scene.background = new THREE.Color(0x000000);

// Estrelas animadas
const starGeometry = new THREE.BufferGeometry();
const starMaterial = new THREE.PointsMaterial({ size: 0.05, vertexColors: true });
const starsVertices = [];
const starColors = [];
for (let i = 0; i < 10000; i++) {
  const x = (Math.random() - 0.5) * 2000;
  const y = (Math.random() - 0.5) * 2000;
  const z = (Math.random() - 0.5) * 2000;
  starsVertices.push(x, y, z);

  const color = new THREE.Color();
  color.setRGB(Math.random(), Math.random(), Math.random());
  starColors.push(color.r, color.g, color.b);
}
starGeometry.setAttribute('position', new THREE.Float32BufferAttribute(starsVertices, 3));
starGeometry.setAttribute('color', new THREE.Float32BufferAttribute(starColors, 3));
const stars = new THREE.Points(starGeometry, starMaterial);
scene.add(stars);
// PLANETAS ( e lua )
// Criação da lua
const moonGeometry = new THREE.SphereGeometry(10, 32, 32);
const moonTexture = new THREE.TextureLoader().load('../assets/moontxt.jpg');
const moonMaterial = new THREE.MeshBasicMaterial({ map: moonTexture });
const moon = new THREE.Mesh(moonGeometry, moonMaterial);

// Criação do Sol
const sunGeometry = new THREE.SphereGeometry(100,200, 200);
const sunTexture = new THREE.TextureLoader().load('../assets/suntxt.png');
const sunMaterial = new THREE.MeshBasicMaterial({ map: sunTexture });
const sun = new THREE.Mesh(sunGeometry, sunMaterial);

// Criação da Terra
const earthGeometry = new THREE.SphereGeometry(33, 42, 42);
const earthTexture = new THREE.TextureLoader().load('../assets/earthtxt.jpg');
const earthMaterial = new THREE.MeshBasicMaterial({ map: earthTexture });
const earth = new THREE.Mesh(earthGeometry, earthMaterial);

// Criação de Netuno
const neptuneGeometry = new THREE.SphereGeometry(3, 32, 32);
const neptuneTexture = new THREE.TextureLoader().load('../assets/netunotxt.jpg');
const neptuneMaterial = new THREE.MeshBasicMaterial({ map: neptuneTexture });
const neptune = new THREE.Mesh(neptuneGeometry, neptuneMaterial);

// Criação de Urano
const uranusGeometry = new THREE.SphereGeometry(3, 32, 32);
const uranusTexture = new THREE.TextureLoader().load('../assets/uranustxt.jpg');
const uranusMaterial = new THREE.MeshBasicMaterial({ map: uranusTexture });
const uranus = new THREE.Mesh(uranusGeometry, uranusMaterial);

// Criação de Saturno
const saturnGeometry = new THREE.SphereGeometry(20, 52, 52);
const saturnTexture = new THREE.TextureLoader().load('../assets/saturntxt.jpg');
const saturnMaterial = new THREE.MeshBasicMaterial({ map: saturnTexture });
const saturn = new THREE.Mesh(saturnGeometry, saturnMaterial);
saturn.position.set(0, 0, -60);

// Criação do anel de Saturno
const ringGeometry = new THREE.RingGeometry(32, 26, 32);
const ringTexture = new THREE.TextureLoader().load('../assets/ringtxt.png'); // Adapte para a textura do anel
const ringMaterial = new THREE.MeshBasicMaterial({
  map: ringTexture,
  side: THREE.DoubleSide,
  transparent: true,
  opacity: 1
});
const ring = new THREE.Mesh(ringGeometry, ringMaterial);
ring.rotation.x = Math.PI / 4; // Rotação do anel para que fique na posição correta

// Criação de Jupiter
const jupiterGeometry = new THREE.SphereGeometry(100, 100, 172);
const jupiterTexture = new THREE.TextureLoader().load('../assets/jupitertxt.jpg');
const jupiterMaterial = new THREE.MeshBasicMaterial({ map: jupiterTexture });
const jupiter = new THREE.Mesh(jupiterGeometry, jupiterMaterial);

// Criação de Marte
const marsGeometry = new THREE.SphereGeometry(30, 52, 52);
const marsTexture = new THREE.TextureLoader().load('../assets/marstxt.jpg');
const marsMaterial = new THREE.MeshBasicMaterial({ map: marsTexture });
const mars = new THREE.Mesh(marsGeometry, marsMaterial);

// Criação de Venus
const venusGeometry = new THREE.SphereGeometry(40, 52, 52);
const venusTexture = new THREE.TextureLoader().load('../assets/venustxt.jpg');
const venusMaterial = new THREE.MeshBasicMaterial({ map: venusTexture });
const venus = new THREE.Mesh(venusGeometry, venusMaterial);

// Criação de Mercurio
const mercurioGeometry = new THREE.SphereGeometry(50, 52, 52);
const mercurioTexture = new THREE.TextureLoader().load('../assets/mercuriotxt.jpg');
const mercurioMaterial = new THREE.MeshBasicMaterial({ map: mercurioTexture });
const mercurio = new THREE.Mesh(mercurioGeometry, mercurioMaterial);

scene.add(sun);
scene.add(mercurio)
scene.add(venus);
scene.add(earth);
scene.add(moon);
scene.add(mars);
scene.add(jupiter);
scene.add(ring);
scene.add(saturn);
scene.add(uranus);
scene.add(neptune);

// Configuração da posição inicial
const setInitialPosition = () => {
  sun.position.set(0, 0, -1550);
  mercurio.position.set(0, 0, -1345);
  venus.position.set(50, 0, -1150);
  moon.position.set(-10, 35, -1040);
  earth.position.set(10, 3, -950);
  mars.position.set(0, 0, -550);
  jupiter.position.set(-4, 1, -500);
  ring.position.set(0, 0, -140);
  saturn.position.set(0, 0, -140);
  uranus.position.set(0, 0, -40);
  neptune.position.set(0, 0, -12);
}

setInitialPosition();

const startTrip = () => {
  const startcontainer = document.getElementById('start-container');
  startcontainer.style.display = 'none';
  let step = 0;

  const movePlanets = () => {
    if (step >= 10000) {
      return; // Condição de parada
    }
    neptune.position.z += 0.01;
    neptune.position.x -= 0.3;
    uranus.position.z += 0.2;
    uranus.position.y += 0.05;
    uranus.position.x += 0.1;
    moon.position.z += 1.4;
    moon.position.x -= 0.2;
    earth.position.x -= 0.2;
    earth.position.z += 1.2;
    saturn.position.x -= 0.2;
    saturn.position.z += .3;
    saturn.position.y += 0.01;
    ring.position.x -= 0.2;
    ring.position.z += .3;
    ring.position.y += 0.01;
    jupiter.position.z += .6;
    jupiter.position.x += 0.1;
    jupiter.position.y += 0.5;
    mars.position.z += .7;
    mars.position.y -= 0.2;
    mars.position.x += 0.09;
    mercurio.position.z += 1.2;
    mercurio.position.x += 0.02;
    mercurio.position.y -= 0.1;
    venus.position.z += 1.3;
    venus.position.x += 0.2;
    venus.position.y += 0.06;
    sun.position.z += 1;
    if (sun.position.z >= -400) {
      startcontainer.style.display = 'flex';
      step = 10000;
      setInitialPosition();
      return;
    }
    step++;
    setTimeout(movePlanets, 30); 
  }
  movePlanets();
}

const start = document.getElementById('btn-start');
start.addEventListener('click', startTrip)

// Configuração da velocidade de rotação da lua
const moonRotationSpeed = 0.002; // Ajuste conforme necessário
// Função de animação
const animate = () => {
  requestAnimationFrame(animate);
  // Rotação da lua
  jupiter.rotation.y += 0.0005;
  moon.rotation.y += moonRotationSpeed;
  earth.rotation.y += 0.002;
  uranus.rotation.x += 0.0005;
  mars.rotation.y += 0.0005;
  neptune.rotation.y += 0.0005;
  ring.rotation.y += 0.005;
  saturn.rotation.y += 0.0005;
  sun.position.x = 7 * Math.cos(moon.rotation.y);
  sun.rotation.y += 0.0002;
  // Piscar suave das estrelas
  for (let i = 0; i < starColors.length; i += 3) {
    const color = starGeometry.attributes.color.array;
    color[i] = Math.random();
    color[i + 1] = Math.random();
    color[i + 2] = Math.random();
  }
  starGeometry.attributes.color.needsUpdate = true;

  // Renderiza a cena
  renderer.render(scene, camera);
};

// Inicia a animação
animate();
});
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500&family=Play:wght@400;700&display=swap');
body {
    margin: 0;
    overflow-x: hidden;
}
#canvas-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
}
* {
    margin: 0;
    transition: all 0.3s linear;
}
h1{
    background: transparent!important;
    background-color: transparent!important;
    color: #fff;
    position: absolute
}

#start-container {
    position: relative;
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #0505058d;
    backdrop-filter: blur(4px);
    transition: all 0.3s linear;
}

#btn-start {
    background-color: transparent;
    color: #7c7c7c;
    outline: none;
    border: none;
    border: 2px solid #7c7c7c;
    padding: 2rem;
    cursor: pointer;
    transition: all 0.3s linear;
    font-family: 'Play', sans-serif;
    font-size: 2rem;
    font-weight: bold;
    border-radius: 4px;
}

#btn-start:hover {
    color: #00d51c;
    border-color: #00d51c;
}

.by {
    position: absolute;
    bottom: 8px;
    color: #959595;
    text-decoration: none;
    font-family: 'Montserrat', sans-serif;
}

a{
    text-decoration: none;
    color: #d4d4d4;
    transition: all 0.3s linear;
}

a:hover{
    color: #1381ff;
}
</style>