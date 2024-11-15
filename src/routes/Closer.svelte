<script>


</script>

<main class="closer">
    <div class="closer-content-wrapper">
        <div class="title-blocks">
            <span class="block-line">
                {#each "BLOCK" as char}
                    <div class="block light">{char}</div>
                {/each}
            </span>
            <span class="block-line">
                {#each "BY" as char}
                    <div class="block light">{char === " " ? "\u00A0" : char}</div>
                {/each}
            </span>
            <span class="block-line">
                {#each "BLOCK" as char}
                    <div class="block light">{char}</div>
                {/each}
            </span>
            <h1 class="subtitle">Young Voices on a Changing East Harlem</h1>
        </div>
    <!-- <h1><br>Block by Block<br> Young Voices on a Changing East Harlem</h1> -->
    <h2>A Community Conversation</h2>
    <h3> by <a href="https://www.theconfinedarts.org/" target="_blank"><b>The Confined Arts</b>
    </a> at <a href="https://anotherchoicenyc.org/" target="_blank"><b>Another Choice Youth and Family Outreach</b></a></h3>

    <p> Young people in East Harlem understand exactly what’s happening to their neighborhood. Maybe it’s time that we listen to them.
        
        </p>

        <div class="scroll-arrow">
            <svg width="40" height="20" viewBox="0 0 40 20">
                <path d="M20 20 L40 0 L0 0 Z" fill="#d2d1d1"/>
            </svg>
        </div>
    </div>
</main>

<style>
    .closer-content-wrapper {
        
        max-width: 640px;
        margin: 0 auto;
        padding: 80px 50px 0 50px;
    }

    .title-blocks {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 0.5rem;
        margin-bottom: 2rem;
    }

    .block-line {
        display: flex;
        gap: 4px;
    }

    .block {
        width: 3.5rem;
        height: 3.5rem;
        display: flex;
        align-items: center;
        justify-content: center;
        font-family: 'Lora', sans-serif;
        font-size: 2rem;
        font-weight: 600;
        border: 2px solid #d2d1d1;
        color: #d2d1d1;
    }

    .light {
        background: #2a2a2a;
    }

    .dark {
        background: #1a1a1a;
    }

    .subtitle {
        font-family: 'Lora', sans-serif;
        font-size: 1.8rem;
        font-weight: 600;
        text-align: center;
        color: #d2d1d1;
        margin: 1rem 0;
    }


    .closer h2 {
        /* height: 100vh;
        width: 100vw; */
        font-family:'Lora', sans-serif;
        font-size: 1.5em;
        font-weight: 200;
        text-align: center;
        color:#d2d1d1;
    }

    .closer h3 {
        /* height: 100vh;
        width: 100vw; */
        font-family:'Lora', sans-serif;
        font-size: 1em;
        font-weight: 100;
        text-align: center;
        color:#d2d1d1;
    }

    .closer p {
        /* height: 100vh;
        width: 100vw; */
        font-family:'Lora', serif;
        
      
        text-align: justify;
    
        color:#d2d1d1;
        line-height: 1.6;
    }

    a {
        color: #d2d1d1;
        text-decoration: underline;
    }

    a:hover {
        text-decoration: underline;
    }

    .scroll-arrow {
    display: flex;
    justify-content: center;
    margin: 2rem 0;
    animation: bounce 2s infinite;
}

@keyframes bounce {
    0%, 20%, 50%, 80%, 100% {
        transform: translateY(0);
    }
    40% {
        transform: translateY(-20px);
    }
    60% {
        transform: translateY(-10px);
    }
}

</style>