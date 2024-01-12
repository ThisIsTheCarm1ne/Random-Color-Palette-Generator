<script lang="ts">
  import { onMount } from 'svelte';
  import ColorModesDropdown from './lib/ColorModesDropdown.svelte';
  import AboutAuthor from './lib/AboutAuthor.svelte';
  import { mode } from "./lib/store.ts";
  import chroma from 'chroma-js';

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
    colors = generateColorPalette(colorHarmonyMode);
  }


  function generateColorPalette(mode: string): string[] {
    const baseColor = getRandomColor();
    let palette: string[];

    switch (mode) {
      case 'complementary':
        palette = chroma.scale([baseColor, chroma(baseColor).saturate(2).hex()]).colors(colors.length);
        break;
      case 'triad':
        palette = chroma.scale([
          baseColor,
          chroma(baseColor).saturate(2).set('hsl.h', '+120').hex(),
          chroma(baseColor).saturate(2).set('hsl.h', '-120').hex()
        ]).colors(colors.length);
        break;
      case 'square':
        palette = chroma.scale([
          baseColor,
          chroma(baseColor).saturate(2).set('hsl.h', '+90').hex(),
          chroma(baseColor).saturate(2).set('hsl.h', '+180').hex(),
          chroma(baseColor).saturate(2).set('hsl.h', '-90').hex()
        ]).colors(colors.length);
        break;
      default:
        palette = colors.map(() => getRandomColor());
        break;
    }

    return palette;
  }

  function getRandomColor() {
    const letters = '0123456789ABCDEF';
    let color = '#';
    for (let i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
  }

  // Function to check if the color is dark
  function isColorDark(hexColor: string): boolean {
    const rgb = hexToRgb(hexColor);
    const brightness = (rgb[0] * 299 + rgb[1] * 587 + rgb[2] * 114) / 1000;
    return brightness < 128;
  }

  // Function to convert hex color to RGB
  function hexToRgb(hex: string): number[] {
    const bigint = parseInt(hex.slice(1), 16);
    const r = (bigint >> 16) & 255;
    const g = (bigint >> 8) & 255;
    const b = bigint & 255;
    return [r, g, b];
  }
</script>

<main>
  <AboutAuthor />
  <ColorModesDropdown />

  <div class="color_stripes">
    {#each colors as color}
      <div class="stripe" style="background-color: {color}">
        <p class="color_code" style="color: {isColorDark(color) ? 'white' : 'black'}">{color}</p>
      </div>
    {/each}
  </div>
  <div class="hint">
    <p>Press &ltspacebar&gt to randomly generate color palette</p>
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
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .color_code {
    height: min-content;
    font-size: 2.5em;
  }
  .hint {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 2em;
    background-color: #D1D1D195;
    border: 2px black solid;
    border-radius: 11px;
    margin-left: auto; 
    margin-right: auto; 
    font-size: 1.25em;
    text-align: center;
    padding: 1em;
    width: 35%;
  }
  .hint > p {
    margin: 0;
  }
  @media only screen and (max-width: 720px) {
    .color_stripes {
      flex-direction: column;
    }
    .stripe {
      height: min-content;
    }
    .hint {
      width: 80%;
      bottom: 1em;
    }
  }
</style>
