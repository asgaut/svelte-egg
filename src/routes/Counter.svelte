<script lang="ts">
	import { spring } from 'svelte/motion';

	let omkrets = 15;
	let altitude = 100;
	let temp_egg_start = 4;
	let temp_egg_slutt = 65; /* t_Yolk */

	$: koke_temp = 100 - altitude/100 * 0.3;
	$: t = 0.0152*omkrets*omkrets*Math.log(2*(koke_temp-temp_egg_start)/(koke_temp-temp_egg_slutt));

	let pad = function(num: number, size: number) {
		var s = String(num);
		while (s.length < (size||2)) {s = "0" + s;}
		return s;
	};
</script>

<h2>
	Omkrets på eggene:
</h2>

<div class="counter">
	<button on:click={() => (omkrets -= 0.5)} aria-label="Reduser omkrets med 1 cm">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5" />
		</svg>
	</button>

	<div class="counter-viewport">
		<div class="counter-digits">
			<strong>{omkrets} cm</strong>
		</div>
	</div>

	<button on:click={() => (omkrets += 0.5)} aria-label="Øk omkretsen med 1 cm">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5 M0.5,0 L0.5,1" />
		</svg>
	</button>

</div>

<h2>
Beregnet koketid:
</h2>

<div class="counter-viewport">
	<div class="counter-digits">
		<strong>{Math.trunc(t)}:{pad(Math.round((t - Math.trunc(t))*60),2)}</strong>
	</div>
</div>

<div>
	<br />
	<label for="altitude">Høyde over havet:</label>
	<input id="altitude" type=number bind:value={altitude} min=0 max=8848 step=100>&nbsp;m
	<br />
	<label for="teggstart">Start-temperatur:</label>
	<input id="teggstart" type=number bind:value={temp_egg_start} min=0 max=30>&nbsp;°C
	<br />
	<label for='teggslutt'>Ferdig-temperatur:</label>
	<input id='teggslutt' type=number bind:value={temp_egg_slutt} min=0 max=100>&nbsp;°C
	<p>Tips: 65 grader for smilende egg</p>

</div>


<style>
	.counter {
		display: flex;
		border-top: 1px solid rgba(0, 0, 0, 0.1);
		border-bottom: 1px solid rgba(0, 0, 0, 0.1);
		margin: 1rem 0;
	}

	.counter button {
		width: 2em;
		padding: 0;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 0;
		background-color: transparent;
		touch-action: manipulation;
		font-size: 2rem;
	}

	.counter button:hover {
		background-color: var(--color-bg-1);
	}

	svg {
		width: 25%;
		height: 25%;
	}

	path {
		vector-effect: non-scaling-stroke;
		stroke-width: 2px;
		stroke: #444;
	}

	.counter-viewport {
		width: 20em;
		height: 4em;
		overflow: hidden;
		text-align: center;
		position: relative;
	}

	.counter-viewport strong {
		position: absolute;
		display: flex;
		width: 100%;
		height: 100%;
		font-weight: 400;
		color: var(--color-theme-1);
		font-size: 4rem;
		align-items: center;
		justify-content: center;
	}

	.counter-digits {
		position: absolute;
		width: 100%;
		height: 100%;
	}

	.hidden {
		top: -100%;
		user-select: none;
	}
</style>
