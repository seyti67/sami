<script>
	import Sliders from "./icons/sliders.svelte";
	
	export let value = 0;
	export let min = 0;
	export let max = 1;
	let showSettings = value !== 0;
</script>

<button class="open-btn" on:click={() => showSettings = !showSettings}>
	<Sliders />
</button>
<div class="config" class:open={showSettings}>
	<div class="text">
		<h2>Set up</h2>
		<p>Configure the circle to fit the moon</p>
	</div>
	
	<input type="range" class="slider" bind:value {min} {max} step={0.01}>
	<input class="nb-input" type="number" {min} {max} bind:value>
</div>

<style>
	.open-btn {
		position: fixed;
		top: 1.5rem;
		right: .5rem;
		background: none;
		border: none;
		font-size: 2rem;
		text-align: center;
		grid-area: b;
		padding: 0 0.5rem;
		transition: inherit;
	}

	.config {
		position: absolute;
		width: 100%;
		left: 0;
		bottom: 0;
		padding: 1rem;
		background-image: linear-gradient(to top, #0004 20%, #0000);
		transition: transform 500ms;
		display: grid;
		align-items: center;
		grid-template-columns: 5fr 5rem;
		gap: 0.5rem;
		grid-template-areas: 'a a' 'b c';
	}
	.config.open {
		transform: translateY(100%);
	}
	.text {
		grid-area: a;
	}
	.text p {
		color: #eee;
		font-size: 1.2rem;
	}
	
	.open-btn :global(svg) {
		stroke: white;
		width: 2.5rem;
		height: 2.5rem;
	}
	.nb-input {
		grid-area: c;
		font-size: 1.5rem;
		width: 100%;
		border: none;
		padding: 0.5rem;
		color: white;
		background: transparent;
		outline: none;
		border-bottom: solid 3px transparent;
		transition: border-color 400ms;
	}
	.nb-input:focus {
		border-bottom: solid 3px white;
	}
	
	.slider {
		max-width: 30rem;
		grid-area: b;
		-webkit-appearance: none;
		width: 100%;
		height: 10px;
		border-radius: 5px;
		background: #d3d3d3;
		outline: none;
	}
	
	.slider::-webkit-slider-thumb {
		-webkit-appearance: none;
		appearance: none;
		width: 1.5rem;
		height: 1.5rem;
		border: 0;
		background-image: var(--accent-gradient);
		background-size: 400%;
		cursor: pointer;
	}
	
	.slider::-moz-range-thumb {
		width: 1.5rem;
		height: 1.5rem;
		border: 0;
		background-image: var(--accent-gradient);
		background-size: 400%;
		cursor: pointer;
	}
</style>