<script lang="ts">
    import { onMount } from 'svelte';
    //import { ListPlaceholder } from 'flowbite-svelte'
	import PlanetTable from './PlanetTable.svelte';
    
    // planet data
    let planets: Planet[] = [];
    // boolean representing whether all data has been loaded
    let planetsLoaded = false;

    async function getPlanets() {
        let next:string;

        const res = await fetch('https://swapi.dev/api/planets/');
        const data = await res.json();
        next = data.next;
        planets = data.results;
        while (next !== null) {
            const res = await fetch(next);
            const data = await res.json();
            next = data.next;
            console.log(next);
            console.log(data.results);
            planets.push(...data.results);
            planets = planets;
        }
        planetsLoaded = true;
    }

    onMount(async () => {
        await getPlanets();
    });
</script>

<div class= "flex-auto min-w-0 lg:static lg:max-h-full lg:overflow-visible dark:divide-gray-700">
    <!--
    {#if planets.length === 0}
        <ListPlaceholder divClass="p-4 rounded border border-gray-200 shadow animate-pulse md:p-6 dark:border-gray-700"/>
    {:else}
        <PlanetList {planets} {planetsLoaded} />
    {/if}
    -->
    <PlanetTable {planets} {planetsLoaded} />
</div>
