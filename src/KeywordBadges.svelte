<script>
    export let keywords;
</script>

<div class="flex items-center space-x-3">
    <!-- check if value is null -->
    {#if keywords === null}
        <div class="font-bold opacity-50">---</div>
    {:else}
        <div class="flex flex-wrap space-x-1">
            {#each keywords as keyword}
                <span class="badge badge-ghost badge-sm my-1">{keyword}</span>
            {/each}
        </div>
    {/if}
</div>

<style global lang="postcss">
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
</style>
