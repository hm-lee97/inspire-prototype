<script>
    import { fly, fade } from "svelte/transition";
    import { spring } from "svelte/motion";
    import {
        ChevronLeft,
        ChevronRight,
        Fingerprint,
        QrCode,
    } from "lucide-svelte";
    import DBiometricAuth from "./DBiometricAuth.svelte";

    export let active = false;

    // Vouchers
    const vouchers = [
        {
            id: 1,
            title: "Complimentary Breakfast",
            location: "Chef's Kitchen",
            count: 2,
            expires: "2025.03.31",
            type: "dining",
        },
        {
            id: 2,
            title: "Casino VIP Dining",
            location: "VIP Lounge",
            count: 1,
            expires: "2025.02.28",
            type: "casino",
        },
        {
            id: 3,
            title: "Spa Voucher",
            location: "Inspire Spa",
            count: 1,
            expires: "2025.06.30",
            type: "spa",
        },
        {
            id: 4,
            title: "Arena Ticket",
            location: "Inspire Arena",
            count: 2,
            expires: "2025.04.15",
            type: "entertainment",
        },
        {
            id: 5,
            title: "Ocean Tower Stay",
            location: "Ocean Tower",
            count: 1,
            expires: "2025.12.31",
            type: "hotel",
        },
    ];

    // Use a larger set for the carousel feel
    const displayVouchers = [...vouchers, ...vouchers];

    let currentIndex = 2; // Start in middle for demo

    // Motion value for smooth snapping
    const progress = spring(currentIndex, {
        stiffness: 0.1,
        damping: 0.8,
    });

    let isDragging = false;
    let startX = 0;
    let startProgress = 0;
    let containerWidth = 0;

    // Sensitivity: how many pixels per card index change
    // Since we use degrees now, we map pixel distance to "index change" directly.
    // e.g. moving 150px = 1 card (18 degrees)
    const DRAG_SENSITIVITY = 180;

    function handleMousedown(e) {
        isDragging = true;
        startX = e.pageX;
        startProgress = $progress;

        window.addEventListener("mousemove", handleMousemove);
        window.addEventListener("mouseup", handleMouseup);
    }

    function handleMousemove(e) {
        if (!isDragging) return;
        const dx = e.pageX - startX;
        const indexChange = -dx / DRAG_SENSITIVITY;
        progress.set(startProgress + indexChange, { hard: true });
    }

    function handleMouseup() {
        isDragging = false;
        window.removeEventListener("mousemove", handleMousemove);
        window.removeEventListener("mouseup", handleMouseup);

        // Snap to nearest index
        const snapped = Math.round($progress);
        // Clamp
        const clamped = Math.max(
            0,
            Math.min(displayVouchers.length - 1, snapped),
        );

        progress.set(clamped, { hard: false }); // Animate to snap
        currentIndex = clamped;
    }

    // Touch support
    function handleTouchStart(e) {
        isDragging = true;
        startX = e.touches[0].pageX;
        startProgress = $progress;

        window.addEventListener("touchmove", handleTouchMove, {
            passive: false,
        });
        window.addEventListener("touchend", handleTouchEnd);
    }

    function handleTouchMove(e) {
        if (!isDragging) return;
        e.preventDefault(); // Prevent scroll
        const dx = e.touches[0].pageX - startX;
        const indexChange = -dx / DRAG_SENSITIVITY;
        progress.set(startProgress + indexChange, { hard: true });
    }

    function handleTouchEnd() {
        isDragging = false;
        window.removeEventListener("touchmove", handleTouchMove);
        window.removeEventListener("touchend", handleTouchEnd);

        const snapped = Math.round($progress);
        const clamped = Math.max(
            0,
            Math.min(displayVouchers.length - 1, snapped),
        );
        progress.set(clamped);
        currentIndex = clamped;
    }

    $: currentVoucher = displayVouchers[currentIndex] || displayVouchers[0];

    // Auth State
    let isAuthenticating = false;

    function handleUseVoucher() {
        isAuthenticating = true;
    }

    function handleAuthComplete(event) {
        if (event.detail.success) {
            isAuthenticating = false;
            alert("Voucher successfully used!");
        }
    }

    function handleAuthCancel() {
        isAuthenticating = false;
    }

    function handleKeydown(e) {
        if (e.key === "ArrowLeft") {
            e.preventDefault();
            const newIndex = Math.max(0, currentIndex - 1);
            progress.set(newIndex);
            currentIndex = newIndex;
        } else if (e.key === "ArrowRight") {
            e.preventDefault();
            const newIndex = Math.min(
                displayVouchers.length - 1,
                currentIndex + 1,
            );
            progress.set(newIndex);
            currentIndex = newIndex;
        }
    }
</script>

<svelte:window />

{#if active}
    <div class="d-wallet" in:fade={{ duration: 400 }}>
        <!-- Header -->
        <header class="header">
            <h1>MY WALLET</h1>
            <p class="count">{currentIndex + 1} / {displayVouchers.length}</p>
        </header>

        <!-- 3D Carousel Display -->
        <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
        <!-- svelte-ignore a11y-no-noninteractive-tabindex -->
        <div
            class="carousel-stage"
            bind:clientWidth={containerWidth}
            on:mousedown={handleMousedown}
            on:touchstart={handleTouchStart}
            role="region"
            aria-roledescription="carousel"
            aria-label="Voucher Carousel"
            tabindex="0"
            on:keydown={handleKeydown}
        >
            <div class="carousel-track">
                {#each displayVouchers as voucher, i}
                    <!-- 
                        2D Fan / Wheel Layout
                        Cards are placed on a large circle centered FAR BELOW the screen.
                        - They arch UP (center is highest).
                        - They rotate Z (tilt) to follow the curve.
                        - No 3D flipping (rotateY).
                    -->
                    {@const offset = i - $progress}
                    {@const absOffset = Math.abs(offset)}
                    {@const isActive = Math.round($progress) === i}

                    <!-- Config -->
                    {@const RADIUS = 1500}
                    <!-- Large radius for subtle curve -->
                    {@const ANGLE_STEP = 9}
                    <!-- Degrees per card -->

                    <!-- Calculate Angle -->
                    {@const thetaDeg = offset * ANGLE_STEP}
                    {@const thetaRad = thetaDeg * (Math.PI / 180)}

                    {@const x = RADIUS * Math.sin(thetaRad)}
                    {@const y = RADIUS * (1 - Math.cos(thetaRad))}

                    <!-- Rotation matches the angle on the circle -->
                    {@const rotateZ = thetaDeg}

                    {@const zIndex = 100 - Math.round(absOffset)}

                    <div
                        class="card-wrapper"
                        style:transform="
                        translate({x}px, {y}px) rotate({rotateZ}deg)
                        "
                        style:z-index={zIndex}
                        style:filter={isActive ? "none" : "brightness(0.95)"}
                        style:pointer-events={isActive ? "auto" : "none"}
                    >
                        <div class="voucher-card" class:active-card={isActive}>
                            <!-- Shine/Glow for active card -->
                            <div class="card-glow"></div>

                            <div class="card-content">
                                <span class="count-badge">x{voucher.count}</span
                                >
                                <div class="card-info">
                                    <h2>{voucher.title}</h2>
                                    <p class="location">{voucher.location}</p>
                                </div>
                            </div>

                            <div class="ticket-footer">
                                <div class="barcode">
                                    <QrCode size={48} />
                                </div>
                                <p class="expires">
                                    Valid until {voucher.expires}
                                </p>
                            </div>
                        </div>
                    </div>
                {/each}
            </div>
        </div>

        <!-- Use Button -->
        <section class="action-section">
            <button class="use-btn" on:click={handleUseVoucher}>
                <Fingerprint size={20} />
                <span>USE VOUCHER</span>
            </button>
            <p class="hint">Biometric authentication required</p>
        </section>

        <!-- Biometric Auth Overlay -->
        <DBiometricAuth
            active={isAuthenticating}
            on:complete={handleAuthComplete}
            on:cancel={handleAuthCancel}
        />
    </div>
{/if}

<style>
    .d-wallet {
        height: 100%;
        display: flex;
        flex-direction: column;
        padding: var(--space-6) 0;
        color: var(--color-text-primary);
        overflow: hidden;
    }

    /* Header */
    .header {
        text-align: center;
        margin-bottom: var(--space-4);
        flex-shrink: 0;
    }

    .header h1 {
        font-size: var(--font-size-lg);
        font-weight: 600;
        letter-spacing: 0.08em;
        margin-bottom: var(--space-2);
    }

    .header .count {
        font-size: var(--font-size-sm);
        color: var(--color-text-tertiary);
    }

    /* Carousel Stage */
    .carousel-stage {
        flex: 1;
        width: 100%;
        perspective: 800px; /* High perspective for 3D depth */
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: grab;
        touch-action: none;
        overflow: hidden;
    }

    .carousel-stage:active {
        cursor: grabbing;
    }

    .carousel-track {
        position: relative;
        width: 100%;
        height: 100%; /* Fill stage */
        transform-style: preserve-3d;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .card-wrapper {
        position: absolute;
        width: 200px; /* Reduced width */
        height: 300px; /* Reduced height */
        transform-style: preserve-3d;
        /* Center anchor */
        left: 50%;
        top: 50%;
        margin-left: -100px; /* Half width */
        margin-top: -150px; /* Half height */

        transition: opacity 0.1s;
        will-change: transform;
    }

    /* Voucher Card */
    .voucher-card {
        width: 100%;
        height: 100%;
        background: linear-gradient(
            135deg,
            rgba(30, 30, 30, 0.95),
            rgba(10, 10, 10, 0.98)
        );
        border-radius: 24px;
        display: flex;
        flex-direction: column;
        overflow: hidden;
        box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5);
        position: relative;
    }

    .voucher-card.active-card {
        box-shadow:
            0 25px 60px rgba(0, 0, 0, 0.6),
            0 0 30px rgba(212, 175, 55, 0.1);
    }

    /* Gold Gradient Border Effect via pseudo-element */
    .voucher-card::before {
        content: "";
        position: absolute;
        inset: 0;
        border-radius: 24px;
        padding: 1px;
        background: linear-gradient(
            135deg,
            rgba(255, 255, 255, 0.4) 0%,
            transparent 40%,
            transparent 60%,
            rgba(212, 175, 55, 0.8) 100%
        );
        -webkit-mask:
            linear-gradient(#fff 0 0) content-box,
            linear-gradient(#fff 0 0);
        mask:
            linear-gradient(#fff 0 0) content-box,
            linear-gradient(#fff 0 0);
        -webkit-mask-composite: xor;
        mask-composite: exclude;
        pointer-events: none;
    }

    .card-glow {
        position: absolute;
        top: -50%;
        left: -50%;
        width: 200%;
        height: 200%;
        background: radial-gradient(
            circle at center,
            rgba(212, 175, 55, 0.15) 0%,
            transparent 70%
        );
        pointer-events: none;
        opacity: 0;
        transition: opacity 0.3s;
    }

    .active-card .card-glow {
        opacity: 1;
    }

    .card-content {
        flex: 1;
        padding: var(--space-6);
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        text-align: center;
        gap: var(--space-4);
    }

    .count-badge {
        display: inline-flex;
        align-items: center;
        justify-content: center;
        background: rgba(212, 175, 55, 0.2);
        color: #d4af37;
        font-size: var(--font-size-sm);
        font-weight: 600;
        padding: 4px 12px;
        border-radius: 20px;
        border: 1px solid rgba(212, 175, 55, 0.3);
    }

    .voucher-card h2 {
        font-size: 24px;
        font-weight: 600;
        line-height: 1.2;
        color: #fff;
        margin: 0;
    }

    .location {
        font-size: var(--font-size-sm);
        color: rgba(255, 255, 255, 0.6);
        letter-spacing: 0.05em;
    }

    .ticket-footer {
        background: rgba(0, 0, 0, 0.3);
        padding: var(--space-4);
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: var(--space-2);
        border-top: 1px dashed rgba(255, 255, 255, 0.1);
        position: relative;
    }

    /* Ticket Notches */
    .ticket-footer::before,
    .ticket-footer::after {
        content: "";
        position: absolute;
        top: -10px;
        width: 20px;
        height: 20px;
        background: var(--color-bg-primary);
        border-radius: 50%;
        /* Ensure simple color matching since parent might be transparent in 3D but here we assume dark bg */
        background-color: #000;
    }

    .ticket-footer::before {
        left: -10px;
    }

    .ticket-footer::after {
        right: -10px;
    }

    .barcode {
        background: #fff;
        padding: 6px;
        border-radius: 6px;
        display: flex;
        color: #000;
    }

    .expires {
        font-size: 10px;
        color: rgba(255, 255, 255, 0.4);
        letter-spacing: 0.1em;
        text-transform: uppercase;
    }

    /* Action */
    .action-section {
        text-align: center;
        padding-top: var(--space-4);
        flex-shrink: 0;
        z-index: 10; /* Ensure button is clickable above 3D plane if overlap */
    }

    .use-btn {
        display: inline-flex;
        align-items: center;
        gap: var(--space-2);
        background: var(--color-primary);
        color: var(--color-text-inverse);
        border: none;
        padding: var(--space-3) var(--space-6);
        border-radius: 4px;
        font-size: var(--font-size-base);
        font-weight: 600;
        cursor: pointer;
        margin-bottom: var(--space-2);
        box-shadow: 0 4px 12px rgba(212, 175, 55, 0.3);
        transition: transform 0.2s;
    }

    .use-btn:active {
        transform: scale(0.95);
    }

    .hint {
        font-size: var(--font-size-xs);
        color: var(--color-text-tertiary);
    }
</style>
