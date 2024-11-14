<script>
    import { onMount, onDestroy } from 'svelte';
    import { Play, Pause, Volume2, VolumeX, Loader } from "lucide-svelte";
    
    export let artist;
    export let artworkSrc;
    export let audioSrc;
    
    let audio;
    let isPlaying = false;
    let currentTime = 0;
    let duration = 0;
    let isLoading = true;
    let loadError = null;
    let playAttempted = false;
    
    onMount(() => {
        console.log('Sticker mounted with audio source:', audioSrc);
        
        try {
            audio = new Audio(audioSrc);
            
            audio.addEventListener('play', () => {
                console.log('Play event fired');
                isPlaying = true;
                isLoading = false;
            });
    
            audio.addEventListener('pause', () => {
                console.log('Pause event fired');
                isPlaying = false;
                isLoading = false;
            });
    
            audio.addEventListener('loadeddata', () => {
                console.log('Audio data loaded successfully');
                isLoading = false;
            });
    
            audio.addEventListener('loadedmetadata', () => {
                console.log('Audio metadata loaded, duration:', audio.duration);
                duration = audio.duration;
                isLoading = false;
            });
            
            audio.addEventListener('timeupdate', () => {
                currentTime = audio.currentTime;
            });
            
            audio.addEventListener('error', (e) => {
                console.error('Audio error:', {
                    error: audio.error,
                    code: audio.error?.code,
                    message: audio.error?.message,
                    state: audio.readyState
                });
                loadError = `Audio error: ${audio.error?.message || 'Unknown error'}`;
                isLoading = false;
            });
    
            audio.addEventListener('playing', () => {
                console.log('Audio is now playing');
                isPlaying = true;
                isLoading = false;
            });
    
            audio.addEventListener('waiting', () => {
                console.log('Audio is waiting');
                isLoading = true;
            });
    
            audio.preload = 'auto';
            audio.load();
    
        } catch (error) {
            console.error('Error initializing audio:', error);
            loadError = `Error initializing audio: ${error.message}`;
            isLoading = false;
        }
        
        return () => {
            if (audio) {
                audio.pause();
                audio.removeEventListener('play', () => {});
                audio.removeEventListener('pause', () => {});
                audio.removeEventListener('loadeddata', () => {});
                audio.removeEventListener('loadedmetadata', () => {});
                audio.removeEventListener('timeupdate', () => {});
                audio.removeEventListener('error', () => {});
                audio.removeEventListener('playing', () => {});
                audio.removeEventListener('waiting', () => {});
                audio = null;
            }
        };
    });
    
    async function togglePlay() {
        console.log('Toggle play clicked');
        console.log('Current state:', { isPlaying, isLoading, playAttempted });
        
        if (!audio || isLoading) {
            console.log('Cannot play: audio not ready or loading');
            return;
        }
    
        try {
            if (isPlaying) {
                console.log('Attempting to pause');
                await audio.pause();
                isPlaying = false;
            } else {
                console.log('Attempting to play');
                isLoading = true;
                playAttempted = true;
    
                try {
                    const playPromise = audio.play();
                    if (playPromise !== undefined) {
                        await playPromise;
                        console.log('Play promise resolved successfully');
                    } else {
                        console.log('Play initiated without promise');
                    }
                } catch (playError) {
                    console.error('Play error:', playError);
                    loadError = `Play error: ${playError.message}`;
                    isPlaying = false;
                }
            }
        } catch (error) {
            console.error('Toggle play error:', error);
            loadError = `Playback error: ${error.message}`;
            isPlaying = false;
        } finally {
            isLoading = false;
        }
    }
    
    function formatTime(time) {
        const minutes = Math.floor(time / 60);
        const seconds = Math.floor(time % 60);
        return `${minutes}:${seconds.toString().padStart(2, '0')}`;
    }
    
    onDestroy(() => {
        if (audio) {
            audio.pause();
            audio = null;
        }
    });
    </script>
    
    <div class="sticker">
        <div class="content">
            <h3 class="title">{artist}</h3>
            <img 
                src={artworkSrc} 
                alt="Artwork by {artist}" 
                class="artwork"
            />
        </div>
    
        <div class="audio-player">
            {#if loadError}
                <div class="error-message">
                    {loadError}
                </div>
            {/if}
    
            <div class="controls-container">
                <button
                    class="play-button"
                    on:click={togglePlay}
                    disabled={isLoading}
                >
                    {#if isLoading}
                        <Loader class="spin" />
                    {:else if isPlaying}
                        <Pause />
                    {:else}
                        <Play />
                    {/if}
                </button>
    
                <div class="player-controls">
                    <div class="time-display">
                        {formatTime(currentTime)} / {formatTime(duration)}
                    </div>
                </div>
            </div>
    
            <!-- Debug info -->
            <!-- <div class="debug-info" style="color: #666; font-size: 12px; margin-top: 8px;">
                Status: {isPlaying ? 'Playing' : 'Paused'} | 
                Loading: {isLoading ? 'Yes' : 'No'} | 
                Duration: {duration}s
            </div> -->
        </div>
    </div>
    
    <style>
        .sticker {
            width: 90vw;
            max-width: 600px;
            background: #1a1a1a;
            border-radius: 12px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
            /* Removed position: fixed and transform since it's handled by parent */
        }
    
        .content {
            padding: 1.5rem;
        }
    
        .title {
            font-size: 1.75rem;
            font-weight: 600;
            margin-bottom: 1.5rem;
            color: #d2d1d1;
        }
    
        .artwork {
            width: 100%;
            height: auto;
            max-height: 60vh;
            object-fit: contain;
            border-radius: 8px;
            margin-bottom: 1.5rem;
        }
    
        .audio-player {
            padding: 1.5rem;
            background: #262626;
            border-radius: 0 0 12px 12px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }
    
        .error-message {
            color: #ef4444;
            margin-bottom: 1rem;
        }
    
        .controls-container {
            display: flex;
            align-items: center;
            gap: 1rem;
        }
    
        .play-button {
            background: #3b3939;
            border: none;
            color: #d2d1d1;
            cursor: pointer;
            padding: 0.5rem;
            border-radius: 50%;
            width: 48px;
            height: 48px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
    
        .play-button:hover:not(:disabled) {
            background: #666;
        }
    
        .play-button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
    
        .time-display {
            color: #d2d1d1;
            font-variant-numeric: tabular-nums;
        }
    
        .spin {
            animation: spin 1s linear infinite;
        }
    
        @keyframes spin {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }
    </style>