<script>
    import Scroller from "@sveltejs/svelte-scroller";
    import Map from "./Map.svelte";
    import Sticker from "./Sticker.svelte";
   

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
            showStreet: false,
            showEastHarlem: false,
            showSticker: false
        },
        {
            class: "visible",
            content: "<p>Our story begins at the <b>125 St Subway Station</b>.</p>",
            mapState: {
                center: [-73.9656, 40.7826],
                zoom: 10,
            },
            showMarker: false,
            showStreet: false,
            showEastHarlem: false,
            showSticker: false
        },
        {
            class: "visible",
            content: "<p> Historically significant, this subway station sits right where Lexington Avenue meets what’s now officially co-named <b>Dr Martin Luther King Jr Boulevard</b>.</p>",
            mapState: {
                center: [-73.9373, 40.8044],
                zoom: 14,
            },
            showMarker: true,
            showStreet: false,
            showEastHarlem: false,
            showSticker: false
        },
        {
            class: "visible",
            content: "<p> Most folks still call it 125th Street, but back in 1984 the community fought to add Dr. King’s name to the boulevard.  The name change wasn’t just about honoring Dr. King and his contributions to civil rights and social justice, though -  It was also about showing respect more broadly for Black political struggle and activism in the surrounding New York City neighborhoods.</p>",
            mapState: {
                center: [-73.9373, 40.8044],
            },
            showMarker: true,
            showStreet: true,
            showEastHarlem: false,
            showSticker: false
        },

        {
            class: "visible",
            content: "<p>Take <b>East Harlem</b> for instance. This neighborhood knows all about standing up for what’s right. Traditionally home to Black, Puerto Rican, and immigrant families, East Harlem has never been afraid to speak up. Back in the ‘60s and ‘70s, groups like the Black Panthers Party and the Young Lords Party got together right here to fight for better healthcare, better schools, and better housing.</p>",
            mapState: {
                center: [-73.9373, 40.8044],
                zoom: 13
            },
            showMarker: false,
            showStreet: false,
            showEastHarlem: true,
            showSticker: false
        },

        {
            class: "visible",
            content: "<p> Today, East Harlem is still mostly Black, Puerto Rican, and immigrant families. Local groups are still fighting for the community’s rights. But things are starting to look different around here.</p>",
            mapState: {
                center: [-73.9373, 40.8044],
                zoom: 13
            },
            showMarker: false,
            showStreet: false,
            showEastHarlem: true,
            showSticker: false
        },

        {
            class: "visible",
            content: "<p>Don’t just take our word for it, take it from <b>Tihanna</b>.</p>",
            mapState: {
                center: [-73.9373, 40.8044],
                zoom: 14
            },
            showMarker: false,
            showStreet: false,
            showEastHarlem: true,
            showSticker: false
        },
        {
            class: "hidden",
            content: "<p> </p>",
            mapState: {
                center: [-73.9373, 40.8044],
                zoom: 14
            },
            showMarker: false,
            showStreet: false,
            showEastHarlem: true,
            showSticker: true,
            artist: {
                name: "TIHANNA",
                artworkSrc: "/images/1.jpg",
                audioSrc: "/audio/1.mp3",
            }
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
    $: showEastHarlem = index !== undefined ? steps[index].showEastHarlem : false;
    $: showSticker = index !== undefined ? steps[index].showSticker : false;
    $: artist = index !== undefined ? steps[index].artist : null;

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
            <Map {show} {currentState} {showMarker} {showStreet} {showEastHarlem} />
        </div>

        <div slot="foreground">
            <!-- Text content on the left -->
             <div class = "text-column">
                {#each steps as step, i}
                    <section class={step.class}>
                    <div class="text-block">{@html step.content}</div>
                </section>
                {/each}
            </div>

            <!-- Sticker overlay in the center-->
            {#if showSticker && artist}
                <div class="sticker-overlay">
                    <Sticker
                        artist={artist.name}
                        artworkSrc={artist.artworkSrc}
                        audioSrc={artist.audioSrc}
                    />
                </div>
            {/if}
        </div>
    </Scroller>
</div>

<style>

    .text-column {
        width: 50%;
        padding-left: 50%;
    }
    
    .sticker-overlay {
        position: fixed;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        z-index: 1000;
        /* make sure it is clickable */
        pointer-events: all;
    }
    .text-block {
        width: 100%;
        border: 1px solid rgb(242, 242, 233);
        background-color: black;
        padding: 10px;
    }

    .demo {
        padding: 0;
    }

    [slot="background"] {
        background-color: rgba(255, 62, 0, 0.05);
        border-top: 2px solid white;
        border-bottom: 2px solid white;
        font-size: 1.4em;
        overflow: hidden;
    }

    section {
        height: 100vh;
        color: white;
        padding: 1em;
        margin: 0 0 2em 0;
        display: flex;
        justify-content: center;
        align-items: center;
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
        /* // background-color: black; */
        color: white;
        padding: 1em;
        margin: 0 0 2em 0;
        display: flex;
        justify-content: center;
        align-items: center;
    }
</style>