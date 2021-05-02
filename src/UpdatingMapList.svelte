<script lang="ts">
  import { onMount } from "svelte";
  import { searchItems } from "@esri/arcgis-rest-portal";
  import type { ISearchOptions, IItem } from "@esri/arcgis-rest-portal";
  import Item from "./Item.svelte";

  let items: IItem[];
  $: items = [];

  onMount(async () => {
    const searchOptions: ISearchOptions = {
      q: '((type: "Web Map")) AND NOT type:"Web Mapping Application"',
      sortField: "modified",
      sortOrder: "desc",
    };
    const searchItemsResults = await searchItems(searchOptions);
    items = searchItemsResults.results;
    console.log("items", items);
  });
</script>

<!-- `https://www.arcgis.com/home/item.html?id=${item.id}` -->
{#each items as item}
  <Item {item} />
{/each}

<style>
</style>
