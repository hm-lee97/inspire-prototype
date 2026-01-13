<!-- DNotifications.svelte - In-App Notification Center -->
<script>
    import { fly, fade } from "svelte/transition";
    import { X, Gift, Calendar, Crown, Sparkles } from "lucide-svelte";
    import { createEventDispatcher } from "svelte";

    export let isOpen = false;

    const dispatch = createEventDispatcher();

    const notifications = [
        {
            id: 1,
            type: "benefit",
            icon: Gift,
            title: "PLATINUM EXCLUSIVE",
            message: "Complimentary breakfast voucher has arrived.",
            time: "2 min ago",
            unread: true,
        },
        {
            id: 2,
            type: "reservation",
            icon: Calendar,
            title: "RESERVATION CONFIRMED",
            message: "VIP Lounge dinner on Jan 15, 7:00 PM",
            time: "1 hour ago",
            unread: true,
        },
        {
            id: 3,
            type: "tier",
            icon: Crown,
            title: "TIER UPGRADE",
            message: "Congratulations! You've reached BLACK status.",
            time: "Yesterday",
            unread: false,
        },
        {
            id: 4,
            type: "promo",
            icon: Sparkles,
            title: "SPECIAL OFFER",
            message: "Double points on all casino games this weekend.",
            time: "2 days ago",
            unread: false,
        },
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

    <!-- Panel -->
    <div class="notification-panel" transition:fly={{ x: 300, duration: 300 }}>
        <header class="panel-header">
            <h2>NOTIFICATIONS</h2>
            <button class="close-btn" on:click={close}>
                <X size={20} />
            </button>
        </header>

        <div class="notification-list">
            {#each notifications as notif}
                <div class="notification-item" class:unread={notif.unread}>
                    <div class="icon-wrapper {notif.type}">
                        <svelte:component this={notif.icon} size={18} />
                    </div>
                    <div class="content">
                        <p class="title">{notif.title}</p>
                        <p class="message">{notif.message}</p>
                        <span class="time">{notif.time}</span>
                    </div>
                    {#if notif.unread}
                        <div class="unread-dot"></div>
                    {/if}
                </div>
            {/each}
        </div>

        <footer class="panel-footer">
            <button class="mark-read-btn">MARK ALL AS READ</button>
        </footer>
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

    .notification-panel {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        width: 100%;
        max-width: 100%;
        background: var(--color-bg-primary);
        border-left: none;
        z-index: 101;
        display: flex;
        flex-direction: column;
    }

    .panel-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: calc(var(--safe-area-top) + var(--space-3)) var(--space-5)
            var(--space-3);
        border-bottom: 1px solid var(--color-surface-border);
    }

    .panel-header h2 {
        font-size: var(--font-size-sm);
        font-weight: 600;
        letter-spacing: 0.08em;
        color: var(--color-text-primary);
    }

    .close-btn {
        background: none;
        border: none;
        color: var(--color-text-secondary);
        cursor: pointer;
        padding: var(--space-1);
    }

    .notification-list {
        flex: 1;
        overflow-y: auto;
        padding: var(--space-3);
    }

    .notification-item {
        display: flex;
        gap: var(--space-3);
        padding: var(--space-4);
        border-bottom: 1px solid var(--color-surface-border);
        position: relative;
        transition: background 0.2s;
    }

    .notification-item:hover {
        background: var(--color-surface);
    }

    .notification-item.unread {
        background: rgba(255, 255, 255, 0.03);
    }

    .icon-wrapper {
        width: 40px;
        height: 40px;
        border-radius: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-shrink: 0;
    }

    .icon-wrapper.benefit {
        background: var(--color-primary);
        color: var(--color-text-inverse);
    }

    .icon-wrapper.reservation {
        background: var(--color-accent);
        color: var(--color-text-primary);
    }

    .icon-wrapper.tier {
        background: linear-gradient(135deg, #1a1a2e 0%, #0a0a14 100%);
        color: var(--color-primary);
        border: 1px solid var(--color-surface-border);
    }

    .icon-wrapper.promo {
        background: var(--color-surface);
        color: var(--color-text-secondary);
        border: 1px solid var(--color-surface-border);
    }

    .content {
        flex: 1;
        min-width: 0;
    }

    .title {
        font-size: 11px;
        font-weight: 600;
        letter-spacing: 0.05em;
        color: var(--color-text-secondary);
        margin-bottom: 4px;
    }

    .message {
        font-size: var(--font-size-sm);
        color: var(--color-text-primary);
        line-height: 1.4;
        margin-bottom: 6px;
    }

    .time {
        font-size: 10px;
        color: var(--color-text-tertiary);
    }

    .unread-dot {
        width: 8px;
        height: 8px;
        background: var(--color-primary);
        border-radius: 50%;
        position: absolute;
        top: var(--space-4);
        right: var(--space-3);
    }

    .panel-footer {
        padding: var(--space-4) var(--space-4)
            calc(var(--safe-area-bottom) + var(--space-4));
        border-top: 1px solid var(--color-surface-border);
    }

    .mark-read-btn {
        width: 100%;
        background: transparent;
        border: 1px solid var(--color-surface-border);
        color: var(--color-text-secondary);
        padding: var(--space-3);
        font-size: 11px;
        font-weight: 500;
        letter-spacing: 0.05em;
        cursor: pointer;
        transition: all 0.2s;
    }

    .mark-read-btn:hover {
        background: var(--color-surface);
        color: var(--color-text-primary);
    }
</style>
