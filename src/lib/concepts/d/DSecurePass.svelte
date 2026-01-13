<!-- DSecurePass.svelte - Minimalist QR Focus -->
<script>
    import { fade } from "svelte/transition";
    import { QrCode, Shield } from "lucide-svelte";
    import { onMount, onDestroy } from "svelte";

    export let active = false;

    let timeLeft = 60;
    let timerInterval;

    onMount(() => {
        if (active) startTimer();
    });

    onDestroy(() => {
        if (timerInterval) clearInterval(timerInterval);
    });

    function startTimer() {
        timeLeft = 60;
        if (timerInterval) clearInterval(timerInterval);
        timerInterval = setInterval(() => {
            timeLeft--;
            if (timeLeft <= 0) timeLeft = 60;
        }, 1000);
    }

    $: if (active && !timerInterval) startTimer();
    $: progressPercent = (timeLeft / 60) * 100;
</script>

{#if active}
    <div class="d-pass" in:fade={{ duration: 400 }}>
        <!-- Header -->
        <header class="header">
            <Shield size={16} />
            <h1>SECURE PASS</h1>
        </header>

        <!-- QR Section -->
        <section class="qr-section">
            <div class="qr-container">
                <div class="qr-code">
                    <QrCode size={140} strokeWidth={1} />
                </div>
                <div class="watermark">INSPIRE VIP</div>
            </div>

            <!-- Timer -->
            <div class="timer">
                <div class="timer-bar">
                    <div
                        class="timer-fill"
                        style="width: {progressPercent}%"
                    ></div>
                </div>
                <p>Auto-refresh in {timeLeft}s</p>
            </div>
        </section>

        <!-- Usage -->
        <section class="usage">
            <p>Casino Entry · POS Payment · VIP Check-in</p>
        </section>
    </div>
{/if}

<style>
    .d-pass {
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding: var(--space-6);
        color: var(--color-text-primary);
    }

    /* Header */
    .header {
        display: flex;
        align-items: center;
        gap: var(--space-2);
        margin-bottom: var(--space-10);
    }

    .header h1 {
        font-size: var(--font-size-sm);
        font-weight: 600;
        letter-spacing: 0.1em;
        color: var(--color-text-secondary);
    }

    .header :global(svg) {
        color: var(--color-primary);
    }

    /* QR Section */
    .qr-section {
        text-align: center;
    }

    .qr-container {
        position: relative;
        background: #fff;
        width: 200px;
        height: 200px;
        border-radius: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        margin-bottom: var(--space-6);
        box-shadow: 0 20px 40px -15px rgba(0, 0, 0, 0.3);
    }

    .qr-code {
        color: #000;
    }

    .watermark {
        position: absolute;
        bottom: 12px;
        right: 12px;
        font-size: 8px;
        font-weight: 700;
        letter-spacing: 0.05em;
        color: rgba(0, 0, 0, 0.2);
    }

    /* Timer */
    .timer {
        margin-bottom: var(--space-8);
    }

    .timer-bar {
        width: 200px;
        height: 3px;
        background: var(--color-surface-border);
        border-radius: 2px;
        overflow: hidden;
        margin-bottom: var(--space-2);
    }

    .timer-fill {
        height: 100%;
        background: var(--color-primary);
        transition: width 1s linear;
    }

    .timer p {
        font-size: var(--font-size-xs);
        color: var(--color-text-tertiary);
    }

    /* Usage */
    .usage p {
        font-size: var(--font-size-sm);
        color: var(--color-text-secondary);
        letter-spacing: 0.02em;
    }
</style>
