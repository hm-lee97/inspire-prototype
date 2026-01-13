<!-- BottomNav.svelte - Floating Chip Design -->
<script>
    import { createEventDispatcher } from "svelte";
    import { Home, Wallet, QrCode, User } from "lucide-svelte";

    const dispatch = createEventDispatcher();

    export let activeView = "home";

    function navigate(view) {
        dispatch("navigate", { view });
    }
</script>

<div class="nav-container">
    <div class="bottom-nav">
        <button
            class="nav-item"
            class:active={activeView === "home" || activeView === "main"}
            on:click={() => navigate("home")}
            title="Home"
        >
            <Home size={20} />
        </button>
        <button
            class="nav-item"
            class:active={activeView === "wallet"}
            on:click={() => navigate("wallet")}
            title="Benefits"
        >
            <Wallet size={20} />
        </button>
        <button
            class="nav-item"
            class:active={activeView === "pass"}
            on:click={() => navigate("pass")}
            title="Pass"
        >
            <QrCode size={20} />
        </button>
        <button
            class="nav-item"
            class:active={activeView === "my"}
            on:click={() => navigate("my")}
            title="My"
        >
            <User size={20} />
        </button>
    </div>
</div>

<style>
    .nav-container {
        position: absolute;
        bottom: calc(var(--safe-area-bottom) + 10px);
        left: 0;
        right: 0;
        display: flex;
        justify-content: center;
        z-index: 50;
        pointer-events: none;
    }

    .bottom-nav {
        display: flex;
        align-items: center;
        gap: var(--space-1);
        padding: var(--space-2) var(--space-3);
        background: rgba(255, 255, 255, 0.08);
        backdrop-filter: blur(20px);
        -webkit-backdrop-filter: blur(20px);
        border: 1px solid rgba(255, 255, 255, 0.1);
        border-radius: 50px;
        box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        pointer-events: auto;
    }

    .nav-item {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 48px;
        height: 48px;
        background: transparent;
        border: none;
        border-radius: 50%;
        color: rgba(255, 255, 255, 0.5);
        cursor: pointer;
        transition: all 0.2s ease;
    }

    .nav-item:hover {
        color: rgba(255, 255, 255, 0.8);
        background: rgba(255, 255, 255, 0.05);
    }

    .nav-item.active {
        color: #d4af37;
        background: rgba(255, 255, 255, 0.1);
    }
</style>
