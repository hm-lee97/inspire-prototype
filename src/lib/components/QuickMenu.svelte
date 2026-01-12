<!-- QuickMenu.svelte -->
<script>
    import {
        SquareParking,
        ClipboardList,
        CreditCard,
        Bell,
        Ticket,
    } from "lucide-svelte";
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    function handleClick(action) {
        dispatch("quick", { action });
    }
</script>

<div class="quick-menu">
    <button
        class="quick-item"
        on:click={() => handleClick("parking")}
        title="주차/셔틀 안내"
    >
        <SquareParking class="icon" />
    </button>
    <button
        class="quick-item"
        on:click={() => handleClick("prepare")}
        title="출입 준비물"
    >
        <ClipboardList class="icon" />
    </button>
    <button
        class="quick-item"
        on:click={() => handleClick("membership")}
        title="멤버십 사전 개설"
    >
        <CreditCard class="icon" />
    </button>
    <button
        class="quick-item"
        on:click={() => handleClick("alerts")}
        title="알림 설정"
    >
        <Bell class="icon" />
    </button>
    <button
        class="quick-item"
        on:click={() => handleClick("tickets")}
        title="티켓 목록"
    >
        <Ticket class="icon" />
    </button>
</div>

<style>
    .quick-menu {
        display: flex;
        justify-content: space-around;
        padding: var(--space-4) 0;
    }
    .quick-item {
        background: none;
        border: none;
        color: #fff;
        cursor: pointer;
        padding: var(--space-2);
    }
    .quick-item:hover .icon {
        color: var(--color-primary);
    }
    .icon {
        width: 24px;
        height: 24px;
    }
</style>
