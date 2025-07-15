<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher<{
		submit: { message: string };
	}>();

	let message = '';

	function handleSubmit() {
		if (message.trim()) {
			dispatch('submit', { message: message.trim() });
			message = '';
		}
	}

	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Enter' && !event.shiftKey) {
			event.preventDefault();
			handleSubmit();
		}
	}
</script>

<div class="sticky right-0 bottom-0 left-0 border-t border-gray-200 bg-white p-4">
	<form on:submit|preventDefault={handleSubmit} class="flex items-end gap-2">
		<div class="relative flex-1">
			<textarea
				bind:value={message}
				on:keydown={handleKeydown}
				placeholder="Type your message..."
				rows="1"
				class="w-full resize-none rounded-lg border border-gray-300 p-3 pr-12 focus:border-purple-700 focus:ring-1 focus:ring-purple-700 focus:outline-none"
				style="min-height: 44px; max-height: 120px;"
			></textarea>
			<button
				type="submit"
				disabled={!message.trim()}
				aria-label="Send message"
				class="absolute right-2 bottom-2 rounded-full bg-purple-700 p-2 text-white transition-colors hover:bg-purple-800 disabled:cursor-not-allowed disabled:bg-gray-300"
			>
				<svg class="h-4 w-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"
					></path>
				</svg>
			</button>
		</div>
	</form>
</div>
