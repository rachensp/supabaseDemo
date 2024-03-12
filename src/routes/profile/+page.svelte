<script lang="ts">
	import { onMount } from 'svelte';
	import type { AuthSession } from '@supabase/supabase-js';
	import { supabase } from '$lib/supabase.js';
	import { session } from '$lib/store.js';
	import Avatar from '$lib/Avatar.svelte';

	let loading = false;
	let username: string | null = null;
	let website: string | null = null;
	let avatarUrl: string | null = null;

	onMount(() => {
		getProfile();
	});

	async function getProfile() {
		try {
			loading = true;
			const { user } = $session;

			const { data, error, status } = await supabase
				.from('profiles')
				.select('username, website, avatar_url')
				.eq('id', user.id)
				.single();

			if (error && status !== 406) throw error;

			if (data) {
				username = data.username;
				website = data.website;
				avatarUrl = data.avatar_url;
			}
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message);
			}
		} finally {
			loading = false;
		}
	}

	async function updateProfile() {
		try {
			loading = true;
			const { user } = $session;

			const updates = {
				id: user.id,
				username,
				website,
				avatar_url: avatarUrl,
				updated_at: new Date().toISOString()
			};

			const { error } = await supabase.from('profiles').upsert(updates);

			if (error) {
				throw error;
			}
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message);
			}
		} finally {
			loading = false;
		}
	}
</script>

<div class="flex justify-center items-center h-screen">
	<div class="w-96">
		<div class="card bg-neutral text-neutral-content">
			<div class="card-body">
				{#if $session}
					<form on:submit|preventDefault={updateProfile} class="form-widget space-y-4">
						<div class="flex justify-center">
							<Avatar size={100} bind:url={avatarUrl} on:upload={updateProfile} />
						</div>
						<div>Email: {$session.user.email}</div>
						<div>
							<label for="username">Name</label>
							<input id="username" type="text" class="input" bind:value={username} />
						</div>
						<div>
							<label for="website">Website</label>
							<input id="website" type="text" class="input" bind:value={website} />
						</div>
						<div class="flex justify-center">
							<button type="submit" class="btn btn-primary" disabled={loading}>
								{loading ? 'Saving ...' : 'Update profile'}
							</button>
						</div>
						<div class="flex justify-center">
							<button type="button" class="btn" on:click={() => supabase.auth.signOut()}>
								Sign Out
							</button>
						</div>
					</form>
				{:else}
					<div class="text-center">Seems you are not logged in</div>
				{/if}
			</div>
		</div>
	</div>
</div>
