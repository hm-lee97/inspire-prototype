<!--
  SmartCheckin.svelte
  AI Concept - Scene 1: 인식
  Reference: linear.app (glassmorphism, technical precision, subtle glows, crisp details)
-->
<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, fly, scale } from "svelte/transition";
    import { backOut, cubicOut } from "svelte/easing";

    const dispatch = createEventDispatcher();

    let showContent = false;
    let showSkeleton = true;
    let isCheckedIn = false;
    let typingText = "";
    const fullText = "Welcome back.";

    onMount(() => {
        setTimeout(() => (showContent = true), 200);
        setTimeout(() => (showSkeleton = false), 1500);

        // Typing effect
        let i = 0;
        const typeInterval = setInterval(() => {
            if (i < fullText.length) {
                typingText = fullText.slice(0, i + 1);
                i++;
            } else {
                clearInterval(typeInterval);
            }
        }, 80);
    });

    function handleCheckin() {
        isCheckedIn = true;
        setTimeout(() => dispatch("next"), 1200);
    }
</script>

<div class="checkin-scene">
    {#if showContent}
        <!-- Location indicator (Linear style pill) -->
        <div class="location-pill" in:fly={{ y: -10, duration: 400 }}>
            <span class="pulse-dot"></span>
            <span class="location-text">INSPIRE Lobby</span>
        </div>

        <!-- AI Greeting with typing effect -->
        <div class="ai-greeting" in:fade={{ duration: 400, delay: 200 }}>
            <div class="ai-avatar">
                <div class="avatar-glow"></div>
                <svg
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="1.5"
                >
                    <path
                        d="M12 2a2 2 0 012 2c0 .74-.4 1.39-1 1.73V7h1a7 7 0 017 7h1a1 1 0 110 2h-1v1a7 7 0 01-7 7h-4a7 7 0 01-7-7v-1H2a1 1 0 110-2h1a7 7 0 017-7h1V5.73c-.6-.34-1-.99-1-1.73a2 2 0 012-2z"
                    />
                    <circle cx="9" cy="14" r="1.5" />
                    <circle cx="15" cy="14" r="1.5" />
                </svg>
            </div>
            <div class="greeting-content">
                <p class="greeting-name">김민수님</p>
                <p class="greeting-text">
                    {typingText}<span class="cursor">|</span>
                </p>
            </div>
        </div>

        <!-- Main check-in card (Linear style glass card) -->
        <div
            class="checkin-card"
            class:checked-in={isCheckedIn}
            in:fly={{ y: 30, duration: 500, delay: 400 }}
        >
            <!-- Card glow effect -->
            <div class="card-glow-effect"></div>
            <div class="card-border-glow"></div>

            <!-- Card header -->
            <div class="card-header">
                <div class="header-left">
                    <span class="card-icon">🎯</span>
                    <span class="card-label">Smart Check-in</span>
                </div>
                <div class="header-right">
                    <span class="status-badge" class:active={isCheckedIn}>
                        {isCheckedIn ? "Complete" : "Ready"}
                    </span>
                </div>
            </div>

            <!-- Card body -->
            <div class="card-body">
                {#if showSkeleton}
                    <div class="skeleton-content">
                        <div class="skeleton-row">
                            <div class="skeleton-block w-full"></div>
                        </div>
                        <div class="skeleton-row">
                            <div class="skeleton-block w-half"></div>
                            <div class="skeleton-block w-half"></div>
                        </div>
                    </div>
                {:else}
                    <div class="room-info" in:fade={{ duration: 300 }}>
                        <div class="room-visual">
                            <span class="room-emoji">🏨</span>
                            <div class="room-badge">VIP</div>
                        </div>
                        <div class="room-details">
                            <h3 class="room-name">Signature Suite</h3>
                            <p class="room-number">Room 2801</p>
                        </div>
                        <div class="room-status">
                            <span
                                class="status-dot"
                                class:ready={!isCheckedIn}
                                class:done={isCheckedIn}
                            ></span>
                            <span class="status-text"
                                >{isCheckedIn ? "Unlocked" : "Waiting"}</span
                            >
                        </div>
                    </div>
                {/if}
            </div>

            <!-- Card action -->
            {#if !showSkeleton}
                <div
                    class="card-action"
                    in:fade={{ duration: 300, delay: 100 }}
                >
                    {#if isCheckedIn}
                        <div
                            class="success-state"
                            in:scale={{ duration: 400, easing: backOut }}
                        >
                            <div class="success-icon">
                                <svg
                                    viewBox="0 0 24 24"
                                    fill="none"
                                    stroke="currentColor"
                                    stroke-width="2.5"
                                >
                                    <path d="M20 6L9 17l-5-5" />
                                </svg>
                            </div>
                            <span class="success-text">Check-in successful</span
                            >
                        </div>
                    {:else}
                        <button class="checkin-btn" on:click={handleCheckin}>
                            <span class="btn-bg"></span>
                            <span class="btn-text">Check in now</span>
                            <span class="btn-arrow">→</span>
                        </button>
                    {/if}
                </div>
            {/if}
        </div>

        <!-- Status widgets (Linear-style modular dashboard) -->
        <div class="widget-grid" in:fade={{ duration: 400, delay: 600 }}>
            <div class="widget">
                <div class="widget-icon">🌡️</div>
                <div class="widget-info">
                    <span class="widget-value">23°C</span>
                    <span class="widget-label">Room temp</span>
                </div>
            </div>
            <div class="widget">
                <div class="widget-icon">🅿️</div>
                <div class="widget-info">
                    <span class="widget-value">B2-A04</span>
                    <span class="widget-label">Parking</span>
                </div>
            </div>
            <div class="widget">
                <div class="widget-icon">🛄</div>
                <div class="widget-info">
                    <span class="widget-value">En route</span>
                    <span class="widget-label">Luggage</span>
                </div>
            </div>
            <div class="widget">
                <div class="widget-icon">☕</div>
                <div class="widget-info">
                    <span class="widget-value">Ready</span>
                    <span class="widget-label">Welcome</span>
                </div>
            </div>
        </div>

        <!-- Today's schedule -->
        <div
            class="schedule-card"
            in:fly={{ y: 20, duration: 400, delay: 800 }}
        >
            <div class="schedule-header">
                <span class="schedule-title">Today's Schedule</span>
                <span class="schedule-count">1 event</span>
            </div>
            <div class="schedule-item">
                <div class="schedule-time">
                    <span class="time-value">19:00</span>
                    <span class="time-label">PM</span>
                </div>
                <div class="schedule-divider"></div>
                <div class="schedule-content">
                    <span class="event-name">K-POP Galaxy Concert</span>
                    <span class="event-location">INSPIRE Arena · VIP A-15</span>
                </div>
                <div class="schedule-arrow">→</div>
            </div>
        </div>
    {/if}
</div>

<style>
    .checkin-scene {
        width: 100%;
        height: 100%;
        padding: 60px 20px 40px;
        display: flex;
        flex-direction: column;
        gap: 20px;
        overflow-y: auto;
    }

    /* Location pill (Linear style) */
    .location-pill {
        display: inline-flex;
        align-items: center;
        gap: 8px;
        padding: 6px 12px;
        background: rgba(255, 255, 255, 0.8);
        backdrop-filter: blur(12px);
        border: 1px solid rgba(0, 0, 0, 0.06);
        border-radius: 20px;
        width: fit-content;
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04);
    }

    .pulse-dot {
        width: 8px;
        height: 8px;
        background: var(--color-accent);
        border-radius: 50%;
        animation: pulse 2s ease-in-out infinite;
    }

    @keyframes pulse {
        0%,
        100% {
            opacity: 1;
            transform: scale(1);
        }
        50% {
            opacity: 0.6;
            transform: scale(0.9);
        }
    }

    .location-text {
        font-size: 12px;
        font-weight: 500;
        color: var(--color-text-secondary);
    }

    /* AI Greeting */
    .ai-greeting {
        display: flex;
        align-items: center;
        gap: 16px;
    }

    .ai-avatar {
        position: relative;
        width: 48px;
        height: 48px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: var(--gradient-ai);
        border-radius: 14px;
        color: white;
    }

    .avatar-glow {
        position: absolute;
        inset: -4px;
        background: var(--gradient-ai);
        border-radius: 18px;
        opacity: 0.3;
        filter: blur(12px);
        z-index: -1;
    }

    .ai-avatar svg {
        width: 24px;
        height: 24px;
    }

    .greeting-content {
        display: flex;
        flex-direction: column;
        gap: 2px;
    }

    .greeting-name {
        font-size: 22px;
        font-weight: 600;
        color: var(--color-text-primary);
    }

    .greeting-text {
        font-size: 14px;
        color: var(--color-text-tertiary);
    }

    .cursor {
        animation: blink 1s step-end infinite;
        color: var(--color-primary);
    }

    @keyframes blink {
        0%,
        50% {
            opacity: 1;
        }
        51%,
        100% {
            opacity: 0;
        }
    }

    /* Check-in card (Linear style) */
    .checkin-card {
        position: relative;
        background: rgba(255, 255, 255, 0.85);
        backdrop-filter: blur(20px);
        border: 1px solid rgba(0, 0, 0, 0.06);
        border-radius: 16px;
        padding: 20px;
        overflow: hidden;
        transition: all 0.4s ease;
    }

    .checkin-card.checked-in {
        border-color: var(--color-accent);
    }

    .card-glow-effect {
        position: absolute;
        top: -50%;
        right: -30%;
        width: 200px;
        height: 200px;
        background: var(--gradient-ai);
        filter: blur(80px);
        opacity: 0.15;
        pointer-events: none;
    }

    .card-border-glow {
        position: absolute;
        inset: -1px;
        border-radius: 17px;
        background: linear-gradient(
            135deg,
            rgba(79, 140, 255, 0.2),
            rgba(99, 102, 241, 0.1),
            transparent
        );
        z-index: -1;
        opacity: 0;
        transition: opacity 0.3s;
    }

    .checkin-card:hover .card-border-glow {
        opacity: 1;
    }

    .card-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 16px;
    }

    .header-left {
        display: flex;
        align-items: center;
        gap: 8px;
    }

    .card-icon {
        font-size: 16px;
    }

    .card-label {
        font-size: 13px;
        font-weight: 600;
        color: var(--color-text-primary);
    }

    .status-badge {
        padding: 4px 10px;
        background: var(--color-bg-secondary);
        border-radius: 10px;
        font-size: 11px;
        font-weight: 500;
        color: var(--color-text-tertiary);
        transition: all 0.3s;
    }

    .status-badge.active {
        background: rgba(34, 211, 187, 0.15);
        color: #0d9488;
    }

    /* Skeleton */
    .skeleton-content {
        display: flex;
        flex-direction: column;
        gap: 12px;
    }

    .skeleton-row {
        display: flex;
        gap: 12px;
    }

    .skeleton-block {
        height: 44px;
        background: linear-gradient(
            90deg,
            var(--color-bg-secondary) 25%,
            var(--color-bg-tertiary) 50%,
            var(--color-bg-secondary) 75%
        );
        background-size: 200% 100%;
        border-radius: 10px;
        animation: shimmer 1.5s infinite;
    }

    .w-full {
        flex: 1;
    }
    .w-half {
        flex: 0.5;
    }

    @keyframes shimmer {
        0% {
            background-position: 200% 0;
        }
        100% {
            background-position: -200% 0;
        }
    }

    /* Room info */
    .room-info {
        display: flex;
        align-items: center;
        gap: 14px;
        padding: 14px;
        background: var(--color-bg-secondary);
        border-radius: 12px;
    }

    .room-visual {
        position: relative;
        font-size: 28px;
    }

    .room-badge {
        position: absolute;
        top: -4px;
        right: -8px;
        padding: 2px 6px;
        background: var(--gradient-ai);
        border-radius: 6px;
        font-size: 8px;
        font-weight: 700;
        color: white;
    }

    .room-details {
        flex: 1;
    }

    .room-name {
        font-size: 15px;
        font-weight: 600;
        color: var(--color-text-primary);
    }

    .room-number {
        font-size: 12px;
        color: var(--color-text-tertiary);
    }

    .room-status {
        display: flex;
        align-items: center;
        gap: 6px;
    }

    .status-dot {
        width: 8px;
        height: 8px;
        border-radius: 50%;
        transition: background 0.3s;
    }

    .status-dot.ready {
        background: #fbbf24;
    }

    .status-dot.done {
        background: var(--color-accent);
    }

    .status-text {
        font-size: 11px;
        color: var(--color-text-tertiary);
    }

    /* Card action */
    .card-action {
        margin-top: 16px;
    }

    .checkin-btn {
        position: relative;
        width: 100%;
        padding: 14px;
        background: var(--gradient-ai);
        border-radius: 12px;
        color: white;
        font-size: 14px;
        font-weight: 600;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        overflow: hidden;
        transition:
            transform 0.2s,
            box-shadow 0.2s;
    }

    .checkin-btn:hover {
        transform: translateY(-1px);
        box-shadow: 0 4px 20px rgba(99, 102, 241, 0.4);
    }

    .btn-bg {
        position: absolute;
        inset: 0;
        background: linear-gradient(
            90deg,
            transparent,
            rgba(255, 255, 255, 0.1),
            transparent
        );
        transform: translateX(-100%);
        animation: shine 2s infinite;
    }

    @keyframes shine {
        0% {
            transform: translateX(-100%);
        }
        100% {
            transform: translateX(100%);
        }
    }

    .btn-arrow {
        transition: transform 0.2s;
    }

    .checkin-btn:hover .btn-arrow {
        transform: translateX(4px);
    }

    .success-state {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
        padding: 14px;
        background: rgba(34, 211, 187, 0.1);
        border-radius: 12px;
        border: 1px solid rgba(34, 211, 187, 0.2);
    }

    .success-icon {
        width: 24px;
        height: 24px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: var(--color-accent);
        border-radius: 50%;
        color: white;
    }

    .success-icon svg {
        width: 14px;
        height: 14px;
    }

    .success-text {
        font-size: 14px;
        font-weight: 600;
        color: #0d9488;
    }

    /* Widget grid */
    .widget-grid {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 10px;
    }

    .widget {
        display: flex;
        align-items: center;
        gap: 12px;
        padding: 14px;
        background: rgba(255, 255, 255, 0.7);
        backdrop-filter: blur(12px);
        border: 1px solid rgba(0, 0, 0, 0.04);
        border-radius: 12px;
        transition: all 0.2s;
    }

    .widget:hover {
        background: rgba(255, 255, 255, 0.9);
        border-color: rgba(0, 0, 0, 0.08);
    }

    .widget-icon {
        font-size: 20px;
    }

    .widget-info {
        display: flex;
        flex-direction: column;
        gap: 1px;
    }

    .widget-value {
        font-size: 13px;
        font-weight: 600;
        color: var(--color-text-primary);
    }

    .widget-label {
        font-size: 10px;
        color: var(--color-text-tertiary);
    }

    /* Schedule card */
    .schedule-card {
        background: rgba(255, 255, 255, 0.7);
        backdrop-filter: blur(12px);
        border: 1px solid rgba(0, 0, 0, 0.04);
        border-radius: 12px;
        padding: 14px;
    }

    .schedule-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 12px;
    }

    .schedule-title {
        font-size: 13px;
        font-weight: 600;
        color: var(--color-text-primary);
    }

    .schedule-count {
        font-size: 11px;
        color: var(--color-text-tertiary);
    }

    .schedule-item {
        display: flex;
        align-items: center;
        gap: 12px;
        padding: 12px;
        background: var(--color-bg-secondary);
        border-radius: 10px;
    }

    .schedule-time {
        display: flex;
        align-items: baseline;
        gap: 2px;
    }

    .time-value {
        font-size: 18px;
        font-weight: 600;
        color: var(--color-primary);
        font-family: var(--font-mono);
    }

    .time-label {
        font-size: 10px;
        color: var(--color-text-tertiary);
    }

    .schedule-divider {
        width: 2px;
        height: 28px;
        background: var(--gradient-ai);
        border-radius: 1px;
    }

    .schedule-content {
        flex: 1;
        display: flex;
        flex-direction: column;
        gap: 2px;
    }

    .event-name {
        font-size: 13px;
        font-weight: 500;
        color: var(--color-text-primary);
    }

    .event-location {
        font-size: 11px;
        color: var(--color-text-tertiary);
    }

    .schedule-arrow {
        color: var(--color-text-tertiary);
        font-size: 14px;
    }
</style>
