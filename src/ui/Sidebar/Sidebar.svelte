<script lang="ts">
  import Folders from "./Folders.svelte";
  import Search from "./Search.svelte";
  import IconDelete from '../icons/IconDelete.svelte'
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
    <div class="icon-close" on:click={handleClose}>
        <IconDelete />
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

  .icon-close
  {
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    padding: 16px;
    color: var(--fm-color);
    transition: .3s color;
    background-color: transparent;
    border: none;
    cursor: pointer;
  }
</style>
