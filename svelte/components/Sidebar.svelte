<script lang="ts">
		import { onMount } from 'svelte';
    import axios from 'axios'

    let loadingState: "initial" | "more" | "refetch" | "ready" = "initial";
    let users = [];
    let error = null;
    
    const fetchData = async () => { 
      try {
        const res = await axios.get(`${apiBaseUrl}/api/users/contacts?access_token=${accessToken}`);
        users = res.data;
      } catch (err) {
        error = err.message
      }
      loadingState = "ready";
     }

    onMount(async () => {
      await fetchData()
    });
</script>

<style>
  .contact-card {
    height: 40px;
    border-style: solid;
    border: none;
  }
  .contact-card:hover {
    background-color: #252526;
    filter: brightness(120%);
    cursor: pointer;
  }
  .contact-img {
    height: 30px;
    width: 30px;
    margin-top: 5px;
    border-radius: 50%;
  }
  .contact-name {
    margin-left: 15px;
  }
  .inline {
    display: flex;
    flex-flow: row wrap;
    align-items: center;
  }
</style>

<main>
  {#if error}
    <p>Error: {error.message}</p>
  {/if}
    <h2>Contacts ({users.length})</h2><br>
    {#each users as user}
    <div class="contact-card" on:click={() => {
      tsvscode.postMessage({ type: 'onContactPress', value: user });
    }}>
      <div class="inline">
        <img class="contact-img" src="{user.avatar_url}" alt="{user.username}"/>
        <h3 class="contact-name">{user.username}</h3>
      </div>
    </div><br>
    {:else}
      <p>loading..</p>
	  {/each}
</main>