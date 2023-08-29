<script lang="ts">
  import { createEventDispatcher } from 'svelte';

  export let isactive: boolean;
  export let id: number;

  const dispatch = createEventDispatcher();

  function handleClick() {
    dispatch('message', id);
  }

</script>

<span on:click={handleClick} class="dot {isactive ? 'active' : ''}" />

<style>
  .dot {
    height: 15px;
    width: 15px;
    margin: 0 2px;
    background-color: #bbb;
    border-radius: 50%;
    display: inline-block;
    transition: background-color 0.6s ease;
  }

  .active {
    background-color: var(--unissa-green);
    padding: 5px 9px;
    border-radius: 4px;
  }
</style>