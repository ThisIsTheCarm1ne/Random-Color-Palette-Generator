<script lang="ts">
  import { slide } from 'svelte/transition';
  let isOpen = false;
</script>

{#if !isOpen}
<button
  class="dropdown"
  on:click="{() => isOpen = !isOpen}"
  transition:slide={{ duration: 150, axis: 'x' }}
>
  About author
</button>
{/if}
{#if isOpen}
<aside transition:slide={{ duration: 300, axis: 'x' }}>
  <button
    class="exit_btn"
    on:click="{() => isOpen = !isOpen}"
  >
    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
      <path stroke-linecap="round" stroke-linejoin="round" d="m9.75 9.75 4.5 4.5m0-4.5-4.5 4.5M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
    </svg>
  </button>
  <h1>Random color palette generator</h1>
  <p>Was made by <a href="https://thisisthecarm1ne.github.io/">Carm1ne</a> with <a href="https://svelte.dev/">Svelte</a></p>
  <a href="https://github.com/ThisIsTheCarm1ne/Random-Color-Palette-Generator">GitHub repo</a>
</aside>
{/if}

<style>
  .dropdown {
    position: absolute;
    top: 20px;
    left: 20px;
    background-color: #D1D1D195;
    border: 2px black solid;
    border-radius: 11px;
    font-size: 1.25em;
    text-align: center;
    padding: 0.5em;
  }
  .dropdown:hover {
    cursor: pointer;
  }
  aside {
    z-index: 10;
    height: 100%;
    position: absolute;
    background-color: rgba(26, 32, 44, 0.8);
    backdrop-filter: blur(5px);
    padding-left: 1em;
    padding-right: 2em;
    color: white;
    font-size: 1.25em;
  }
  .exit_btn {
    margin-top: 1.5em;
    border: none;
    background: none;
    color: white;
    width: 3em;
    height: auto;
  }
  h1 {
    font-size: 1.5em;
    text-transform: capitalize;
  }
  @media only screen and (max-width: 720px) {
    .exit_btn {
      width: 4.5em;
    }
  }
</style>
