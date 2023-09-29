<script>
    import PocketBase from "pocketbase";

    const pb = new PocketBase("http://127.0.0.1:8090");

    let username = "";
    let password = "";
    let error;
    let success;

    function login() {
        error = undefined;
        success = undefined;

        pb.collection("users").authWithPassword(
            username,
            password,
        ).then(() => {
            success = "Successfully Logged In!";
            let cookie = pb.authStore.exportToCookie();
            localStorage.setItem("token", cookie);
        }).catch((e) => {
            console.log(e);
            error = e;
        });
    }
</script>

<main class="p-5 text-center grid gap-1 place-content-center">
    {#if error}
        <p class="text-red-600">Failed to login</p>
    {:else if success}
        <p class="text-green-600">{success}</p>
    {/if}

    <div class="bg-white text-black w-max text-lg rounded-lg text-center grid gap-1">
        <label for="username">Username:</label>
        <input id="username" bind:value={username} class="text-black rounded-lg text-2xl text-center focus:outline-none" />
        <br />
        <label for="password">Password:</label>
        <input id="password" bind:value={password} class="text-black rounded-lg text-2xl text-center focus:outline-none" type="password" />
        <br />
        <button on:click={() => login()} class="text-black bg-white p-1 rounded-lg text-lg">Login</button>
    </div>
</main>
