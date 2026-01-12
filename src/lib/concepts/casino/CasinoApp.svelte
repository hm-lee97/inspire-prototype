<!-- CasinoApp.svelte -->
<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    function goBack() {
        dispatch("back");
    }
</script>

<div class="casino-app">
    <h2>Casino</h2>
    <p>잭팟 금액 현황, 포인트 적립, 오늘의 미션</p>
    <button class="back-button" on:click={goBack}>Back</button>
</div>

<style>
    .casino-app {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        color: #fff;
        background: var(--color-bg-primary);
        padding: var(--space-5);
    }
    .back-button {
        margin-top: var(--space-4);
        padding: var(--space-2) var(--space-4);
        background: rgba(255, 255, 255, 0.1);
        border: none;
        border-radius: var(--radius-md);
        color: #fff;
        cursor: pointer;
        backdrop-filter: blur(10px);
        transition: background var(--transition-normal);
    }
    .back-button:hover {
        background: rgba(255, 255, 255, 0.2);
    }
</style>
