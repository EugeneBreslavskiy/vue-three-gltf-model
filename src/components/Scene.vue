<template>
  <section class="scene" ref="scene"></section>
</template>

<script>
    import * as THREE from 'three'
    import GLTFLoader from 'three-gltf-loader'
    const OrbitControls = require('three-orbitcontrols')
    
    export default {
        name: 'Scene',
        data() {
            return {
                container: null,
                camera: null,
                controls: null,
                scene: null,
                renderer: null
            }
        },
        methods: {
            init() {
                this.container = this.$refs.scene
                this.scene = new THREE.Scene()
                
                this.createCamera()
                this.createControls()
                this.createLights()
                this.createRenderer()
                this.loadModel()
                
                this.renderer.setAnimationLoop(() => {
                    this.update()
                    this.render()
                })

                window.addEventListener( 'resize', this.resizeWindowHandler, false )
            },
            createCamera() {
                this.camera = new THREE.PerspectiveCamera( 45, window.innerWidth / window.innerHeight, 1, 2000 )
                this.camera.position.set( 0, 0, 18 )
            },
            createControls() {
                this.controls = new OrbitControls( this.camera, this.container )
                this.controls.enableZoom = true
                this.controls.minDistance = 5
                this.controls.maxDistance = 25
                this.controls.enablePan = false
                this.controls.enableDamping = true
                this.controls.dampingFactor = 0.35
                this.controls.minPolarAngle = 1.5708
                this.controls.maxPolarAngle = 1.5708
                this.controls.autoRotate = true
                this.controls.autoRotateSpeed = 0.25
            },
            createLights() {
                const ambientLight = new THREE.AmbientLight( 0xffffff, 1 )
                
                const light_1 = new THREE.DirectionalLight( 0xdddddd, 0.5 )
                light_1.position.set( 0, 5, 0 )

                const helper_1 = new THREE.DirectionalLightHelper( light_1, 5 )

                const light_2 = new THREE.DirectionalLight( 0xdddddd, 0.5 )
                light_2.position.set( 5, 0, 0 )

                const helper_2 = new THREE.DirectionalLightHelper( light_2, 5 )

                const light_3 = new THREE.DirectionalLight( 0xdddddd, 0.5 )
                light_3.position.set( -5, 0, 0 )

                const helper_3 = new THREE.DirectionalLightHelper( light_3, 5 )
                
                const light_4 = new THREE.DirectionalLight( 0xdddddd, 0.5 )
                light_4.position.set( 0, 0, 5 )

                const helper_4 = new THREE.DirectionalLightHelper( light_4, 5 )
                
                const light_5 = new THREE.DirectionalLight( 0xdddddd, 0.5 )
                light_5.position.set( 0, 0, -5 )

                const helper_5 = new THREE.DirectionalLightHelper( light_5, 5 )

                const light_6 = new THREE.DirectionalLight( 0xdddddd, 0.5 )
                light_6.position.set( 0, -5, 0 )

                const helper_6 = new THREE.DirectionalLightHelper( light_6, 5 )
                
                this.scene.add(
                    ambientLight,
                    light_1, helper_1,
                    light_2, helper_2,
                    light_3, helper_3,
                    light_4, helper_4,
                    light_5, helper_5,
                    light_6, helper_6
                )
            },
            createRenderer() {
                this.renderer = new THREE.WebGLRenderer( { antialias: true, alpha: true } )
                this.renderer.setSize( window.innerWidth, window.innerHeight )
                this.renderer.setPixelRatio( window.devicePixelRatio )
                this.renderer.autoClear = false;
                this.renderer.setClearColor(0x000000, 0.0)
                this.container.appendChild( this.renderer.domElement )
            },
            loadModel() {
                const loader = new GLTFLoader()
                
                loader.load(
                    '/static/models/sculpture/scene.gltf',
                    ( gltf ) => {
                        gltf.scene.position.set( 0, 0, 0)
                        this.scene.add(gltf.scene)
                    },
                    ( xhr ) => {
                        console.log( `${( xhr.loaded / xhr.total * 100 )}% loaded` )
                    },
                    ( error ) => {
                        console.error( 'An error happened', error )
                    },
                )
            },
            update() {
                this.controls.update()
            },
            render() {
                this.renderer.render( this.scene, this.camera )
            },
            resizeWindowHandler() {
                this.camera.aspect = window.innerWidth / window.innerHeight
                this.camera.updateProjectionMatrix()

                this.renderer.setSize( window.innerWidth, window.innerHeight )
            }
        },
        mounted() {
            this.init()
        }
    }
</script>

<style lang="sass">
  
  .scene
    width: 100%
    height: 100%
    position: absolute

</style>
