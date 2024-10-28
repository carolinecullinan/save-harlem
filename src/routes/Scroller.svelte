<script>
    import Scroller from "@sveltejs/svelte-scroller";
    // import Map from "./Map.svelte";
   

    let count;
    let index;
    let offset;
    let progress;
    let top = 0.01;
    let threshold = 0.5;
    let bottom = 0.9;
    let marker; //marker for 125th street station

    const steps = [
        {
            class: "hidden",
            content: `
        <p>intro</p>
      `,
            mapState: {
                center: [-73.9656, 40.7826],
                zoom: 10,
            },
        },
        {
            class: "visible",
            content: "<p>Exploring the changes around 125th Street...</p>",
            mapState: {
                center: [-73.9656, 40.7826],
                //-74.006, 40.7128
                zoom: 10,
            },
        },
        {
            class: "visible",
            content: "<p>Focusing on East Harlem...</p>",
            mapState: {
                center: [-73.9373, 40.8044],
                zoom: 14,
            },
            
        },
        // {
        //     class: "visible",
        //     content:
        //         "<p>The 125th Street Station area has seen significant changes...</p>",
        //     mapState: {
        //         center: [-73.9369, 40.8044],
        //         zoom: 16,
        //     },
            
        //},
    ];

    $: console.log("testing");
    $: show = index === 0 ? "manhattan" : "harlem";
    $: currentState = index !== undefined ? steps[index].mapState : null;

    // $: console.log(currentState);
</script>

<div class="demo">
    <Scroller
        {top}
        {threshold}
        {bottom}
        bind:count
        bind:index
        bind:offset
        bind:progress
    >
        <div slot="background">
            <!-- <Map {show} {currentState}/> -->
        </div>

        <div slot="foreground" style="padding: 0 0 0 50%;">
            {#each steps as step, i}
                <section class={step.class}>
                    <div class="text-block">{@html step.content}</div>
                </section>
            {/each}
        </div>
    </Scroller>
</div>

<style>
    .text-block {
        width: 100%;
        border: 1px solid rgb(242, 242, 233);
        padding: 10px;
    }

    .hidden {
        visibility: hidden;
    }

    .visible {
        visibility: visible;
    }

    .demo {
        padding: 0 0px 0 0px;
    }

    [slot="background"] {
        background-color: rgba(255, 62, 0, 0.05);
        border-top: 2px solid white;
        border-bottom: 2px solid white;
        font-size: 1.4em;
        overflow: hidden;
        /* padding: 1em; */
    }

    [slot="background"] p {
        margin: 0;
    }

    [slot="foreground"] {
        pointer-events: none;
    }

    [slot="foreground"] section {
        pointer-events: all;
    }

    section {
        height: 100vh;
        /* background-color: rgba(0, 0, 0, 0.5); */
        color: white;
        padding: 1em;
        margin: 0 0 2em 0;
        display: flex;
        justify-content: center;
        align-items: center;
    }
</style>
