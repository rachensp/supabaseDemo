<script>
	import { supabase } from '$lib/supabase';

	let promise = supabase.from('countries').select();
</script>

{#await promise}
	<span class="loading loading-infinity loading-lg" />
{:then result}
	<ul>
		{#each result.data as country}
			<li>{country.name}</li>
		{/each}
	</ul>
{/await}
