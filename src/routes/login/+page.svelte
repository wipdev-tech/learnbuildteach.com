<script lang="ts">
	import { goto } from '$app/navigation';
	import Alert from '$components/alert.svelte';
	import { supabase } from '$lib/helpers/supabase';
	import { alertMessage, updateAlert } from '$stores/alertStore';
	import Form from '$components/layout/form.svelte';
	import { onDestroy } from 'svelte';

	let email = '';
	let password = '';

	onDestroy(() => {
		updateAlert('');
	});

	const handleLogIn = async () => {
		if (!email || !password) {
			return updateAlert('Email and password are required.');
		}
		const { error } = await supabase.auth.signInWithPassword({
			email,
			password
		});
		if (error) {
			console.error(error);
			updateAlert('Failed to login');
		} else {
			goto('/admin');
		}
	};

	const handleSendMagicLink = async () => {
		if (!email) {
			return updateAlert('Email is required for magic links');
		}

		const { error } = await supabase.auth.signInWithOtp({ email });
		if (error) {
			console.error(error);
			updateAlert('Failed to send magic link');
		} else {
			updateAlert('Please check your email');
		}
	};
</script>

<h1>Log in</h1>
<Form on:submit={handleLogIn}>
	<div>
		<label for="email">Email</label>
		<input type="email" name="email" id="email" bind:value={email} />
	</div>

	<div>
		<label for="password">Password</label>
		<input type="password" name="password" id="password" bind:value={password} />
	</div>

	<button>Log in</button>
	<button type="button" on:click={handleSendMagicLink}><small>Send magic link</small></button>
	<small>Don't have an account yet? <a href="/signup">Sign up</a></small>

	<Alert />
</Form>
