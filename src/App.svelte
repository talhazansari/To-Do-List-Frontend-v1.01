<script>
  import Router from "svelte-spa-router";
  import Home from "./Home.svelte";
  import New from "./New.svelte";
  import Edit from "./Edit.svelte";
  import Show from "./Show.svelte";

  const routes = {
    "/": Home,
    "/new": New,
    "/edit/:id": Edit,
    "/show/:id": Show,
  };
</script>

<main>
  <Router {routes} />
</main>
