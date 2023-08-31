<script>
  let title = "";
  let description = "";
  let markdown = "";
  let item;
  let status = false;

  function toggleCheckbox() {
    status = !status;
  }
  async function submitPost() {
    try {
      item = {
        description: description,
        status: true,
        title: title,
        markdown: markdown,
        status: status,
      };
      const response = await fetch("http://localhost:3000/item", {
        method: "POST",
        body: JSON.stringify(item),
        headers: {
          "Content-Type": "application/json",
        },
      });
      if (response.ok) {
        console.log("Post Submitted Successfully");
      } else {
        console.error("Failed to submit post");
      }
    } catch (error) {
      console.error("Error sending data:", error);
    }
  }
</script>

<main>
  <div class="container">
    <h1 class="mb-4 mt-4">New Item</h1>
    <form>
      <div class="form-group">
        <label id="titleLabel" for="title">Title</label>
        <input
          required
          bind:value={title}
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
          bind:value={description}
          class="form-control"
        />
      </div>
      <div class="form-group">
        <label id="titleLabel" for="markdown">Markdown</label>
        <textarea
          required
          name="markdown"
          id="markdown"
          bind:value={markdown}
          class="form-control"
        />
      </div>
    </form>
    <a href="/">
      <button on:click={submitPost} class="btn btn-primary mt-4"> Save </button>
    </a>
    <a href="/" class="btn btn-secondary mt-4">Cancel</a>
    <br />
    <h1 id="h1status" class="mt-4">Status:</h1>
    <input
      class="mt-4"
      id="checkbox"
      type="checkbox"
      bind:checked={status}
      on:click={toggleCheckbox}
    />
  </div>
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
