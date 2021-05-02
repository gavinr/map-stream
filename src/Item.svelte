<script lang="ts">
  import type { IItem } from "@esri/arcgis-rest-portal";

  export let item: IItem;
</script>

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
