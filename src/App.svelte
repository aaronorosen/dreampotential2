<script>
  import { onMount } from "svelte";
  import page from "page";
  import { Home, Team, Page, Labs, Education } from "./pages";

  let active = Home;
  let redirecting = true;

  page.start();

  page("*", (c, next) => {
    scrollTo(0, 0);
    next();
  });

  page("/", (context) => {
    active = Home;
  });
  // page("/philosophy", (context) => {
  //   active = GoBeyond;
  // });
  page("/labs", (context) => {
    active = Labs;
  });
  page("/education", (context) => {
    active = Education;
  });
  // page("/team", (context) => {
  //   active = Team;
  // });

  page("*", () => page.redirect("/"));

  onMount(() => {
    if (!location.host.match(/^(localhost)|(127.0.0.1)(:\d\d+)?/)) {
      if (location.protocol === "http:") {
        location.protocol = "https:";
        return;
      }
    }
    redirecting = false;
    page.replace(location.pathname);
  });
</script>

{#if redirecting}
  <section class="redirecting">
    <span>redirecting...</span>
  </section>
{:else}
  <Page>
    <svelte:component this={active || Home} />
  </Page>
{/if}

<style>
  .redirecting {
    display: flex;
    width: 100vw;
    height: 100vh;
    justify-content: center;
    align-items: center;
    background: #eee;
    color: black;
  }
</style>
