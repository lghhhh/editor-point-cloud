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
  // const geometry = new THREE.BoxGeometry(100, 100, 100);
  // const material = new THREE.MeshLambertMaterial({
  //   color: 0x0000ff,
  // });
  // const mesh = new THREE.Mesh(geometry, material);
  const geomerty = new THREE.BufferGeometry();
  const vertices = new Float32Array([
    0, 0, 0, // 顶点1坐标
    80, 0, 0, // 顶点2坐标
    80, 80, 0, // 顶点3坐标
    0, 80, 0, // 顶点4坐标
  ]);
  const attribute = new THREE.BufferAttribute(vertices, 3);
  geomerty.attributes.position = attribute;

  // 法向量
  const normals = new Float32Array([
    0, 0, 1,
    0, 0, 1,
    0, 0, 1,
    0, 0, 1,
  ]);
  geomerty.attributes.normals = new THREE.BufferAttribute(normals, 3);

  // 顶点索引
  // 类型化数组 根据顶点个数 选择适合的uint16、uint32等等。
  const vertexIndex = new Uint16Array([
    // 0对应第1个顶点位置数据、第1个顶点法向量数据
  // 1对应第2个顶点位置数据、第2个顶点法向量数据
  // 索引值3个为一组，表示一个三角形的3个顶点
  0, 1, 2,
  0, 2, 3,
  ]);
  geomerty.index = new THREE.BufferAttribute(vertexIndex, 1);

  const material = new THREE.MeshBasicMaterial({
    color: 0x33cccc,
    side: THREE.DoubleSide,
  });

  const mesh = new THREE.Mesh(geomerty, material);

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
