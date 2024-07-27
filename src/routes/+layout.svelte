<script lang="ts">
  import Button from "$lib/button.svelte"
  import { onMount } from "svelte"
  import "./style.scss"

  $: theme = true
  onMount(() => {
    theme = localStorage.theme === "dark"
    document.body.classList.remove("no-anim")
  })
  function toggleTheme(): void {
    theme = !theme
    localStorage.theme = theme ? "dark" : "light"
    document.documentElement.classList.toggle("dark")
    document.documentElement.classList.toggle("light")
  }

  let search: HTMLInputElement
  function searchFor(): void {
    if (!search.value) search.focus()
    else window.location.href = `/search/?q=${encodeURIComponent(search.value)}`
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

  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="true" />
  <link href="https://fonts.googleapis.com/css2?family=Sniglet:wght@400;800&display=swap" rel="stylesheet" />
</svelte:head>

<nav id="nav">
  <div class="left">
    <Button name="Home" icon="Home" href="/" />
  </div>
  <div class="middle"></div>
  <div class="right">
    <Button name="About" icon="Info" href="/about" />
    <Button name="Projects" icon="Code" href="/projects" />
    <Button name="Music" icon="Headphones" href="/music" />
    <Button name="Photos" icon="Camera" href="/photos" />
    <Button icon="Search" func={searchFor}>
      <input
        type="text"
        bind:this={search}
        placeholder="Search..."
        size="20"
        on:keydown={(e) => e.key === "Enter" && searchFor()}
      />
    </Button>
    <Button icon={theme ? "Moon" : "Sun"} func={toggleTheme} />
  </div>
</nav>
<div class="wrapper">
  <slot />
</div>

<style lang="scss">
  @use "sass:map";
  @import "$lib/fonts.scss";
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
    transition: background 0.1s ease-out;
    height: calc(100vh - 46px);
  }

  input[type="text"] {
    border: none;
    font-family: $font;
    background: none;
    font-size: inherit;
    font-weight: inherit;
  }

  @each $name, $theme in $themes {
    :global(.#{$name}) .wrapper {
      background: map.get($theme, "base");
      border: 2px solid map.get($theme, "crust");
      border-radius: 10px;
    }
    :global(.#{$name}) input[type="text"] {
      color: map.get($theme, "text");

      &::placeholder {
        color: inherit;
        font-size: inherit;
        font-weight: inherit;
      }
    }
  }
</style>
