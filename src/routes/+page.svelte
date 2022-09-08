<script>
	import Canvas from '$lib/Canvas.svelte';
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<Canvas />
</section>

<section id="pitch">
	<div class="gutters">
		<h2>
			We'll deliver your end to end web solution <br />
			<u>on time</u> and <u>on budget</u>. When we're done, <br />
			<u>we'll train your team</u> to maintain it.
		</h2>
	</div>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}
	
	#pitch {
		min-height: 400px;
	}
	#pitch h2 {
		margin: auto;
	}
</style>
