<script>
	import Canvas from '$lib/Canvas.svelte';
	import Metrics from '$lib/Metrics.svelte';
</script>

<svelte:head>
	<title>Floatplane</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<Metrics />

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

<section id="team">
	<div class="gutters">
		<h2>Our Team</h2>
		<ul>
			<li>Jan</li>
			<li>Rich</li>
			<li>Shuwen</li>
			<li>Madeleine</li>
		</ul>
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

	#team div {
		display: flex;
		flex-direction: column;
		align-items: center;
	}
</style>
