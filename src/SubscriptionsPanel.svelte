<script>
    import { onMount } from "svelte";
    import CreateModal from "./CreateModal.svelte";
    import SubscriptionsTable from "./SubscriptionsTable.svelte";

    export let user;
    export let onBack;

    let subscriptions = [];
    let checkboxes = [];
    let selected_subscriptions = [];

    let update_table = async () => {
        const res = await fetch(
            `https://seap-subscription-api.shuttleapp.rs/subscriptions?email=${user.email}`,
            {
                method: "GET",
            }
        );
        subscriptions = await res.json();
        checkboxes = new Array(subscriptions.length).fill(false);
    };

    $: selected_subscriptions = checkboxes
        .map((checkbox, index) => {
            if (checkbox) {
                return index;
            }
        })
        .filter((index) => index !== undefined);

    onMount(async () => {
        await update_table();
    });

    $: console.log("Selected subscriptions:", selected_subscriptions);

    let onDelete = async () => {
        for (let i = 0; i < selected_subscriptions.length; i++) {
            const res = await fetch(
                `https://seap-subscription-api.shuttleapp.rs/subscriptions/${
                    subscriptions[selected_subscriptions[i]].id
                }`,
                {
                    method: "DELETE",
                }
            );
            const data = await res.json();
        }

        await update_table();
        checkboxes = new Array(subscriptions.length).fill(false);
    };
</script>

<div class="card w-auto bg-base-100 shadow-xl">
    <div class="card-actions justify-left">
        <label for="my-modal" class="btn btn-primary">Create</label>
        <button
            class="btn btn-primary"
            disabled={selected_subscriptions.length !== 1}>See more</button
        >
        <button
            class="btn btn-primary"
            disabled={selected_subscriptions.length !== 1}>Update</button
        >
        <button
            class="btn btn-primary"
            on:click={onDelete}
            disabled={selected_subscriptions.length === 0}>Delete</button
        >
        <button class="btn btn-primary" on:click={onBack}>Back</button>
    </div>
</div>
<div class="card w-auto bg-base-100 shadow-xl my-5">
    <SubscriptionsTable
        page_size={5}
        bind:selected_subscriptions
        bind:subscriptions
        bind:checkboxes
    />
</div>

<CreateModal onCreate={update_table} idUser={user.id} />

<style global lang="postcss">
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
</style>
