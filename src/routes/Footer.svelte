<script>
	import { Footer, FooterCopyright, FooterBrand } from 'flowbite-svelte';
	import logo from '$lib/assets/deathstar-logo.png';
</script>

<Footer footerType="logo">
	<div class="sm:flex sm:items-center sm:justify-between">
		<FooterBrand href="/" src={logo} alt="Rebel Logo" name="Planet Explorer" />
	</div>
	<hr class="my-6 border-gray-200 sm:mx-auto dark:border-gray-700 lg:my-8" />
	<FooterCopyright href="/" by="Gunnar Sundberg" />
</Footer>
