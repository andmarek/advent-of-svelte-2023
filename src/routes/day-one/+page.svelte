<script lang="ts">
    import { onMount } from 'svelte';

    type Elf = {
        name: string;
        tally: number;
    }

    let elvesNames: Elf[];
    let displayElvesNames: Elf[];

    let searchInput: string = "";

    let elfToAdd: string;
    let scoreToAdd: string;
    $: displayElvesNames = searchInput.length > 0
        ? elvesNames.filter(elf => elf.name.includes(searchInput))
        : elvesNames;
    /*
    function handleSearchElf() {
        if (searchInput.length == 0) {
            console.log("Search input is empty");
            displayElvesNames = elvesNames;
        } else {
            displayElvesNames = elvesNames.filter(elf => elf.name.includes(searchInput));
        }
    }
    */

    function handleAddElf() {
        console.log(elfToAdd) 
        console.log(scoreToAdd);
        elvesNames.push({name: elfToAdd, tally: parseInt(scoreToAdd)});
        elvesNames = elvesNames;
        displayElvesNames = elvesNames;
    }

    async function fetchData() {
        try {
            const response = await fetch('https://advent.sveltesociety.dev/data/2023/day-one.json');
            if (!response.ok) {
                throw new Error(response.statusText);
            }
            elvesNames = await response.json();
            displayElvesNames = elvesNames;
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
        <h2 class="subtitle"> Add an Elf and Their Score!</h2>
        <input class="searchBox" type="text" placeholder="Elf Name" bind:value={elfToAdd} />
        <input class="searchBox" type="text" placeholder="Score" bind:value={scoreToAdd} />
        <button on:click={handleAddElf}> Add </button>
    </div>

    <div class="inputSection"> 
        <h2 class="subtitle"> Filter By Name </h2>
        <input type="text" placeholder="Filter by name" bind:value={searchInput} />
    </div>

    <!-- https://advent.sveltesociety.dev/data/2023/day-one.json -->
    {#if elvesNames}
        <h1 class="displayTitle">Present Tally</h1>
        <ul class="elvesNamesList">
            {#key displayElvesNames}
                {#each displayElvesNames as elfName}
                    <li class="elfNameItem">{elfName.name}: {elfName.tally}</li>
                {/each}   
            {/key}
            
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
        color: #B6BBC4;
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

    .subtitle {
        color: #B6BBC4
    }

    .elfNameItem:hover {
        background-color: #31304D;
    }
    
    .searchBox {
        padding: 10px 15px;
        margin: 8px 0;
        box-sizing: border-box;
        border: 2px solid #ccc;
        border-radius: 4px;
        background-color: white;
        resize: vertical;
        font-size: 16px;
        transition: border-color 0.3s;
    }

    .inputSection {
        text-align: center;
    }
</style>