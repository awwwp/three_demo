
<template>
  <canvas id="c"></canvas>
</template>

<script setup>
  import * as THREE from "three"
  import {onMounted} from "vue";

  onMounted(() => {
    main();
  })

  function main() {
    // 创建画布
    const canvas = document.querySelector('#c');

    // 创建渲染器
    const renderer = new THREE.WebGLRenderer({antialias: true, canvas});

    // 创建相机
    const fov = 75;
    const aspect = 2;  // 相机默认值
    const near = 0.1;
    const far = 30;
    const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
    camera.position.z = 5;
    camera.lookAt(0, 0, 0)

    // 创建场景
    const scene = new THREE.Scene();

    // 创建几何立方体
    const boxWidth = 1;
    const boxHeight = 1;
    const boxDepth = 1;
    const geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth);

    // 创建材质
    // const material = new THREE.MeshBasicMaterial({color: 0x44aa88});
    const material = new THREE.MeshPhongMaterial({color: 0x44aa88});  // 绿蓝色

    // 创建网格
    const cube = new THREE.Mesh(geometry, material);
    scene.add(cube);

    renderer.render(scene, camera);

    // 让立方体旋转起来
    requestAnimationFrame(render);
    function render(time) {
      time *= 0.001;  // 将时间单位变为秒

      if (resizeRendererToDisplaySize(renderer)) {
        const canvas = renderer.domElement;
        camera.aspect = canvas.clientWidth / canvas.clientHeight;
        camera.updateProjectionMatrix();
      }

      cube.rotation.x = time;
      cube.rotation.y = time;

      renderer.render(scene, camera);

      requestAnimationFrame(render);
    }

    // 加入一个平行光
    const color = 0xFFFFFF;
    const intensity = 3;
    const light = new THREE.DirectionalLight(color, intensity);
    light.position.set(-1, 2, 4);
    scene.add(light);

  }

  function resizeRendererToDisplaySize(renderer) {
    const canvas = renderer.domElement;
    const pixelRatio = window.devicePixelRatio;
    const width = Math.floor( canvas.clientWidth * pixelRatio );
    const height = Math.floor( canvas.clientHeight * pixelRatio );
    const needResize = canvas.width !== width || canvas.height !== height;
    if (needResize) {
      renderer.setSize(width, height, false);
    }
    return needResize;
  }

</script>

<style scoped>
  * {
    margin: 0;
    padding: 0;
  }

  canvas {
    display: block;
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
  }
</style>
