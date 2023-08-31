<script>
  let url = window.location.href;
  url = url.split("/");
  let id = url[5];
  let item;
  let status = false;
  function toggleCheckbox() {
    status = !status;
  }
  async function fetchData() {
    let link = "http://localhost:3000/item/edit/";
    let res = link.concat(id);
    try {
      const response = await fetch(res);
      if (!response.ok) {
        throw new Error("Network response was not ok");
      }
      item = await response.json();
      item = item.itemData;
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  }

  async function submitItem() {
    let link = "http://localhost:3000/item/edit/";
    let res = link.concat(id);

    fetch(res, {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        title: item.title,
        description: item.description,
        markdown: item.markdown,
        status: item.status,
      }),
    })
      .then((response) => response.json())
      .then((result) => console.log(result));
  }
</script>

<main>
  {#await fetchData() then}
    <div class="container">
      <h1 class="mb-4 mt-3">Edit Item</h1>
      <form>
        <div class="form-group">
          <label id="titleLabel" for="title">Title</label>
          <input
            required
            bind:value={item.title}
            type="text"
            name="title"
            id="title"
            class="form-control"
          />
        </div>
        <div class="form-group">
          <label id="titleLabel" for="description">Description</label>
          <textarea
            name="description"
            id="description"
            bind:value={item.description}
            class="form-control"
          />
        </div>

        <div class="form-group">
          <label id="markdownShow" for="markdown">Markdown</label>
          <textarea
            required
            name="markdown"
            id="markdown"
            bind:value={item.markdown}
            class="form-control"
          />
        </div>
      </form>
      <a href="/">
        <button on:click={submitItem} class="btn btn-primary mt-4">
          Save
        </button>
      </a>
      <a href="/" class="btn btn-secondary mt-4">Cancel</a>
      <br />

      <h1 id="h1status" class="mt-4">Status:</h1>
      <input
        class="mt-4"
        id="checkbox"
        type="checkbox"
        bind:checked={item.status}
        on:click={toggleCheckbox}
      />
    </div>
  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}
</main>

<style>
  #checkbox {
    width: 30px;
    height: 30px;
    margin-left: 10px;
  }
  #h1status {
    display: inline;
  }
</style>
