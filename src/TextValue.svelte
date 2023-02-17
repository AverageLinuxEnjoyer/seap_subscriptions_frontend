<script>
    export let value;
</script>

<div class="flex items-center space-x-3">
    <!-- check if value is null -->
    {#if value === null}
        <div class="font-bold opacity-50">---</div>
    {:else}
        <div class="font-bold">{value}</div>
    {/if}
</div>

<style global lang="postcss">
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
</style>
