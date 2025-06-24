<script lang="ts">
    import Folders from "./Folders.svelte"
    import Search from "./Search.svelte"
    import IconUpload from "../icons/IconUpload.svelte"

    import { useQueryClient } from "../../query";
    import { uploadFile, folder, getOptions, uploads } from '../../store';

    export let lazyFolders: boolean

    const queryClient = useQueryClient();
    const options = getOptions()

    function handleUpload() {
        // Crée dynamiquement un input de type file
        const input = document.createElement('input') as HTMLInputElement
        input.type = 'file'
        input.multiple = options.uploadButton.multiple// Permet de sélectionner plusieurs fichiers si true
        input.accept = options.uploadButton.accept // Définit les types de fichiers acceptés


        // Ajoute un gestionnaire d'événement pour le fichier sélectionné
        input.onchange = function (event) {
          const target = event.target;

          if (target instanceof HTMLInputElement && target.files) {
            Array.from(target.files as FileList).forEach(async (file: File) => {
              uploads.push(file)
              await uploadFile(options, queryClient, file, $folder)
              uploads.remove(file)
            });
          }
        }

        // Déclenche la boîte de dialogue
        input.click()
    }
</script>

<template>
  <aside class="fm-sidebar">
    <div class="icons">
      {#if options.uploadButton.visible}
      <span class="upload" on:click={handleUpload}>
        <IconUpload/>
      </span>
      {/if}
    </div>
    <Search/>
    <Folders folders={[null]} lazyLoad={lazyFolders}/>
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

    .icons {
        display: flex;
        justify-content: end; /* Can be space-between if "close PR" is merged */
    }

    .upload {
        color: var(--fm-color);
        cursor: pointer;
    }
</style>
