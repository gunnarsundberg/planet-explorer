<script lang="ts">
	import { Navbar, NavBrand, DarkMode } from 'flowbite-svelte';
	import darklogo from '$lib/assets/empire-logo.png';
	import lightlogo from '$lib/assets/rebel-logo.png';
</script>

<Navbar let:hidden>
	<NavBrand href="/">
		<img src={lightlogo} class="block dark:hidden mr-3 h-6 sm:h-9" alt="Rebel Logo" />
		<img src={darklogo} class="hidden dark:block mr-3 h-6 sm:h-9" alt="Empire Logo" />
		<span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">
			Planet Explorer
		</span>
	</NavBrand>
	<DarkMode class="ml-auto" />
</Navbar>
