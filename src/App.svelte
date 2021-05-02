<script lang="ts">
  import { onMount } from "svelte";
  import { searchItems } from "@esri/arcgis-rest-portal";
  import type { ISearchOptions, IItem } from "@esri/arcgis-rest-portal";

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

<main>
  <h1>Map Stream</h1>
  <div>
    <!-- `https://www.arcgis.com/home/item.html?id=${item.id}` -->
    {#each items as item}
      <div class="item">
        <a
          href={`https://www.arcgis.com/apps/mapviewer/index.html?webmap=${item.id}`}
          target="_blank"
          ><img
            src={`https://www.arcgis.com/sharing/rest/content/items/${item.id}/info/${item.thumbnail}`}
            alt="Map thumbnail"
          /></a
        >

        Title:
        <a
          href={`https://www.arcgis.com/apps/mapviewer/index.html?webmap=${item.id}`}
          target="_blank">{item.title}</a
        ><br />
        Owner: {item.owner}<br />
        Modified: {new Date(item.modified)}
        <div class="clear" />
      </div>
    {/each}
  </div>
</main>

<style>
  .item {
    padding: 10px;
    border: 1px solid gray;
    margin: 10px;
  }
  .item img {
    float: left;
    padding-right: 10px;
  }
  .clear {
    clear: both;
  }
</style>
