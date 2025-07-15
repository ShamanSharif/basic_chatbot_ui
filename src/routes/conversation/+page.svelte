<!-- Main Content Area -->
<main class="container mx-auto max-w-lg p-4 pt-28">
	<!-- About Section Toggle -->
	{#if showAbout}
		<section class="mb-8 rounded-lg bg-slate-100 p-4 shadow-sm">
			<button on:click={setShowAbout} class="mb-2 font-medium text-slate-700 hover:text-slate-900">
				← Back
			</button>
			<h2 class="mb-4 text-2xl font-bold text-slate-800">About This Chatbot</h2>
			<p class="mb-4 text-slate-700">
				This is a Svelte-based chatbot frontend using Tailwind CSS for styling. The interface
				includes:
			</p>
			<ul class="mb-4 list-disc pl-6 text-slate-700">
				<li>Theme switching between day and night modes</li>
				<li>Font size customization options</li>
				<li>WebSocket integration to communicate with a backend server</li>
				<li>A responsive layout optimized for modern browsers</li>
			</ul>
			<p class="text-slate-700">
				All user preferences are saved in localStorage and will be preserved between browser
				sessions.
			</p>
		</section>
	{/if}

	<!-- Chat Messages Area -->
	<div class="mb-8 h-[400px] overflow-y-auto rounded-lg border bg-white p-4 shadow-sm">
		{#each messages as message, index (index)}
			<div class="mb-4 px-2">
				<div class="flex items-end">
					<!-- Message bubble -->
					<div
						class={`mb-2 w-full max-w-xs rounded-lg p-3 ${
							message.sender === 'user'
								? 'rounded-br-lg bg-blue-100 text-slate-800'
								: 'rounded-bl-lg bg-gray-100 text-slate-800'
						}`}
					>
						<div class="mb-1 font-medium">
							{#if message.sender === 'user'}You{:else}Assistant{/if}:
						</div>
						<p class="whitespace-pre-wrap">{message.text}</p>
					</div>
				</div>
			</div>
		{/each}

		<!-- Empty state if there are no messages -->
		{#if !messages.length}
			<div class="p-8 text-center text-slate-500">
				Send a message to start the conversation with our AI assistant.
			</div>
		{/if}
	</div>

	<!-- Input Area -->
	<form
		on:submit|preventDefault={handleSubmit}
		class="flex items-center overflow-hidden rounded-lg border"
	>
		<input
			type="text"
			bind:value={currentMessage}
			placeholder="Type your message..."
			required
			class="flex-grow rounded-l-lg border-r p-3 focus:outline-none"
		/>
		<button
			type="submit"
			class="rounded-r-lg bg-blue-500 p-3 text-white transition-colors hover:bg-blue-600"
		>
			Send
		</button>
	</form>
</main>
