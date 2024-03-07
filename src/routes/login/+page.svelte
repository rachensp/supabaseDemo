<script>
	import Auth from '$lib/Auth.svelte';
</script>

<div class="flex justify-center items-center min-h-screen bg-gray-100">
	<div class="w-full md:max-w-md p-8 bg-white rounded-lg shadow-md top-third text-black">
		<Auth />
	</div>
</div>

<style>
	.top-third {
		margin-top: calc(
			33vh - 20px
		); /* Berechnet 1/3 der Bildschirmhöhe abzüglich des oberen Abstands der Karte */
	}
</style>
