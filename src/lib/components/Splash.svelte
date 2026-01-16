<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, scale } from "svelte/transition";

    const dispatch = createEventDispatcher();
    let loaded = false;
    let clicked = false;

    onMount(() => {
        // Add a small delay to ensure video can start playing
        setTimeout(() => {
            loaded = true;
        }, 500);
    });

    function handleStart() {
        clicked = true;
        // Add a small delay for the click animation
        setTimeout(() => {
            dispatch("enter");
        }, 800);
    }
</script>

{#if !clicked}
    <div class="splash-container" out:fade={{ duration: 1000 }}>
        <!-- Video Background -->
        <video
            autoplay
            muted
            loop
            playsinline
            poster="/images/splash.png"
            class="video-bg"
        >
            <source src="/videos/splash.mp4" type="video/mp4" />
        </video>

        <!-- Overlay Content -->
        <div class="overlay" in:fade={{ duration: 1500, delay: 500 }}>
            <div class="logo-wrapper" in:scale={{ duration: 2000, start: 0.9 }}>
                <img
                    src="/images/inpsire-logo-white.png"
                    alt="INSPIRE"
                    class="logo-image"
                />
                <p class="tagline">ENTERTAINMENT RESORT</p>
            </div>

            <button
                class="enter-button"
                on:click={handleStart}
                in:fade={{ duration: 1000, delay: 2000 }}
            >
                <div class="button-content">
                    <span class="pulse-ring"></span>
                    <span class="btn-label">TOUCH TO ENTER</span>
                </div>
            </button>
        </div>
    </div>
{/if}

<style>
    .splash-container {
        position: absolute;
        inset: 0;
        width: 100%;
        height: 100%;
        background: #000;
        z-index: 1000;
        overflow: hidden;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .video-bg {
        position: absolute;
        top: 50%;
        left: 50%;
        min-width: 100%;
        min-height: 100%;
        width: auto;
        height: auto;
        transform: translate(-50%, -50%);
        object-fit: cover;
        filter: brightness(0.6) contrast(1.1) blur(2px);
    }

    .overlay {
        position: relative;
        z-index: 2;
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        background: radial-gradient(
            circle at center,
            transparent 0%,
            rgba(0, 0, 0, 0.4) 100%
        );
        color: white;
        text-align: center;
        padding: 0 30px;
    }

    .logo-wrapper {
        margin-top: 60px;
    }

    .logo-image {
        width: 240px;
        height: auto;
    }

    .tagline {
        font-size: 10px;
        letter-spacing: 0.6em;
        margin-top: 8px;
        opacity: 0.6;
        font-weight: 300;
    }

    .enter-button {
        margin-top: 100px;
        background: none;
        border: none;
        cursor: pointer;
        position: relative;
        padding: 20px;
    }

    .button-content {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 15px;
    }

    .btn-label {
        font-size: 11px;
        letter-spacing: 0.3em;
        font-weight: 400;
        color: rgba(255, 255, 255, 0.8);
        transition: all 0.3s ease;
    }

    .pulse-ring {
        width: 60px;
        height: 60px;
        border: 1px solid rgba(255, 255, 255, 0.3);
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
    }

    .pulse-ring::before {
        content: "";
        width: 6px;
        height: 6px;
        background: white;
        border-radius: 50%;
        box-shadow: 0 0 15px white;
    }

    .pulse-ring::after {
        content: "";
        position: absolute;
        inset: -10px;
        border: 1px solid rgba(255, 255, 255, 0.2);
        border-radius: 50%;
        animation: pulse 2s infinite ease-out;
    }

    @keyframes pulse {
        0% {
            transform: scale(1);
            opacity: 1;
        }
        100% {
            transform: scale(2.5);
            opacity: 0;
        }
    }

    .enter-button:hover .btn-label {
        color: white;
        letter-spacing: 0.45em;
    }
</style>
