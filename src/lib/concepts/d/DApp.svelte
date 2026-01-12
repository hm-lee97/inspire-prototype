<!-- DApp.svelte -->
<script>
    import { createEventDispatcher } from "svelte";
    import { Globe, Bell } from "lucide-svelte";
    import { fly } from "svelte/transition";
    import QuickMenu from "../../components/QuickMenu.svelte";
    import DMain from "./DMain.svelte";
    import DCasino from "./DCasino.svelte";
    import DMap from "./DMap.svelte";
    import DMy from "./DMy.svelte";

    export let view = "main"; // 'main' | 'casino' | 'map' | 'my'

    // Normalize 'd' to 'main'
    $: activeView = view === "d" ? "main" : view;

    const dispatch = createEventDispatcher();

    function handleBack() {
        dispatch("back");
    }
</script>

<div class="d-app" style="background: var(--color-bg-primary);">
    <div class="d-header">
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
    </div>

    <main class="d-content">
        <DMain active={activeView === "main"} />
        <DCasino active={activeView === "casino"} />
        <DMap active={activeView === "map"} />
        <DMy active={activeView === "my"} />
    </main>
</div>

<style>
    .d-app {
        width: 100%;
        height: 100%;
        position: relative;
        overflow: hidden;
        color: var(--color-text-primary);
    }

    .d-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: var(--safe-area-top) 20px 10px;
        z-index: 10;
        position: relative;
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
        height: 100%; /* Adjust as needed */
        position: relative;
    }

    .back-btn:hover {
        background: rgba(255, 255, 255, 0.1);
    }
</style>
