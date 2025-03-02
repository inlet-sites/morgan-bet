<script>
    import {getContext} from "svelte";

    const loader = getContext("loader");
    const notify = getContext("notify");
    let {requests, gameId, accept, close} = $props();

    const submitAccept = (user)=>{
        loader(true);
        fetch(`${import.meta.env.VITE_APIURL}/rankinggame/${gameId}/accept`, {
            method: "put",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${localStorage.getItem("userToken")}`
            },
            body: JSON.stringify({user: user._id})
        })
            .then(r=>r.json())
            .then((response)=>{
                if(response.error){
                    notify("error", response.error.message);
                }else{
                    accept(user);
                    for(let i = 0; i < requests.length; i++){
                        if(requests[i] === user){
                            requests.splice(i, 1);
                            break;
                        }
                    }
                }
            })
            .catch((err)=>{
                notify("error", "Something went wrong, try refreshing the page");
            })
            .finally(()=>{
                loader(false);
            });
    }
</script>

<div class="ViewRequests">
    {#if !requests}
        <h1>No current requests</h1>
    {:else}
        <h1>Player Requests</h1>
    {/if}

    {#each requests as user}
        <div class="user">
            <p>{user.name}</p>

            <button class="button" onclick={()=>{submitAccept(user)}}>Accept</button>
        </div>
    {/each}

    <button class="button" onclick={close}>Done</button>
</div>

<style>
    .user{
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 75px;
        width: 100%;
        max-width: 750px;
        background: var(--prussianBlue);
        color: var(--platinum);
        padding: 0 35px;
        margin: 15px 0;
    }
</style>
