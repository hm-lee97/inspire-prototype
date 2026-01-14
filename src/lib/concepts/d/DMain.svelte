<!-- DMain.svelte - Minimalist Redesign -->
<script>
    import { fly, fade } from "svelte/transition";
    import { Eye, EyeOff, ChevronRight } from "lucide-svelte";
    import { createEventDispatcher } from "svelte";
    import DBenefitsSheet from "./DBenefitsSheet.svelte";

    const dispatch = createEventDispatcher();

    export let active = false;

    // Privacy toggle
    let showPoints = false;
    const maskedPoints = "P ****,***";
    const realPoints = "P 1,250,500";

    // Benefits sheet
    let showBenefits = false;

    // User data
    const user = {
        name: "James W.",
        resortTier: "PLATINUM",
        casinoTier: "BLACK",
    };

    function togglePoints() {
        showPoints = !showPoints;
    }

    function openBenefits() {
        showBenefits = true;
    }

    function closeBenefits() {
        showBenefits = false;
    }
</script>

{#if active}
    <div class="d-main" in:fade={{ duration: 400 }}>
        <!-- Welcome Section -->
        <section class="welcome animate-entry delay-1">
            <p class="greeting">Welcome back,</p>
            <h1 class="name">{user.name}</h1>
        </section>

        <!-- Unified Membership Card -->
        <section class="card-section animate-entry delay-1">
            <div class="card-perspective">
                <div class="membership-card animate-card-reveal">
                    <div class="card-inner">
                        <!-- Tiers -->
                        <div class="tiers">
                            <div class="tier resort">
                                <span class="tier-label">RESORT</span>
                                <span class="tier-value">{user.resortTier}</span
                                >
                            </div>
                            <div class="tier-divider"></div>
                            <div class="tier casino">
                                <span class="tier-label">CASINO</span>
                                <span class="tier-value">{user.casinoTier}</span
                                >
                            </div>
                        </div>

                        <!-- Points -->
                        <div class="points-row">
                            <div class="points-info">
                                <span class="points-label">MY POINTS</span>
                                <span class="points-value"
                                    >{showPoints
                                        ? realPoints
                                        : maskedPoints}</span
                                >
                            </div>
                            <button class="toggle-btn" on:click={togglePoints}>
                                {#if showPoints}
                                    <EyeOff size={20} />
                                {:else}
                                    <Eye size={20} />
                                {/if}
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Benefits Link -->
            <button class="benefits-link" on:click={openBenefits}>
                <span>View all benefits</span>
                <ChevronRight size={14} />
            </button>
        </section>

        <!-- Benefits Bottom Sheet -->
        <DBenefitsSheet isOpen={showBenefits} on:close={closeBenefits} />
    </div>
{/if}

<style>
    .d-main {
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        padding: var(--space-8) var(--space-6);
        color: var(--color-text-primary);
        overflow: hidden;
    }

    /* Welcome */
    .welcome {
        text-align: center;
        margin-bottom: var(--space-10);
    }

    .greeting {
        font-size: var(--font-size-sm);
        color: var(--color-text-secondary);
        letter-spacing: 0.05em;
        text-transform: uppercase;
    }

    .name {
        font-size: var(--font-size-4xl);
        font-weight: 300;
        letter-spacing: 0.005em;
        text-transform: uppercase;
        color: var(--color-text-primary);
    }

    /* Card Section */
    .card-section {
        flex: 0 0 auto;
        margin-bottom: var(--space-10);
    }

    .membership-card {
        background: rgba(255, 255, 255, 0.1);
        backdrop-filter: blur(40px);
        -webkit-backdrop-filter: blur(40px);
        border: 1px solid transparent;
        border-radius: var(--radius-md);
        padding: var(--space-6);
        position: relative;
        overflow: hidden;
        box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        background-clip: padding-box;
    }

    /* Gold-White Gradient Border */
    .membership-card::after {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        border-radius: inherit;
        padding: 1px;
        background: linear-gradient(
            205deg,
            rgba(212, 175, 55, 0.3) 0%,
            rgba(255, 255, 255, 0.1) 100%
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

    /* Frost Texture Overlay */
    .membership-card::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        pointer-events: none;
        z-index: 0;
        border-radius: inherit;
    }

    .card-inner {
        position: relative;
        z-index: 1;
    }

    /* Tiers */
    .tiers {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: var(--space-6);
        margin-bottom: var(--space-8);
    }

    .tier {
        text-align: center;
    }

    .tier-label {
        display: block;
        font-size: 10px;
        letter-spacing: 0.08em;
        color: var(--color-text-tertiary);
        margin-bottom: var(--space-1);
    }

    .tier-value {
        font-size: var(--font-size-lg);
        font-weight: 600;
        letter-spacing: 0.05em;
    }

    .tier.resort .tier-value {
        color: var(--color-primary);
    }

    .tier.casino .tier-value {
        color: var(--color-text-primary);
    }

    .tier-divider {
        width: 1px;
        height: 40px;
        background: rgba(255, 255, 255, 0.1);
    }

    /* Points */
    .points-row {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding-top: var(--space-6);
        border-top: 1px solid rgba(255, 255, 255, 0.05);
    }

    .points-label {
        display: block;
        font-size: var(--font-size-xs);
        color: var(--color-text-tertiary);
        margin-bottom: 2px;
    }

    .points-value {
        font-size: var(--font-size-2xl);
        font-weight: 600;
        color: var(--color-primary);
        font-variant-numeric: tabular-nums;
    }

    .toggle-btn {
        background: var(--color-surface);
        border: none;
        color: var(--color-text-secondary);
        width: 44px;
        height: 44px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        transition: background 0.2s;
    }

    .toggle-btn:active {
        background: rgba(255, 255, 255, 0.1);
    }

    /* Benefits Link */
    .benefits-link {
        display: flex;
        width: 100%;
        align-items: center;
        justify-content: center;
        gap: var(--space-1);
        background: none;
        border: none;
        color: var(--color-text-secondary);
        font-size: var(--font-size-sm);
        letter-spacing: 0.05em;
        cursor: pointer;
        margin-top: var(--space-4);
        transition: color 0.2s;
    }

    .benefits-link:hover {
        color: var(--color-text-primary);
    }

    /* Entry Animation */
    @keyframes slideUp {
        from {
            opacity: 0;
            transform: translateY(24px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }

    .animate-entry {
        animation: slideUp 0.6s ease-out forwards;
        opacity: 0;
    }

    .delay-1 {
        animation-delay: 0.1s;
    }

    /* 3D Card Reveal Animation (Chanel-inspired) */
    .card-perspective {
        perspective: 1000px;
    }

    @keyframes cardReveal {
        0% {
            opacity: 0;
            transform: rotateY(-45deg) translateZ(-80px);
        }
        100% {
            opacity: 1;
            transform: rotateY(0deg) translateZ(0);
        }
    }

    .animate-card-reveal {
        animation: cardReveal 1s cubic-bezier(0.16, 1, 0.3, 1) forwards;
        animation-delay: 0.15s;
        opacity: 0;
        transform-style: preserve-3d;
    }
</style>
