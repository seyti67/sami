<script lang="ts">
	import { AstroTime, GeoMoon, MoonPhase } from "../scripts/astronomy";
	import { tweened } from "svelte/motion";
	import cookie from "easy-cookie";
  	import Config from "./Config.svelte";

	let distanceKm = tweened(0, { duration: 1000 } );
	console.log(Number(cookie.get('sizeFactor')));
	let sizeFactor = Number(cookie.get('sizeFactor')) || 0; // smallest: 0.2, largest: 2.0 (input factor)
	$: if(sizeFactor) {
		sizeFactor = Math.min(2, Math.max(0.2, sizeFactor));
		cookie.set('sizeFactor', sizeFactor.toString())
	};
	let size = 1; // smallest : 1, biggest : 1.118 (moon size)
	let phase = 0;
	const update = () => {
		const now = new AstroTime(new Date('2022-01-01T00:00:00Z'));
		const moonVect = GeoMoon(now);

		const distance = moonVect.Length();
		distanceKm.set(149597870.7 * distance);

		size = 0.00270993161937 / distance; // max moon distance
		phase = MoonPhase(now);
	}
	update();
	setInterval(update, 10000);
</script>

<h2>🌙 distance: {Math.round($distanceKm)}km</h2>
<div class="moon-cirlce" style:--size="{15*sizeFactor*size}ch">
	<div class="moon-bg" />
</div>

<Config bind:value={sizeFactor} min={0.2} max={2} />

<style>
	.moon-cirlce {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		border-radius: 50%;
		border: solid 2px rgba(240, 183, 252, 0.664);
		width: var(--size);
		height: var(--size);
	}
	.moon-bg {
		width: 100%;
		height: 100%;
		background-image: var(--accent-gradient);
		background-size: 300%;
		border-radius: 50%;
		opacity: 0.3;
	}
</style>