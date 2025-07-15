<script lang="ts">
	import { onMount } from 'svelte';
	import Navigation from '$lib/components/Navigation.svelte';
	import TextInput from '$lib/components/TextInput.svelte';

	// Store chat messages
	let messages: any[] = [];

	// Show/hide about section
	let showAbout: boolean = false;

	// UI preferences stored in localStorage for persistence
	type ThemeSettings = {
		themeColor: 'default' | 'day' | 'night';
		fontSize: 'system' | 'small' | 'medium' | 'large';
	};

	let userSettings: ThemeSettings = {
		themeColor: 'default',
		fontSize: 'system'
	};

	// Connect to the backend via WebSocket
	let socket: WebSocket | null = null;

	async function connectWebSocket() {
		try {
			const endpoint = 'wss://your-chatbot-endpoint.com'; // Replace with your actual WebSocket endpoint
			socket = new WebSocket(endpoint);

			console.log('Connected to chat backend');

			// Handle incoming messages
			socket.onmessage = (event) => {
				const data = JSON.parse(event.data);
				messages = [...messages, data]; // Trigger reactivity
			};

			// Handle connection errors
			socket.onerror = (error) => {
				console.error('WebSocket error:', error);
			};

			// Handle connection close
			socket.onclose = () => {
				console.log('WebSocket connection closed');
			};
		} catch (error) {
			console.error('Failed to connect to WebSocket:', error);
		}
	}

	// Update UI preferences from localStorage when component mounts
	function loadSettings() {
		try {
			const savedSettings = localStorage.getItem('chatbot-settings');
			if (savedSettings) {
				userSettings = JSON.parse(savedSettings);
			}
		} catch (error) {
			console.error('Error loading settings:', error);
		}
	}

	// Save current UI preferences to localStorage
	function saveSettings() {
		try {
			localStorage.setItem('chatbot-settings', JSON.stringify(userSettings));
		} catch (error) {
			console.error('Error saving settings:', error);
		}
	}

	// Toggle between day/night themes
	function toggleTheme() {
		if (userSettings.themeColor === 'default') {
			userSettings.themeColor = 'night';
		} else if (userSettings.themeColor === 'night') {
			userSettings.themeColor = 'default';
		} else {
			userSettings.themeColor = 'night'; // If day mode, switch to night
		}
		saveSettings();
	}

	function changeFontSize(event: CustomEvent) {
		userSettings.fontSize = event.detail.fontSize as ThemeSettings['fontSize'];
		saveSettings();
	}

	function setShowAbout() {
		showAbout = !showAbout;
	}

	function handleMessageSubmit(event: CustomEvent) {
		const { message } = event.detail;

		// Add user message to chat
		const userMessage = {
			sender: 'user',
			text: message,
			timestamp: new Date().toISOString()
		};
		messages = [...messages, userMessage];

		// Send message via WebSocket if connected
		if (socket && socket.readyState === WebSocket.OPEN) {
			socket.send(JSON.stringify(userMessage));
		} else {
			// Mock response for demo purposes
			setTimeout(() => {
				const botMessage = {
					sender: 'bot',
					text: `Echo: ${message}`,
					timestamp: new Date().toISOString()
				};
				messages = [...messages, botMessage];
			}, 1000);
		}
	}

	onMount(() => {
		loadSettings();
		connectWebSocket();

		// Cleanup on unmount
		return () => {
			if (socket) {
				socket.close();
			}
		};
	});
</script>

<Navigation />

<!-- Main Content Area -->
<div class="flex h-screen flex-col">
	<!-- About Section Toggle -->
	{#if showAbout}
		<section class="mx-4 mt-20 mb-8 rounded-lg bg-slate-100 p-4 shadow-sm">
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
	<div class="flex-1 overflow-y-auto px-4 pt-20 pb-4">
		{#each messages as message, index (index)}
			<div class="mb-4 flex {message.sender === 'user' ? 'justify-end' : 'justify-start'}">
				<div class="max-w-xs lg:max-w-md">
					<div
						class={`rounded-2xl px-4 py-3 ${
							message.sender === 'user'
								? 'rounded-br-md bg-purple-700 text-white'
								: 'rounded-bl-md bg-gray-100 text-gray-800'
						}`}
					>
						<p class="text-sm whitespace-pre-wrap">{message.text}</p>
						<div class="mt-1 text-xs opacity-70">
							{new Date(message.timestamp).toLocaleTimeString([], {
								hour: '2-digit',
								minute: '2-digit'
							})}
						</div>
					</div>
				</div>
			</div>
		{/each}

		<!-- Empty state if there are no messages -->
		{#if !messages.length}
			<div class="flex h-full items-center justify-center">
				<div class="text-center text-gray-500">
					<div class="mb-4">
						<svg
							class="mx-auto h-16 w-16 text-gray-300"
							fill="none"
							stroke="currentColor"
							viewBox="0 0 24 24"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="1"
								d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"
							></path>
						</svg>
					</div>
					<h3 class="mb-2 text-lg font-medium">Start a conversation</h3>
					<p class="text-sm">Send a message to begin chatting with Jcena AI assistant.</p>
				</div>
			</div>
		{/if}
	</div>

	<!-- Input Area - Stuck to bottom -->
	<TextInput on:submit={handleMessageSubmit} />
</div>
