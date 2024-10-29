<script>
    import Scroller from "@sveltejs/svelte-scroller";
    import Map from "./Map.svelte";
   

    let count;
    let index;
    let offset;
    let progress;
    let top = 0.01;
    let threshold = 0.5;
    let bottom = 0.9;

    const steps = [
        {
            class: "hidden",
            content: `p>intro</p>`,
            mapState: {
                center: [-73.9656, 40.7826],
                zoom: 10,
            },
            showMarker: false,
            showStreet: false
        },
        {
            class: "visible",
            content: "<p>Our story begins at the 125 St Subway Station, located at the interesection of Lexington Avenue and what is now officially co-named Dr Martin Luther King Jr Boulevard.</p>",
            mapState: {
                center: [-73.9656, 40.7826],
                zoom: 10,
            },
            showMarker: false,
            showStreet: false
        },
        {
            class: "visible",
            content: "<p>Formerly known as 125th Street, this vibrant crosstown street was con-named in 1984 to Dr Martin Luther King Jr Boulevard in honor of the civil rights leader’s legacy. The community initiative to rename this main street was not just fueled by a desire to recognize Dr. King’s contributions to civil rights and social justice, but it also served as a means to visibly honor Black political struggle and activism in the Harlem neighborhood.</p>",
            mapState: {
                center: [-73.9373, 40.8044],
                zoom: 14,
            },
            showMarker: true,
            showStreet: true   
        },
        {
            class: "visible",
            content: "<p>More text More text...</p>",
            mapState: {
                center: [-73.9373, 40.8044],
            },
            showMarker: true,
            showStreet: false
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

    $: console.log("index: ", index);
    $: show = index === 0 ? "manhattan" : "harlem";
    $: currentState = index !== undefined ? steps[index].mapState : null;
    $: showMarker = index !== undefined ? steps[index].showMarker : false;
    $: showStreet = index !== undefined ? steps[index].showStreet : false;


    // $: console.log("currentState: ", currentState);
    // $: console.log("showMarker: ", showMarker);
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
            <Map {show} {currentState} {showMarker} {showStreet} />
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
