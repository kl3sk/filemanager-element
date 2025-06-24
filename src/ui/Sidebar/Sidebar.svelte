<script lang="ts">
  import Folders from "./Folders.svelte";
  import Search from "./Search.svelte";
  import IconDelete from '../icons/IconDelete.svelte'
  import IconUpload from "../icons/IconUpload.svelte"
  export let lazyFolders: boolean

  let componentEl: HTMLElement;

  function handleClose() {
    const event = new CustomEvent('close', {
      bubbles: true,
      composed: true
    });

    // Dispatch depuis l’élément racine du composant
    componentEl.dispatchEvent(event);
  }
</script>

<template>
  <aside class="fm-sidebar" bind:this={componentEl}>
    <div class="icons">
      <span class="close" on:click={handleClose}>
        <IconDelete on:click={handleClose}/>
      </span>
    </div>
    <Search />
    <Folders folders={[null]} lazyLoad={lazyFolders} />
  </aside>
</template>

<style>
  .fm-sidebar {
    border-right: 1px solid var(--fm-border);
    padding: 24px;
    overflow: auto;
  }
  .fm-sidebar > :global(*) + :global(*)::before {
    content: "";
    display: block;
    height: 1px;
    background-color: var(--fm-border);
    margin: 1.5em 0;
  }

  .icons
  {
    display: flex;
    justify-content: space-between;
  }

  .close
  {
    color: var(--fm-color);
    border: none;
    cursor: pointer;
  }
</style>
