<script>

	import { onMount } from "svelte"
	import { fade, fly } from "svelte/transition"
	let anime = "";
	let character = "";
	let quote = "";

	let loading = false;

	async function  getQuote() {
		loading = true
		const res = await fetch('https://animechan.vercel.app/api/random')
		console.log('res', res);
		const data = await res.json();
		console.log('data', data);

		anime = data.anime;
		character = data.character;
		quote = data.quote

		loading = false
	}

	onMount(async () => {
		getQuote()
	})
</script>

<main>
  <div>
	  <button on:click={getQuote}>New Quote</button>
	  {#if loading}
	  <!-- <h1>loading...</h1> -->
	  {:else}
<div in:fly="{{ y: 100, duration: 1000 }}" out:fade >
		  <h1>{quote}</h1>
		  <h2>- {character}</h2>
		  <h3>from { anime} </h3>
</div>
	  {/if}
  </div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
