<!--
  RefinedApp.svelte
  Concept B: The Signature Collection
  Minimal luxury, champagne gold, editorial style
-->
<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, fly, slide } from "svelte/transition";
    import { cubicOut, quintOut } from "svelte/easing";

    // Scene Components
    import Welcome from "./scenes/Welcome.svelte";
    import Curation from "./scenes/Curation.svelte";
    import Concierge from "./scenes/Concierge.svelte";

    const dispatch = createEventDispatcher();

    let currentScene = "welcome"; // 'welcome' | 'curation' | 'concierge'
    let isTransitioning = false;

    function goToScene(scene) {
        if (isTransitioning) return;
        isTransitioning = true;
        setTimeout(() => {
            currentScene = scene;
            isTransitioning = false;
        }, 400);
    }

    function handleBack() {
        dispatch("back");
    }
</script>

<div class="refined-app" data-theme="refined">
    <!-- Subtle Background Pattern -->
    <div class="bg-pattern">
        <div class="pattern-line"></div>
    </div>

    <!-- Back Button -->
    <button class="back-button" on:click={handleBack}>
        <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.5"
        >
            <path d="M19 12H5M12 19l-7-7 7-7" />
        </svg>
    </button>

    <!-- Scene Navigation -->
    <div class="scene-nav">
        <button
            class="nav-item"
            class:active={currentScene === "welcome"}
            on:click={() => goToScene("welcome")}
        >
            <span class="nav-text">환대</span>
            <span class="nav-line"></span>
        </button>
        <button
            class="nav-item"
            class:active={currentScene === "curation"}
            on:click={() => goToScene("curation")}
        >
            <span class="nav-text">탐색</span>
            <span class="nav-line"></span>
        </button>
        <button
            class="nav-item"
            class:active={currentScene === "concierge"}
            on:click={() => goToScene("concierge")}
        >
            <span class="nav-text">요청</span>
            <span class="nav-line"></span>
        </button>
    </div>

    <!-- Scenes -->
    <div class="scene-container">
        {#if currentScene === "welcome"}
            <div
                class="scene"
                in:fade={{ duration: 500, easing: quintOut }}
                out:fade={{ duration: 300 }}
            >
                <Welcome on:next={() => goToScene("curation")} />
            </div>
        {:else if currentScene === "curation"}
            <div
                class="scene"
                in:fade={{ duration: 500, easing: quintOut }}
                out:fade={{ duration: 300 }}
            >
                <Curation
                    on:next={() => goToScene("concierge")}
                    on:prev={() => goToScene("welcome")}
                />
            </div>
        {:else if currentScene === "concierge"}
            <div
                class="scene"
                in:fade={{ duration: 500, easing: quintOut }}
                out:fade={{ duration: 300 }}
            >
                <Concierge on:prev={() => goToScene("curation")} />
            </div>
        {/if}
    </div>
</div>

<style>
    .refined-app {
        width: 100%;
        height: 100%;
        background: var(--color-bg-primary);
        position: relative;
        overflow: hidden;
        font-family: var(--font-serif);
    }

    /* Background Pattern */
    .bg-pattern {
        position: absolute;
        inset: 0;
        overflow: hidden;
        opacity: 0.03;
        pointer-events: none;
    }

    .pattern-line {
        position: absolute;
        top: 0;
        left: 50%;
        width: 1px;
        height: 100%;
        background: linear-gradient(
            180deg,
            transparent 0%,
            var(--color-primary) 20%,
            var(--color-primary) 80%,
            transparent 100%
        );
    }

    /* Back Button */
    .back-button {
        position: absolute;
        top: calc(var(--safe-area-top) + 8px);
        left: var(--grid-margin);
        width: 40px;
        height: 40px;
        border-radius: 50%;
        background: transparent;
        border: 1px solid var(--color-surface-border);
        color: var(--color-text-primary);
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 100;
        transition: all var(--transition-normal);
    }

    .back-button:hover {
        background: var(--color-surface);
        border-color: var(--color-text-tertiary);
    }

    .back-button svg {
        width: 20px;
        height: 20px;
    }

    /* Scene Navigation */
    .scene-nav {
        position: absolute;
        top: calc(var(--safe-area-top) + 12px);
        left: 50%;
        transform: translateX(-50%);
        display: flex;
        gap: var(--space-6);
        z-index: 100;
    }

    .nav-item {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 4px;
        padding: var(--space-2) 0;
        background: transparent;
        transition: all var(--transition-normal);
    }

    .nav-text {
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        font-weight: var(--font-weight-medium);
        color: var(--color-text-tertiary);
        letter-spacing: 0.05em;
        transition: color var(--transition-normal);
    }

    .nav-line {
        width: 20px;
        height: 1px;
        background: var(--color-text-tertiary);
        transform: scaleX(0);
        transition: transform var(--transition-normal);
    }

    .nav-item.active .nav-text {
        color: var(--color-text-primary);
    }

    .nav-item.active .nav-line {
        transform: scaleX(1);
        background: var(--color-primary);
    }

    /* Scene Container */
    .scene-container {
        position: absolute;
        inset: 0;
        padding-top: var(--safe-area-top);
        padding-bottom: var(--safe-area-bottom);
    }

    .scene {
        width: 100%;
        height: 100%;
        position: absolute;
        inset: 0;
    }
</style>
