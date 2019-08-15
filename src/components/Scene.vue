<template>
  <section class="scene" ref="scene"></section>
</template>

<script>
    import * as THREE from 'three'
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
                
                this.scene = new THREE.Scene({
                    background: new THREE.Color( 0x000000 )
                })
                
                this.createCamera()
                this.createControls()
                this.createLights()
                this.createRenderer()
                this.createObject()
                
                this.renderer.setAnimationLoop(() => {
                    this.update()
                    this.render()
                })
            },
            createCamera() {
                this.camera = new THREE.PerspectiveCamera( 45, window.innerWidth / window.innerHeight, 1, 2000 )
                this.camera.position.set( 0, 0, 20 )
            },
            createControls() {
                this.controls = new OrbitControls( this.camera, this.container )
                this.controls.noZoom = true
                this.controls.noPan = true
                this.controls.autoRotate = true
                this.controls.autoRotateSpeed = 1
                this.controls.enableDamping = true
                this.controls.dampingFactor = 0.5
            },
            createLights() {
                const ambientLight = new THREE.AmbientLight( 0x404040, 2 )
                const directionalLight = new THREE.DirectionalLight( 0xffffff, 5 )
                directionalLight.position.set( 10, 10, 10 )
                
                this.scene.add( ambientLight, directionalLight )
            },
            createRenderer() {
                this.renderer = new THREE.WebGLRenderer( { antialias: true } )
                this.renderer.setSize( window.innerWidth, window.innerHeight )

                this.renderer.setPixelRatio( window.devicePixelRatio )

                this.container.appendChild( this.renderer.domElement )
            },
            createObject() {
                let sphereGeometry = new THREE.SphereGeometry( 2, 32, 32 )
                let sphereMaterial = new THREE.MeshPhysicalMaterial({
                    color: 0xffffff,
                    metalness: 0.5,
                    roughness: 0.5,
                    reflectivity: 0.5
                })
                
                let sphere = new THREE.Mesh( sphereGeometry, sphereMaterial )
                
                this.scene.add( sphere )
            },
            update() {
                this.controls.update()
            },
            render() {
                this.renderer.render( this.scene, this.camera )
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
