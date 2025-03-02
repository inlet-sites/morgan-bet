<script>
    import {onMount, getContext} from "svelte";
    import Game from "./Game.svelte";

    const userToken = getContext("userToken");
    const loader = getContext("loader");
    const notify = getContext("notify");
    let games = $state([]);

    onMount(()=>{
        loader(true);
        fetch(`${import.meta.env.VITE_APIURL}/rankinggame`, {
            method: "get",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${localStorage.getItem("userToken")}`
            }
        })
            .then(r=>r.json())
            .then((response)=>{
                if(response.error){
                    notify("error", response.error.message);
                }else{
                    games = response;
                }
            })
            .catch((err)=>{
                notify("error", "Something went wrong, try refreshing the page");
            })
            .finally(()=>{
                loader(false);
            });
    });
</script>

<div class="Dashboard">
    <h1>My Games</h1>

    {#each games as game}
        <Game game={game}/>
    {/each}
</div>

<style>
    .Dashboard{
        padding: 35px;
    }

    h1{
        text-decoration: underline;
    }

    @media screen and (max-width: 500px){
        .Dashboard{
            padding: 15px;
        }
    }
</style>
