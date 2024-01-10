<script lang="ts">
  import { onMount } from 'svelte';
  import ColorModesDropdown from './lib/ColorModesDropdown.svelte';
  import { mode } from "./lib/store.ts";

  let colorHarmonyMode: string;
  mode.subscribe((value: string) => {
    colorHarmonyMode = value;
  })

  let colors: string[] = ['#ff0000', '#00ff00', '#0000ff', '#ffff00', '#ff00ff', '#00ffff'];

  onMount(() => {
    window.addEventListener('keydown', handleKeyDown);
    return () => {
      window.removeEventListener('keydown', handleKeyDown);
    };
  });

  function handleKeyDown(event: KeyboardEvent) {
    if (event.key === ' ') {
      updateColors();
    }
  }

  function updateColors() {
    colors = colors.map(() => getRandomColor());
  }

  function getRandomColor() {
    const letters = '0123456789ABCDEF';
    let color = '#';
    for (let i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
  }
</script>

<main>
  <ColorModesDropdown />

  <div class="color_stripes">
    {#each colors as color}
      <div class="stripe" style="background-color: {color}"></div>
    {/each}
  </div>
</main>

<style>
  .color_stripes {
    display: flex;
    width: 100vw;
    height: 100vh;
  }
  .stripe {
    flex: 1;
  }
</style>
