<script>
	import { goto } from '$app/navigation';
	import supabase from '$lib/db';
	import { user } from '$lib/test';
	let email = '';
	let isNewRegistration = false;
	const signUp = async () => {
		let { user: userDetails, error } = await supabase.auth.signUp({
			email: email,
			password: 'VRBBcygxwRculfTvPjat'
		});
		$user = userDetails;
		goto('/');
	};
	const logIn = async () => {
		let { user: userDetails, error } = await supabase.auth.signIn({
			email: email,
			password: 'VRBBcygxwRculfTvPjat'
		});
		$user = userDetails;
		goto('/');
	};
	const logOut = async () => {
		let { error } = await supabase.auth.signOut();
		$user = false;
		goto('/login');
	};
</script>

<label for="">
	Email:
	<input bind:value={email} type="email" placeholder="email@email.com" />
</label>
<br />
<br />
{#if isNewRegistration}
	<button on:click={signUp}>SignUp</button>
	<p class="switch" on:click={() => (isNewRegistration = false)}>Already have an account?</p>
{:else}
	<button on:click={logIn}>Login</button>
	<p class="switch" on:click={() => (isNewRegistration = true)}>Create a new account?</p>
{/if}
{#if $user}
	<button on:click={logOut}>Logout</button>
{/if}
