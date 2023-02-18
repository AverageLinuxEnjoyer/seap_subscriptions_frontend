<script>
  import LoginPanel from "./LoginPanel.svelte";
  import SubscriptionsPanel from "./SubscriptionsPanel.svelte";

  let user = {
    id: 0,
    email: "",
    created_at: "",
  };

  let page = 0;

  let onUserChange = (user) => {
    page = 1;
  };

  let onBack = () => {
    page = 0;
  };
</script>

<!-- <LoginPanel bind:user />
<SubscriptionsPanel /> -->

{#if page === 0}
  <LoginPanel bind:user {onUserChange} />
{:else if page === 1}
  <SubscriptionsPanel {onBack} {user} />
{/if}

<style global lang="postcss">
  @tailwind base;
  @tailwind components;
  @tailwind utilities;

  :global(body) {
    background: #2c333f;
  }
</style>
