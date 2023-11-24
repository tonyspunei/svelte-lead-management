<script>
  import Dropzone from "svelte-file-dropzone/Dropzone.svelte";
  import { db, storage } from '$lib/firebase';
  import { ref } from "firebase/storage";
	import { onMount } from "svelte";

  let files = {
    accepted: [],
    rejected: []
  };
  let uploadedList = false;

  async function uploadFile(file) {
    const storageRef = ref(storage, 'files/' + file.name)
    console.log(storageRef);
  }

  function handleFilesSelect(e) {
    const { acceptedFiles, fileRejections } = e.detail;
    files.accepted = [...files.accepted, ...acceptedFiles];
    files.rejected = [...files.rejected, ...fileRejections];
  }
  $: {
    if(files?.accepted?.length) {
      uploadedList = true;
    } else {
      uploadedList = false;
    }
  }
  onMount(() => {
    uploadFile();
  })
</script>

<section>
  <Dropzone on:drop={handleFilesSelect} disableDefaultStyles="true" containerClasses="h-52 max-w-xl mx-auto mt-10 flex flex-col items-center justify-center border-2 border-dashed bg-transparent rounded-lg" accept=".csv">
    <svg aria-hidden="true" class="mb-3 w-10 h-10 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12" /></svg>
    <p class="mb-2 text-sm text-gray-500 dark:text-gray-400"><span class="font-semibold">Click to upload</span> or drag and drop</p>
    <p class="text-xs text-gray-500 dark:text-gray-400">CSV only (MAX. 100 MG)</p>
  </Dropzone>
  <div class="flex flex-col bg-slate-300 max-w-4xl mx-auto my-20 p-10 text-gray-900">
    <h2 class="text-3xl font-semibold mb-3">Uploaded Files</h2>
    {#if files?.accepted?.length}
      <ol class="pl-5 list-disc">
        {#each files.accepted as item}
          <li>{item.name}</li>
        {/each}
      </ol>
    {:else}
      <span>No files uploaded</span>
    {/if}
  </div>

  <button class="bg-slate-500 px-6 py-2 mx-auto flex disabled:opacity-50" disabled={!uploadedList}>Remove duplicates</button>
</section>

<style>
  
</style>