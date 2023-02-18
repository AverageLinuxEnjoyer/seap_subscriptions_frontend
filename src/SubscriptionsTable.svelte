<script>
    import { onMount } from "svelte";
    import KeywordBadges from "./KeywordBadges.svelte";
    import TextValue from "./TextValue.svelte";

    export let page_size = 5;
    export let subscriptions;
    export let checkboxes;

    let page = 0;

    $: total_pages = subscriptions.length / page_size;

    let prev = () => {
        if (page > 0) {
            page -= 1;
        }
    };

    let next = () => {
        if (page < total_pages - 1) {
            page += 1;
        }
    };
</script>

<div class="overflow-x-auto w-full">
    <table class="table w-full">
        <!-- head -->
        <thead>
            <tr>
                <th>
                    <label>
                        <input type="checkbox" class="checkbox" />
                    </label>
                </th>
                <th>Min price</th>
                <th>Max price</th>
                <th>Title keywords</th>
                <th>Description keywords</th>
                <th>Additional info keywords</th>
                <th />
            </tr>
        </thead>
        <tbody>
            {#each subscriptions.slice(page * page_size, (page + 1) * page_size) as subscription}
                <tr>
                    <th>
                        <label>
                            <input
                                type="checkbox"
                                class="checkbox"
                                bind:checked={checkboxes[
                                    subscriptions.indexOf(subscription)
                                ]}
                            />
                        </label>
                    </th>
                    <td><TextValue value={subscription.min_price} /></td>
                    <td><TextValue value={subscription.max_price} /></td>
                    <td
                        ><KeywordBadges
                            keywords={subscription.title_keywords}
                        /></td
                    >
                    <td
                        ><KeywordBadges
                            keywords={subscription.desc_keywords}
                        /></td
                    >
                    <td
                        ><KeywordBadges
                            keywords={subscription.additional_info_keywords}
                        /></td
                    >
                </tr>{/each}
        </tbody>
    </table>
</div>
<div class="btn-group justify-center">
    <button class="btn" on:click={prev}>Prev</button>
    <button class="btn">Page {page + 1} of {Math.ceil(total_pages)}</button>
    <button class="btn" on:click={next}>Next</button>
</div>

<style global lang="postcss">
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
</style>
