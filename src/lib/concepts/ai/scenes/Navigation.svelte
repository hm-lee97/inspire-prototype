<!--
  Navigation.svelte
  AI Concept - Scene 3: 가이드
  3D map navigation view
-->
<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, fly, scale } from "svelte/transition";

    const dispatch = createEventDispatcher();
    let showContent = false;
    let isNavigating = true;

    const route = [
        { id: 1, name: "현재 위치", icon: "📍", status: "done", time: "" },
        {
            id: 2,
            name: "West Wing 엘리베이터",
            icon: "🛗",
            status: "current",
            time: "2분",
        },
        { id: 3, name: "Level B1", icon: "⬇️", status: "pending", time: "1분" },
        {
            id: 4,
            name: "스플래시 베이",
            icon: "🏊",
            status: "pending",
            time: "도착",
        },
    ];

    onMount(() => setTimeout(() => (showContent = true), 200));
</script>

<div class="nav-scene">
    {#if showContent}
        <!-- Map Area -->
        <div class="map-area" in:fade={{ duration: 500 }}>
            <div class="map-placeholder">
                <div class="map-grid">
                    {#each Array(16) as _, i}
                        <div class="grid-cell"></div>
                    {/each}
                </div>
                <div class="map-path"></div>
                <div class="map-marker current">📍</div>
                <div class="map-marker destination">🏊</div>
            </div>

            <!-- Map Controls -->
            <div class="map-controls">
                <button class="control-btn">+</button>
                <button class="control-btn">−</button>
            </div>
        </div>

        <!-- Route Card -->
        <div class="route-card" in:fly={{ y: 100, duration: 500, delay: 200 }}>
            <div class="route-header">
                <div class="dest-info">
                    <span class="dest-icon">🏊</span>
                    <div class="dest-text">
                        <h3>스플래시 베이</h3>
                        <p>예상 도착 3분</p>
                    </div>
                </div>
                <div class="live-badge">
                    <span class="pulse"></span>
                    <span>실시간</span>
                </div>
            </div>

            <div class="route-steps">
                {#each route as step}
                    <div
                        class="step"
                        class:done={step.status === "done"}
                        class:current={step.status === "current"}
                    >
                        <div class="step-icon">{step.icon}</div>
                        <div class="step-info">
                            <span class="step-name">{step.name}</span>
                            {#if step.time}<span class="step-time"
                                    >{step.time}</span
                                >{/if}
                        </div>
                        <div class="step-line"></div>
                    </div>
                {/each}
            </div>

            <div class="route-actions">
                <button
                    class="action-btn secondary"
                    on:click={() => dispatch("prev")}>취소</button
                >
                <button class="action-btn primary">
                    <span>음성 안내</span>
                    <span>🔊</span>
                </button>
            </div>
        </div>
    {/if}
</div>

<style>
    .nav-scene {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        position: relative;
    }

    /* Map Area */
    .map-area {
        flex: 1;
        position: relative;
        background: linear-gradient(
            180deg,
            var(--color-bg-secondary) 0%,
            var(--color-bg-primary) 100%
        );
    }
    .map-placeholder {
        position: absolute;
        inset: 60px 20px 20px;
        border-radius: 20px;
        background: var(--color-surface);
        border: 1px solid var(--color-surface-border);
        overflow: hidden;
    }
    .map-grid {
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-template-rows: repeat(4, 1fr);
        height: 100%;
        opacity: 0.3;
    }
    .grid-cell {
        border: 1px solid var(--color-surface-border);
    }
    .map-path {
        position: absolute;
        top: 30%;
        left: 20%;
        width: 60%;
        height: 40%;
        border: 3px dashed var(--color-primary);
        border-radius: 20px;
        opacity: 0.5;
    }
    .map-marker {
        position: absolute;
        font-size: 24px;
    }
    .map-marker.current {
        top: 25%;
        left: 15%;
        animation: bounce 2s infinite;
    }
    .map-marker.destination {
        bottom: 25%;
        right: 15%;
    }
    @keyframes bounce {
        0%,
        100% {
            transform: translateY(0);
        }
        50% {
            transform: translateY(-10px);
        }
    }
    .map-controls {
        position: absolute;
        right: 30px;
        top: 80px;
        display: flex;
        flex-direction: column;
        gap: 4px;
    }
    .control-btn {
        width: 36px;
        height: 36px;
        background: var(--color-surface);
        border: 1px solid var(--color-surface-border);
        border-radius: 8px;
        font-size: 18px;
        color: var(--color-text-primary);
    }

    /* Route Card */
    .route-card {
        background: var(--color-surface);
        backdrop-filter: blur(20px);
        border-top: 1px solid var(--color-surface-border);
        border-radius: 24px 24px 0 0;
        padding: 20px;
        padding-bottom: calc(20px + var(--safe-area-bottom));
    }
    .route-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
    .dest-info {
        display: flex;
        align-items: center;
        gap: 12px;
    }
    .dest-icon {
        font-size: 32px;
    }
    .dest-text h3 {
        font-size: 18px;
        font-weight: 600;
        color: var(--color-text-primary);
    }
    .dest-text p {
        font-size: 12px;
        color: var(--color-text-tertiary);
    }
    .live-badge {
        display: flex;
        align-items: center;
        gap: 6px;
        padding: 6px 12px;
        background: rgba(34, 211, 187, 0.15);
        border-radius: 20px;
        font-size: 12px;
        color: #22d3bb;
    }
    .pulse {
        width: 8px;
        height: 8px;
        background: #22d3bb;
        border-radius: 50%;
        animation: pulse-anim 1.5s infinite;
    }
    @keyframes pulse-anim {
        0%,
        100% {
            opacity: 1;
        }
        50% {
            opacity: 0.5;
        }
    }

    /* Route Steps */
    .route-steps {
        display: flex;
        flex-direction: column;
        gap: 0;
        margin-bottom: 20px;
    }
    .step {
        display: flex;
        align-items: center;
        gap: 12px;
        padding: 12px 0;
        position: relative;
    }
    .step-icon {
        width: 32px;
        height: 32px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: var(--color-bg-secondary);
        border-radius: 10px;
        font-size: 16px;
        z-index: 1;
    }
    .step.done .step-icon {
        background: var(--color-primary);
    }
    .step.current .step-icon {
        background: var(--gradient-ai);
        box-shadow: 0 0 12px rgba(99, 102, 241, 0.4);
    }
    .step-info {
        flex: 1;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .step-name {
        font-size: 14px;
        font-weight: 500;
        color: var(--color-text-primary);
    }
    .step.done .step-name {
        color: var(--color-text-tertiary);
    }
    .step-time {
        font-size: 12px;
        color: var(--color-text-tertiary);
    }
    .step-line {
        position: absolute;
        left: 15px;
        top: 44px;
        width: 2px;
        height: calc(100% - 32px);
        background: var(--color-surface-border);
    }
    .step:last-child .step-line {
        display: none;
    }
    .step.done .step-line {
        background: var(--color-primary);
    }

    /* Actions */
    .route-actions {
        display: flex;
        gap: 12px;
    }
    .action-btn {
        flex: 1;
        padding: 14px;
        border-radius: 12px;
        font-size: 14px;
        font-weight: 600;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
    }
    .action-btn.secondary {
        background: var(--color-bg-secondary);
        color: var(--color-text-secondary);
    }
    .action-btn.primary {
        background: var(--gradient-ai);
        color: white;
    }
</style>
