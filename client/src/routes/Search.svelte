<script>
    import PocketBase from "pocketbase";
    import MiniSearch from "minisearch";
    import { Link } from "svelte-routing";

    const pb = new PocketBase("http://127.0.0.1:8090");
    const notecards = pb.collection("notecard").getFullList({
        filter: "verified = true && author.verified = true",
    });

    var idx;

    function createSearch(notecards) {
        let documents = [];
        for (let i = 0; i < notecards.length; i++) {
            documents.push({
                id: notecards[i].id,
                text: notecards[i].name,
            })
        }

        idx = new MiniSearch({
            fields: ["text"],
            storeFields: ["id", "text"],
        });

        idx.addAll(documents);
    }

    function handleSearch() {
        results = [];
        let resultsSearch = idx.search(searchQuery, { prefix: true, fuzzy: 0.2 });
        for (let i = 0; i < resultsSearch.length; i++) {
            pb.collection("notecard").getOne(resultsSearch[i].id).then((res) => {
                results.push(res);
                results = results
            });
        }
    }

    let searchQuery = "";
    let results = [];
</script>

{#await notecards}
    <p>Loading Notecards...</p>
{:then res}
    {void createSearch(res) ?? ""}
    <main class="text-center grid gap-1 place-content-center p-5">
        <input bind:value={searchQuery} class="text-black rounded-lg text-2xl text-center" />
        <button on:click={() => handleSearch()} class="text-black bg-white p-1 rounded-lg text-lg">Search!</button>
        <br />
        <div class="p-5 flex gap-x-10 gap-y-10 flex-wrap">
            {#each {length: results.length} as _, i}
                <Link to={`/notecards/${results[i].id}`} class="bg-white text-black text-xl text-center items-center justify-center rounded-lg p-3 min-w-[100px]">
                    {results[i].name}
                </Link>
            {/each}
        </div>
    </main>
{:catch error}
    {console.log(error)}
{/await}