<template>
  <div id="scene-container" ref="sceneContainer"></div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'


export default {
  name: 'HelloWorld',
  data () {
    return {
      container: null,
      scene: null,
      camera: null,
      controls: null,
      renderer: null,
      stats: null
    }
  },
  methods: {
    init () {
      // 设置容器
      this.container = this.$refs.sceneContainer
      // 设置容器
   
     

      // 添加相机
      const fov = 2 // Field of view
      const aspect = this.container.clientWidth / this.container.clientHeight
      const near = 0.1 // the near clipping plane
      const far = 30 // the far clipping plane
      const camera = new THREE.PerspectiveCamera(fov, aspect, near, far)
      camera.position.set(0, 5, 10)
      this.camera = camera
       // 添加相机


      // 添加场景
      this.scene = new THREE.Scene()
      this.scene.background = new THREE.Color('white')
      // 添加场景

      //创建灯光
      const ambientLight = new THREE.HemisphereLight(
        0xffffff, // bright sky color
        0x222222, // dim ground color
        1 // intensity
      )
      const mainLight = new THREE.DirectionalLight(0xffffff, 4.0)
      const mainLight2 = new THREE.DirectionalLight(0xffffff, 4.0)
      mainLight.position.set(0, 10, 10)
      mainLight2.position.set(0, -50, 10)
      this.scene.add(ambientLight, mainLight,mainLight2)
      //创建灯光

      //创建控制
      this.controls = new OrbitControls(this.camera, this.container)
      //创建控制

      //创建渲染器
      this.renderer = new THREE.WebGLRenderer({ antialias: true })
      this.renderer.setSize(this.container.clientWidth, this.container.clientHeight)
      this.renderer.setPixelRatio(window.devicePixelRatio)
      this.renderer.gammaFactor = 2.2
      this.renderer.outputEncoding = THREE.sRGBEncoding
      this.renderer.physicallyCorrectLights = true
      this.container.appendChild(this.renderer.domElement)
      //创建渲染器

      //设置长宽比以匹配新的浏览器窗口长宽比
      this.camera.aspect = this.container.clientWidth / this.container.clientHeight
      this.camera.updateProjectionMatrix()
      this.renderer.setSize(this.container.clientWidth, this.container.clientHeight)
      //设置长宽比以匹配新的浏览器窗口长宽比

      //加载模型
      const loader = new GLTFLoader()
      loader.load(
        '/three-assets/OBJ.glb',
        gltf => {
          this.scene.add(gltf.scene)
        },
        undefined,
        undefined
      )
      //加载模型
      this.renderer.setAnimationLoop(() => {
        this.render()
      })
    },
    render () {
      this.renderer.render(this.scene, this.camera)
    }
  },
  mounted () {
    this.init()
  }
}
</script>

<style scoped>

#scene-container {
  height: 100%;
}
</style>
