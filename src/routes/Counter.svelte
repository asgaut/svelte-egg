<script lang="ts">
	import { spring } from 'svelte/motion';

	let omkrets = 15;
	let temp_egg_start = 4;
	let temp_egg_slutt = 65; /* tYolk */
	let temp_vann = 100; // koketemperatur på vannnet

	$: t = 0.0152*omkrets*omkrets*Math.log(2*(temp_vann-temp_egg_start)/(temp_vann-temp_egg_slutt));

	function modulo(n: number, m: number) {
		// handle negative numbers
		return ((n % m) + m) % m;
	}
</script>

<div class="counter">
	<button on:click={() => (omkrets -= 1)} aria-label="Reduser omkrets med 1 cm">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5" />
		</svg>
	</button>

	<div class="counter-viewport">
		<div class="counter-digits">
			<strong>{omkrets} cm</strong>
		</div>
	</div>


	<button on:click={() => (omkrets += 1)} aria-label="Øk omkretsen med 1 cm">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5 M0.5,0 L0.5,1" />
		</svg>
	</button>


</div>

<p>
Koketid:
</p>

<div class="counter-viewport">
	<div class="counter-digits">
		<strong>{Math.trunc(t)} m {Math.round((t - Math.trunc(t))*60)} s </strong>
	</div>
</div>

<div>
	<br />
	<label for="teggstart">Start temperatur (grader C):</label>
	<input id="teggstart" type=number bind:value={temp_egg_start} min=0 max=30>
	<br />
	<label for='teggslutt'>Ferdig-temperatur (grader C):</label>
	<input id='teggslutt' type=number bind:value={temp_egg_slutt} min=0 max=100>
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
