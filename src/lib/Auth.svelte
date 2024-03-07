<script lang="ts">
	import { supabase } from '$lib/supabase';

	let loading = false;
	let email = '';
	let showModal = false;
	let modalMessage = '';

	const showModalAlert = (message: string) => {
		modalMessage = message;
		showModal = true;
	};

	const handleLogin = async () => {
		try {
			loading = true;
			const { error } = await supabase.auth.signInWithOtp({ email });
			if (error) throw error;
			showModalAlert('Check your email for login link!');
		} catch (error) {
			if (error instanceof Error) {
				showModalAlert(error.message);
			}
		} finally {
			loading = false;
		}
	};
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
	</div>
</div>

<DaisyModal bind:show={showModal}>
	<div class="p-4">
		<h1 class="text-2xl font-bold">{modalMessage}</h1>
		<button class="mt-4 button block" on:click={() => (showModal = false)}>Close</button>
	</div>
</DaisyModal>
