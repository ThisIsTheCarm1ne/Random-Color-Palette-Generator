<script lang="ts">
  import { mode } from "./store.ts";
  function handleModeChange(event: Event) {
    const selectedMode = (event.target as HTMLSelectElement).value;
    mode.set(selectedMode);
  }
</script>

<div class="dropdown">
  <label for="colorModeSelect">Modes:</label>
  <select id="colorModeSelect" on:change={handleModeChange}>
    <option value="auto">Auto</option>
    <option value="complementary">Complementary</option>
    <option value="triad">Triad</option>
    <option value="square">Square</option>
  </select>
</div>

<style>
  .dropdown {
    position: absolute;
    top: 20px;
    right: 20px;
    background-color: #D1D1D195;
    border: 2px black solid;
    border-radius: 11px;
    font-size: 1.25em;
    text-align: center;
    padding: 0.5em;
  }
</style>
