<script lang="ts">
	import { goto } from '$app/navigation';

	function handleClick() {
		goto('/conversation');
	}
</script>

<button
	class="mx-auto my-4 flex w-[calc(100%-2rem)] max-w-[600px] cursor-pointer items-center justify-between rounded-xl border-none bg-purple-800 p-4 shadow-md transition-all duration-200 hover:-translate-y-0.5 hover:bg-purple-900 hover:shadow-lg active:translate-y-0 disabled:transform-none disabled:cursor-not-allowed disabled:bg-gray-300 sm:p-3"
	aria-label="Start conversation with Jcena AI"
	on:click={handleClick}
>
	<div class="flex-1 text-left">
		<h3 class="mb-1 text-lg font-semibold text-white sm:text-base">Talk with Jcena AI</h3>
		<p class="text-sm leading-relaxed text-white opacity-90 sm:text-xs">
			You cannot see Jcena AI, but it will respond immediately.
		</p>
	</div>

	<div
		class="ml-4 flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-white sm:h-11 sm:w-11"
	>
		<svg
			class="h-5 w-5 text-purple-800 sm:h-4.5 sm:w-4.5"
			width="24"
			height="24"
			viewBox="0 0 24 24"
			fill="none"
			xmlns="http://www.w3.org/2000/svg"
		>
			<path
				d="M20 2H4C2.9 2 2 2.9 2 4V22L6 18H20C21.1 18 22 17.1 22 16V4C22 2.9 21.1 2 20 2ZM20 16H6L4 18V4H20V16Z"
				fill="currentColor"
			/>
		</svg>
	</div>
</button>
