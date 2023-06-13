<script lang="ts">
  enum Theme {
    light = "light",
    dark = "dark",
    cupcake = "cupcake",
    bumblebee = "bumblebee",
    emerald = "emerald",
    corporate = "corporate",
    synthwave = "synthwave",
    retro = "retro",
    cyberpunk = "cyberpunk",
    valentine = "valentine",
    halloween = "halloween",
    garden = "garden",
    forest = "forest",
    aqua = "aqua",
    lofi = "lofi",
    pastel = "pastel",
    fantasy = "fantasy",
    wireframe = "wireframe",
    black = "black",
    luxury = "luxury",
    dracula = "dracula",
    cmyk = "cmyk",
    autumn = "autumn",
    business = "business",
    acid = "acid",
    lemonade = "lemonade",
    night = "night",
    coffee = "coffee",
    winter = "winter",
  }
  const themes: Theme[] = Object.values(Theme);

  function changeTheme() {
    const theme = (event.target as HTMLSelectElement).value;
    document.documentElement.setAttribute("data-theme", theme);
  }
</script>

<select on:change={changeTheme} class="select select-ghost w-auto max-w-xs">
  <option disabled selected>Theme</option>
  {#each themes as theme}
    <option value={theme}>{theme}</option>
  {/each}
</select>
