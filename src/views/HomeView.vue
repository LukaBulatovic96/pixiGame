
<script setup>
import * as THREE from "three";
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import {onMounted,ref} from "vue";


onMounted(() => {
    const scene = new THREE.Scene();

const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

const renderer = new THREE.WebGLRenderer({
  canvas: document.querySelector('#maincanvas'),
});

renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(window.innerWidth, window.innerHeight);
camera.position.setZ(30);
camera.position.setX(-3);

renderer.render(scene, camera);

// Torus

const geometry = new THREE.TorusGeometry(10, 2, 16, 100);
const material = new THREE.MeshStandardMaterial({ color: 0xff6347});// ,wireframe: true
const torus = new THREE.Mesh(geometry, material);

scene.add(torus);


const pointLight = new THREE.PointLight(0xffffff);
pointLight.position.set(5, 5, 5);

const ambientLight = new THREE.AmbientLight(0xffffff);
scene.add(pointLight, ambientLight);

// Helpers

const lightHelper = new THREE.PointLightHelper(pointLight)
const gridHelper = new THREE.GridHelper(200, 50);
scene.add(lightHelper, gridHelper)
const controls = new OrbitControls(camera, renderer.domElement);

function addStar() {
  const geometry = new THREE.SphereGeometry(0.25, 24, 24);
  const colorindex = Math.floor(Math.random() * 5);
  const colors = [0xffffff,0x00ff00,0xffff00,0x00ffff,0x0000ff]
  const material = new THREE.MeshStandardMaterial({ color: colors[colorindex] });
  const star = new THREE.Mesh(geometry, material);

  const [x, y, z] = Array(3)
    .fill()
    .map(() => THREE.MathUtils.randFloatSpread(100));

  star.position.set(x, y, z);
  scene.add(star);
}

Array(200).fill().forEach(addStar);



//ANIMATE
function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
    
  torus.rotation.x += 0.01;
  torus.rotation.y += 0.005;
  torus.rotation.z += 0.01;

}
animate();

})



</script>
<template >
<canvas id="maincanvas"></canvas>
</template>
<style lang="css">
    canvas {
        position: fixed;
        top:0;
        left:0;
    }
</style>