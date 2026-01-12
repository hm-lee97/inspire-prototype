<!-- DMain.svelte - Minimalist Redesign -->
<script>
    import { fly, fade } from "svelte/transition";
    import { Eye, EyeOff, ChevronRight } from "lucide-svelte";

    export let active = false;

    // Privacy toggle
    let showPoints = false;
    const maskedPoints = "P ****,***";
    const realPoints = "P 1,250,500";

    // User data
    const user = {
        name: "James W.",
        resortTier: "PLATINUM",
        casinoTier: "BLACK",
    };

    function togglePoints() {
        showPoints = !showPoints;
    }
</script>

{#if active}
    <div class="d-main" in:fade={{ duration: 400 }}>
        <!-- Welcome Section -->
        <section class="welcome">
            <p class="greeting">Welcome back,</p>
            <h1 class="name">{user.name}</h1>
        </section>

        <!-- Unified Membership Card -->
        <section class="card-section">
            <div class="membership-card">
                <div class="card-inner">
                    <!-- Tiers -->
                    <div class="tiers">
                        <div class="tier resort">
                            <span class="tier-label">RESORT</span>
                            <span class="tier-value">{user.resortTier}</span>
                        </div>
                        <div class="tier-divider"></div>
                        <div class="tier casino">
                            <span class="tier-label">CASINO</span>
                            <span class="tier-value">{user.casinoTier}</span>
                        </div>
                    </div>

                    <!-- Points -->
                    <div class="points-row">
                        <div class="points-info">
                            <span class="points-label">보유 포인트</span>
                            <span class="points-value"
                                >{showPoints ? realPoints : maskedPoints}</span
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

                    <!-- Card Shimmer -->
                    <div class="shimmer"></div>
                </div>
            </div>
        </section>

        <!-- CTA -->
        <section class="cta-section">
            <button class="cta-btn">
                <span>PLATINUM 전용 혜택 확인</span>
                <ChevronRight size={18} />
            </button>
        </section>
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
        letter-spacing: 0.1em;
        text-transform: uppercase;
        margin-bottom: var(--space-2);
    }

    .name {
        font-size: var(--font-size-4xl);
        font-weight: 300;
        letter-spacing: 0.05em;
        background: var(--gradient-gold);
        -webkit-background-clip: text;
        background-clip: text;
        -webkit-text-fill-color: transparent;
    }

    /* Card Section */
    .card-section {
        flex: 0 0 auto;
        margin-bottom: var(--space-10);
    }

    .membership-card {
        background: linear-gradient(145deg, #1a1a1a 0%, #0a0a0a 100%);
        border: 1px solid rgba(212, 175, 55, 0.2);
        border-radius: var(--radius-2xl);
        padding: var(--space-6);
        position: relative;
        overflow: hidden;
        box-shadow:
            0 20px 40px -15px rgba(0, 0, 0, 0.5),
            0 0 0 1px rgba(212, 175, 55, 0.1);
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
        letter-spacing: 0.15em;
        color: var(--color-text-tertiary);
        margin-bottom: var(--space-1);
    }

    .tier-value {
        font-size: var(--font-size-lg);
        font-weight: 600;
        letter-spacing: 0.1em;
    }

    .tier.resort .tier-value {
        color: var(--color-primary);
    }

    .tier.casino .tier-value {
        color: #fff;
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
        background: rgba(255, 255, 255, 0.05);
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

    /* Shimmer */
    .shimmer {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: linear-gradient(
            135deg,
            transparent 0%,
            rgba(212, 175, 55, 0.03) 50%,
            transparent 100%
        );
        pointer-events: none;
    }

    /* CTA */
    .cta-section {
        text-align: center;
    }

    .cta-btn {
        display: inline-flex;
        align-items: center;
        gap: var(--space-2);
        background: transparent;
        border: 1px solid rgba(212, 175, 55, 0.3);
        color: var(--color-primary);
        padding: var(--space-3) var(--space-5);
        border-radius: var(--radius-full);
        font-size: var(--font-size-sm);
        letter-spacing: 0.05em;
        cursor: pointer;
        transition: all 0.2s;
    }

    .cta-btn:active {
        background: rgba(212, 175, 55, 0.1);
    }
</style>
