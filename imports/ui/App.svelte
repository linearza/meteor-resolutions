<script>
  import { useTracker } from "meteor/rdb:svelte-meteor-data";
  import { Resolutions } from "../api/resolutions";
  import { LoginWindow, Logout } from "meteor/levelup:svelte-accounts-ui";

  import Resolution from "./resolutions/Resolution.svelte";
  let newRes = "";

  function handleSubmit(event) {
    Resolutions.insert({
      title: newRes,
    });

    newRes = "";
  }

  $: resolutions = useTracker(() => Resolutions.find({}).fetch());
  $: user = useTracker(() => Meteor.user());
</script>

<header>
  <h1>Yo! thats fresh</h1>

  {#if $user && $user._id}
    <Logout />
  {:else}
    <LoginWindow />
  {/if}

  <form on:submit|preventDefault={handleSubmit}>
    <input type="text" placeholder="Add your resolution" bind:value={newRes} />
    <button>Add</button>
  </form>

  {#each $resolutions as resolution}
    <Resolution {resolution} />
  {/each}
</header>
