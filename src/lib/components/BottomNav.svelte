<!-- BottomNav.svelte -->
<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    function navigate(view) {
        dispatch("navigate", { view });
    }
</script>

<div class="bottom-nav">
    <button class="nav-item" on:click={() => navigate("home")} title="Home">
        <svg
            viewBox="0 0 24 24"
            width="24"
            height="24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            ><path d="M3 12l9-9 9 9v9a3 3 0 0 1-3 3h-12a3 3 0 0 1-3-3z" /></svg
        >
    </button>
    <button class="nav-item" on:click={() => navigate("casino")} title="Casino">
        <svg
            viewBox="0 0 24 24"
            width="24"
            height="24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            ><circle cx="12" cy="12" r="10" /><path d="M12 8v4l2 2" /></svg
        >
    </button>
    <button class="nav-item" on:click={() => navigate("map")} title="Map">
        <svg
            viewBox="0 0 24 24"
            width="24"
            height="24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            ><path d="M21 10c0 6-9 12-9 12s-9-6-9-12a9 9 0 0 1 18 0z" /><circle
                cx="12"
                cy="10"
                r="3"
            /></svg
        >
    </button>
    <button class="nav-item" on:click={() => navigate("my")} title="My">
        <svg
            viewBox="0 0 24 24"
            width="24"
            height="24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            ><path
                d="M20 21v-2a4 4 0 0 0-3-3.87M4 21v-2a4 4 0 0 1 3-3.87"
            /><circle cx="12" cy="7" r="4" /></svg
        >
    </button>
</div>

<style>
    .bottom-nav {
        position: absolute;
        bottom: 0;
        left: 0;
        right: 0;
        height: var(--tab-bar-height);
        display: flex;
        justify-content: space-around;
        align-items: center;
        background: rgba(255, 255, 255, 0.05);
        backdrop-filter: blur(10px);
        border-top: 1px solid rgba(255, 255, 255, 0.1);
        z-index: 10;
    }
    .nav-item {
        background: none;
        border: none;
        color: #fff;
        padding: var(--space-2);
        cursor: pointer;
    }
    .nav-item:hover {
        color: var(--color-primary);
    }
</style>
