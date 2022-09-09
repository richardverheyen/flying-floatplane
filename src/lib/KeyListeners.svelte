<script>
// @ts-nocheck
	import { keysHeld } from './stores.js';

	const targetedKeys = [
		"ArrowDown",
		"ArrowUp",
		"ArrowLeft",
		"ArrowRight"
	]

	// add the held key to the array of keysHeld
	function handleKeydown(e) { 
		if (targetedKeys.includes(e.code)) {
			e.preventDefault();

			if (!$keysHeld.includes(e.code)) {
				keysHeld.update(arr => [...arr, e.code]);
			}
		}
		
	}
	// remove the held key from the array of keysHeld
	function handleKeyup(e) { 
		if (targetedKeys.includes(e.code)) {
			e.preventDefault();
			keysHeld.update(arr => arr.filter(held => held !== e.code));
		}
	}

	// // https://trekhleb.dev/blog/2021/gyro-web/
	function handleOrientation(event) {
		if (event.gamma > 10) {
			keysHeld.update(arr => [...arr, "ArrowRight"]);
		} else if (event.gamma < -10) {
			keysHeld.update(arr => [...arr, "ArrowLeft"]);
		} 

		if (event.beta > 10) {
			keysHeld.update(arr => [...arr, "ArrowDown"]);
		} else if (event.beta < -10) {
			keysHeld.update(arr => [...arr, "ArrowUp"]);
		} 
		
		if (event.gamma > -10 && event.gamma < 10) {
			keysHeld.update(arr => arr.filter(held => held !== "ArrowRight" || held !== "ArrowLeft"));
		}
		if (event.beta > -10 && event.beta < 10) {
			keysHeld.update(arr => arr.filter(held => held !== "ArrowDown" || held !== "ArrowUp"));
		}
	}

	function onClick() {
		if (typeof DeviceMotionEvent.requestPermission === 'function') {
			// Handle iOS 13+ devices.
			DeviceMotionEvent.requestPermission()
			.then((state) => {
				if (state === 'granted') {
					window.addEventListener('deviceorientation', handleOrientation);
				} else {
					console.error('Request to access the orientation was rejected');
				}
			})
			.catch(console.error);
		} else {
			// Handle regular non iOS 13+ devices.
			window.addEventListener('deviceorientation', handleOrientation);
		}
	}

</script>

<!-- https://svelte.dev/tutorial/svelte-window -->
<svelte:window on:keydown={handleKeydown} on:keyup={handleKeyup} on:deviceorientation={handleOrientation} />

<button>watch orientation</button>
<style lang="scss">
	button {
		position: absolute;
		margin-top: 80px;
		margin-right: auto;
	}
</style>
