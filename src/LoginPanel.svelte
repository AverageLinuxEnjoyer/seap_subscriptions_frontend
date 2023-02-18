<script>
    export let user;
    export let onUserChange;

    let email_input = "";
    let error = "";
    function handleClick() {
        fetch(
            "https://seap-subscription-api.shuttleapp.rs/users?or_return=true",
            {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({ email: email_input }),
            }
        )
            .then((res) => res.json())
            .then((data) => {
                if (data.Error) {
                    error = data.Error;
                } else {
                    error = "";
                    user.id = data.id;
                    user.email = data.email;
                    user.created_at = data.created_at;

                    onUserChange();
                }
            });
    }
</script>

<div class="card w-96 bg-base-100 shadow-xl border-2 border-gray-700">
    <!-- use tailwind css to  -->
    <div class="card-body">
        <h2 class="card-title">What's your email?</h2>
        <input
            bind:value={email_input}
            type="text"
            placeholder="Type here"
            class="my-2.5 input input-bordered input-primary w-full max-w-xs"
        />
        <div class="card-actions justify-left">
            <button on:click={handleClick} class="btn btn-primary">Click</button
            >
        </div>
        <!-- red colored h5 below the button, aligned to the left -->
        <h5 class="text-red-500 text-left">{error}</h5>
    </div>
</div>

<style global lang="postcss">
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
</style>
