<!-- DApp.svelte -->
<script>
    import { createEventDispatcher } from "svelte";
    import { Bell } from "lucide-svelte";
    import { fly } from "svelte/transition";
    import BottomNav from "../../components/BottomNav.svelte";
    import DMain from "./DMain.svelte";
    import DWallet from "./DWallet.svelte";
    import DSecurePass from "./DSecurePass.svelte";
    import DCasino from "./DCasino.svelte";
    import DMy from "./DMy.svelte";
    import DNotifications from "./DNotifications.svelte";

    export let view = "main"; // 'main' | 'wallet' | 'pass' | 'casino' | 'my'

    // Normalize 'd' and 'home' to 'main'
    $: activeView = view === "d" || view === "home" ? "main" : view;

    const dispatch = createEventDispatcher();

    // Notification state
    let showNotifications = false;
    let unreadCount = 2;

    function handleBack() {
        dispatch("back");
    }

    function handleNav(event) {
        dispatch("navigate", event.detail);
    }

    function toggleNotifications() {
        showNotifications = !showNotifications;
    }

    function closeNotifications() {
        showNotifications = false;
    }
</script>

<div class="d-app">
    <!-- Background Blobs for Glassmorphism -->
    <div class="blob-container">
        <div class="blob blob-1"></div>
        <div class="blob blob-2"></div>
        <div class="blob blob-3"></div>
    </div>

    <header class="d-header">
        <button class="back-btn" on:click={handleBack} title="Exit to Selector">
            <svg
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
            >
                <path d="M19 12H5M12 19l-7-7 7-7" />
            </svg>
        </button>
        <div class="utility-menu">
            <button
                class="util-btn notification-btn"
                on:click={toggleNotifications}
            >
                <Bell size={20} />
                {#if unreadCount > 0}
                    <span class="badge"></span>
                {/if}
            </button>
        </div>
    </header>

    <main class="d-content">
        <DMain active={activeView === "main"} on:navigate={handleNav} />
        <DWallet active={activeView === "wallet"} />
        <DSecurePass active={activeView === "pass"} />
        <DCasino active={activeView === "casino"} />
        <DMy active={activeView === "my"} />
    </main>

    <BottomNav {activeView} on:navigate={handleNav} />

    <!-- Notification Panel -->
    <DNotifications isOpen={showNotifications} on:close={closeNotifications} />
</div>

<style>
    .d-app {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        background: var(--color-bg-primary);
        color: var(--color-text-primary);
        overflow: hidden;
        font-family: "Archivo", sans-serif;
    }

    .d-header {
        flex-shrink: 0;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: calc(var(--safe-area-top) + 10px) 20px 10px;
        z-index: 10;
    }

    .back-btn,
    .util-btn {
        background: var(--color-surface);
        border: 1px solid var(--color-surface-border);
        color: #fff;
        width: 40px;
        height: 40px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        backdrop-filter: blur(10px);
        position: relative;
    }

    .utility-menu {
        display: flex;
        gap: 8px;
    }

    .d-content {
        flex: 1;
        overflow: hidden;
        position: relative;
        padding-bottom: calc(var(--safe-area-bottom) + 80px);
    }

    .back-btn:hover,
    .util-btn:hover {
        background: var(--color-surface-hover);
    }

    /* Notification Badge */
    .badge {
        position: absolute;
        top: 6px;
        right: 10px;
        width: 5px;
        height: 5px;
        background: #e3b83a;
        border-radius: 50%;
    }

    /* Background Blobs for Glassmorphism */
    .blob-container {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        overflow: hidden;
        pointer-events: none;
        z-index: 0;
    }

    .blob {
        position: absolute;
        border-radius: 50%;
        filter: blur(88px);
        opacity: 0.4;
    }

    .blob-1 {
        width: 300px;
        height: 300px;
        background: #1e3a5f;
        top: -100px;
        right: -50px;
    }

    .blob-2 {
        width: 300px;
        height: 300px;
        background: #d4af37;
        bottom: 120px;
        left: -60px;
        opacity: 0.12;
    }
</style>
