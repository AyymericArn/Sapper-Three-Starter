<script lang="ts">
	import * as THREE from "three"
	import TWEEN from "@tweenjs/tween.js"
	import { onMount } from "svelte";

	const state = {
		frameCounter: 0
	}
	const raycaster = new THREE.Raycaster()
	const mouse = new THREE.Vector2(1, 1)
	let renderer: THREE.WebGLRenderer
	let scene: THREE.Scene
	let camera: THREE.PerspectiveCamera
	
	function setup () {
		scene = new THREE.Scene()
		camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 )
		renderer = new THREE.WebGLRenderer({alpha: true, antialias: true})

		// renderer
		renderer.setClearAlpha(0.0)
		renderer.setClearColor(0x000000, 0)
		renderer.setPixelRatio( window.devicePixelRatio )
		renderer.setSize(window.innerWidth, window.innerWidth)
		renderer.domElement.classList.add('three-scene')
        renderer.domElement.setAttribute('style', `width: 100%; height: 100%;`)
        document.body.appendChild( renderer.domElement )
		window.addEventListener("resize", () => {
            renderer.setSize(window.innerWidth, window.innerHeight)
            camera.updateProjectionMatrix()
        })

		// lights
		const lights = {            
			ambient: new THREE.AmbientLight(0xffffff, 1.4),
			frontal: new THREE.DirectionalLight(0xffcccc, 0.3)
		}
		lights.frontal.position.x = 1
        lights.frontal.position.y = 1
        lights.frontal.position.z = 1
        lights.frontal.castShadow = true
        lights.frontal.shadow.camera.top = 0.6
        lights.frontal.shadow.camera.right = 0.6
        lights.frontal.shadow.camera.bottom = -0.6
        lights.frontal.shadow.camera.left = -0.6
        for (const l of Object.values(lights)) {
            scene.add(l)
		}
		
		const light = new THREE.HemisphereLight( 0xffffff, 0x444444 )
        light.position.set( 0, 200, 0 )
		scene.add( light )
		
		animate()
	}

	function updateMouse () {
		raycaster.setFromCamera(mouse, camera)
		// const intersects = raycaster.intersectObjects()
		// if (intersects) ...
	}

	function animate () {
		requestAnimationFrame(animate)

		// keep track of frame number to give custom rythm to animations
		state.frameCounter === 60 ? state.frameCounter = 0 : state.frameCounter++

		updateMouse()

		renderer.render(scene, camera)

		TWEEN.update()
	}

	onMount(setup)
</script>

<style>
	h1, figure, p {
		text-align: center;
		margin: 0 auto;
	}

	h1 {
		font-size: 2.8em;
		text-transform: uppercase;
		font-weight: 700;
		margin: 0 0 0.5em 0;
	}

	figure {
		margin: 0 0 1em 0;
	}

	img {
		width: 100%;
		max-width: 400px;
		margin: 0 0 1em 0;
	}

	p {
		margin: 1em auto;
	}

	@media (min-width: 480px) {
		h1 {
			font-size: 4em;
		}
	}
</style>

<svelte:head>
	<title>Sapper project template</title>
</svelte:head>

<h1>Great success!</h1>

<figure>
	<img alt='Success Kid' src='successkid.jpg'>
	<figcaption>Have fun with Sapper!</figcaption>
</figure>

<p><strong>Try editing this file (src/routes/index.svelte) to test live reloading.</strong></p>
