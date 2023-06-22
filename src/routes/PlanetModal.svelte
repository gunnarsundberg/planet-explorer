<script lang="ts">
    import { Button, Modal } from 'flowbite-svelte'

    export let planet: Planet;
    export let open = false;
</script>

{#if planet !== undefined}
<Modal title={planet.name} bind:open={open} autoclose outsideclose>
    <p class="mb-3 font-normal text-gray-700 dark:text-gray-400 leading-tight">
      {planet.name} is a {planet.climate} planet with a population of {planet.population}.
    </p>
</Modal>
{/if}
