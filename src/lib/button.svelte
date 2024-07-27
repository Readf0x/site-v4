<!--@component
Standard button, can be passed a function to execute on click
-->

<script lang="ts">
  import * as FeatherIcon from "svelte-feathers"
  import { error } from "@sveltejs/kit"

  const empty = () => {}
  export let func: () => void = empty
  export let href: string = ""
  export let name: string = ""
  export let icon: string = ""

  if (href != "" && func != empty) {
    throw error(500, "Cannot pass both a function and href")
  }

  function redirect(): void {
    window.location.href = href
  }
</script>

<button on:click={href ? redirect : func}>
  {#if icon}
    <svelte:component this={FeatherIcon[icon]} size="12" />
  {/if}
  <slot>
    {#if name}
      <span>
        {name}
      </span>
    {/if}
  </slot>
</button>

<style lang="scss">
  @use "sass:map";
  @import "$lib/fonts.scss";
  @import "$lib/palette.scss";

  button {
    border: none;
    border-radius: 10px;

    font-family: $font;
    font-size: 12px;

    padding: 0 8px;
    min-height: 26px;
    display: flex;
    align-items: center;
    gap: 3px;

    transition: background 0.1s ease-out;
  }

  @each $name, $theme in $themes {
    :global(.#{$name}) button {
      color: map.get($theme, "text");
      background: map.get($theme, "surface-0");
      &:hover {
        background: map.get($theme, "surface-1");
      }
      &:active {
        background: map.get($theme, "surface-2");
      }
    }
  }
</style>
