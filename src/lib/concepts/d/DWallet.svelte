<!-- DWallet.svelte - Minimalist Card Focus -->
<script>
    import { fly, fade } from "svelte/transition";
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
    ];

    let currentIndex = 0;

    function next() {
        if (currentIndex < vouchers.length - 1) currentIndex++;
    }

    function prev() {
        if (currentIndex > 0) currentIndex--;
    }

    $: currentVoucher = vouchers[currentIndex];

    // Auth State
    let isAuthenticating = false;

    function handleUseVoucher() {
        isAuthenticating = true;
    }

    function handleAuthComplete(event) {
        if (event.detail.success) {
            // In a real app, this would consume the voucher
            isAuthenticating = false;
            alert("Voucher successfully used!");
        }
    }

    function handleAuthCancel() {
        isAuthenticating = false;
    }
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
                <div class="card-glow"></div>
                <div class="card-content">
                    <span class="count-badge">x{currentVoucher.count}</span>
                    <div class="card-info">
                        <h2>{currentVoucher.title}</h2>
                        <p class="location">{currentVoucher.location}</p>
                    </div>
                </div>
                <div class="ticket-footer">
                    <div class="barcode">
                        <QrCode size={48} />
                    </div>
                    <p class="expires">Valid until {currentVoucher.expires}</p>
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
        max-width: 320px;
        background: rgba(255, 255, 255, 0.03);
        backdrop-filter: blur(20px);
        -webkit-backdrop-filter: blur(20px);
        border-radius: 24px;
        padding: 0;
        display: flex;
        flex-direction: column;
        position: relative;
        overflow: hidden;
        box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
    }

    /* Gold Gradient Border Effect */
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
            rgba(212, 175, 55, 0.05) 0%,
            transparent 60%
        );
        pointer-events: none;
    }

    .card-content {
        flex: 1;
        padding: var(--space-6) var(--space-6);
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
        font-size: 28px;
        font-weight: 500;
        line-height: 1.2;
        color: #fff;
        margin: 0;
        background: linear-gradient(135deg, #fff 0%, #d4af37 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
    }

    .location {
        font-size: var(--font-size-md);
        color: rgba(255, 255, 255, 0.6);
        letter-spacing: 0.05em;
    }

    .ticket-footer {
        background: rgba(0, 0, 0, 0.2);
        padding: var(--space-6);
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: var(--space-3);
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
        background: var(--color-bg-primary); /* Match app background */
        border-radius: 50%;
    }

    .ticket-footer::before {
        left: -10px;
    }

    .ticket-footer::after {
        right: -10px;
    }

    .barcode {
        background: #fff;
        padding: 8px;
        border-radius: 8px;
        color: #000;
        display: flex; /* Centering fix */
    }

    .expires {
        font-size: var(--font-size-xs);
        color: rgba(255, 255, 255, 0.4);
        letter-spacing: 0.1em;
        text-transform: uppercase;
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
