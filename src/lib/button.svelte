<!--@component
Standard button, can be passed a function to execute on click
-->

<script lang="ts">
  import * as FeatherIcon from "svelte-feathers"

  export let func: () => void = () => {
    return
  }
  export let name: string = ""
  export let icon: string = ""
</script>

<button on:click={func}>
  {#if icon}
    <svelte:component this={FeatherIcon[icon]} size="12" />
  {/if}
  {#if name}
    <span>
      <slot>
        {name}
      </slot>
    </span>
  {/if}
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
    font-weight: 700;

    padding: 0 8px;
    min-height: 26px;
    display: flex;
    align-items: center;
    gap: 3px;

    transition: background 0.1s ease-out;

    &:focus {
      outline: none;
    }
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
