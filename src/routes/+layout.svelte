<script lang="ts">
	import { goto } from '$app/navigation';
	import { authStore } from '$lib/stores/auth.svelte';
	import '../app.css';
	let { children } = $props();

</script>

<div class="flex h-screen flex-col gap-2">
	<div
		class={`flex h-14 items-center justify-between gap-7 px-10 ${authStore.authState?.id && 'border-b border-primary'}`}
	>
		<button class="text-lg font-bold" onclick={() => goto('/')}>Tranquility</button>
		<div class="flex items-center justify-center gap-3">
			{#if authStore.authState?.id}
				<button
					class="flex items-center justify-center rounded-lg bg-primary px-2 py-1"
					onclick={() => authStore.logout()}>Logout</button
				>
				<span>{authStore.authState.username}</span>
			{:else}
				<button
					class="flex items-center justify-center rounded-lg bg-primary px-2 py-1"
					onclick={() => goto('/login')}>Login</button
				>
			{/if}
		</div>
	</div>
	<div class="flex flex-1 flex-col">
		{@render children()}
	</div>
</div>
