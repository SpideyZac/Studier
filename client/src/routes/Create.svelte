<script>
    import PocketBase from "pocketbase";

    const pb = new PocketBase("http://127.0.0.1:8090");
    let error;
    let success;
    let errorCreate;
    pb.authStore.loadFromCookie(localStorage.getItem("token"));
    if (!pb.authStore.isValid) error = "You must be logged in!";

    let name;
    let files;

    function create() {
        success = undefined;
        errorCreate = undefined;

        let formData = new FormData();
        if (files) {
            formData.append("card", files[0]);
            formData.append("author", pb.authStore.model.id);
            formData.append("name", name);

            pb.collection("notecard").create(formData).then(() => {
                success = "1";
            }).catch((e) => {
                console.log(e);
                errorCreate = e;
            });
        } else {
            errorCreate = "1"
        }
    }
</script>

<main class="p-5 text-center grid gap-1 place-content-center">
    {#if error}
        <p class="text-red-500">{error}</p>
    {:else}
        {#if errorCreate}
            <p class="text-red-500">Failed!</p>
        {:else if success}
            <p class="text-green-500">Successfully Created Notecard!</p>
        {/if}

        <div class="bg-white text-black w-max text-lg rounded-lg text-center grid gap-1 max-w-[500px] items-center justify-center">
            <label for="name">Card Name:</label>
            <input id="name" bind:value={name} class="text-black rounded-lg text-2xl text-center focus:outline-none" />
            <br />
            <label for="card">Card Image:
                {#if !files}
                    <h1>Click to upload a file</h1>
                {:else}
                    <h1>Uploaded!</h1>
                {/if}
                <input type="file" accept=".png,.jpg,.webp" id="card" bind:files class="text-black rounded-lg focus:outline-none bg-white file:bg-white file:border-none" hidden />
            </label>
            <br />
            <button on:click={() => create()} class="text-black bg-white p-1 rounded-lg text-lg">Create</button>
        </div>
    {/if}
</main>