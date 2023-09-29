<script>
    export let notecardID;

    import PocketBase from "pocketbase";

    const pb = new PocketBase("http://127.0.0.1:8090");
    const notecard = pb.collection("notecard").getOne(notecardID);

    var author = new Promise(() => {});

    function findAuthor(notecard) {
        author = pb.collection("users").getOne(notecard.author);
    }
</script>

{#await notecard}
    <p>Loading Notecard...</p>
{:then res}
    {void findAuthor(res) ?? ""}
    {#await author}
        <p>Loading Author...</p>
    {:then foundAuthor}
        <main class="p-5 text-center grid gap-1 place-content-center">
            <div class="justify-center">
                <h1 class="text-4xl">{res.name}</h1>
                <img src={pb.files.getUrl(res, res.card)} alt="notecard" />
                <p class="text-xl">Author: {foundAuthor.username}</p>
            </div>
        </main>
    {:catch error}
        {console.log(error)}
    {/await}
{:catch error}
    {console.log(error)}
{/await}
