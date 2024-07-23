<script lang="ts">
  import Button from "$lib/button.svelte"
  import { onMount } from "svelte"
  import "./style.scss"

  $: theme = true
  onMount(() => {
    theme = localStorage.theme === "dark"
  })
  function toggleTheme(): void {
    theme = !theme
    localStorage.theme = theme ? "dark" : "light"
    document.documentElement.classList.toggle("dark")
    document.documentElement.classList.toggle("light")
  }
</script>

<svelte:head>
  <!-- https://dev.to/lubiah/how-to-prevent-theme-colour-from-flickering-in-svelte-or-sveltekit-20cm -->
  <script>
    if (document) {
      if (localStorage.theme) mode = localStorage.theme
      else mode = window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light"
      if (mode === "dark") {
        document.documentElement.classList.add("dark")
        document.documentElement.classList.remove("light")
        localStorage.theme = "dark"
      } else {
        document.documentElement.classList.add("light")
        document.documentElement.classList.remove("dark")
        localStorage.theme = "light"
      }
    }
  </script>
</svelte:head>

<nav id="nav">
  <div class="left">
    <Button name="Home" icon="Home" />
  </div>
  <div class="middle"></div>
  <div class="right">
    <Button name="About" icon="Info" />
    <Button name="Music" icon="Headphones" />
    <Button name="Photos" icon="Camera" />
    <Button icon={theme ? "Moon" : "Sun"} func={toggleTheme} />
  </div>
</nav>
<div class="wrapper">
  <slot />
</div>

<style lang="scss">
  @use "sass:map";
  @import "$lib/palette.scss";

  nav {
    padding: 8px;
    display: flex;
    * {
      display: flex;
      gap: 8px;
      justify-content: flex-end;
    }
    .middle,
    .right {
      flex: 1;
    }
  }

  .wrapper {
    height: calc(100vh - 46px);
  }
  @each $name, $theme in $themes {
    :global(.#{$name}) .wrapper {
      background: map.get($theme, "base");
      border: 2px solid map.get($theme, "crust");
      border-radius: 10px;
    }
  }
</style>
