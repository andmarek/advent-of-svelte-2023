<script>
    import { onMount } from 'svelte';

    let elvesNames;
    let searchInput;

    async function fetchData() {
        try {
            const response = await fetch('https://advent.sveltesociety.dev/data/2023/day-one.json');
            if (!response.ok) {
                throw new Error(response.statusText);
            }
            elvesNames = await response.json();
        } catch (error) {
            console.error(error);
        }
    }
    $: if (elvesNames) {
        elvesNames.sort((a, b) => b.tally - a.tally);
    }
    onMount(() => {
        fetchData();
    });
</script>

<svelte:head>
    <title>Day One</title>
    <meta name="description" content="Svelte demo app" />
</svelte:head>
<section class="rootSection">
    <div class="inputSection"> 
        <input type="text" placeholder="Filter by name" bind:value={searchInput} />
    </div>
    <!-- https://advent.sveltesociety.dev/data/2023/day-one.json -->
    {#if elvesNames}
        <h1 class="displayTitle">Present Tally</h1>
        <ul class="elvesNamesList">
            {#each elvesNames as elfName}
                <li class="elfNameItem">{elfName.name}: {elfName.tally}</li>
            {/each}
        </ul>
    {:else}
        <h1>Loading...</h1>
    {/if}
</section>

<style>
    .rootSection {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        flex: 0.6;
        background-color: #161A30;
    }
    .displayTitle {
        color: #F0ECE5;
    }
    .elvesNamesList {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        flex: 0.6;
       padding: 0;
    }
    .elfNameItem {
        list-style: none;
        width: 100px;
        text-align: center;

        padding: 10px;
        margin: 10px;

        /* Content */
        border: #F0ECE5 1px solid;
        color: #F0ECE5;

        transition:
            background-color 0.5s;
    }
    .elfNameItem:hover {
        background-color: #31304D;
    }
    

</style>