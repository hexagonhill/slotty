<script>
	import { onMount } from 'svelte';
	import * as THREE from 'three';
	import { OrbitControls } from './examples/jsm/controls/OrbitControls.js';
	import { KMZLoader } from 'three/examples/jsm/loaders/KMZLoader.js';
	import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';


	onMount(() => {
var camera, scene, renderer;
			init();

		function createModel() {
			//create SkyBox
			var reflectionCube = new THREE.CubeTextureLoader()
        .load( [ 'Background.png', 'Background.png','Background.png','Background.png','Background.png','Background.png' ] );
			reflectionCube.format = THREE.RGBFormat;
			scene.background = reflectionCube;
			scene.environment = reflectionCube;
	}


			function init() {

				scene = new THREE.Scene();
				scene.background = new THREE.Color( 0x999999 );

				var light = new THREE.DirectionalLight( 0xffffff );
				light.position.set( 0.5, 1.0, 0.5 ).normalize();

				scene.add( light );

				camera = new THREE.PerspectiveCamera( 35, window.innerWidth / window.innerHeight, 1, 500 );

				camera.position.y = 5;
				camera.position.z = 10;

				scene.add( camera );

				var grid = new THREE.GridHelper( 50, 50, 0xffffff, 0x555555 );
				scene.add( grid );

				renderer = new THREE.WebGLRenderer( { antialias: true } );
				renderer.setPixelRatio( window.devicePixelRatio );
				renderer.setSize( window.innerWidth, window.innerHeight );
				document.querySelector('main').appendChild( renderer.domElement );

				var loader = new GLTFLoader();
				loader.load( './JACKPOT_FINAL.glb', function ( kmz ) {
					createModel();
					kmz.scene.position.y = 0.5;
					scene.add( kmz.scene );
					render();

				} );

				var controls = new OrbitControls( camera, renderer.domElement );
				controls.addEventListener( 'change', render );
				controls.update();

				window.addEventListener( 'resize', onWindowResize, false );

			}

			function onWindowResize() {

				camera.aspect = window.innerWidth / window.innerHeight;
				camera.updateProjectionMatrix();

				renderer.setSize( window.innerWidth, window.innerHeight );

				render();

			}

			function render() {

				renderer.render( scene, camera );

			}


	});


</script>


<main></main>

