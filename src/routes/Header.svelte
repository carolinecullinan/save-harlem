<script>


</script>

<main class="header">
    <div class="header-content-wrapper">
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

    <p> When a neighborhood changes, it usually means that working-class families get pushed out by wealthier folks moving in. This process is often described as <b>“gentrification”</b>. In the United States, given histories of segregation and disinvestment in communities of color, this <b>gentrification</b> usually happens along racial lines. Simply put, white people with money move in, and Black and Brown residents who have lived in a place for generations get forced out. When this happens, the whole feel of a neighborhood - its culture, its personality, and its soul - starts to change.<br><br>

        We wanted to understand what this looks like through the eyes of young people living through it all right now. So we sat down with the <b>summer 2024 Community Service Learning Initiative</b> cohort hosted by <b>Another Choice Youth and Family Outreach</b>. We focused on <b>East Harlem</b>, where these changes are happening day by day.<br><br>
        
        Keep scrolling to see <b>East Harlem</b> through the eyes of young people who call it home.
        
        </p>

        <div class="scroll-arrow">
            <svg width="40" height="20" viewBox="0 0 40 20">
                <path d="M20 20 L40 0 L0 0 Z" fill="#d2d1d1"/>
            </svg>
        </div>
    </div>
</main>

<style>
    .header-content-wrapper {
        
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


    .header h2 {
        /* height: 100vh;
        width: 100vw; */
        font-family:'Lora', sans-serif;
        font-size: 1.5em;
        font-weight: 200;
        text-align: center;
        color:#d2d1d1;
    }

    .header h3 {
        /* height: 100vh;
        width: 100vw; */
        font-family:'Lora', sans-serif;
        font-size: 1em;
        font-weight: 100;
        text-align: center;
        color:#d2d1d1;
    }

    .header p {
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