<template>
  <div id="d3" style="height: 800px;width:100%">
  </div>
</template>
<script setup lang="ts">
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import { onMounted } from 'vue';

const init3d = () => {
  // 场景
  const scene = new THREE.Scene();
  // 模型
const geometry = new THREE.BufferGeometry();

const p1 = new THREE.Vector3(50, 0, 0); // 顶点1坐标
const p2 = new THREE.Vector3(0, 70, 0); // 顶点2坐标
const p3 = new THREE.Vector3(80, 70, 0); // 顶点3坐标
// 顶点坐标添加到geometry对象
geometry.setFromPoints([p1, p2, p3]);

const c1 = new THREE.Color(0x00ff00); // 顶点1颜色——绿色
const c2 = new THREE.Color(0xff0000); // 顶点2颜色——红色
const c3 = new THREE.Color(0x0000ff); // 顶点3颜色——蓝色
// geometry.attributes.color = new THREE.BufferAttribute([c1, c2, c3], 1);
  const material = new THREE.MeshBasicMaterial({
    color: 0x33cccc,
    side: THREE.DoubleSide,
  });

  const mesh = new THREE.Mesh(geometry, material);

  scene.add(mesh);

  // ----------------- 坐标轴
  const axesHelper = new THREE.AxesHelper(150);
  scene.add(axesHelper);

  //  -----------------------------------------------光源设置

  // 点光源
  const point = new THREE.PointLight(0xffffff);
  point.position.set(400, 200, 300);//  设置光源位置
  scene.add(point);

  // 环境光
  const ambient = new THREE.AmbientLight(0x444444);
  scene.add(ambient);

  // -----------------------------------------------设置相机
  const width = document.getElementById('d3')?.clientWidth as number;
  const height = document.getElementById('d3')?.clientHeight as number;
  // const width = window.innerWidth; // 窗口宽度
  // const height = window.innerHeight; // 窗口高度

  const k = width / height;// 窗口宽高比
  const s = 500;// 三维场景显示范围控制系数，系数越大，显示范围越大。
  const camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
  camera.position.set(50, 50, 50); // 设置相机位置
  camera.lookAt(scene.position); // 设置相机方向(指向的场景对象)

  // -------------------------------------------- 创建渲染器
  const renderer = new THREE.WebGLRenderer();
  // renderer.setSize(width, height);
  renderer.setSize(width, height);
  renderer.setClearColor(0xb9d3ff, 1); // 设置背景颜色
  document.getElementById('d3')?.appendChild(renderer.domElement);
  // document.body.appendChild(renderer.domElement); // body元素中插入canvas对象
  renderer.render(scene, camera);// 执行渲染操作

  // let T0 = Date.now();// 上次时间
  // function render() {
  //   const T1 = Date.now();// 本次时间
  //   const t = T1 - T0;// 时间差
  //   T0 = T1;// 把本次时间赋值给上次时间
  //   requestAnimationFrame(render);
  //   renderer.render(scene, camera);// 执行渲染操作
  //   mesh.rotateY(0.001 * t);// 旋转角速度0.001弧度每毫秒
  // }
  // render();

  // ---------------------------------------------------------- 控制器
  // const controls = new OrbitControls(camera, renderer.domElement);
  // controls.addEventListener('change', render);// 监听鼠标、键盘事件
};
onMounted(() => {
  init3d();
});

</script>
