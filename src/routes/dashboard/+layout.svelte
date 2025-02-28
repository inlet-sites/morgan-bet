<script>
    import "$lib/global.css";
    import {setContext} from "svelte";
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

    setContext("notify", notify);
    setContext("loader", setLoader);
</script>

{#if loader}
    <Loader/>
{/if}

{#if notifier}
    <Notifier type={notifier.type} message={notifier.message}/>
{/if}

<Header/>

{@render children()}
