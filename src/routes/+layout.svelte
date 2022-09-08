<script>
	import Header from '$lib/header/Header.svelte';
	import '../app.css';
</script>

<Header />

<main>
	<slot />
</main>

<footer>
	<div class="gutters">
		<button>Reach out</button>
	</div>
</footer>

<style>
	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		width: 100%;
		margin: 0 auto;
		box-sizing: border-box;
	}

	footer div {
		display: flex;
		flex-direction: column;
		align-items: center;
		height: 50px;
	}

	footer div button {
		margin: auto;
	}
</style>
