<script>
  let url = window.location.href;
  url = url.split("/");
  let id = url[5];
  let item;
  async function fetchData() {
    let link = "http://localhost:3000/item/show/";
    let res = link.concat(id);
    try {
      const response = await fetch(res);
      if (!response.ok) {
        throw new Error("Network response was not ok");
      }
      item = await response.json();
      item = item.itemData;
      console.log(item);
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  }
</script>

<main>
  {#await fetchData() then}
    <div class="container">
      <h1 class="mb-1">Title: {item.title}</h1>
      <div id="createdAt">Created at: 20/02/20</div>
      <a href="/" class="btn btn-secondary"> All Items</a>
      <a href="/" class="btn btn-info"> Edit</a>
      <!-- <div>
        <img src="{post.image}" alt="A Pic">
        {console.log(post.image)}
      </div> -->

      <div id="markdownShow">
        {item.markdown}
      </div>
    </div>
  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}
</main>

<style>
</style>
