<script>
    export let onCreate;
    export let idUser;

    export let preset_subscription = null;

    let min_price = null;
    let max_price = null;
    let title_keywords = null;
    let desc_keywords = null;
    let additional_info_keywords = null;

    let onChange = (preset_subscription) => {
        if (preset_subscription !== null) {
            min_price = preset_subscription.min_price;
            max_price = preset_subscription.max_price;
            title_keywords = preset_subscription.title_keywords;
            desc_keywords = preset_subscription.desc_keywords;
            additional_info_keywords =
                preset_subscription.additional_info_keywords;
        } else {
            min_price = null;
            max_price = null;
            title_keywords = null;
            desc_keywords = null;
            additional_info_keywords = null;
        }
    };

    $: onChange(preset_subscription);

    let sanitize_keywords = (keywords) => {
        if (Array.isArray(keywords)) {
            keywords = keywords.join(", ");
        }

        if (keywords !== null) {
            keywords = keywords
                .split(",")
                .map((x) => x.trim())
                .filter((x) => x !== "")
                .filter((x) => x !== "null");
        }
        return keywords;
    };

    let createSubscription = async (subscription) =>
        await fetch(
            "https://seap-subscription-api.shuttleapp.rs/subscriptions",
            {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(subscription),
            }
        );

    let updateSubscription = async (subscription) =>
        await fetch(
            `https://seap-subscription-api.shuttleapp.rs/subscriptions/${subscription.id}`,
            {
                method: "PUT",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(subscription),
            }
        );

    let executeSubscription = async () => {
        title_keywords = sanitize_keywords(title_keywords);
        desc_keywords = sanitize_keywords(desc_keywords);
        additional_info_keywords = sanitize_keywords(additional_info_keywords);

        let subscription = {
            id: 0,
            id_user: idUser,
            min_price: min_price,
            max_price: max_price,
            title_keywords: title_keywords,
            desc_keywords: desc_keywords,
            additional_info_keywords: additional_info_keywords,
        };

        let response = null;

        if (preset_subscription === null) {
            response = await createSubscription(subscription);
            console.log("Creating subscription...");
        } else {
            subscription.id = preset_subscription.id;
            response = await updateSubscription(subscription);
            console.log("Updating subscription...");
        }

        let data = await response.json();

        if (data.Error) {
            console.log(data.Error);
        } else {
            onCreate();
        }
    };
</script>

<input type="checkbox" id="my-modal" class="modal-toggle" />
<div class="modal">
    <div class="modal-box gap-3 flex flex-col justify-left w-11/12 max-w-5xl">
        <h3 class="font-bold text-lg mb-4">Create a new subscription</h3>
        <div class="flex flex-row gap-3">
            <input
                type="number"
                placeholder="minimum price"
                class="input input-bordered inline-block"
                bind:value={min_price}
            />
            <input
                type="number"
                placeholder="maximum price"
                class="input input-bordered inline-block"
                bind:value={max_price}
            />
        </div>
        <input
            type="text"
            placeholder="title keywords (comma separated)"
            class="input input-bordered inline-block"
            bind:value={title_keywords}
        />
        <input
            type="text"
            placeholder="description keywords (comma separated)"
            class="input input-bordered inline-block"
            bind:value={desc_keywords}
        />
        <input
            type="text"
            placeholder="additional info keywords (comma separated)"
            class="input input-bordered inline-block"
            bind:value={additional_info_keywords}
        />
        <div class="modal-action">
            <label for="my-modal" class="btn">Cancel</label>
            <label
                for="my-modal"
                class="btn btn-primary"
                on:click={executeSubscription}
                >{preset_subscription === null ? "Create" : "Update"}</label
            >
        </div>
    </div>
</div>

<style global lang="postcss">
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
</style>
