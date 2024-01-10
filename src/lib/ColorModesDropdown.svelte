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
  }
</style>
