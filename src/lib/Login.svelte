<script lang="ts">
  import { currentUser, pb } from "./pocketbase";

  let username: string;
  let password: string;

  async function login() {
    const user = await pb
      .collection("users")
      .authWithPassword(username, password);
    console.log(user);
  }

  async function signUp() {
    try {
      const data = {
        username,
        password,
        passwordConfirm: password,
        name: "bloop blap",
      };
      const createdUser = await pb.collection("users").create(data);
      await login();
    } catch (error) {
      console.log(error);
    }
  }

  function signOut() {
    pb.authStore.clear();
  }
</script>

{#if $currentUser}
  <p>
    Signed in as {$currentUser.username}
    <button on:click={signOut}>Sign Out</button>
  </p>
{:else}
  <form on:submit|preventDefault class="container mx-auto my-12 flex flex-col">
    <input
      class="input input-bordered w-full max-w-xs mx-auto mb-3"
      placeholder="Username"
      type="text"
      bind:value={username}
    />
    <input
      class="input input-bordered w-full max-w-xs mx-auto mb-3"
      placeholder="Password"
      type="password"
      bind:value={password}
    />

    <button
      on:click={signUp}
      class="mx-auto mb-3 btn btn-primary w-full max-w-xs">Sign Up</button
    >
    <button
      on:click={login}
      class="mx-auto mb-3 btn btn-secondary w-full max-w-xs">Login</button
    >
  </form>
{/if}
