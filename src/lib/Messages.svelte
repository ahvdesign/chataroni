<script>
  import { onMount, onDestroy } from "svelte";
  import { currentUser, pb } from "./pocketbase";
  import { identity } from "svelte/internal";

  let messages = [];

  onMount(async () => {
    const resultList = await pb.collection("messages").getList(1, 50, {
      sort: "created",
      expand: "user",
    });

    messages = resultList.items;
  });
</script>

<div class="messages">
  {#each messages as message (message.id)}
    <div class="chat chat-start">
      <div class="chat-image avatar">
        <div class="w-10 rounded-full">
          <img
            src="https://cdn2.iconfinder.com/data/icons/avatars-99/62/avatar-375-456327-512.png"
            alt="user avatar"
          />
        </div>
      </div>
      <div class="chat-header">
        Obi-Wan Kenobi
        <time class="text-xs opacity-50">12:45</time>
      </div>
      <div class="chat-bubble">You were the Chosen One!</div>
      <div class="chat-footer opacity-50">Delivered</div>
    </div>
    <div class="chat chat-end">
      <div class="chat-image avatar">
        <div class="w-10 rounded-full">
          <img
            src="https://cdn2.iconfinder.com/data/icons/avatars-99/62/avatar-375-456327-512.png"
            alt="user avatar"
          />
        </div>
      </div>
      <div class="chat-header">
        Anakin
        <time class="text-xs opacity-50">12:46</time>
      </div>
      <div class="chat-bubble">I hate you!</div>
      <div class="chat-footer opacity-50">Seen at 12:46</div>
    </div>
  {/each}
</div>
