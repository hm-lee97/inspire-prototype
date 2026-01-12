<!--
  Welcome.svelte
  Refined Concept - Scene 1: 환대
  Reference: blesscollectionhotels.com (minimal luxury, serif typography, whitespace)
-->
<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, fly } from "svelte/transition";
    import { quintOut } from "svelte/easing";

    const dispatch = createEventDispatcher();

    let showContent = false;
    let greeting = "";

    onMount(() => {
        const hour = new Date().getHours();
        if (hour < 12) greeting = "Good Morning";
        else if (hour < 18) greeting = "Good Afternoon";
        else greeting = "Good Evening";

        setTimeout(() => (showContent = true), 400);
    });
</script>

<div class="welcome-scene">
    {#if showContent}
        <!-- Decorative top element -->
        <div class="decor-top" in:fade={{ duration: 1000 }}>
            <div class="decor-line"></div>
            <div class="decor-diamond">◆</div>
            <div class="decor-line"></div>
        </div>

        <!-- Main greeting -->
        <div class="greeting-section">
            <p
                class="greeting-label"
                in:fly={{ y: 20, duration: 800, delay: 200 }}
            >
                {greeting}
            </p>
            <h1
                class="guest-name"
                in:fly={{ y: 30, duration: 1000, delay: 400 }}
            >
                김민수<span class="honorific">님</span>
            </h1>
            <p
                class="welcome-msg"
                in:fly={{ y: 20, duration: 800, delay: 600 }}
            >
                A warm welcome to<br />The Signature Collection
            </p>
        </div>

        <!-- Elegant divider -->
        <div class="elegant-divider" in:fade={{ duration: 800, delay: 800 }}>
            <span class="divider-text">Your Reservation</span>
        </div>

        <!-- Stay card (editorial magazine style) -->
        <div class="stay-card" in:fly={{ y: 40, duration: 800, delay: 1000 }}>
            <div class="card-accent"></div>

            <div class="card-row header-row">
                <span class="card-label">Suite</span>
                <span class="suite-name">Signature Suite</span>
            </div>

            <div class="card-divider"></div>

            <div class="card-row dates-row">
                <div class="date-block">
                    <span class="date-label">Arrival</span>
                    <span class="date-day">09</span>
                    <span class="date-month">January</span>
                </div>

                <div class="stay-duration">
                    <span class="duration-line"></span>
                    <span class="duration-text">2 nights</span>
                    <span class="duration-line"></span>
                </div>

                <div class="date-block">
                    <span class="date-label">Departure</span>
                    <span class="date-day">11</span>
                    <span class="date-month">January</span>
                </div>
            </div>

            <div class="card-divider"></div>

            <div class="card-row room-row">
                <span class="room-label">Room</span>
                <span class="room-number">2801</span>
            </div>
        </div>

        <!-- Services grid (minimal icons) -->
        <div class="services-grid" in:fade={{ duration: 600, delay: 1200 }}>
            <button class="service-item" on:click={() => dispatch("next")}>
                <span class="service-icon">✦</span>
                <span class="service-name">Dining</span>
            </button>
            <button class="service-item">
                <span class="service-icon">✦</span>
                <span class="service-name">Concierge</span>
            </button>
            <button class="service-item">
                <span class="service-icon">✦</span>
                <span class="service-name">Wellness</span>
            </button>
        </div>

        <!-- Bottom CTA -->
        <div class="bottom-cta" in:fly={{ y: 20, duration: 600, delay: 1400 }}>
            <button class="cta-btn" on:click={() => dispatch("next")}>
                <span class="cta-text">Explore Signature Dining</span>
                <span class="cta-arrow">→</span>
            </button>
        </div>

        <!-- Decorative bottom -->
        <div class="decor-bottom" in:fade={{ duration: 800, delay: 1600 }}>
            <span class="decor-text"
                >Est. 2023 · INSPIRE · Yeongjong Island</span
            >
        </div>
    {/if}
</div>

<style>
    .welcome-scene {
        width: 100%;
        height: 100%;
        background: var(--color-bg-primary);
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 60px 28px 40px;
        position: relative;
    }

    /* Decorative top */
    .decor-top {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-bottom: 32px;
    }

    .decor-line {
        width: 24px;
        height: 1px;
        background: var(--color-primary);
        opacity: 0.5;
    }

    .decor-diamond {
        font-size: 8px;
        color: var(--color-primary);
    }

    /* Greeting section */
    .greeting-section {
        text-align: center;
        margin-bottom: 32px;
    }

    .greeting-label {
        font-family: var(--font-sans);
        font-size: 11px;
        font-weight: 500;
        letter-spacing: 0.25em;
        text-transform: uppercase;
        color: var(--color-primary);
        margin-bottom: 8px;
    }

    .guest-name {
        font-family: var(--font-serif);
        font-size: 42px;
        font-weight: 300;
        color: var(--color-text-primary);
        letter-spacing: 0.02em;
        line-height: 1.1;
    }

    .honorific {
        font-size: 24px;
        color: var(--color-text-tertiary);
        font-weight: 300;
    }

    .welcome-msg {
        font-family: var(--font-serif);
        font-size: 14px;
        font-weight: 300;
        font-style: italic;
        color: var(--color-text-secondary);
        line-height: 1.6;
        margin-top: 12px;
    }

    /* Elegant divider */
    .elegant-divider {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        margin-bottom: 20px;
    }

    .divider-text {
        font-family: var(--font-sans);
        font-size: 9px;
        font-weight: 500;
        letter-spacing: 0.3em;
        text-transform: uppercase;
        color: var(--color-text-tertiary);
        padding: 0 16px;
        background: var(--color-bg-primary);
        position: relative;
    }

    .divider-text::before,
    .divider-text::after {
        content: "";
        position: absolute;
        top: 50%;
        width: 60px;
        height: 1px;
        background: var(--color-surface-border);
    }

    .divider-text::before {
        right: 100%;
    }
    .divider-text::after {
        left: 100%;
    }

    /* Stay card (Bless Hotels inspired - editorial) */
    .stay-card {
        width: 100%;
        background: var(--color-surface);
        border: 1px solid var(--color-surface-border);
        position: relative;
        padding: 24px;
        margin-bottom: 24px;
    }

    .card-accent {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        height: 2px;
        background: linear-gradient(
            90deg,
            var(--color-primary),
            var(--color-primary-light)
        );
    }

    .card-row {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .card-divider {
        height: 1px;
        background: var(--color-surface-border);
        margin: 16px 0;
    }

    .card-label {
        font-family: var(--font-sans);
        font-size: 9px;
        font-weight: 500;
        letter-spacing: 0.2em;
        text-transform: uppercase;
        color: var(--color-text-tertiary);
    }

    .suite-name {
        font-family: var(--font-serif);
        font-size: 16px;
        font-weight: 400;
        color: var(--color-text-primary);
        font-style: italic;
    }

    /* Dates row */
    .dates-row {
        justify-content: center;
        gap: 24px;
    }

    .date-block {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 2px;
    }

    .date-label {
        font-family: var(--font-sans);
        font-size: 9px;
        font-weight: 500;
        letter-spacing: 0.15em;
        text-transform: uppercase;
        color: var(--color-text-tertiary);
    }

    .date-day {
        font-family: var(--font-serif);
        font-size: 32px;
        font-weight: 300;
        color: var(--color-text-primary);
        line-height: 1;
    }

    .date-month {
        font-family: var(--font-serif);
        font-size: 12px;
        font-weight: 400;
        color: var(--color-text-secondary);
        font-style: italic;
    }

    .stay-duration {
        display: flex;
        align-items: center;
        gap: 8px;
    }

    .duration-line {
        width: 20px;
        height: 1px;
        background: var(--color-surface-border);
    }

    .duration-text {
        font-family: var(--font-sans);
        font-size: 10px;
        color: var(--color-primary);
        white-space: nowrap;
    }

    .room-label {
        font-family: var(--font-sans);
        font-size: 9px;
        font-weight: 500;
        letter-spacing: 0.2em;
        text-transform: uppercase;
        color: var(--color-text-tertiary);
    }

    .room-number {
        font-family: var(--font-serif);
        font-size: 20px;
        font-weight: 300;
        color: var(--color-text-primary);
        letter-spacing: 0.1em;
    }

    /* Services grid */
    .services-grid {
        display: flex;
        gap: 12px;
        margin-bottom: 24px;
    }

    .service-item {
        flex: 1;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 8px;
        padding: 16px 8px;
        background: transparent;
        border: 1px solid var(--color-surface-border);
        transition: all 0.3s ease;
    }

    .service-item:hover {
        border-color: var(--color-primary);
        background: rgba(201, 169, 98, 0.05);
    }

    .service-icon {
        font-size: 12px;
        color: var(--color-primary);
    }

    .service-name {
        font-family: var(--font-sans);
        font-size: 10px;
        font-weight: 500;
        letter-spacing: 0.1em;
        color: var(--color-text-secondary);
        text-transform: uppercase;
    }

    /* Bottom CTA */
    .bottom-cta {
        width: 100%;
        margin-bottom: 20px;
    }

    .cta-btn {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 12px;
        padding: 16px;
        background: var(--color-secondary);
        color: var(--color-text-inverse);
        font-family: var(--font-sans);
        font-size: 11px;
        font-weight: 500;
        letter-spacing: 0.15em;
        text-transform: uppercase;
        transition: all 0.3s ease;
    }

    .cta-btn:hover {
        background: var(--color-secondary-light);
    }

    .cta-arrow {
        transition: transform 0.3s;
    }

    .cta-btn:hover .cta-arrow {
        transform: translateX(4px);
    }

    /* Decorative bottom */
    .decor-bottom {
        margin-top: auto;
    }

    .decor-text {
        font-family: var(--font-sans);
        font-size: 9px;
        letter-spacing: 0.2em;
        color: var(--color-text-tertiary);
        opacity: 0.6;
    }
</style>
