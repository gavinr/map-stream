<script lang="ts">
  import { onMount } from "svelte";
  import { searchItems } from "@esri/arcgis-rest-portal";
  import type { ISearchOptions, IItem } from "@esri/arcgis-rest-portal";
  import Item from "./Item.svelte";

  let items: IItem[];
  $: items = [];

  /**
   * Compare newItems to existing (`items`) and insert new only.
   * @param newItems
   */
  const mergeItems = (newItems) => {
    if (items && items.length > 0) {
      const itemsToInsert = [];
      const currentFirstId = items[0].id;
      newItems.find((item) => {
        if (item.id === currentFirstId) {
          return true;
        } else {
          itemsToInsert.push(item);
          return false;
        }
      });

      items = [...itemsToInsert, ...items];
    } else {
      items = newItems;
    }
  };

  const looper = async () => {
    console.log("LOOP");
    const searchOptions: ISearchOptions = {
      q: '((type: "Web Map")) AND NOT type:"Web Mapping Application"',
      sortField: "modified",
      sortOrder: "desc",
    };
    const searchItemsResults = await searchItems(searchOptions);
    mergeItems(searchItemsResults.results);
    setTimeout(looper, 10000);
  };

  onMount(async () => {
    looper();
  });
</script>

<!-- `https://www.arcgis.com/home/item.html?id=${item.id}` -->
{#each items as item}
  <Item {item} />
{/each}

<style>
</style>
