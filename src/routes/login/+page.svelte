<script>
	import Auth from '$lib/Auth.svelte';
</script>

<div class="flex justify-center items-center min-h-screen">
	<div class="w-full md:w-3/4 lg:w-1/2 xl:w-1/3 p-8 bg-white rounded-xl shadow-xl text-black auth">
		<Auth />
	</div>
</div>

<style>
	.text-black {
		--tw-text-opacity: 1;
		color: rgba(0, 0, 0, var(--tw-text-opacity));
	}

	/* Style für das E-Mail-Input-Feld innerhalb von <Auth /> */
	:global(.auth input[type='email']) {
		background-color: white !important; /* Hintergrundfarbe auf Weiß setzen */
	}
</style>
