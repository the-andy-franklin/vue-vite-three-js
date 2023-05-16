<template>
  <div ref="mount"></div>
</template>

<script lang="ts">
import { onMounted, onBeforeUnmount, ref, Ref } from 'vue';
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

THREE

export default {
  setup() {
    const mount: Ref<HTMLDivElement | null> = ref(null);
    let frameId: number;
    let scene: THREE.Scene;
    let camera: THREE.PerspectiveCamera;
    let renderer: THREE.WebGLRenderer;
    let cube: THREE.Mesh;
    let controls: OrbitControls;

    onMounted(() => {
      initThree();
    });

    onBeforeUnmount(() => {
      stop();
      renderer.domElement.removeEventListener('animation', animate);
      mount.value?.removeChild(renderer.domElement);
    });

    function initThree() {
      scene = new THREE.Scene();
      camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      renderer = new THREE.WebGLRenderer();

      renderer.setSize(window.innerWidth, window.innerHeight);
      mount.value.appendChild(renderer.domElement);

      const geometry = new THREE.BoxGeometry(1, 1, 1);
      const material = new THREE.MeshBasicMaterial({ color: '#433F81' });
      cube = new THREE.Mesh(geometry, material);

      scene.add(cube);
      camera.position.z = 3;

      controls = new OrbitControls(camera, renderer.domElement);
      controls.enableDamping = true; // an animation loop is required when damping or auto-rotation are enabled
      controls.dampingFactor = 0.25;
      controls.enableZoom = false;

      start();
    }

    function start() {
      if (!renderer) return;
      stop();
      animate();
    }

    function stop() {
      cancelAnimationFrame(frameId);
    }

    function animate() {
      frameId = requestAnimationFrame(animate);

      // required if controls.enableDamping or controls.autoRotate are set to true
      controls.update();

      renderScene();
    }

    function renderScene() {
      renderer.render(scene, camera);
    }

    return {
      mount
    };
  }
};
</script>

<style scoped>
div {
  width: 100vw;
  height: 100vh;
}
</style>