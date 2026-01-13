<!-- DBenefitsSheet.svelte - Bottom Sheet for Platinum Benefits -->
<script>
    import { fly, fade } from "svelte/transition";
    import { X, Check } from "lucide-svelte";
    import { createEventDispatcher } from "svelte";

    export let isOpen = false;

    const dispatch = createEventDispatcher();

    const benefits = [
        {
            id: 1,
            title: "Complimentary Breakfast",
            desc: "Daily buffet at Chef's Kitchen",
        },
        {
            id: 2,
            title: "VIP Lounge Access",
            desc: "24/7 exclusive lounge entry",
        },
        { id: 3, title: "Spa Discount 20%", desc: "All treatments & packages" },
        {
            id: 4,
            title: "Priority Casino Seating",
            desc: "Reserved tables on request",
        },
        { id: 5, title: "Late Checkout", desc: "Extend your stay until 2 PM" },
        { id: 6, title: "Room Upgrade", desc: "Subject to availability" },
    ];

    function close() {
        dispatch("close");
    }
</script>

{#if isOpen}
    <!-- Backdrop -->
    <div
        class="backdrop"
        on:click={close}
        on:keydown={(e) => e.key === "Escape" && close()}
        role="button"
        tabindex="0"
        transition:fade={{ duration: 200 }}
    ></div>

    <!-- Bottom Sheet -->
    <div class="bottom-sheet" transition:fly={{ y: 400, duration: 350 }}>
        <div class="sheet-header">
            <div class="handle"></div>
            <h2>PLATINUM BENEFITS</h2>
            <button class="close-btn" on:click={close}>
                <X size={20} />
            </button>
        </div>

        <div class="sheet-content">
            <p class="subtitle">Your exclusive membership perks</p>

            <ul class="benefits-list">
                {#each benefits as benefit}
                    <li class="benefit-item">
                        <div class="check-icon">
                            <Check size={16} />
                        </div>
                        <div class="benefit-info">
                            <span class="benefit-title">{benefit.title}</span>
                            <span class="benefit-desc">{benefit.desc}</span>
                        </div>
                    </li>
                {/each}
            </ul>
        </div>
    </div>
{/if}

<style>
    .backdrop {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: rgba(0, 0, 0, 0.6);
        z-index: 100;
    }

    .bottom-sheet {
        position: absolute;
        bottom: 0;
        left: 0;
        right: 0;
        max-height: 75%;
        background: var(--color-bg-primary);
        border-top: 1px solid var(--color-surface-border);
        border-radius: 16px 16px 0 0;
        z-index: 101;
        display: flex;
        flex-direction: column;
        padding-bottom: calc(var(--safe-area-bottom) + var(--space-4));
    }

    .sheet-header {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: var(--space-4) var(--space-5);
        border-bottom: 1px solid var(--color-surface-border);
        position: relative;
    }

    .handle {
        width: 40px;
        height: 4px;
        background: var(--color-surface-border);
        border-radius: 2px;
        margin-bottom: var(--space-3);
    }

    .sheet-header h2 {
        font-size: var(--font-size-sm);
        font-weight: 600;
        letter-spacing: 0.1em;
        color: var(--color-text-primary);
    }

    .close-btn {
        position: absolute;
        right: var(--space-4);
        top: 50%;
        transform: translateY(-50%);
        background: none;
        border: none;
        color: var(--color-text-secondary);
        cursor: pointer;
        padding: var(--space-2);
    }

    .sheet-content {
        flex: 1;
        overflow-y: auto;
        padding: var(--space-5);
    }

    .subtitle {
        font-size: var(--font-size-sm);
        color: var(--color-text-secondary);
        text-align: center;
        margin-bottom: var(--space-6);
    }

    .benefits-list {
        list-style: none;
        padding: 0;
        margin: 0;
    }

    .benefit-item {
        display: flex;
        align-items: flex-start;
        gap: var(--space-3);
        padding: var(--space-4) 0;
        border-bottom: 1px solid var(--color-surface-border);
    }

    .benefit-item:last-child {
        border-bottom: none;
    }

    .check-icon {
        width: 24px;
        height: 24px;
        background: var(--color-primary);
        color: var(--color-text-inverse);
        border-radius: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-shrink: 0;
    }

    .benefit-info {
        display: flex;
        flex-direction: column;
        gap: 2px;
    }

    .benefit-title {
        font-size: var(--font-size-base);
        font-weight: 500;
        color: var(--color-text-primary);
    }

    .benefit-desc {
        font-size: var(--font-size-sm);
        color: var(--color-text-secondary);
    }
</style>
