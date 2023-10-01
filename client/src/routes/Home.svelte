<script>
    import PocketBase from "pocketbase";
    import { Link } from "svelte-routing";

    const pb = new PocketBase("http://127.0.0.1:8090");
    const notecards = pb.collection("notecard").getFullList({
        filter: "verified = true && author.verified = true",
    });
</script>

{#await notecards}
    <p>Loading Notecards...</p>
{:then res}
    <main class="p-5">
        <h1 class="text-4xl">Notecards:</h1> 
        <br />
        <div class="flex gap-x-10 gap-y-10 flex-wrap">
            {#each {length: res.length} as _, i}
                <Link to={`/notecards/${res[i].id}`} class="bg-white text-black text-xl text-center items-center justify-center rounded-lg p-3 min-w-[100px]">
                    {res[i].name}
                </Link>
            {/each}
        </div>
    </main>
{:catch error}
    {console.log(error)}
{/await}
