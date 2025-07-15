<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	// Props
	export let suggestions = [
		'What can you help me with?',
		'Tell me about your features',
		'How do I get started?',
		'Show me some examples'
	];

	// Function with callback
	function handleSuggestionClick(suggestion: string) {
		// Callback function that dispatches an event
		dispatch('suggestionSelected', {
			suggestion,
			timestamp: new Date().toISOString()
		});

		// Additional callback logic
		console.log('Suggestion clicked:', suggestion);
	}

	// Helper function to format suggestion text
	function formatSuggestion(text: string) {
		return text.charAt(0).toUpperCase() + text.slice(1);
	}
</script>

<div class="suggestion-container">
	<div class="help-section">
		<h2 class="help-title">How can I help you?</h2>
		<p class="help-description">
			Choose from the suggestions below or ask me anything you'd like to know.
		</p>
	</div>

	<div class="suggestions-grid">
		{#each suggestions as suggestion, index}
			<button
				class="suggestion-button"
				on:click={() => handleSuggestionClick(suggestion)}
				on:keydown={(e) => e.key === 'Enter' && handleSuggestionClick(suggestion)}
			>
				<span class="suggestion-text">{formatSuggestion(suggestion)}</span>
				<svg
					class="arrow-icon"
					width="16"
					height="16"
					viewBox="0 0 24 24"
					fill="none"
					stroke="currentColor"
					stroke-width="2"
				>
					<path d="M5 12h14M12 5l7 7-7 7" />
				</svg>
			</button>
		{/each}
	</div>
</div>

<style>
	.suggestion-container {
		max-width: 600px;
		margin: 0 auto;
		padding: 2rem;
		background: white;
		border-radius: 12px;
		box-shadow:
			0 4px 6px -1px rgba(0, 0, 0, 0.1),
			0 2px 4px -1px rgba(0, 0, 0, 0.06);
	}

	.help-section {
		text-align: center;
		margin-bottom: 2rem;
	}

	.help-title {
		font-size: 1.875rem;
		font-weight: 700;
		color: #1f2937;
		margin-bottom: 0.5rem;
	}

	.help-description {
		color: #6b7280;
		font-size: 1rem;
		line-height: 1.5;
	}

	.suggestions-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		gap: 1rem;
		margin-bottom: 2rem;
	}

	.suggestion-button {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 1rem 1.5rem;
		background: #f8fafc;
		border: 2px solid #e2e8f0;
		border-radius: 8px;
		cursor: pointer;
		transition: all 0.2s ease;
		text-align: left;
		font-size: 0.875rem;
		color: #374151;
	}

	.suggestion-button:hover {
		background: #f1f5f9;
		border-color: #cbd5e1;
		transform: translateY(-1px);
	}

	.suggestion-button:focus {
		outline: none;
		border-color: #3b82f6;
		box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
	}

	.suggestion-text {
		flex: 1;
		margin-right: 0.5rem;
	}

	.arrow-icon {
		color: #9ca3af;
		transition: transform 0.2s ease;
	}

	.suggestion-button:hover .arrow-icon {
		transform: translateX(2px);
		color: #6b7280;
	}

	@media (max-width: 640px) {
		.suggestion-container {
			padding: 1.5rem;
			margin: 1rem;
		}

		.help-title {
			font-size: 1.5rem;
		}

		.suggestions-grid {
			grid-template-columns: 1fr;
		}
	}
</style>
