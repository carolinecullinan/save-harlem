<script>
    import { Play, Pause, Volume2, VolumeX } from "lucide-svelte";
    
    export let artist;
    export let artworkSrc;
    export let audioSrc;
    export let position = { top: '0%', left: '50%' };
    export let onClose = () => {};

    let isPlaying = false;
    let currentTime = 0;
    let duration = 0;
    let isMuted = false;
    let audio;

    function handleLoadedMetadata() {
        duration = audio.duration;
    }

    function handleTimeUpdate() {
        currentTime = audio.currentTime;
    }

    function togglePlay() {
        if (audio) {
            if (isPlaying) {
                audio.pause();
            } else {
                audio.play();
            }
            isPlaying = !isPlaying;
        }
    }

    function toggleMute() {
        if (audio) {
            audio.muted = !isMuted;
            isMuted = !isMuted;
        }
    }

    function handleSeek(e) {
        const seekTime = (e.offsetX / e.target.offsetWidth) * duration;
        if (audio) {
            audio.currentTime = seekTime;
            currentTime = seekTime;
        }
    }

    function formatTime(time) {
        const minutes = Math.floor(time / 60);
        const seconds = Math.floor(time % 60);
        return `${minutes}:${seconds.toString().padStart(2, '0')}`;
    }
</script>

<div class="sticker" style="top: {position.center}; left: {position.center}; transform: translateX(-50%)">
    <!-- Artist Info & Artwork -->
    <div class="content">
        <h3 class="title">{artist}</h3>
        <img 
            src={artworkSrc} 
            alt="Artwork by {artist}" 
            class="artwork"
        />
    </div>

    <!-- Audio Player -->
    <div class="audio-player">
        <audio 
            bind:this={audio}
            src={audioSrc}
            on:loadedmetadata={handleLoadedMetadata}
            on:timeupdate={handleTimeUpdate}
        />
        
        <!-- Progress Bar -->
        <div 
            class="progress-bar"
            on:click={handleSeek}
        >
            <div 
                class="progress"
                style="width: {(currentTime / duration) * 100}%"
            />
        </div>
        
        <!-- Controls -->
        <div class="controls">
            <div class="buttons">
                <button 
                    on:click={togglePlay}
                    class="control-button"
                >
                    {#if isPlaying}
                        <Pause size={24} />
                    {:else}
                        <Play size={24} />
                    {/if}
                </button>
                <button 
                    on:click={toggleMute}
                    class="control-button"
                >
                    {#if isMuted}
                        <VolumeX size={24} />
                    {:else}
                        <Volume2 size={24} />
                    {/if}
                </button>
            </div>
            <div class="time">
                {formatTime(currentTime)} / {formatTime(duration)}
            </div>
        </div>
    </div>
</div>

<style>
    .sticker {
        position: fixed;
        z-index: 50;
        width: 90vw;
        max-width: 1200px;
        background: black;
        backdrop-filter: blur(8px);
        border-radius: 8px;
        /* box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); */
        transition: all 0.3s ease-in-out;
        /* center the sticker */
        left: 50%;
        transform: translateX(-50%);
        top: 5% /* position from top of screen */
    }

    .content {
        padding: 1rem;
    }

    .title {
        font-size: 1.5rem;
        font-weight: bold;
        margin-bottom: 1rem;
        color: white /* added to ensure text visble on black background */
    }

    .artwork {
        width: 100%;
        height: auto;
        max-height: 60vh;
        object-fit: contain;
        border-radius: 0.375rem;
        margin-bottom: 1rem;
    }

    .audio-player {
        padding: 1rem;
        background: rgba(249, 250, 251, 0.9);
        border-radius: 0 0 8px 8px;
    }

    .progress-bar {
        height: 0.5rem;
        background: #e5e7eb;
        border-radius: 9999px;
        margin-bottom: 0.5rem;
        cursor: pointer;
    }

    .progress {
        height: 100%;
        background: #3b82f6;
        border-radius: 9999px;
        transition: width 0.1s ease;
    }

    .controls {
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .buttons {
        display: flex;
        gap: 0.5rem;
    }

    .control-button {
        padding: 0.5rem;
        border-radius: 9999px;
        transition: background-color 0.2s;
        display: flex;
        align-items: center;
        justify-content: center;
        background: none;
        border: none;
        cursor: pointer;
    }

    .control-button:hover {
        background-color: #e5e7eb;
    }

    .time {
        font-size: 0.875rem;
        color: #4b5563;
    }
</style>