<!-- DCasino.svelte -->
<script>
    import { fly } from "svelte/transition";
    import { Trophy, Target, History } from "lucide-svelte";
    export let active = false;
</script>

{#if active}
    <div class="d-casino" in:fly={{ y: 20, duration: 400 }}>
        <h2 class="section-title">CASINO</h2>

        <!-- Live Jackpot -->
        <div class="jackpot-card">
            <span class="label">LIVE JACKPOT</span>
            <div class="amount">₩ 4,823,940,200</div>
            <div class="timer">최근 당첨: 12분 전</div>
        </div>

        <!-- Points Summary -->
        <div class="points-summary">
            <div class="point-item">
                <span class="label">오늘의 승리</span>
                <span class="value win">+ ₩ 2,500,000</span>
            </div>
            <div class="divider"></div>
            <div class="point-item">
                <span class="label">마이 포인트</span>
                <span class="value">12,500 P</span>
            </div>
        </div>

        <!-- Missions -->
        <h3 class="subtitle">오늘의 미션</h3>
        <div class="mission-list">
            <div class="mission-item completed">
                <div class="check">✓</div>
                <div class="info">
                    <div class="title-row">
                        <span class="title">일일 출석 체크</span>
                        <span class="reward">100 P</span>
                    </div>
                    <div class="progress-bar-bg">
                        <div
                            class="progress-bar-fill"
                            style="width: 100%"
                        ></div>
                    </div>
                </div>
            </div>
            <div class="mission-item">
                <div class="check empty"></div>
                <div class="info">
                    <div class="title-row">
                        <span class="title">슬롯 게임 3회 플레이</span>
                        <span class="reward">500 P</span>
                    </div>
                    <div class="progress-bar-bg">
                        <div class="progress-bar-fill" style="width: 33%"></div>
                    </div>
                    <span class="progress-text">1/3 완료</span>
                </div>
            </div>
        </div>
    </div>
{/if}

<style>
    .d-casino {
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

    .jackpot-card {
        background: linear-gradient(
            135deg,
            #b8942d 0%,
            #f3e5ab 50%,
            #b8942d 100%
        );
        border-radius: var(--radius-xl);
        padding: var(--space-5);
        text-align: center;
        color: #000;
        box-shadow: 0 10px 30px rgba(212, 175, 55, 0.3);
        margin-bottom: var(--space-6);
    }

    .jackpot-card .label {
        font-size: var(--font-size-sm);
        font-weight: 700;
        letter-spacing: 2px;
        opacity: 0.8;
    }

    .jackpot-card .amount {
        font-size: var(--font-size-3xl);
        font-weight: 800;
        margin: var(--space-2) 0;
        font-variant-numeric: tabular-nums;
    }

    .jackpot-card .timer {
        font-size: var(--font-size-sm);
        font-weight: 600;
        background: rgba(0, 0, 0, 0.1);
        display: inline-block;
        padding: var(--space-1) var(--space-3);
        border-radius: var(--radius-full);
    }

    .points-summary {
        display: flex;
        justify-content: space-between;
        background: var(--color-bg-secondary);
        padding: var(--space-4);
        border-radius: var(--radius-lg);
        margin-bottom: var(--space-6);
        border: 1px solid var(--color-surface-border);
    }

    .point-item {
        display: flex;
        flex-direction: column;
        align-items: center;
        flex: 1;
    }

    .point-item .label {
        font-size: var(--font-size-xs);
        color: var(--color-text-secondary);
        margin-bottom: var(--space-1);
    }

    .point-item .value {
        font-size: var(--font-size-lg);
        font-weight: 700;
    }

    .divider {
        width: 1px;
        background: var(--color-surface-border);
    }

    .subtitle {
        font-size: var(--font-size-lg);
        margin-bottom: var(--space-3);
        color: var(--color-text-secondary);
    }

    .mission-list {
        display: flex;
        flex-direction: column;
        gap: var(--space-3);
    }

    .mission-item {
        display: flex;
        align-items: center;
        background: var(--color-surface);
        padding: var(--space-3);
        border-radius: var(--radius-lg);
        gap: var(--space-3);
    }

    .check {
        width: 24px;
        height: 24px;
        border-radius: 50%;
        background: var(--color-primary);
        color: #000;
        display: flex;
        align-items: center;
        justify-content: center;
        font-weight: bold;
        font-size: 12px;
    }

    .check.empty {
        background: none;
        border: 2px solid var(--color-text-tertiary);
    }

    .info {
        flex: 1;
        display: flex;
        flex-direction: column;
        gap: 6px;
    }

    .title-row {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .info .title {
        font-size: var(--font-size-sm);
        font-weight: 500;
    }

    .info .reward {
        font-size: var(--font-size-xs);
        color: var(--color-primary);
        font-weight: 700;
    }

    .progress-bar-bg {
        width: 100%;
        height: 4px;
        background: rgba(255, 255, 255, 0.1);
        border-radius: 2px;
        overflow: hidden;
    }

    .progress-bar-fill {
        height: 100%;
        background: var(--color-primary);
        border-radius: 2px;
        transition: width 0.3s ease;
    }

    .progress-text {
        font-size: 10px;
        color: var(--color-text-tertiary);
    }
</style>
