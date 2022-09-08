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

</script>

<!-- https://svelte.dev/tutorial/svelte-window -->
<svelte:window on:keydown={handleKeydown} on:keyup={handleKeyup}/>

<style lang="scss">
	
</style>
