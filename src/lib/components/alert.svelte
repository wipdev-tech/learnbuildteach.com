<script lang="ts">
	import { alertMessage } from '$stores/alertStore';
	import { fly } from 'svelte/transition';
</script>

<div class="alert-container">
	{#if $alertMessage}
		<div class="alert" in:fly={{ x: -16, duration: 150 }}>
			<p>{$alertMessage}</p>
		</div>
	{/if}
</div>

<style>
	.alert {
		background: var(--primary-dark);
		color: var(--text);
		font-size: clamp(var(--text-base), 1rem + 1vw, var(--text-text-lg));
		border-radius: 4px;
		padding: 8px 20px;
		min-height: 30px;
		font-size: 18px;
	}
	.alert-container {
		height: 46px;
		margin-bottom: 20px;
	}
</style>
