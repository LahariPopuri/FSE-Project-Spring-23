<script lang="js">
  import FruitFusion from "./FruitFusion.svelte";
  let mode = "menu";
</script>

<main>
  {#if mode === "menu"}
      <button
        class="font-mono font-extrabold text-3xl"
        on:click={() => {
          mode = "fruitFusion";
        }}>Fruit Fusion</button
      >
  {:else if mode === "fruitFusion"}
    <div class="space-y-3">
      <FruitFusion />

      <button
        class="font-mono font-extrabold text-3xl"
        on:click={() => {
          mode = "menu";
        }}>BACK</button
      >
    </div>
  {/if}
</main>
