<script lang="ts">
  export let name = "wifi";

  // Svg icons from https://tablericons.com/
  const paths = {
    wifi: [
      "M9.172 15.172a4 4 0 0 1 5.656 0",
      "M6.343 12.343a8 8 0 0 1 11.314 0",
      "M3.515 9.515c4.686 -4.687 12.284 -4.687 17 0",
    ],
  };
</script>

<svg
  id={`icon-${name}`}
  xmlns="http://www.w3.org/2000/svg"
  width="22"
  height="22"
  viewBox="0 0 24 24"
  stroke-width="1.5"
  stroke="#2c3e50"
  fill="none"
  stroke-linecap="round"
  stroke-linejoin="round"
>
  {#each paths[name] as path}
    <path d={path} />
  {/each}
</svg>
