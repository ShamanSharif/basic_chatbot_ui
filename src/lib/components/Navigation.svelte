<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	// Props
	export let userSettings: {
		themeColor: 'default' | 'day' | 'night';
		fontSize: 'system' | 'small' | 'medium' | 'large';
	};

	// Events
	const dispatch = createEventDispatcher();

	// Local state
	let isMenuOpen = false;

	// Functions
	function toggleTheme() {
		dispatch('toggleTheme');
	}

	function changeFontSize(event: Event) {
		const target = event.target as HTMLSelectElement;
		dispatch('changeFontSize', { fontSize: target.value });
	}

	function toggleAbout() {
		dispatch('toggleAbout');
	}

	function toggleMenu() {
		isMenuOpen = !isMenuOpen;
	}

	function closeMenu() {
		isMenuOpen = false;
	}
</script>

<!-- Navigation Bar -->
<nav class="sticky top-0 z-50 flex items-center justify-between bg-orange-600 p-4 text-white">
	<div class="flex items-center space-x-4">
		<!-- Logo or app name -->
		<div class="text-xl font-bold">Chatbot</div>
	</div>

	<div class="flex items-center space-x-4">
		<!-- Hamburger Menu Button -->
		<button
			on:click={toggleMenu}
			class="flex items-center rounded-full p-2 transition-colors hover:bg-slate-700"
			aria-label="Toggle menu"
		>
			<svg
				class="h-6 w-6"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M4 6h16M4 12h16M4 18h16"
				></path>
			</svg>
		</button>

		<!-- Close Button -->
		<button
			on:click={() => dispatch('close')}
			class="ml-2 flex items-center rounded-full p-2 transition-colors hover:bg-slate-700"
			aria-label="Close"
		>
			<svg
				class="h-6 w-6"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
				xmlns="http://www.w3.org/2000/svg"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M6 18L18 6M6 6l12 12"
				></path>
			</svg>
		</button>
	</div>

	<!-- Dropdown Menu -->
	{#if isMenuOpen}
		<!-- Backdrop to close menu when clicking outside -->
		<div class="fixed inset-0 z-40" on:click={closeMenu}></div>

		<!-- Menu Content -->
		<div class="absolute top-16 right-4 z-50 w-64 rounded-lg bg-slate-700 p-4 shadow-lg">
			<!-- Theme Toggle -->
			<div class="mb-4">
				<button
					on:click={toggleTheme}
					class="flex w-full items-center justify-between rounded-md p-3 transition-colors hover:bg-slate-600"
				>
					<span class="font-medium">Theme</span>
					{#if userSettings.themeColor === 'night'}
						<div class="flex items-center">
							<span class="mr-2 text-lg">🌙</span>
							<span>Night Mode</span>
						</div>
					{:else}
						<div class="flex items-center">
							<span class="mr-2 text-lg">☀️</span>
							<span>Day Mode</span>
						</div>
					{/if}
				</button>
			</div>

			<!-- Font Size Selector -->
			<div class="mb-4">
				<label class="mb-2 block font-medium">Font Size</label>
				<select
					on:change={changeFontSize}
					bind:value={userSettings.fontSize}
					class="w-full rounded-md border-none bg-slate-600 px-3 py-2 text-white hover:bg-slate-500 focus:ring-2 focus:ring-blue-500 focus:outline-none"
				>
					<option value="system">System Font</option>
					<option value="small">Small Text</option>
					<option value="medium">Medium Text</option>
					<option value="large">Large Text</option>
				</select>
			</div>

			<!-- About Link -->
			<div class="border-t border-slate-600 pt-4">
				<button
					on:click={toggleAbout}
					class="flex w-full items-center justify-between rounded-md p-3 transition-colors hover:bg-slate-600"
				>
					<span class="font-medium">About</span>
					<span>→</span>
				</button>
			</div>
		</div>
	{/if}
</nav>
