<!--
  TicketActivation.svelte
  Joyful Concept - Scene 3: 액션
  Minimal ticket activation inspired by breachbunny.com
-->
<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, fly, scale } from "svelte/transition";
    import { elasticOut, backOut, cubicOut } from "svelte/easing";

    const dispatch = createEventDispatcher();

    let isActivated = false;
    let showConfetti = false;
    let showVibrateToast = false;
    let confettiPieces = [];
    let pulseActive = false;

    onMount(() => {
        // Start pulse animation
        const interval = setInterval(() => {
            pulseActive = !pulseActive;
        }, 1500);
        return () => clearInterval(interval);
    });

    function activateTicket() {
        if (isActivated) return;

        isActivated = true;
        showConfetti = true;

        // Haptic feedback - celebration pattern
        // Visual feedback for desktop
        showVibrateToast = true;
        setTimeout(() => (showVibrateToast = false), 1000);

        if (navigator.vibrate) {
            navigator.vibrate([50, 30, 50, 30, 100]); // Short-short-long pattern
        }

        // Generate confetti
        confettiPieces = Array(40)
            .fill(null)
            .map((_, i) => ({
                id: i,
                x: 50 + (Math.random() - 0.5) * 80,
                delay: Math.random() * 0.3,
                duration: 1.5 + Math.random(),
                color: ["#FF2DCB", "#9B59FF", "#00D4FF", "#FFD700", "#FF4ECD"][
                    Math.floor(Math.random() * 5)
                ],
                rotation: Math.random() * 720,
                size: 6 + Math.random() * 8,
            }));

        setTimeout(() => {
            showConfetti = false;
        }, 2500);
    }
</script>

<div class="ticket-scene">
    <!-- Decorative grid background -->
    <div class="grid-bg"></div>

    <!-- Corner markers -->
    <div class="corner-marker top-left">+</div>
    <div class="corner-marker top-right">+</div>
    <div class="corner-marker bottom-left">+</div>
    <div class="corner-marker bottom-right">+</div>

    <!-- Confetti Layer -->
    {#if showConfetti}
        <div class="confetti-container" transition:fade={{ duration: 500 }}>
            {#each confettiPieces as piece (piece.id)}
                <div
                    class="confetti-piece"
                    style="
                        --x: {piece.x}%;
                        --delay: {piece.delay}s;
                        --duration: {piece.duration}s;
                        --color: {piece.color};
                        --rotation: {piece.rotation}deg;
                        --size: {piece.size}px;
                    "
                ></div>
            {/each}
        </div>
    {/if}

    <!-- Main Content -->
    <div class="content" in:fade={{ duration: 600 }}>
        <!-- Status Badge -->
        <div
            class="status-badge"
            class:active={isActivated}
            in:fly={{ y: -10, duration: 400 }}
        >
            {isActivated ? "ACTIVATED" : "STANDBY"}
        </div>

        <!-- Main Title -->
        <h1 class="title" in:fly={{ y: 20, duration: 500, delay: 100 }}>
            {#if isActivated}
                <span class="highlight">TICKET</span> READY
            {:else}
                <span class="highlight">INSPIRE</span> ARENA
            {/if}
        </h1>

        <!-- Event Info -->
        <div class="event-info" in:fly={{ y: 20, duration: 500, delay: 200 }}>
            <p class="event-name">K-POP Galaxy Concert</p>
            <p class="event-meta">2026.01.09 · 19:00</p>
        </div>

        <!-- Ticket Visual -->
        <div
            class="ticket-visual"
            class:activated={isActivated}
            in:scale={{
                start: 0.9,
                duration: 600,
                delay: 300,
                easing: backOut,
            }}
        >
            {#if isActivated}
                <div
                    class="qr-container"
                    in:scale={{ duration: 400, easing: elasticOut }}
                >
                    <div class="qr-code">
                        <svg viewBox="0 0 100 100" class="qr-svg">
                            <!-- Simplified QR pattern -->
                            <rect
                                x="10"
                                y="10"
                                width="25"
                                height="25"
                                fill="currentColor"
                            />
                            <rect
                                x="65"
                                y="10"
                                width="25"
                                height="25"
                                fill="currentColor"
                            />
                            <rect
                                x="10"
                                y="65"
                                width="25"
                                height="25"
                                fill="currentColor"
                            />
                            <rect
                                x="40"
                                y="40"
                                width="20"
                                height="20"
                                fill="currentColor"
                            />
                            <rect
                                x="15"
                                y="15"
                                width="15"
                                height="15"
                                fill="white"
                            />
                            <rect
                                x="70"
                                y="15"
                                width="15"
                                height="15"
                                fill="white"
                            />
                            <rect
                                x="15"
                                y="70"
                                width="15"
                                height="15"
                                fill="white"
                            />
                            <rect
                                x="18"
                                y="18"
                                width="9"
                                height="9"
                                fill="currentColor"
                            />
                            <rect
                                x="73"
                                y="18"
                                width="9"
                                height="9"
                                fill="currentColor"
                            />
                            <rect
                                x="18"
                                y="73"
                                width="9"
                                height="9"
                                fill="currentColor"
                            />
                            <!-- Random blocks -->
                            <rect
                                x="45"
                                y="15"
                                width="5"
                                height="5"
                                fill="currentColor"
                            />
                            <rect
                                x="55"
                                y="20"
                                width="5"
                                height="5"
                                fill="currentColor"
                            />
                            <rect
                                x="40"
                                y="25"
                                width="5"
                                height="5"
                                fill="currentColor"
                            />
                            <rect
                                x="50"
                                y="30"
                                width="5"
                                height="5"
                                fill="currentColor"
                            />
                            <rect
                                x="15"
                                y="45"
                                width="5"
                                height="5"
                                fill="currentColor"
                            />
                            <rect
                                x="25"
                                y="50"
                                width="5"
                                height="5"
                                fill="currentColor"
                            />
                            <rect
                                x="70"
                                y="45"
                                width="5"
                                height="5"
                                fill="currentColor"
                            />
                            <rect
                                x="80"
                                y="50"
                                width="5"
                                height="5"
                                fill="currentColor"
                            />
                            <rect
                                x="45"
                                y="70"
                                width="5"
                                height="5"
                                fill="currentColor"
                            />
                            <rect
                                x="55"
                                y="75"
                                width="5"
                                height="5"
                                fill="currentColor"
                            />
                            <rect
                                x="65"
                                y="65"
                                width="5"
                                height="5"
                                fill="currentColor"
                            />
                        </svg>
                    </div>
                    <p class="scan-text">SCAN TO ENTER</p>
                </div>
            {:else}
                <div class="ticket-placeholder" class:pulse={pulseActive}>
                    <span class="seat-label">VIP</span>
                    <span class="seat-number">A-15</span>
                </div>
            {/if}
        </div>

        <!-- Details Row -->
        <div class="details-row" in:fly={{ y: 20, duration: 500, delay: 400 }}>
            <div class="detail-item">
                <span class="detail-label">SEAT</span>
                <span class="detail-value">VIP A-15</span>
            </div>
            <div class="detail-divider"></div>
            <div class="detail-item">
                <span class="detail-label">QTY</span>
                <span class="detail-value">2</span>
            </div>
        </div>

        <!-- Action Button -->
        {#if !isActivated}
            <button
                class="activate-btn"
                on:click={activateTicket}
                in:fly={{ y: 20, duration: 500, delay: 500 }}
            >
                <span class="btn-glow"></span>
                <span class="btn-text">ACTIVATE</span>
            </button>
        {:else}
            <div
                class="activated-message"
                in:scale={{ duration: 400, easing: elasticOut }}
            >
                <span class="check-icon">✓</span>
                <span>입장 준비 완료</span>
            </div>
        {/if}
    </div>

    <!-- Bottom Note -->
    <div class="bottom-note" in:fade={{ duration: 400, delay: 600 }}>
        입장 30분 전부터 활성화 가능
    </div>
    {#if showVibrateToast}
        <div class="vibrate-toast" transition:scale>📳 VIBRATE</div>
    {/if}
</div>

<style>
    .ticket-scene {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        padding: var(--space-6);
        position: relative;
        overflow: hidden;
        background: linear-gradient(
            180deg,
            #0a0a12 0%,
            #12061f 50%,
            #0a0a12 100%
        );
        font-family: "Sansation", sans-serif;
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

    /* Confetti */
    .confetti-container {
        position: absolute;
        inset: 0;
        pointer-events: none;
        overflow: hidden;
        z-index: 100;
    }

    .confetti-piece {
        position: absolute;
        width: var(--size);
        height: var(--size);
        left: var(--x);
        top: 30%;
        background: var(--color);
        border-radius: 2px;
        animation: confetti-fall var(--duration) ease-out var(--delay) forwards;
        opacity: 0;
    }

    @keyframes confetti-fall {
        0% {
            opacity: 1;
            transform: translateY(0) rotate(0deg) scale(1);
        }
        100% {
            opacity: 0;
            transform: translateY(400px) rotate(var(--rotation)) scale(0.3);
        }
    }

    .vibrate-toast {
        position: absolute;
        bottom: 100px;
        left: 50%;
        transform: translateX(-50%);
        background: rgba(0, 0, 0, 0.8);
        color: #ff2dcb;
        padding: 8px 16px;
        border-radius: 20px;
        font-family: monospace;
        font-weight: bold;
        border: 1px solid #ff2dcb;
        z-index: 1000;
        pointer-events: none;
    }

    /* Content */
    .content {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: var(--space-5);
        z-index: 1;
    }

    /* Status Badge */
    .status-badge {
        padding: 6px 16px;
        font-size: 11px;
        font-weight: 700;
        letter-spacing: 0.15em;
        font-family: monospace;
        color: rgba(255, 255, 255, 0.5);
        background: rgba(255, 255, 255, 0.05);
        border: 1px solid rgba(255, 255, 255, 0.1);
        border-radius: 4px;
        transition: all 0.3s ease;
    }

    .status-badge.active {
        color: #00ff88;
        background: rgba(0, 255, 136, 0.1);
        border-color: rgba(0, 255, 136, 0.3);
        box-shadow: 0 0 20px rgba(0, 255, 136, 0.2);
    }

    /* Title */
    .title {
        font-size: 32px;
        font-weight: 900;
        letter-spacing: -0.02em;
        color: white;
        text-align: center;
        line-height: 1.1;
    }

    .highlight {
        color: #ff2dcb;
        text-shadow: 0 0 30px rgba(255, 45, 203, 0.5);
    }

    /* Event Info */
    .event-info {
        text-align: center;
    }

    .event-name {
        font-size: 14px;
        font-weight: 600;
        color: white;
        margin-bottom: 4px;
    }

    .event-meta {
        font-size: 12px;
        font-family: monospace;
        color: rgba(255, 255, 255, 0.4);
        letter-spacing: 0.05em;
    }

    /* Ticket Visual */
    .ticket-visual {
        width: 180px;
        height: 180px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: rgba(255, 255, 255, 0.03);
        border: 1px solid rgba(255, 255, 255, 0.1);
        border-radius: 16px;
        transition: all 0.5s ease;
    }

    .ticket-visual.activated {
        background: white;
        border-color: white;
        box-shadow: 0 0 60px rgba(255, 255, 255, 0.3);
    }

    /* QR Code */
    .qr-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 8px;
    }

    .qr-code {
        width: 130px;
        height: 130px;
        padding: 10px;
    }

    .qr-svg {
        width: 100%;
        height: 100%;
        color: #0a0a12;
    }

    .scan-text {
        font-size: 10px;
        font-weight: 700;
        letter-spacing: 0.15em;
        color: #0a0a12;
        font-family: monospace;
    }

    /* Ticket Placeholder */
    .ticket-placeholder {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 4px;
        transition: all 0.5s ease;
    }

    .ticket-placeholder.pulse {
        transform: scale(1.05);
    }

    .seat-label {
        font-size: 12px;
        font-weight: 700;
        letter-spacing: 0.2em;
        color: rgba(255, 45, 203, 0.6);
        font-family: monospace;
    }

    .seat-number {
        font-size: 48px;
        font-weight: 900;
        color: white;
        letter-spacing: -0.02em;
        text-shadow: 0 0 40px rgba(255, 45, 203, 0.3);
    }

    /* Details Row */
    .details-row {
        display: flex;
        align-items: center;
        gap: var(--space-5);
    }

    .detail-item {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 2px;
    }

    .detail-label {
        font-size: 10px;
        font-weight: 600;
        letter-spacing: 0.15em;
        color: rgba(255, 255, 255, 0.3);
        font-family: monospace;
    }

    .detail-value {
        font-size: 14px;
        font-weight: 700;
        color: white;
    }

    .detail-divider {
        width: 1px;
        height: 24px;
        background: rgba(255, 255, 255, 0.1);
    }

    /* Activate Button */
    .activate-btn {
        position: relative;
        padding: 16px 48px;
        background: linear-gradient(135deg, #9b59ff 0%, #ff2dcb 100%);
        border: none;
        font-size: 14px;
        font-weight: 800;
        letter-spacing: 0.15em;
        color: white;
        cursor: pointer;
        overflow: hidden;
        transition: all 0.3s ease;
        /* Clipped corners like breachbunny */
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
        filter: blur(15px);
        opacity: 0.5;
        animation: glow-pulse 2s ease-in-out infinite;
        z-index: -1;
    }

    @keyframes glow-pulse {
        0%,
        100% {
            opacity: 0.4;
            transform: scale(0.95);
        }
        50% {
            opacity: 0.7;
            transform: scale(1.05);
        }
    }

    .btn-text {
        position: relative;
        z-index: 1;
    }

    .activate-btn:hover {
        transform: scale(1.05);
        box-shadow: 0 0 40px rgba(255, 45, 203, 0.5);
    }

    .activate-btn:active {
        transform: scale(0.98);
    }

    /* Activated Message */
    .activated-message {
        display: flex;
        align-items: center;
        gap: var(--space-2);
        padding: 12px 24px;
        background: rgba(0, 255, 136, 0.1);
        border: 1px solid rgba(0, 255, 136, 0.3);
        border-radius: 8px;
        font-size: 14px;
        font-weight: 600;
        color: #00ff88;
    }

    .check-icon {
        font-size: 16px;
    }

    /* Bottom Note */
    .bottom-note {
        position: absolute;
        bottom: var(--space-6);
        font-size: 11px;
        color: rgba(255, 255, 255, 0.25);
        font-family: monospace;
    }
</style>
