<script>
    import { onMount } from "svelte";
    import CreateModal from "./CreateModal.svelte";
    import SubscriptionsTable from "./SubscriptionsTable.svelte";

    export let user;
    export let onBack;

    let subscriptions = [];
    let checkboxes = [];
    let selected_subscriptions = [];

    let preset_subscription = null;

    $: if (selected_subscriptions.length === 1) {
        preset_subscription = subscriptions[selected_subscriptions[0]];
    } else {
        preset_subscription = null;
    }

    let update_table = async () => {
        const res = await fetch(
            `https://seap-subscription-api.shuttleapp.rs/subscriptions?email=${user.email}`,
            {
                method: "GET",
            }
        );
        subscriptions = await res.json();
        subscriptions.sort((a, b) => a.id - b.id);
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

<div class="card w-auto bg-base-100 shadow-xl my-8 border-2 border-gray-700">
    <div class="card-actions justify-left">
        <label for="my-modal" class="btn btn-primary"
            >{preset_subscription === null ? "Create" : "Update"}</label
        >
        <button
            class="btn btn-primary"
            disabled={selected_subscriptions.length !== 1}>See more</button
        >
        <button
            class="btn btn-primary"
            on:click={onDelete}
            disabled={selected_subscriptions.length === 0}>Delete</button
        >
        <button class="btn btn-primary" on:click={onBack}>Back</button>
    </div>
</div>
<div class="card w-auto bg-base-100 shadow-xl my-5 border-2 border-gray-700">
    <SubscriptionsTable
        page_size={5}
        bind:selected_subscriptions
        bind:subscriptions
        bind:checkboxes
    />
</div>

<CreateModal
    onCreate={update_table}
    idUser={user.id}
    bind:preset_subscription
/>

<style global lang="postcss">
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
</style>
