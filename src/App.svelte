<script lang="ts">
  import ThemeSelect from "./lib/ThemeSelect.svelte";
  import Login from "./lib/Login.svelte";
  import Messages from "./lib/Messages.svelte";
</script>

<div class="navbar bg-base-100">
  <div class="flex-1">
    <a href="/" class="btn btn-ghost normal-case text-xl"> Chataroni </a>
  </div>
  <div class="flex-none">
    <ul class="menu menu-horizontal px-1">
      <ThemeSelect />
    </ul>
  </div>
</div>

<div class="hero min-h-screen bg-base-200">
  <div class="hero-content text-center">
    <div class="max-w-md">
      <h1 class="text-5xl font-bold">Chataroni</h1>
      <p class="py-6">The clean, modern, and simple communication app.</p>
      <button class="btn btn-primary">Get Started</button>
    </div>
  </div>
</div>

<Login />
<Messages />
