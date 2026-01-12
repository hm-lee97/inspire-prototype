<!--
  Countdown.svelte
  Joyful Concept - Scene 2: 인지
  Minimal upcoming shows with breachbunny-inspired design
-->
<script>
    import { createEventDispatcher, onMount, onDestroy } from "svelte";
    import { fade, fly, scale } from "svelte/transition";
    import { backOut, elasticOut } from "svelte/easing";

    const dispatch = createEventDispatcher();

    // Countdown timer (demo: 2 hours from now)
    let hours = 2;
    let minutes = 34;
    let seconds = 56;
    let interval;

    // Long press preview state
    let previewEvent = null;
    let longPressTimer = null;
    let isLongPressing = false;

    const events = [
        {
            id: 1,
            time: "20:30",
            name: "Aurora Light Show",
            venue: "Aurora Street",
            tag: "FREE",
            tagType: "free",
            description:
                "세계 최대 규모의 LED 천장에서 펼쳐지는 환상적인 오로라 라이트 쇼",
            image: "/images/aurora.png",
        },
        {
            id: 2,
            time: "22:00",
            name: "DJ Night Party",
            venue: "Club Inspire",
            tag: "VIP",
            tagType: "vip",
            description: "최고의 DJ와 함께하는 열정적인 클럽 파티",
            image: "/images/arena.png",
        },
    ];

    onMount(() => {
        interval = setInterval(() => {
            seconds--;
            if (seconds < 0) {
                seconds = 59;
                minutes--;
                if (minutes < 0) {
                    minutes = 59;
                    hours--;
                    if (hours < 0) {
                        hours = 0;
                        minutes = 0;
                        seconds = 0;
                        clearInterval(interval);
                    }
                }
            }
        }, 1000);
    });

    onDestroy(() => {
        if (interval) clearInterval(interval);
        if (longPressTimer) clearTimeout(longPressTimer);
    });

    function pad(n) {
        return n.toString().padStart(2, "0");
    }

    // Long press handlers
    function handlePressStart(event) {
        isLongPressing = true;
        longPressTimer = setTimeout(() => {
            if (isLongPressing) {
                previewEvent = event;
                // Haptic feedback
                if (navigator.vibrate) {
                    navigator.vibrate(30);
                }
            }
        }, 400); // 400ms for long press
    }

    function handlePressEnd() {
        isLongPressing = false;
        if (longPressTimer) {
            clearTimeout(longPressTimer);
            longPressTimer = null;
        }
    }

    function closePreview() {
        previewEvent = null;
    }
</script>

<div class="countdown-scene">
    <!-- Decorative grid background -->
    <div class="grid-bg"></div>

    <!-- Corner markers -->
    <div class="corner-marker top-left">+</div>
    <div class="corner-marker top-right">+</div>
    <div class="corner-marker bottom-left">+</div>
    <div class="corner-marker bottom-right">+</div>

    <!-- Header -->
    <header class="header" in:fly={{ y: -20, duration: 400 }}>
        <span class="header-tag">LIVE</span>
        <h1 class="title"><span class="highlight">UPCOMING</span> SHOWS</h1>
    </header>

    <!-- Main Event -->
    <div
        class="main-event"
        in:scale={{ start: 0.95, duration: 500, delay: 200, easing: backOut }}
    >
        <div class="event-header">
            <span class="venue-tag">INSPIRE ARENA</span>
            <span class="live-dot"></span>
        </div>

        <h2 class="event-name">K-POP Galaxy Concert</h2>
        <p class="event-time">TODAY · 19:00</p>

        <!-- Countdown -->
        <div class="countdown">
            <div class="countdown-block">
                <span class="countdown-value">{pad(hours)}</span>
                <span class="countdown-label">HR</span>
            </div>
            <span class="countdown-sep">:</span>
            <div class="countdown-block">
                <span class="countdown-value">{pad(minutes)}</span>
                <span class="countdown-label">MIN</span>
            </div>
            <span class="countdown-sep">:</span>
            <div class="countdown-block">
                <span class="countdown-value">{pad(seconds)}</span>
                <span class="countdown-label">SEC</span>
            </div>
        </div>

        <!-- CTA Button -->
        <button class="cta-btn" on:click={() => dispatch("next")}>
            <span class="btn-glow"></span>
            <span class="btn-text">티켓 활성화하기</span>
            <svg
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2.5"
            >
                <path d="M5 12h14M12 5l7 7-7 7" />
            </svg>
        </button>
    </div>

    <!-- Other Events -->
    <div class="other-events" in:fade={{ duration: 400, delay: 400 }}>
        <h3 class="section-label">MORE SHOWS</h3>

        <div class="event-list">
            {#each events as event (event.id)}
                <button
                    class="event-item"
                    on:mousedown={() => handlePressStart(event)}
                    on:touchstart|passive={() => handlePressStart(event)}
                    on:mouseup={handlePressEnd}
                    on:mouseleave={handlePressEnd}
                    on:touchend={handlePressEnd}
                    on:contextmenu|preventDefault={() => {
                        // Desktop debug: right click to preview
                        previewEvent = event;
                    }}
                >
                    <div class="item-time">{event.time}</div>
                    <div class="item-info">
                        <span class="item-name">{event.name}</span>
                        <span class="item-venue">{event.venue}</span>
                    </div>
                    <span class="item-tag {event.tagType}">{event.tag}</span>
                </button>
            {/each}
        </div>
    </div>

    <!-- Preview Modal -->
    {#if previewEvent}
        <button
            class="preview-overlay"
            transition:fade={{ duration: 200 }}
            on:click={closePreview}
        >
            <div
                class="preview-card"
                transition:scale={{
                    duration: 300,
                    easing: elasticOut,
                    start: 0.8,
                }}
                role="dialog"
            >
                <div class="preview-image">
                    <img src={previewEvent.image} alt={previewEvent.name} />
                    <div class="preview-tag">{previewEvent.tag}</div>
                </div>
                <div class="preview-content">
                    <h3 class="preview-title">{previewEvent.name}</h3>
                    <p class="preview-desc">{previewEvent.description}</p>
                    <div class="preview-meta">
                        <span>🕒 {previewEvent.time}</span>
                        <span>📍 {previewEvent.venue}</span>
                    </div>
                </div>
            </div>
        </button>
    {/if}
</div>

<style>
    .countdown-scene {
        width: 100%;
        height: 100%;
        padding: var(--space-6);
        display: flex;
        flex-direction: column;
        gap: var(--space-5);
        position: relative;
        overflow: hidden;
        background: linear-gradient(
            180deg,
            #0a0a12 0%,
            #12061f 50%,
            #0a0a12 100%
        );
        font-family: "Sansation", sans-serif;
        font-style: normal;
    }

    /* Grid Background */
    .grid-bg {
        position: absolute;
        inset: 0;
        background-image: linear-gradient(
                rgba(255, 45, 203, 0.03) 1px,
                transparent 1px
            ),
            linear-gradient(
                90deg,
                rgba(255, 45, 203, 0.03) 1px,
                transparent 1px
            );
        background-size: 40px 40px;
        pointer-events: none;
    }

    /* Corner Markers */
    .corner-marker {
        position: absolute;
        font-size: 16px;
        color: rgba(255, 45, 203, 0.3);
        font-family: monospace;
        pointer-events: none;
    }
    .top-left {
        top: 20px;
        left: 20px;
    }
    .top-right {
        top: 20px;
        right: 20px;
    }
    .bottom-left {
        bottom: 20px;
        left: 20px;
    }
    .bottom-right {
        bottom: 20px;
        right: 20px;
    }

    /* Header */
    .header {
        text-align: center;
        z-index: 1;
    }

    .header-tag {
        display: inline-block;
        padding: 4px 12px;
        background: #ff2d2d;
        border-radius: 4px;
        font-size: 10px;
        font-weight: 800;
        letter-spacing: 0.1em;
        color: white;
        margin-bottom: 8px;
        animation: pulse-glow 2s ease-in-out infinite;
    }

    @keyframes pulse-glow {
        0%,
        100% {
            box-shadow: 0 0 10px rgba(255, 45, 45, 0.5);
        }
        50% {
            box-shadow: 0 0 20px rgba(255, 45, 45, 0.8);
        }
    }

    .title {
        font-size: 24px;
        font-weight: 900;
        color: white;
        letter-spacing: -0.02em;
    }

    .highlight {
        color: #ff2dcb;
        text-shadow: 0 0 30px rgba(255, 45, 203, 0.5);
    }

    /* Main Event */
    .main-event {
        flex: 1;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: var(--space-4);
        z-index: 1;
    }

    .event-header {
        display: flex;
        align-items: center;
        gap: 8px;
    }

    .venue-tag {
        font-size: 11px;
        font-weight: 700;
        letter-spacing: 0.15em;
        color: rgba(255, 255, 255, 0.5);
        font-family: monospace;
    }

    .live-dot {
        width: 6px;
        height: 6px;
        background: #00ff88;
        border-radius: 50%;
        animation: live-pulse 1.5s ease-in-out infinite;
    }

    @keyframes live-pulse {
        0%,
        100% {
            opacity: 1;
            transform: scale(1);
        }
        50% {
            opacity: 0.5;
            transform: scale(1.2);
        }
    }

    .event-name {
        font-size: 28px;
        font-weight: 900;
        color: white;
        text-align: center;
        letter-spacing: -0.02em;
    }

    .event-time {
        font-size: 12px;
        font-family: monospace;
        color: rgba(255, 255, 255, 0.4);
        letter-spacing: 0.1em;
    }

    /* Countdown */
    .countdown {
        display: flex;
        align-items: center;
        gap: var(--space-3);
        padding: var(--space-4) var(--space-5);
        background: rgba(255, 255, 255, 0.03);
        border: 1px solid rgba(255, 255, 255, 0.08);
        border-radius: 12px;
    }

    .countdown-block {
        display: flex;
        flex-direction: column;
        align-items: center;
        min-width: 50px;
    }

    .countdown-value {
        font-size: 36px;
        font-weight: 900;
        font-family: monospace;
        background: linear-gradient(135deg, #ff2dcb 0%, #9b59ff 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        line-height: 1;
    }

    .countdown-label {
        font-size: 10px;
        font-weight: 600;
        letter-spacing: 0.15em;
        color: rgba(255, 255, 255, 0.3);
        margin-top: 4px;
    }

    .countdown-sep {
        font-size: 28px;
        font-weight: 300;
        color: rgba(255, 255, 255, 0.2);
        margin-bottom: 16px;
    }

    /* CTA Button */
    .cta-btn {
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        padding: 14px 32px;
        background: linear-gradient(135deg, #9b59ff 0%, #ff2dcb 100%);
        border: none;
        font-size: 14px;
        font-weight: 700;
        letter-spacing: 0.05em;
        color: white;
        cursor: pointer;
        overflow: hidden;
        transition: all 0.3s ease;
        clip-path: polygon(
            8px 0,
            100% 0,
            100% calc(100% - 8px),
            calc(100% - 8px) 100%,
            0 100%,
            0 8px
        );
    }

    .btn-glow {
        position: absolute;
        inset: -2px;
        background: linear-gradient(135deg, #9b59ff 0%, #ff2dcb 100%);
        filter: blur(12px);
        opacity: 0.4;
        animation: glow-pulse 2s ease-in-out infinite;
        z-index: -1;
    }

    @keyframes glow-pulse {
        0%,
        100% {
            opacity: 0.3;
            transform: scale(0.95);
        }
        50% {
            opacity: 0.6;
            transform: scale(1.05);
        }
    }

    .btn-text {
        position: relative;
        z-index: 1;
    }

    .cta-btn svg {
        width: 16px;
        height: 16px;
        position: relative;
        z-index: 1;
    }

    .cta-btn:hover {
        transform: scale(1.05);
        box-shadow: 0 0 30px rgba(255, 45, 203, 0.5);
    }

    .cta-btn:active {
        transform: scale(0.98);
    }

    /* Other Events */
    .other-events {
        z-index: 1;
    }

    .section-label {
        font-size: 11px;
        font-weight: 700;
        letter-spacing: 0.15em;
        color: rgba(255, 255, 255, 0.3);
        margin-bottom: var(--space-3);
        font-family: monospace;
    }

    .event-list {
        display: flex;
        flex-direction: column;
        gap: var(--space-2);
    }

    .event-item {
        display: flex;
        align-items: center;
        gap: var(--space-3);
        padding: var(--space-3) var(--space-4);
        background: rgba(255, 255, 255, 0.03);
        border: 1px solid rgba(255, 255, 255, 0.06);
        border-radius: 8px;
        cursor: pointer;
        transition: all 0.2s ease;
        text-align: left;
        width: 100%;
    }

    .event-item:hover {
        background: rgba(255, 255, 255, 0.06);
        border-color: rgba(255, 45, 203, 0.3);
    }

    .item-time {
        font-size: 14px;
        font-weight: 700;
        font-family: monospace;
        color: #ff2dcb;
        min-width: 45px;
    }

    .item-info {
        flex: 1;
        display: flex;
        flex-direction: column;
        gap: 2px;
    }

    .item-name {
        font-size: 13px;
        font-weight: 600;
        color: white;
    }

    .item-venue {
        font-size: 11px;
        color: rgba(255, 255, 255, 0.4);
    }

    .item-tag {
        padding: 3px 8px;
        border-radius: 4px;
        font-size: 10px;
        font-weight: 700;
        letter-spacing: 0.05em;
    }

    .item-tag.free {
        background: rgba(0, 255, 136, 0.15);
        color: #00ff88;
        border: 1px solid rgba(0, 255, 136, 0.3);
    }

    .item-tag.vip {
        background: rgba(255, 45, 203, 0.15);
        color: #ff2dcb;
        border: 1px solid rgba(255, 45, 203, 0.3);
    }

    /* Preview Modal */
    .preview-overlay {
        position: fixed;
        inset: 0;
        background: rgba(0, 0, 0, 0.8);
        backdrop-filter: blur(8px);
        z-index: 100;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: var(--space-5);
        border: none;
        cursor: pointer;
    }

    .preview-card {
        width: 100%;
        max-width: 320px;
        background: #12061f;
        border: 1px solid rgba(255, 45, 203, 0.3);
        border-radius: 16px;
        overflow: hidden;
        box-shadow: 0 0 50px rgba(155, 89, 255, 0.3);
        position: relative;
    }

    .preview-image {
        position: relative;
        height: 180px;
        width: 100%;
    }

    .preview-image img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .preview-tag {
        position: absolute;
        top: 12px;
        right: 12px;
        padding: 4px 10px;
        background: rgba(0, 0, 0, 0.7);
        backdrop-filter: blur(4px);
        border-radius: 4px;
        color: #ff2dcb;
        font-size: 11px;
        font-weight: 700;
        border: 1px solid rgba(255, 45, 203, 0.5);
    }

    .preview-content {
        padding: 20px;
    }

    .preview-title {
        font-size: 18px;
        font-weight: 800;
        color: white;
        margin-bottom: 8px;
    }

    .preview-desc {
        font-size: 13px;
        color: rgba(255, 255, 255, 0.7);
        line-height: 1.5;
        margin-bottom: 16px;
    }

    .preview-meta {
        display: flex;
        gap: 16px;
        font-size: 12px;
        color: rgba(255, 255, 255, 0.5);
        font-family: monospace;
    }
</style>
