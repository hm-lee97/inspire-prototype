<!-- BottomNav.svelte -->
<script>
    import { createEventDispatcher } from "svelte";
    import { Home, Wallet, QrCode, User } from "lucide-svelte";

    const dispatch = createEventDispatcher();

    export let activeView = "home";

    function navigate(view) {
        dispatch("navigate", { view });
    }
</script>

<div class="bottom-nav">
    <button
        class="nav-item"
        class:active={activeView === "home" || activeView === "main"}
        on:click={() => navigate("home")}
        title="Home"
    >
        <Home size={22} />
        <span>홈</span>
    </button>
    <button
        class="nav-item"
        class:active={activeView === "wallet"}
        on:click={() => navigate("wallet")}
        title="Wallet"
    >
        <Wallet size={22} />
        <span>혜택</span>
    </button>
    <button
        class="nav-item"
        class:active={activeView === "pass"}
        on:click={() => navigate("pass")}
        title="Pass"
    >
        <QrCode size={22} />
        <span>패스</span>
    </button>
    <button
        class="nav-item"
        class:active={activeView === "my"}
        on:click={() => navigate("my")}
        title="My"
    >
        <User size={22} />
        <span>MY</span>
    </button>
</div>

<style>
    .bottom-nav {
        flex-shrink: 0;
        height: var(--tab-bar-height);
        display: flex;
        justify-content: space-around;
        align-items: center;
        background: rgba(0, 0, 0, 0.95);
        backdrop-filter: blur(10px);
        border-top: 1px solid rgba(255, 255, 255, 0.1);
        z-index: 10;
    }

    .nav-item {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 2px;
        background: none;
        border: none;
        color: rgba(255, 255, 255, 0.5);
        padding: var(--space-2);
        cursor: pointer;
        transition: color 0.2s;
    }

    .nav-item span {
        font-size: 10px;
    }

    .nav-item:hover,
    .nav-item.active {
        color: var(--color-primary);
    }
</style>
