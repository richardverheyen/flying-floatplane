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

	let alpha = 2;
	let beta = 1;
	let gamma = 3;

	function handleOrientation(event) {
		alpha = event.alpha; // yaw
		beta = event.beta; // pitch
		gamma = event.gamma; // roll
	}

	function onClick() {
		if (typeof DeviceMotionEvent.requestPermission === 'function') {
			// Handle iOS 13+ devices.
			DeviceMotionEvent.requestPermission()
			.then((state) => {
				if (state === 'granted') {
					window.addEventListener('devicemotion', handleOrientation);
				} else {
					console.error('Request to access the orientation was rejected');
				}
			})
			.catch(console.error);
		} else {
			// Handle regular non iOS 13+ devices.
			window.addEventListener('devicemotion', handleOrientation);
		}
	}

</script>

<div id="KeyListeners">
	<button on:click={onClick}>grant</button>
	<ul>
		<li>acc x : {alpha}</li>
		<li>acc y : {beta}</li>
		<li>acc z : {gamma}</li>
	</ul>
</div>

<!-- https://svelte.dev/tutorial/svelte-window -->
<svelte:window on:keydown={handleKeydown} on:keyup={handleKeyup}/>

<style lang="scss">
	#KeyListeners {
		position: absolute;
		top: 80px;
		right: 10px;
		background: white;
		width: 140px;
	}
</style>
