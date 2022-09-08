<script>
// @ts-nocheck
	import { keysHeld } from './stores.js';

	function handleKeydown(e) { // add the held key to the array of keysHeld
		e.preventDefault();

		if (!$keysHeld.includes(e.code)) {
			keysHeld.update(arr => [...arr, e.code]);
		}
		console.log($keysHeld);
	}

	function handleKeyup(e) { // remove the held key from the array of keysHeld
		e.preventDefault();

		keysHeld.update(arr => arr.filter(held => held !== e.code));
		console.log($keysHeld);
	}

</script>

<table id="metrics">
	<tr>
		<td>x:</td>
		<td id="x"></td>
	</tr>
	<tr>
		<td>y:</td>
		<td id="y"></td>
	</tr>
	<tr>
		<td>dx:</td>
		<td id="dx"></td>
	</tr>
	<tr>
		<td>dy:</td>
		<td id="dy"></td>
	</tr>
	<tr>
		<td>ddx:</td>
		<td id="ddx"></td>
	</tr>
	<tr>
		<td>ddy:</td>
		<td id="ddy"></td>
	</tr>
</table>

<!-- https://svelte.dev/tutorial/svelte-window -->
<svelte:window on:keydown={handleKeydown} on:keyup={handleKeyup}/>

<style lang="scss">
	#metrics {
		position: absolute;
		top: 100px;
		right: 20px;
		width: 200px;
		height: 200px;
		background: #efefef;
	}
</style>
