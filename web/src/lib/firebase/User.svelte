<script>
  export let persist = null;
  import { onMount, onDestroy, createEventDispatcher } from "svelte";
  import { userStore } from "./auth";

  let store = userStore({ persist });

  const dispatch = createEventDispatcher();
  let unsub;
  onMount(() => {
    unsub = store.subscribe((user) => {
      dispatch("user", {
        user,
      });
    });
  });

  onDestroy(() => unsub());
</script>

<slot name="before" />
{#if $store}
  <slot user={$store} auth={store.auth} />
{:else}
  <slot name="signed-out" />
{/if}
<slot name="after" />
