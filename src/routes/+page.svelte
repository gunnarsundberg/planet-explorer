<script lang="ts">
	import { onMount } from 'svelte';
	//import { ListPlaceholder } from 'flowbite-svelte'
	import PlanetTable from './PlanetTable.svelte';

	// planet data
	let planets: Planet[] = [];
	// boolean representing whether all data has been loaded
    // used to show a loading placeholder
	let planetsLoaded = false;

    // get planet data from API across all pages
	async function getPlanets() {
		let next: string;

		const res = await fetch('https://swapi.dev/api/planets/');
		const data = await res.json();
		planets = data.results;
        
        // API is paginated, so we need to fetch all pages
        next = data.next;
		
        // next will be null when we've reached the last page
        while (next !== null) {
			const res = await fetch(next);
			const data = await res.json();
			next = data.next;
			
            // add the new planets to the array
			planets.push(...data.results);
			// make svelte recognize the change
            planets = planets;
		}
		// all planet data has been loaded
        planetsLoaded = true;
	}

	onMount(async () => {
		await getPlanets();
	});
</script>

<div class="flex-auto min-w-0 lg:static lg:max-h-full lg:overflow-visible dark:divide-gray-700">
	<PlanetTable {planets} {planetsLoaded} />
</div>
