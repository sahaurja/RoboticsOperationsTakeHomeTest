

<script>
  import { onMount } from 'svelte';
  let eventCode = '';
  let matches = [];

  const getEventStats = async () => {
    try {      
      const response = await fetch(`https://www.thebluealliance.com/api/v3/event/${eventCode}/matches`, {
        // I generated my READ API key. These keys are needed to access TBA's read APIv3.
        headers: {
          'X-TBA-Auth-Key': 'Mmopvcks2HquWD7YHHXGDkjHLyVUnCctAfwn6C3IlOulBU6XoAXIrpvlNwVXSWU0	', 
        },
      });

      if (!response.ok) {
        throw new Error('There is possibly a network connection issue!');
      }

      const data = await response.json();
      matches = data;
    } catch (error) {
      console.error('Error getting data ising the API:', error);
    }
  };

  // Retrieve the initializatixed  data
  onMount(() => {    
     getEventStats();
  });
</script>

<main>
  <h1>Statbotics Event Stats by Urja Saha</h1>
  <label for="eventCode"> Event Code:</label>
  <input type="text" id="eventCode" bind:value={eventCode} />
  <button on:click={getEventStats}>Find  Stats</button>

  {#if matches.length > 0}
    {#each matches as match}
      <div class="match-card">
        <h2>Match {match.match_number}</h2>
        <p>Competition Level: {match.comp_level}</p>
        <p>Teams: {match.alliances.red.team_keys.join(', ')} vs {match.alliances.blue.team_keys.join(', ')}</p>
        
      </div>
    {/each}
  {:else if eventCode !== ''}
    <p>Found no matches found for the event code.</p>
  {/if}
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  input,
  button {
    margin-top: 1em;
  }

  .match-card {
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 1em;
    margin-top: 1em;
  }
</style>
