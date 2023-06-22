<script lang="ts">
	import PlanetModal from './PlanetModal.svelte';
	import {
		TableBody,
		TableBodyCell,
		TableBodyRow,
		TableHead,
		TableHeadCell,
		TableSearch,
		ListPlaceholder,
		Button
	} from 'flowbite-svelte';

	export let planets: Planet[];
	export let planetsLoaded = false;

	let searchTerm = '';
	let exploreModal = false;
	let explorePlanetData: Planet;

	$: filteredItems = planets.filter(
		(planet) => planet.name.toLowerCase().indexOf(searchTerm.toLowerCase()) !== -1
	);

	function explorePlanet(planet: Planet) {
		console.log(planet);
		explorePlanetData = planet;
		exploreModal = true;
	}
</script>

<div>
	<PlanetModal planet={explorePlanetData} bind:open={exploreModal} />

	<!-- Searchable table of planets -->
	<TableSearch placeholder="Search by planet name" hoverable={true} bind:inputValue={searchTerm}>
		<!-- Table description -->
		<caption
			class="p-5 text-lg font-semibold text-left text-gray-900 bg-white dark:text-white dark:bg-gray-800"
		>
			Star Wars Planet Explorer
			<p class="mt-1 text-sm font-normal text-gray-500 dark:text-gray-400">
				Browse a list of Star Wars planets, including populations, climate, and terrain. Click
				"Explore" to learn more about a planet.
			</p>
		</caption>
		<TableHead>
			<TableHeadCell>Name</TableHeadCell>
			<TableHeadCell>Population</TableHeadCell>
			<TableHeadCell>Climate</TableHeadCell>
			<TableHeadCell>Terrain</TableHeadCell>
			<TableHeadCell>
				<span class="sr-only"> Explore </span>
			</TableHeadCell>
		</TableHead>
		<TableBody>
			{#if planets.length > 0}
				{#each filteredItems as planet}
					<TableBodyRow>
						<TableBodyCell>{planet.name}</TableBodyCell>
						<TableBodyCell>{planet.population}</TableBodyCell>
						<TableBodyCell>{planet.climate}</TableBodyCell>
						<TableBodyCell>{planet.terrain}</TableBodyCell>
						<TableBodyCell>
							<Button on:click={() => explorePlanet(planet)}>Explore</Button>
						</TableBodyCell>
					</TableBodyRow>
				{/each}
			{/if}
		</TableBody>
	</TableSearch>

	<!-- Display animated placeholder below planet rows to indicate that not all planets have been loaded -->
	{#if !planetsLoaded}
		<ListPlaceholder
			divClass="p-4 rounded border border-gray-200 shadow animate-pulse md:p-6 dark:border-gray-700"
		/>
	{/if}
</div>
