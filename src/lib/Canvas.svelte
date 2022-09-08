<script>
// @ts-nocheck

	import { keysHeld } from './stores.js';
	import { onMount } from 'svelte';

	// https://blog.maximeheckel.com/posts/vaporwave-3d-scene-with-threejs/
	import * as THREE from "three";
	import { RenderPass } from "three/examples/jsm/postprocessing/RenderPass.js";
	import { EffectComposer } from "three/examples/jsm/postprocessing/EffectComposer.js";
	import { GammaCorrectionShader } from "three/examples/jsm/shaders/GammaCorrectionShader.js";
	import { ShaderPass } from "three/examples/jsm/postprocessing/ShaderPass.js";
	import { RGBShiftShader } from "three/examples/jsm/shaders/RGBShiftShader.js";
	import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';

	onMount(async () => {
	
		const TEXTURE_PATH = "https://res.cloudinary.com/dg5nsedzw/image/upload/v1641657168/blog/vaporwave-threejs-textures/grid.png";
		const DISPLACEMENT_PATH = "https://res.cloudinary.com/dg5nsedzw/image/upload/v1641657200/blog/vaporwave-threejs-textures/displacement.png";
		const METALNESS_PATH = "https://res.cloudinary.com/dg5nsedzw/image/upload/v1641657200/blog/vaporwave-threejs-textures/metalness.png";

		// Textures
		const textureLoader = new THREE.TextureLoader();
		const gridTexture = textureLoader.load(TEXTURE_PATH);
		const terrainTexture = textureLoader.load(DISPLACEMENT_PATH);
		const metalnessTexture = textureLoader.load(METALNESS_PATH);

		const canvas = document.getElementById("hero");

		// Scene
		const scene = new THREE.Scene();

		// Fog
		const fog = new THREE.Fog("#000000", 1, 2.5);
		scene.fog = fog;

		// Objects
		const geometry = new THREE.PlaneGeometry(1, 2, 24, 24);
		const material = new THREE.MeshStandardMaterial({
			map: gridTexture,
			displacementMap: terrainTexture,
			displacementScale: 0.4,
			/**
			 * Add a metalnessMap to our material that will tell the renderer
			 * where the "rough" parts of our terrains are
			 */ 
			metalnessMap: metalnessTexture,
			/**
			 * Make the terrain very very metallic so it will reflect the light
			 * and not diffuse it: it will stay black
			 */ 
			metalness: 0.96,
			/**
			 * Make the terrain a bit rough so the rough parts will diffuse the light
			 * well
			 */ 
			roughness: 0.5,
		});

		const plane = new THREE.Mesh(geometry, material);
		plane.rotation.x = -Math.PI * 0.5;
		plane.position.y = 0.0;
		plane.position.z = 0.15;


		const plane2 = new THREE.Mesh(geometry, material);
		plane2.rotation.x = -Math.PI * 0.5;
		plane2.position.y = 0.0;
		plane2.position.z = -1.85; // 0.15 - 2 (the length of the first plane)

		scene.add(plane);
		scene.add(plane2);

		// Light
		// Ambient Light
		const ambientLight = new THREE.AmbientLight("#ffffff", 10);
		scene.add(ambientLight);

		// Right Spotlight aiming to the left
		const spotlight = new THREE.SpotLight("#d53c3d", 20, 25, Math.PI * 0.1, 0.25);
		spotlight.position.set(0.5, 0.75, 2.2);
		// Target the spotlight to a specific point to the left of the scene
		spotlight.target.position.x = -0.25;
		spotlight.target.position.y = 0.25;
		spotlight.target.position.z = 0.25;
		scene.add(spotlight);
		scene.add(spotlight.target);

		// Left Spotlight aiming to the right
		const spotlight2 = new THREE.SpotLight("#d53c3d", 20, 25, Math.PI * 0.1, 0.25);
		spotlight2.position.set(-0.5, 0.75, 2.2);
		// Target the spotlight to a specific point to the right side of the scene
		spotlight2.target.position.x = 0.25;
		spotlight2.target.position.y = 0.25;
		spotlight2.target.position.z = 0.25;
		scene.add(spotlight2);
		scene.add(spotlight2.target);


		// Sizes
		const sizes = {
			width: window.innerWidth,
			height: window.innerHeight,
		};

		// Camera
		const camera = new THREE.PerspectiveCamera(
			75,
			sizes.width / sizes.height,
			0.01,
			20
		);
		camera.position.x = 0;
		camera.position.y = 0.06;
		camera.position.z = 1.1;

		// Controls
		// const controls = new OrbitControls(camera, canvas);
		// controls.enableDamping = true;

		const loader = new GLTFLoader();
		let floatplane;
		loader.load( './model/scene.gltf', ( gltf ) => {

			floatplane = gltf.scene;
			floatplane.scale.set(0.004, 0.004, 0.004);
			floatplane.position.y = 0.2;
			floatplane.rotation.x = Math.PI * -0.1;
			floatplane.rotation.y = Math.PI * 0.65;
			
			floatplane.velocity = new THREE.Vector3();
			floatplane.acceleration = new THREE.Vector3();
			scene.add( gltf.scene );

		}, undefined, function ( error ) {
			console.error( {error} );
		} );

		// Renderer
		const renderer = new THREE.WebGLRenderer({
			canvas: canvas,
		});
		renderer.setSize(sizes.width, sizes.height);
		renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

		// Post Processing
		const effectComposer = new EffectComposer(renderer);
		effectComposer.setSize(sizes.width, sizes.height);
		effectComposer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

		const renderPass = new RenderPass(scene, camera);
		effectComposer.addPass(renderPass);

		const rgbShiftPass = new ShaderPass(RGBShiftShader);
		rgbShiftPass.uniforms["amount"].value = 0.0015;

		effectComposer.addPass(rgbShiftPass);

		const gammaCorrectionPass = new ShaderPass(GammaCorrectionShader);
		effectComposer.addPass(gammaCorrectionPass);

		// Event listener to handle screen resize
		window.addEventListener("resize", () => {
			// Update sizes
			sizes.width = window.innerWidth;
			sizes.height = window.innerHeight;

			// Update camera
			camera.aspect = sizes.width / sizes.height;
			camera.updateProjectionMatrix();

			// Update renderer
			renderer.setSize(sizes.width, sizes.height);
			renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

			// Update effect composer
			effectComposer.setSize(sizes.width, sizes.height);
			effectComposer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
		});

		const clock = new THREE.Clock();

		// Animate
		const tick = () => {
			const elapsedTime = clock.getElapsedTime();

			plane.position.z = -(elapsedTime * 0.15) % 2; // plane flies towards the camera
			plane2.position.z = -((elapsedTime * 0.15) % 2) + 2;

			flyFloatplane(floatplane, $keysHeld);
			
			// Render
			// renderer.render(scene, camera);
			effectComposer.render();

			// Call tick again on the next frame
			window.requestAnimationFrame(tick);
		};

		tick();
	});

	function flyFloatplane(obj, keysArr) {
		if (keysArr.includes("ArrowUp")) {
			obj.position.y += 0.01;
		}
		if (keysArr.includes("ArrowDown")) {
			obj.position.y -= 0.01;
		}
		if (keysArr.includes("ArrowRight")) {
			obj.position.x += 0.01;
		}
		if (keysArr.includes("ArrowLeft")) {
			obj.position.x -= 0.01;
		}
	}
</script>

<canvas id="hero"></canvas>

<style>
	.canvas {
		width: 100vw;
		min-height: 100vh;
	}
</style>
