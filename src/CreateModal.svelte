<script>
    export let onCreate;
    export let idUser;

    let min_price = null;
    let max_price = null;
    let title_keywords = null;
    let desc_keywords = null;
    let additional_info_keywords = null;

    let sanitize_keywords = (keywords) => {
        if (keywords !== null) {
            if (keywords === "") {
                keywords = null;
            } else {
                keywords = keywords
                    .split(",")
                    .map((x) => x.trim())
                    .filter((x) => x !== "");
            }
        }
        return keywords;
    };

    let createSubscription = async () => {
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

        let response = await fetch(
            "https://seap-subscription-api.shuttleapp.rs/subscriptions",
            {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(subscription),
            }
        );

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
                on:click={createSubscription}>Create</label
            >
        </div>
    </div>
</div>

<style global lang="postcss">
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
</style>
