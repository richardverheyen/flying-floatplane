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

	// let alpha = 2;
	// let beta = 1;
	// let gamma = 3;

	// // https://trekhleb.dev/blog/2021/gyro-web/
	// function handleOrientation(event) {
	// 	window.alert(JSON.parse(event));
	// 	alpha = event.alpha; // yaw
	// 	beta = event.beta; // pitch
	// 	gamma = event.gamma; // roll
	// }

</script>

<!-- https://svelte.dev/tutorial/svelte-window -->
<svelte:window on:keydown={handleKeydown} on:keyup={handleKeyup} />

<style lang="scss">
</style>
