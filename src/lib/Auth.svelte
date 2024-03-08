<script lang="ts">
	import { supabase } from '$lib/supabase';
	import { onMount } from 'svelte';

	let loading = false;
	let email = '';
	let successMessage = false;

	const handleLogin = async () => {
		try {
			loading = true;
			const { error } = await supabase.auth.signInWithOtp({ email });
			if (error) throw error;
			successMessage = true;
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message);
			}
		} finally {
			loading = false;
		}
	};

	onMount(() => {
		const timer = setTimeout(() => {
			successMessage = false;
		}, 5000); // Hide success message after 5 seconds
		return () => clearTimeout(timer);
	});
</script>

<div class="row flex-center flex">
	<div class="col-6 form-widget" aria-live="polite">
		<h1 class="header">Supabase + Svelte</h1>
		<p class="description">Sign in via magic link with your email below</p>
		<form class="form-widget" on:submit|preventDefault={handleLogin}>
			<div>
				<label for="email">Email</label>
				<input
					id="email"
					class="inputField"
					type="email"
					placeholder="Your email"
					bind:value={email}
				/>
			</div>
			<div>
				<button type="submit" class="button block" aria-live="polite" disabled={loading}>
					<span>{loading ? 'Loading' : 'Send magic link'}</span>
				</button>
			</div>
		</form>
		{#if successMessage}
			<div role="alert" class="alert alert-success">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					class="stroke-current shrink-0 h-6 w-6"
					fill="none"
					viewBox="0 0 24 24"
					><path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
					/></svg
				>
				<span>An email has been sent to you.</span>
			</div>
		{/if}
	</div>
</div>
