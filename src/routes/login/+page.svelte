<script>
	import Auth from '$lib/Auth.svelte';
</script>

<div class="flex justify-center items-center min-h-screen">
	<div
		class="w-full md:max-w-md p-8 bg-white rounded-lg shadow-md top-quarter flex justify-center items-center"
	>
		<Auth />
	</div>
</div>

<style>
	.top-quarter {
		margin-top: 12.5vh;
	}
</style>
