<!--
  AIApp.svelte
  Concept C: The Smart Navigator (AI)
  Chat-First Interface with Glassmorphism
-->
<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, fly, scale } from "svelte/transition";
    import { quintOut } from "svelte/easing";
    import AssistantService from "./scenes/AssistantService.svelte";

    const dispatch = createEventDispatcher();

    let appState = "idle"; // 'idle' | 'active'
    let searchQuery = "";
    let inputEl;

    function handleSubmit() {
        if (!searchQuery.trim()) return;

        // Transition to active state
        appState = "active";
    }

    function handleBack() {
        dispatch("back");
    }

    function resetApp() {
        appState = "idle";
        searchQuery = "";
        setTimeout(() => inputEl?.focus(), 300);
    }

    const suggestions = [
        "스마트 체크인",
        "오늘의 공연",
        "길 찾기",
        "다이닝 예약",
    ];

    function handleSuggestion(text) {
        searchQuery = text;
        handleSubmit();
    }
</script>

<div class="ai-app" data-theme="ai">
    <!-- Cinematic Background (Slightly visible resort image) -->
    <div class="cinematic-bg">
        <div class="bg-image"></div>
        <div class="bg-overlay"></div>
    </div>

    <!-- Atmospheric Glows (Subtler) -->
    <div class="glass-bg">
        <div class="glass-orb orb-1"></div>
        <div class="glass-orb orb-2"></div>
    </div>

    <!-- Top Bar -->
    <div class="top-bar">
        <!-- Center Wordmark -->
        <div class="brand-wordmark">INSPIRE</div>

        <button class="back-button" on:click={handleBack}>
            <svg
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
            >
                <path d="M19 12H5M12 19l-7-7 7-7" />
            </svg>
        </button>
        {#if appState === "active"}
            <!-- <div class="page-title" in:fade>AI Assistant</div> -->
            <!-- Replaced title with centralized wordmark, so button is just float right -->
            <button class="reset-button" in:fade on:click={resetApp}
                >처음으로</button
            >
        {/if}
    </div>

    <div class="content-container">
        <!-- Idle State: Centered Prompt -->
        {#if appState === "idle"}
            <div
                class="prompt-container"
                out:fly={{ y: -50, duration: 500, opacity: 0 }}
            >
                <div
                    class="greeting"
                    in:fly={{ y: 20, duration: 600, delay: 200 }}
                >
                    <div class="ai-avatar">
                        <div class="avatar-icon">✨</div>
                    </div>
                    <h1 class="greeting-title">
                        인스파이어에 오신 것을 환영합니다
                    </h1>
                    <p class="greeting-subtitle">
                        당신을 위한 특별한 여정이 여기서 시작됩니다.
                    </p>
                </div>

                <div
                    class="input-wrapper"
                    in:scale={{ duration: 500, delay: 400, start: 0.9 }}
                >
                    <form on:submit|preventDefault={handleSubmit}>
                        <input
                            bind:this={inputEl}
                            bind:value={searchQuery}
                            type="text"
                            placeholder="무엇을 도와드릴까요?"
                            class="chat-input"
                        />
                        <button
                            type="submit"
                            class="send-btn"
                            disabled={!searchQuery}
                        >
                            <svg
                                viewBox="0 0 24 24"
                                fill="none"
                                stroke="currentColor"
                                stroke-width="2"
                            >
                                <path
                                    d="M22 2L11 13M22 2l-7 20-4-9-9-4 20-7z"
                                />
                            </svg>
                        </button>
                    </form>

                    <!-- Suggestion Chips -->
                    <div class="suggestions">
                        {#each suggestions as suggestion}
                            <button
                                class="suggestion-chip"
                                on:click={() => handleSuggestion(suggestion)}
                            >
                                {suggestion}
                            </button>
                        {/each}
                    </div>
                </div>
            </div>
        {/if}

        <!-- Active State: Chat Interface -->
        {#if appState === "active"}
            <div class="active-view" in:fade={{ duration: 600, delay: 300 }}>
                <AssistantService initialQuery={searchQuery} />
            </div>
        {/if}
    </div>
</div>

<style>
    :global(:root) {
        --linear-bg: #0f1115;
        --linear-surface: #1c1e23;
        --linear-border: rgba(255, 255, 255, 0.08);
        --linear-text-primary: #f7f8f8;
        --linear-text-secondary: #8a8f98;
        --linear-accent: #5e6ad2;
        --font-ai: "Encode Sans", sans-serif;
    }

    .ai-app {
        width: 100%;
        height: 100%;
        background: var(--linear-bg);
        position: relative;
        overflow: hidden;
        display: flex;
        flex-direction: column;
        font-family: var(--font-ai);
        color: var(--linear-text-primary);
    }

    /* Cinematic Background */
    .cinematic-bg {
        position: absolute;
        inset: 0;
        z-index: 0;
    }

    .bg-image {
        position: absolute;
        inset: 0;
        background-image: url("/src/lib/assets/joyful/exterior_v1.png");
        background-size: cover;
        background-position: center;
        filter: grayscale(100%) contrast(110%);
        opacity: 0.6; /* Increased visibility */
    }

    .bg-overlay {
        position: absolute;
        inset: 0;
        background: linear-gradient(
            180deg,
            rgba(15, 17, 21, 0.85) 0%,
            rgba(15, 17, 21, 0.6) 100%
        ); /* Lighter overlay */
        backdrop-filter: blur(1px);
    }

    /* Subtle Atmospheric Glows */
    .glass-bg {
        position: absolute;
        inset: 0;
        overflow: hidden;
        pointer-events: none;
        z-index: 1;
    }

    .glass-orb {
        position: absolute;
        border-radius: 50%;
        filter: blur(120px);
        opacity: 0.1; /* Very subtle against image bg */
    }

    .orb-1 {
        width: 600px;
        height: 600px;
        background: #5e6ad2;
        top: -300px;
        right: -200px;
        animation: breath 15s ease-in-out infinite;
    }

    .orb-2 {
        width: 500px;
        height: 500px;
        background: #4c5569;
        bottom: -200px;
        left: -100px;
        animation: breath 20s ease-in-out infinite reverse;
    }

    @keyframes breath {
        0%,
        100% {
            transform: scale(1);
            opacity: 0.1;
        }
        50% {
            transform: scale(1.1);
            opacity: 0.15;
        }
    }

    /* Top Bar */
    .top-bar {
        position: relative;
        height: 56px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0 20px;
        z-index: 100;
        margin-top: var(--safe-area-top); /* Ensure it respects safe area */
    }

    .brand-wordmark {
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        font-family: var(--font-english); /* Use Sansation or fallback */
        font-size: 12px;
        font-weight: 700;
        letter-spacing: 0.25em; /* Wide tracking for elegance */
        color: var(--linear-text-primary);
        opacity: 0.4; /* Subtle branding */
        pointer-events: none;
    }

    .back-button {
        width: 32px;
        height: 32px;
        border-radius: 6px;
        background: transparent;
        border: 1px solid var(--linear-border);
        color: var(--linear-text-secondary);
        display: flex;
        align-items: center;
        justify-content: center;
        transition: all 0.2s ease;
        cursor: pointer;
        z-index: 101; /* Above wordmark */
    }

    .back-button:hover {
        background: var(--linear-surface);
        color: var(--linear-text-primary);
        border-color: rgba(255, 255, 255, 0.2);
    }

    .back-button svg {
        width: 16px;
        height: 16px;
    }

    .reset-button {
        font-size: 11px;
        font-weight: 600;
        color: var(--linear-text-secondary);
        background: transparent;
        border: 1px solid var(--linear-border);
        padding: 4px 10px;
        border-radius: 4px;
        cursor: pointer;
        transition: all 0.2s;
        z-index: 101;
    }

    .reset-button:hover {
        background: var(--linear-surface);
        color: var(--linear-text-primary);
    }

    /* Content */
    .content-container {
        flex: 1;
        position: relative;
        overflow: hidden;
        z-index: 10;
        display: flex;
        flex-direction: column;
    }

    /* Idle State */
    .prompt-container {
        flex: 1;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        padding: 0 24px;
        gap: 48px;
    }

    .greeting {
        text-align: center;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 12px;
    }

    .ai-avatar {
        width: 64px;
        height: 64px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: linear-gradient(
            180deg,
            rgba(255, 255, 255, 0.05) 0%,
            rgba(255, 255, 255, 0) 100%
        );
        border: 1px solid var(--linear-border);
        border-radius: 16px;
        box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
        backdrop-filter: blur(10px);
    }

    .avatar-icon {
        font-size: 24px;
        filter: grayscale(1);
    }

    .greeting-title {
        font-size: 24px;
        font-weight: 600;
        color: var(--linear-text-primary);
        letter-spacing: -0.02em;
        margin: 0;
    }

    .greeting-subtitle {
        font-size: 15px;
        color: var(--linear-text-secondary);
        margin: 0;
        font-weight: 400;
    }

    .input-wrapper {
        width: 100%;
        max-width: 440px;
        display: flex;
        flex-direction: column;
        gap: 16px;
    }

    form {
        position: relative;
        width: 100%;
    }

    /* Suggestion Chips */
    .suggestions {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        gap: 8px;
    }

    .suggestion-chip {
        padding: 8px 16px;
        background: rgba(255, 255, 255, 0.03);
        border: 1px solid var(--linear-border);
        border-radius: 20px;
        color: var(--linear-text-secondary);
        font-size: 13px;
        cursor: pointer;
        transition: all 0.2s;
    }

    .suggestion-chip:hover {
        background: rgba(255, 255, 255, 0.08);
        border-color: rgba(255, 255, 255, 0.2);
        color: var(--linear-text-primary);
        transform: translateY(-1px);
    }

    .chat-input {
        width: 100%;
        padding: 16px 48px 16px 16px;
        background: rgba(20, 22, 27, 0.8); /* Semi-transparent */
        backdrop-filter: blur(12px);
        border: 1px solid var(--linear-border);
        border-radius: 8px;
        font-size: 15px;
        color: var(--linear-text-primary);
        transition: all 0.15s ease-out;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    }

    .chat-input::placeholder {
        color: var(--linear-text-secondary);
        opacity: 0.6;
    }

    .chat-input:focus {
        outline: none;
        border-color: rgba(255, 255, 255, 0.2);
        background: rgba(25, 27, 33, 0.9);
        box-shadow:
            0 0 0 1px rgba(255, 255, 255, 0.1),
            0 8px 24px rgba(0, 0, 0, 0.4);
    }

    .send-btn {
        position: absolute;
        right: 8px;
        top: 50%;
        transform: translateY(-50%);
        width: 32px;
        height: 32px;
        border-radius: 6px;
        background: transparent;
        color: var(--linear-text-secondary);
        border: none;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: all 0.2s ease;
        cursor: pointer;
    }

    .send-btn:disabled {
        opacity: 0.3;
        cursor: default;
    }

    .send-btn:not(:disabled):hover {
        background: rgba(255, 255, 255, 0.1);
        color: var(--linear-text-primary);
    }

    .send-btn svg {
        width: 16px;
        height: 16px;
    }

    /* Active View */
    .active-view {
        width: 100%;
        height: 100%;
    }
</style>
