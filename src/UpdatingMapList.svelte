<script lang="ts">
  import { onMount } from "svelte";
  import ProgressBar from "progressbar.js";
  import { searchItems } from "@esri/arcgis-rest-portal";
  import type { ISearchOptions, IItem } from "@esri/arcgis-rest-portal";
  import Item from "./Item.svelte";

  let progressBarNode;
  let progressBar;
  let items: IItem[];
  $: items = [];

  /**
   * Compare newItems to existing (`items`) and insert new only.
   * @param newItems
   */
  const mergeItems = (newItems) => {
    if (items && items.length > 0) {
      // item hash is ID + modified date
      const itemHashes = new Set(
        items.map((item) => {
          return `${item.id}-${item.modified}`;
        })
      );

      newItems = newItems.map((item) => {
        const hash = `${item.id}-${item.modified}`;
        if (!itemHashes.has(hash)) {
          item.new = true;
        }
        return item;
      });
      items = newItems;
    } else {
      items = newItems;
    }
  };

  const looper = async () => {
    const searchOptions: ISearchOptions = {
      q: '((type: "Web Map")) AND NOT type:"Web Mapping Application"',
      sortField: "modified",
      sortOrder: "desc",
      num: 50,
    };
    const searchItemsResults = await searchItems(searchOptions);
    mergeItems(searchItemsResults.results);

    const duration = 10000;
    setTimeout(looper, duration);
    progressBar.set(1);
    progressBar.animate(0, {
      duration: duration,
    });
  };

  onMount(async () => {
    looper();
    progressBar = new ProgressBar.Circle(progressBarNode, {
      strokeWidth: 6,
    });
  });
</script>

<div>
  <div class="progressBar" bind:this={progressBarNode} />
  <div class="itemsWrapper">
    <!-- `https://www.arcgis.com/home/item.html?id=${item.id}` -->
    {#each items as item (item.id + item.modified)}
      <Item {item} />
    {/each}
  </div>
</div>

<style>
  .itemsWrapper {
    padding-top: 20px;
  }
  .progressBar {
    float: right;
    height: 20px;
    width: 20px;
  }
</style>
