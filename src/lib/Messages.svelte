<script lang="ts">
  import { onMount, onDestroy, afterUpdate, tick } from "svelte";
  import { currentUser, pb } from "./pocketbase";

  let newMessage: string;
  let messages = [];
  let unsubscribe: () => void;
  let messagesContainer: HTMLElement;

  onMount(async () => {
    // Get initial messages
    const resultList = await pb.collection("messages").getList(1, 50, {
      sort: "created",
      expand: "user",
    });
    messages = resultList.items;

    // Subscribe to realtime messages
    unsubscribe = await pb
      .collection("messages")
      .subscribe("*", async ({ action, record }) => {
        if (action === "create") {
          //Fetch associated user
          const user = await pb.collection("users").getOne(record.user);
          record.expand = { user };
          messages = [...messages, record];
        }
        if (action === "delete") {
          messages = messages.filter((message) => message.id !== record.id);
        }
      });
  });

  afterUpdate(() => {
    console.log("afterUpdate");
    scrollToBottom(messagesContainer);
  });

  $: if (messagesContainer && newMessage) {
    console.log("tick");
    scrollToBottom(messagesContainer);
  }

  const scrollToBottom = async (node) => {
    node.scroll({
      top: node.scrollHeight,
      behavior: "smooth",
    });
  };

  // Unsubscribe from realtime messages
  onDestroy(() => {
    unsubscribe?.();
  });

  async function sendMessage() {
    const data = {
      text: newMessage,
      user: $currentUser.id,
    };
    const createdMessage = await pb.collection("messages").create(data);
    newMessage = "";
  }
</script>

<div class="messages" bind:this={messagesContainer}>
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
        {message.expand?.user?.username}
        <time class="text-xs opacity-50">{message.created}</time>
      </div>
      <div class="chat-bubble">{message.text}</div>
      <!-- <div class="chat-footer opacity-50">Delivered</div> -->
    </div>
  {/each}
</div>

<form on:submit|preventDefault={sendMessage}>
  <input type="text" placeholder="Message" bind:value={newMessage} />
  <button type="submit">Send</button>
</form>
