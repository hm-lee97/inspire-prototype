<!-- DApp.svelte -->
<script>
    import { createEventDispatcher } from "svelte";
    import { Globe, Bell } from "lucide-svelte";
    import { fly } from "svelte/transition";
    import BottomNav from "../../components/BottomNav.svelte";
    import DMain from "./DMain.svelte";
    import DWallet from "./DWallet.svelte";
    import DSecurePass from "./DSecurePass.svelte";
    import DCasino from "./DCasino.svelte";
    import DMy from "./DMy.svelte";

    export let view = "main"; // 'main' | 'wallet' | 'pass' | 'casino' | 'my'

    // Normalize 'd' and 'home' to 'main'
    $: activeView = view === "d" || view === "home" ? "main" : view;

    const dispatch = createEventDispatcher();

    function handleBack() {
        dispatch("back");
    }

    function handleNav(event) {
        dispatch("navigate", event.detail);
    }
</script>

<div class="d-app">
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
            <button class="util-btn">
                <Globe size={20} />
            </button>
            <button class="util-btn">
                <Bell size={20} />
            </button>
        </div>
    </header>

    <main class="d-content">
        <DMain active={activeView === "main"} />
        <DWallet active={activeView === "wallet"} />
        <DSecurePass active={activeView === "pass"} />
        <DCasino active={activeView === "casino"} />
        <DMy active={activeView === "my"} />
    </main>

    <BottomNav {activeView} on:navigate={handleNav} />
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
        background: rgba(255, 255, 255, 0.1);
        border: 1px solid rgba(255, 255, 255, 0.1);
        color: #fff;
        width: 40px;
        height: 40px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        backdrop-filter: blur(10px);
    }

    .utility-menu {
        display: flex;
        gap: 8px;
    }

    .d-content {
        flex: 1;
        overflow: hidden;
        position: relative;
    }

    .back-btn:hover {
        background: rgba(255, 255, 255, 0.15);
    }
</style>
