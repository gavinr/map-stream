<script lang="ts">
  import { onMount } from "svelte";
  import { ISearchOptions, searchItems } from "@esri/arcgis-rest-portal";

  export let name: string;

  onMount(async () => {
    const searchOptions: ISearchOptions = {
      q: "test",
    };
    const items = await searchItems(searchOptions);
    console.log("items", items);
  });
</script>

<main>
  <h1>Map Stream</h1>
</main>

<style>
</style>
