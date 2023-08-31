<script>
  let data = null;

  async function fetchData() {
    try {
      const response = await fetch("http://localhost:3000/item/");
      if (!response.ok) {
        throw new Error("Network response was not ok");
      }
      data = await response.json();
      return data.itemData;
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  }

  export function removeItem(id) {
    let link = "http://localhost:3000/item/";
    let result = link.concat(id);
    fetch(result, {
      method: "DELETE",
    })
      .then((response) => response.json())
      .then((res) => console.log(res));
    location.reload();
  }
</script>

<main>
  <div class="container">
    <h1 class="mb-4 mt-3">To-Do List Items</h1>
    <a href="#/new" class="btn btn-success">New Item</a>
    {#await fetchData()}
      <p>Data to be displayed</p>
    {:then data}
      {#each data as item}
        <div class="card mt-4">
          <div class="card-body">
            <h2 id="itemTitle" class="card-title"><b>{item.title}</b></h2>

            <div class="card-text-mb-2">
              Updated on: {item.updatedAt.substring(0, 10)}
            </div>
            <div class="card-text-mb-2">
              {item.description}
            </div>

            <a href="#/show/{item._id}" class="btn btn-primary">Read More</a>
            <a href="#/edit/{item._id}" class="btn btn-info">Edit</a>

            <button on:click={() => removeItem(item._id)} class="btn btn-danger"
              >Delete</button
            >

            {#if item.status}
              <div id="box">
                <img id="checkedBox" src="assets/checked.png" alt="Sorry" />
              </div>
            {/if}
          </div>
        </div>
      {/each}
    {:catch error}
      <p style="color: red">{error.message}</p>
    {/await}
  </div>
</main>

<style>
  #checkedBox {
    width: 50px;
  }

  #box {
    display: inline;
    margin-left: 750px;
  }
</style>
