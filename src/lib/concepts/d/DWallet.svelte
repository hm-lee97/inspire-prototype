<!-- DWallet.svelte - Minimalist Card Focus -->
<script>
    import { fly, fade } from "svelte/transition";
    import { ChevronLeft, ChevronRight, Fingerprint } from "lucide-svelte";

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
    ];

    let currentIndex = 0;

    function next() {
        if (currentIndex < vouchers.length - 1) currentIndex++;
    }

    function prev() {
        if (currentIndex > 0) currentIndex--;
    }

    $: currentVoucher = vouchers[currentIndex];
</script>

{#if active}
    <div class="d-wallet" in:fade={{ duration: 400 }}>
        <!-- Header -->
        <header class="header">
            <h1>MY WALLET</h1>
            <p class="count">{currentIndex + 1} / {vouchers.length}</p>
        </header>

        <!-- Card Display -->
        <section class="card-display">
            <button
                class="nav-btn prev"
                on:click={prev}
                disabled={currentIndex === 0}
            >
                <ChevronLeft size={24} />
            </button>

            <div class="voucher-card">
                <div class="card-type {currentVoucher.type}"></div>
                <div class="card-content">
                    <span class="count-badge">{currentVoucher.count}x</span>
                    <h2>{currentVoucher.title}</h2>
                    <p class="location">{currentVoucher.location}</p>
                    <p class="expires">Expires: {currentVoucher.expires}</p>
                </div>
            </div>

            <button
                class="nav-btn next"
                on:click={next}
                disabled={currentIndex === vouchers.length - 1}
            >
                <ChevronRight size={24} />
            </button>
        </section>

        <!-- Use Button -->
        <section class="action-section">
            <button class="use-btn">
                <Fingerprint size={20} />
                <span>USE VOUCHER</span>
            </button>
            <p class="hint">Biometric authentication required</p>
        </section>
    </div>
{/if}

<style>
    .d-wallet {
        height: 100%;
        display: flex;
        flex-direction: column;
        padding: var(--space-6);
        color: var(--color-text-primary);
    }

    /* Header */
    .header {
        text-align: center;
        margin-bottom: var(--space-8);
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

    /* Card Display */
    .card-display {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: var(--space-4);
    }

    .nav-btn {
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
        transition: all 0.2s;
    }

    .nav-btn:disabled {
        opacity: 0.2;
        cursor: not-allowed;
    }

    .nav-btn:not(:disabled):active {
        background: rgba(255, 255, 255, 0.1);
    }

    /* Voucher Card */
    .voucher-card {
        flex: 1;
        max-width: 280px;
        background: linear-gradient(
            145deg,
            var(--color-bg-secondary) 0%,
            var(--color-bg-primary) 100%
        );
        border: 1px solid var(--color-surface-border);
        border-radius: 0;
        padding: var(--space-6);
        min-height: 320px;
        display: flex;
        flex-direction: column;
        position: relative;
        overflow: hidden;
    }

    .card-type {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        height: 4px;
    }

    .card-type.dining {
        background: var(--color-primary);
    }
    .card-type.casino {
        background: var(--color-primary-dark);
    }
    .card-type.spa {
        background: var(--color-text-secondary);
    }

    .card-content {
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: center;
        text-align: center;
    }

    .count-badge {
        display: inline-block;
        background: var(--color-primary);
        color: var(--color-text-inverse);
        font-size: var(--font-size-xs);
        font-weight: 700;
        padding: var(--space-1) var(--space-3);
        border-radius: 0;
        margin-bottom: var(--space-4);
        align-self: center;
    }

    .voucher-card h2 {
        font-size: var(--font-size-2xl);
        font-weight: 600;
        margin-bottom: var(--space-2);
    }

    .location {
        font-size: var(--font-size-sm);
        color: var(--color-text-secondary);
        margin-bottom: var(--space-4);
    }

    .expires {
        font-size: var(--font-size-xs);
        color: var(--color-text-tertiary);
    }

    /* Action */
    .action-section {
        text-align: center;
        padding-top: var(--space-6);
    }

    .use-btn {
        display: inline-flex;
        align-items: center;
        gap: var(--space-2);
        background: var(--color-primary);
        color: var(--color-text-inverse);
        border: none;
        padding: var(--space-4) var(--space-6);
        border-radius: 0;
        font-size: var(--font-size-base);
        font-weight: 600;
        cursor: pointer;
        margin-bottom: var(--space-2);
    }

    .hint {
        font-size: var(--font-size-xs);
        color: var(--color-text-tertiary);
    }
</style>
