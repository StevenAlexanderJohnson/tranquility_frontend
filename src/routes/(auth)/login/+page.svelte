<script lang="ts">
	import { goto } from '$app/navigation';
	import Turnstile from '$lib/components/turnstile.svelte';
	import { authStore } from '$lib/stores/auth.svelte';
	let username = $state('');
	let password = $state('');
	let submitting = $state(false);

	$effect(() => {
		if (authStore.isAuthenticated()) {
			goto('/');
		}
	})

	function login() {
		const response = turnstile.getResponse();
		if (!response) {
			alert('Unusual activity was detected. Please try again later or refresh the page.');
			return;
		}
		authStore.login(username, password, response)
		.then(() => goto('/'))
		.catch((err: Error) => {
			alert(err.message);
			location.reload();
		});
	}
</script>

<div class="flex h-full w-full items-center justify-center">
	<form class="rounded-xl border-2 border-accent p-6" onsubmit={(e) => e.preventDefault()}>
		<div class="flex flex-col gap-4 pb-10">
			<h1 class="text-xl font-bold">Login</h1>
			<span class="md:col-span-2">Login to gain full access to Tranquility</span>
		</div>
		<div class="grid grid-cols-1 gap-5 md:grid-cols-2">
			<label for="username" class="flex flex-col">
				Username
				<input
					type="text"
					name="username"
					bind:value={username}
					class="h-12 rounded-xl border border-accent bg-background p-2"
				/>
			</label>
			<label for="username" class="flex flex-col">
				Password
				<input
					type="password"
					name="password"
					bind:value={password}
					class="h-12 rounded-xl border border-accent bg-background p-2"
				/>
			</label>

			<button
				type="submit"
				class="h-12 rounded-xl bg-primary md:col-span-2"
				onclick={() => {
					submitting = true;
					login();
				}}
			>
				{#if submitting}
					<div
						class="text-surface inline-block h-8 w-8 animate-spin rounded-full border-4 border-solid border-current border-e-transparent align-[-0.125em] motion-reduce:animate-[spin_1.5s_linear_infinite] dark:text-white"
						role="status"
					>
						<span
							class="!absolute !-m-px !h-px !w-px !overflow-hidden !whitespace-nowrap !border-0 !p-0 ![clip:rect(0,0,0,0)]"
						>
							Loading...</span
						>
					</div>
				{:else}
					Submit
				{/if}
			</button>
			<div class="md:col-span-2">
				<span>Don't have an account? </span>
				<a href="/register" class="text-center text-accent hover:underline md:col-span-2">
					Sign Up
				</a>
			</div>
		</div>
		<div class="flex justify-center">
			<Turnstile />
		</div>
	</form>
</div>
