<!-- DMy.svelte -->
<script>
    import { fly } from "svelte/transition";
    import {
        Crown,
        ChevronRight,
        CreditCard,
        Ticket,
        Bell,
        Settings,
        LogOut,
    } from "lucide-svelte";
    export let active = false;
</script>

{#if active}
    <div class="d-my" in:fly={{ y: 20, duration: 400 }}>
        <h2 class="section-title">MY PAGE</h2>

        <!-- Membership Card -->
        <div class="membership-card">
            <div class="card-header">
                <span class="tier">DIAMOND</span>
                <Crown size={24} color="#d4af37" />
            </div>
            <div class="card-body">
                <div class="user-info">
                    <h3>김와일리</h3>
                    <p class="id">ID: 880123456</p>
                </div>
                <div class="points">
                    <span class="label">보유 포인트</span>
                    <span class="amount">12,500 P</span>
                </div>
            </div>
            <div class="card-footer">
                <div class="barcode">|| | ||| || |||</div>
                <span class="validity">2025.12.31 까지</span>
            </div>
            <div class="shimmer"></div>
        </div>

        <!-- Menu List -->
        <div class="menu-list">
            <button class="menu-item">
                <div class="item-icon"><CreditCard size={20} /></div>
                <span>예약 내역</span>
                <ChevronRight size={16} class="chevron" />
            </button>
            <button class="menu-item">
                <div class="item-icon"><Ticket size={20} /></div>
                <span>내 쿠폰</span>
                <span class="badge">2장 보유</span>
                <ChevronRight size={16} class="chevron" />
            </button>
            <button class="menu-item">
                <div class="item-icon"><Bell size={20} /></div>
                <span>알림함</span>
                <ChevronRight size={16} class="chevron" />
            </button>
            <button class="menu-item">
                <div class="item-icon"><Settings size={20} /></div>
                <span>설정</span>
                <ChevronRight size={16} class="chevron" />
            </button>
        </div>

        <button class="sign-out">
            <LogOut size={16} />
            <span>로그아웃</span>
        </button>
    </div>
{/if}

<style>
    .d-my {
        height: 100%;
        overflow-y: auto;
        padding: var(--space-4) var(--space-5) 100px;
        color: var(--color-text-primary);
    }

    .section-title {
        font-size: var(--font-size-2xl);
        font-weight: 700;
        margin-bottom: var(--space-4);
        letter-spacing: 2px;
    }

    .membership-card {
        background: linear-gradient(135deg, #1a1a1a 0%, #000 100%);
        border: 1px solid rgba(212, 175, 55, 0.3);
        border-radius: var(--radius-xl);
        padding: var(--space-5);
        height: 200px;
        position: relative;
        overflow: hidden;
        margin-bottom: var(--space-6);
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.5);
    }

    .card-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .tier {
        color: var(--color-primary);
        font-weight: 800;
        letter-spacing: 2px;
        font-size: var(--font-size-lg);
    }

    .user-info h3 {
        font-size: var(--font-size-xl);
        margin-bottom: 4px;
    }

    .user-info .id {
        font-size: var(--font-size-xs);
        color: var(--color-text-tertiary);
    }

    .points {
        text-align: right;
    }

    .points .label {
        font-size: 10px;
        color: var(--color-text-secondary);
        display: block;
    }

    .points .amount {
        font-size: var(--font-size-xl);
        font-weight: 700;
        color: var(--color-primary);
    }

    .card-body {
        display: flex;
        justify-content: space-between;
        align-items: flex-end;
    }

    .card-footer {
        display: flex;
        justify-content: space-between;
        align-items: flex-end;
        padding-top: var(--space-4);
        border-top: 1px solid rgba(255, 255, 255, 0.1);
    }

    .barcode {
        font-family: monospace;
        letter-spacing: 4px;
        opacity: 0.5;
        font-size: 10px;
    }

    .validity {
        font-size: 10px;
        color: var(--color-text-tertiary);
    }

    .membership-card::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: radial-gradient(
            circle at top right,
            rgba(212, 175, 55, 0.15),
            transparent 60%
        );
    }

    .shimmer {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: linear-gradient(
            to right,
            transparent 0%,
            rgba(255, 255, 255, 0.05) 50%,
            transparent 100%
        );
        transform: skewX(-20deg) translateX(-150%);
        animation: shimmer 3s infinite;
    }

    @keyframes shimmer {
        100% {
            transform: skewX(-20deg) translateX(150%);
        }
    }

    .menu-list {
        display: flex;
        flex-direction: column;
        gap: var(--space-2);
        margin-bottom: var(--space-6);
    }

    .menu-item {
        display: flex;
        align-items: center;
        background: var(--color-surface);
        padding: var(--space-4);
        border-radius: var(--radius-lg);
        color: var(--color-text-primary);
        border: none;
        width: 100%;
        text-align: left;
        cursor: pointer;
        transition: background 0.2s;
    }

    .menu-item:active {
        background: var(--color-surface-active);
    }

    .item-icon {
        width: 32px;
        height: 32px;
        border-radius: 50%;
        background: rgba(255, 255, 255, 0.05);
        display: flex;
        align-items: center;
        justify-content: center;
        margin-right: var(--space-3);
        color: var(--color-text-secondary);
    }

    .menu-item span {
        flex: 1;
        font-size: var(--font-size-base);
    }

    .badge {
        background: var(--color-primary);
        color: #000;
        font-size: 12px;
        font-weight: 700;
        padding: 2px 8px;
        border-radius: 10px;
    }

    .sign-out {
        width: 100%;
        background: none;
        border: 1px solid var(--color-text-tertiary);
        color: var(--color-text-secondary);
        padding: var(--space-3);
        border-radius: var(--radius-full);
        cursor: pointer;
    }
</style>
