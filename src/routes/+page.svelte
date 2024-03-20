<script>
	import { supabase } from '$lib/supabase';

	let name = '';
	let errorMessage = '';

	async function sendName() {
		if (name && name[0] !== 'J') {
			errorMessage = 'Hier haben nur Hasen mit Anfangsbuchstaben J Zutritt!🐰🫷🏻';
			setTimeout(() => {
				errorMessage = '';
			}, 7000);
			return;
		}

		const response = await fetch('/api/sendName', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({ name })
		});

		if (!response.ok) {
			errorMessage = 'Failed to add rabbit.';
			setTimeout(() => {
				errorMessage = '';
			}, 7000);
			return;
		}

		name = '';
		// Refresh the list of rabbit names after successful addition
		names = supabase.from('rabbits').select();
	}

	let promise = supabase.from('countries').select();
	let names = supabase.from('rabbits').select();
</script>

<div class="prose">
	<h1>APIs</h1>
	<ul>
		<li><a href="/api/hello?name=Peter">Link zur API mit Query-Parameter</a></li>
		<li><a href="/api/hello/Peter">Link zur API mit Route-Paramter </a></li>
	</ul>

	<form>
		<h2>All my rabbits</h2>
		<input type="text" bind:value={name} /> <br />
		<button class="btn" on:click={sendName}>Add rabbit!</button>
	</form>

	{#if errorMessage}
		<div role="alert" class="alert alert-error my-8">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				class="stroke-current shrink-0 h-6 w-6"
				fill="none"
				viewBox="0 0 24 24"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z"
				/>
			</svg>
			<span class="errorMessaging">{errorMessage}</span>
		</div>
	{/if}

	<h2>Rabbit names</h2>

	{#await names}
		<span class="loading loading-infinity loading-lg" />
	{:then result}
		<ul>
			{#each result.data as rabbit (rabbit.id)}
				<li>{rabbit.name}</li>
			{/each}
		</ul>
	{/await}
</div>
