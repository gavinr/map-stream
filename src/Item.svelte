<script lang="ts">
  import { fade, fly } from "svelte/transition";
  import type { IItem } from "@esri/arcgis-rest-portal";

  export let item: IItem;
  let dateString = "";

  $: {
    const itemDate = new Date(item.modified);
    dateString = `${itemDate.getHours()}:${String(
      itemDate.getMinutes()
    ).padStart(2, "0")}:${String(itemDate.getSeconds()).padStart(2, "0")}`;
  }
</script>

<div
  class="item"
  class:new={item.new === true}
  in:fly={{ y: -200, duration: 1000 }}
>
  {#if item.thumbnail}
    <a
      href={`https://www.arcgis.com/apps/mapviewer/index.html?webmap=${item.id}`}
      target="_blank"
      ><img
        src={`https://www.arcgis.com/sharing/rest/content/items/${item.id}/info/${item.thumbnail}`}
        alt="Map thumbnail"
      /></a
    >
  {/if}
  Title:
  <a
    href={`https://www.arcgis.com/apps/mapviewer/index.html?webmap=${item.id}`}
    target="_blank">{item.title}</a
  ><br />
  Owner: {item.owner}<br />
  Modified: {dateString}<br />
  {item.numViews.toLocaleString("en-US")} views<br />
  {#if item.snippet}
    <br /><br /><em>{item.snippet}</em>
  {/if}

  <div class="clear" />
</div>

<style>
  .item {
    padding: 10px;
    border: 1px solid gray;
    margin: 10px;
    transition: background-color 1s linear;
  }
  .item.new {
    background-color: rgba(250, 128, 114, 0.3);
  }
  .item img {
    float: left;
    padding-right: 10px;
  }
  .clear {
    clear: both;
  }
</style>
