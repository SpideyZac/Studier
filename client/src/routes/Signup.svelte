<script>
    import PocketBase from "pocketbase";

    const pb = new PocketBase("http://127.0.0.1:8090");

    let username = "";
    let password = "";
    let passwordConfirm = "";
    let error;
    let success;

    function signup() {
        error = undefined;
        success = undefined;
        pb.collection("users").create({
            username,
            password,
            passwordConfirm,
        }).then(() => {
            success = "Successfully Created Account!";
        }).catch((e) => {
            console.log(e);
            error = e;
        });
    }
</script>

<main class="p-5 text-center grid gap-1 place-content-center">
    {#if error}
        <p class="text-red-600">Failed (Username taken? Password less then 8 characters? Passwords do not match?)</p>
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
        <label for="passwordConfirm">Password Confirm:</label>
        <input id="passwordConfirm" bind:value={passwordConfirm} class="text-black rounded-lg text-2xl text-center focus:outline-none" type="password" />
        <br />
        <button on:click={() => signup()} class="text-black bg-white p-1 rounded-lg text-lg">Signup</button>
    </div>
</main>
