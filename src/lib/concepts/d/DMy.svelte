<!-- DMy.svelte - Minimalist Profile Focus -->
<script>
    import { fade } from "svelte/transition";
    import { Eye, EyeOff, ChevronRight, LogOut } from "lucide-svelte";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let active = false;

    // Privacy
    let showInfo = false;
    const maskedId = "****-****-**56";
    const realId = "8801-2345-6756";

    function toggleInfo() {
        showInfo = !showInfo;
    }

    function navigate(view) {
        dispatch("navigate", { view });
    }
</script>

{#if active}
    <div class="d-my" in:fade={{ duration: 400 }}>
        <!-- Profile Card -->
        <section class="profile-section">
            <div class="profile-card">
                <div class="avatar">
                    <span>JW</span>
                </div>
                <h1 class="name">James W.</h1>
                <div class="id-row">
                    <span class="id">{showInfo ? realId : maskedId}</span>
                    <button class="toggle-btn" on:click={toggleInfo}>
                        {#if showInfo}
                            <EyeOff size={16} />
                        {:else}
                            <Eye size={16} />
                        {/if}
                    </button>
                </div>
                <p class="tier">PLATINUM MEMBER</p>
            </div>
        </section>

        <!-- Menu -->
        <section class="menu-section">
            <button class="menu-item" on:click={() => navigate("reservations")}>
                <span>Reservations</span>
                <ChevronRight size={18} />
            </button>
            <button class="menu-item" on:click={() => navigate("settings")}>
                <span>Settings</span>
                <ChevronRight size={18} />
            </button>
        </section>

        <!-- Sign Out -->
        <button class="sign-out">
            <LogOut size={16} />
            <span>SIGN OUT</span>
        </button>
    </div>
{/if}

<style>
    .d-my {
        height: 100%;
        display: flex;
        flex-direction: column;
        padding: var(--space-6);
        color: var(--color-text-primary);
    }

    /* Profile */
    .profile-section {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .profile-card {
        text-align: center;
    }

    .avatar {
        width: 80px;
        height: 80px;
        background: var(--color-primary);
        border-radius: var(--radius-sm);
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 0 auto var(--space-4);
        font-size: var(--font-size-2xl);
        font-weight: 600;
        color: var(--color-text-inverse);
        letter-spacing: 0.05em;
    }

    .name {
        font-size: var(--font-size-2xl);
        font-weight: 300;
        margin-bottom: var(--space-3);
        letter-spacing: 0.05em;
        text-transform: uppercase;
    }

    .id-row {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: var(--space-2);
        margin-bottom: var(--space-3);
    }

    .id {
        font-size: var(--font-size-sm);
        color: var(--color-text-tertiary);
        font-variant-numeric: tabular-nums;
    }

    .toggle-btn {
        background: none;
        border: none;
        color: var(--color-text-tertiary);
        padding: var(--space-1);
        cursor: pointer;
    }

    .tier {
        font-size: var(--font-size-xs);
        color: var(--color-primary);
        letter-spacing: 0.08em;
    }

    /* Menu */
    .menu-section {
        display: flex;
        flex-direction: column;
        gap: var(--space-2);
        margin-bottom: var(--space-6);
    }

    .menu-item {
        display: flex;
        align-items: center;
        justify-content: space-between;
        background: rgba(255, 255, 255, 0.03);
        border: none;
        color: var(--color-text-primary);
        padding: var(--space-4);
        border-radius: var(--radius-md);
        cursor: pointer;
        transition: background 0.2s;
    }

    .menu-item:active {
        background: rgba(255, 255, 255, 0.06);
    }

    .menu-item :global(svg) {
        color: var(--color-text-tertiary);
    }

    /* Sign Out */
    .sign-out {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: var(--space-2);
        background: none;
        border: 1px solid var(--color-surface-border);
        color: var(--color-text-secondary);
        padding: var(--space-3);
        border-radius: var(--radius-md);
        cursor: pointer;
        letter-spacing: 0.05em;
        text-transform: uppercase;
    }
</style>
