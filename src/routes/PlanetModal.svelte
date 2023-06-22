<script lang="ts">
	import { onMount } from 'svelte';
	import { Modal, Tabs, TabItem } from 'flowbite-svelte';

	export let planet: Planet;
	export let open = false;

	// character & film data
	let characters: Character[] = [];
	let films: Film[] = [];

	// get data from API for a given array of requests (urls) and push it to the given data array
	async function getData(requests: string[], data: any[]) {
		for (const r of requests) {
			const res = await fetch(r);
			const resData = await res.json();
			data.push(resData);
		}
	}

	// get summary of planet facts
	function getFactSummary(planet: Planet) {
		let climateSummary = '';
		let populationSummary = '';

		if (planet.climate !== 'unknown') {
			climateSummary = planet.climate + ' planet with ';
		} else {
			climateSummary = 'planet with an unknown climate and ';
		}

		if (planet.population !== 'unknown') {
			populationSummary = 'a population of ' + planet.population;
		} else {
			populationSummary = 'an unknown population';
		}

		return planet.name + ' is a ' + climateSummary + populationSummary + '.';
	}

	// get summary of planet characters
	function getCharacterSummary(characters: Character[]) {
		switch (characters.length) {
			case 0:
				return planet.name + ' is not home to any Star Wars characters.';
			case 1:
				return planet.name + ' is home to ' + characters.length + ' character.';
			default:
				return planet.name + ' is home to ' + characters.length + ' characters.';
		}
	}

	// get summary of planet films
	function getFilmSummary(films: Film[]) {
		switch (films.length) {
			case 0:
				return planet.name + ' is not featured in any Star Wars films.';
			case 1:
				return planet.name + ' is featured in ' + films.length + ' Star Wars film.';
			default:
				return planet.name + ' is featured in ' + films.length + ' Star Wars films.';
		}
	}

	// on mount, get data for the planet's residents and films
	onMount(async () => {
		await getData(planet.residents, characters);
		await getData(planet.films, films);
	});

	// watch for changes to the planet prop and update the data
	$: if (planet) {
		characters = [];
		films = [];
		getData(planet.residents, characters);
		getData(planet.films, films);
	}
</script>

{#if planet !== undefined}
	<Modal title={planet.name} bind:open outsideclose>
		<Tabs>
			<TabItem open title="Facts">
				<p class="text-sm text-gray-500 dark:text-gray-400">{getFactSummary(planet)}</p>
				<div class="pt-3">
					<h3 class="text-lg font-semibold pb-3">Geology</h3>
					<ul>
						<li><b>Gravity</b>: {planet.gravity}</li>
						<li><b>Terrain:</b> {planet.terrain}</li>
						<li><b>Surface Water:</b> {planet.surface_water}</li>
						<li><b>Orbital Period:</b> {planet.orbital_period}</li>
						<li><b>Rotation Period:</b> {planet.rotation_period}</li>
						<li><b>Diameter:</b> {planet.diameter}</li>
					</ul>
				</div>
			</TabItem>
			<TabItem title="Characters">
				<p class="text-sm text-gray-500 dark:text-gray-400">{getCharacterSummary(characters)}</p>
				<div class="pt-3">
					<h3 class="text-lg font-semibold pb-3">Characters</h3>
					<ul>
						{#each characters as character}
							<li>{character.name}</li>
						{/each}
					</ul>
				</div>
			</TabItem>
			<TabItem title="Films">
				<p class="text-sm text-gray-500 dark:text-gray-400">{getFilmSummary(films)}</p>
				<div class="pt-3">
					<h3 class="text-lg font-semibold pb-3">Films</h3>
					<ul>
						{#each films as film}
							<li>{film.title}</li>
						{/each}
					</ul>
				</div>
			</TabItem>
		</Tabs>
	</Modal>
{/if}
