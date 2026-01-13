<!-- DBiometricAuth.svelte - Fingerprint Authentication Overlay -->
<script>
    import { fade, scale } from "svelte/transition";
    import { Fingerprint, Lock, ShieldCheck } from "lucide-svelte";
    import { createEventDispatcher, onMount } from "svelte";

    export let active = false;

    const dispatch = createEventDispatcher();

    let state = "idle"; // 'idle' | 'scanning' | 'success' | 'failed'
    let message = "Scan your fingerprint";

    // Reset state when active changes
    $: if (active) {
        state = "idle";
        message = "Scan your fingerprint";
    }

    function handleStartScan() {
        if (state !== "idle") return;

        state = "scanning";
        message = "Verifying...";

        // Simulate scanning process
        setTimeout(() => {
            state = "success";
            message = "Authenticated";

            // Dispatch success event after short delay
            setTimeout(() => {
                dispatch("complete", { success: true });
            }, 800);
        }, 1500);
    }

    function handleTouchStart() {
        if (state === "idle") handleStartScan();
    }

    function handleCancel() {
        dispatch("cancel");
    }
</script>

{#if active}
    <div class="auth-overlay" transition:fade={{ duration: 200 }}>
        <div class="auth-content">
            <div class="icon-container">
                <div
                    class="scan-ring"
                    class:scanning={state === "scanning"}
                    class:success={state === "success"}
                ></div>

                <button
                    class="fingerprint-btn"
                    on:click={handleStartScan}
                    on:touchstart|preventDefault={handleTouchStart}
                    class:scanning={state === "scanning"}
                    class:success={state === "success"}
                    disabled={state !== "idle"}
                >
                    {#if state === "success"}
                        <div in:scale>
                            <ShieldCheck size={48} />
                        </div>
                    {:else}
                        <div in:scale>
                            <Fingerprint size={48} />
                        </div>
                    {/if}
                </button>
            </div>

            <h2 class="auth-message">{message}</h2>

            {#if state === "idle"}
                <button class="cancel-btn" on:click={handleCancel}
                    >Cancel</button
                >
            {/if}
        </div>
    </div>
{/if}

<style>
    .auth-overlay {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: rgba(14, 28, 48, 0.15); /* Navy background with opacity */
        backdrop-filter: blur(10px);
        -webkit-backdrop-filter: blur(10px);
        z-index: 200;
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
    }

    .auth-content {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: var(--space-8);
        width: 100%;
        max-width: 320px;
    }

    .icon-container {
        position: relative;
        width: 120px;
        height: 120px;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .scan-ring {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        border: 2px solid rgba(255, 255, 255, 0.1);
        border-radius: 50%;
        transition: all 0.5s;
    }

    .scan-ring.scanning {
        border-color: #d4af37;
        border-top-color: transparent;
        animation: spin 1s linear infinite;
    }

    .scan-ring.success {
        border-color: #4caf50;
        background: rgba(76, 175, 80, 0.1);
    }

    .fingerprint-btn {
        background: none;
        border: none;
        color: rgba(255, 255, 255, 0.5);
        cursor: pointer;
        transition: all 0.3s;
        width: 80px;
        height: 80px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .fingerprint-btn:active,
    .fingerprint-btn.scanning {
        color: #d4af37;
        transform: scale(0.95);
    }

    .fingerprint-btn.success {
        color: #4caf50;
        transform: scale(1);
    }

    .auth-message {
        font-size: var(--font-size-lg);
        font-weight: 500;
        letter-spacing: 0.05em;
        text-align: center;
    }

    .cancel-btn {
        background: none;
        border: 1px solid rgba(255, 255, 255, 0.2);
        color: rgba(255, 255, 255, 0.6);
        padding: var(--space-3) var(--space-8);
        border-radius: 30px;
        font-size: var(--font-size-sm);
        cursor: pointer;
        transition: all 0.2s;
    }

    .cancel-btn:hover {
        background: rgba(255, 255, 255, 0.1);
        color: white;
    }

    @keyframes spin {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
</style>
