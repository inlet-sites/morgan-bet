<script>
    import "$lib/global.css";
    import {onMount, setContext} from "svelte";
    import {goto} from "$app/navigation";
    import {writable} from "svelte/store";
    import Loader from "$lib/Loader.svelte";
    import Notifier from "$lib/Notifier.svelte";
    import Header from "./Header.svelte";

    let {children} = $props();
    let loader = $state(false);
    let notifier = $state(null);

    const setLoader = (on)=>{
        loader = on;
    }

    const notify = (type, message)=>{
        notifier = {
            type: type,
            message: message
        };

        setTimeout(()=>{
            notifier = null;
        }, 7500);
    }

    setLoader(true);

    const user = writable();
    setContext("user", user);
    setContext("notify", notify);
    setContext("loader", setLoader);

    onMount(()=>{
        const userToken = localStorage.getItem("userToken");
        if(!userToken) goto("/");

        fetch(`${import.meta.env.VITE_APIURL}/user`, {
            method: "get",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${userToken}`
            }
        })
            .then(r=>r.json())
            .then((response)=>{
                if(response.error){
                    localStorage.removeItem("userToken");
                    goto("/");
                }else{
                    user.set(response);
                }
            })
            .catch((err)=>{
                localStorage.removeItem("userToken");
                goto("/");
            })
            .finally(()=>{
                setLoader(false);
            });
    });
</script>

{#if loader}
    <Loader/>
{/if}

{#if notifier}
    <Notifier type={notifier.type} message={notifier.message}/>
{/if}

<Header/>

{@render children()}
