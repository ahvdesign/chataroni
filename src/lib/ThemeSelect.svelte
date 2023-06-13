<script lang="ts">
  import { onMount } from "svelte";

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

  function handleMount() {
    const storedTheme = localStorage.getItem("theme");
    const defaultTheme =
      storedTheme && themes.includes(storedTheme as Theme)
        ? storedTheme
        : "dark";
    document.documentElement.setAttribute("data-theme", defaultTheme);
  }

  function changeTheme(event: Event) {
    const theme = (event.target as HTMLSelectElement).value;
    document.documentElement.setAttribute("data-theme", theme);
    localStorage.setItem("theme", theme);
  }

  onMount(handleMount);
</script>

<select on:change={changeTheme} class="select select-ghost w-auto max-w-xs">
  <option disabled selected>Theme</option>
  {#each themes as theme}
    <option value={theme}>{theme}</option>
  {/each}
</select>
