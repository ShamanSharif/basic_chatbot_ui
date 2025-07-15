<script lang="ts">
	import { onMount } from 'svelte';
	import Navigation from '$lib/components/Navigation.svelte';
	import Hero from '$lib/components/Hero.svelte';
	import Suggestion from '$lib/components/Suggestion.svelte';
	import BigPhatButton from '$lib/components/BigPhatButton.svelte';

	// Store chat messages
	let messages: any[] = [];

	// Current message input value
	let currentMessage: string = '';

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

	function handleSubmit() {
		if (currentMessage.trim()) {
			// Add user message to chat
			const userMessage = {
				sender: 'user',
				text: currentMessage,
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
						text: `Echo: ${currentMessage}`,
						timestamp: new Date().toISOString()
					};
					messages = [...messages, botMessage];
				}, 1000);
			}

			// Clear input
			currentMessage = '';
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

<!-- Navigation Bar -->
<Navigation
	{userSettings}
	on:toggleTheme={toggleTheme}
	on:changeFontSize={changeFontSize}
	on:toggleAbout={setShowAbout}
/>

<Hero />

<Suggestion />

<BigPhatButton />
