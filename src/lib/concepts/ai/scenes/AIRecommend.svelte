<!--
  AIRecommend.svelte
  AI Concept - Scene 2: 제안
-->
<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, fly } from "svelte/transition";

    const dispatch = createEventDispatcher();
    let showContent = false;
    let selectedSuggestion = null;

    const suggestions = [
        {
            id: 1,
            icon: "🏊",
            title: "스플래시 베이",
            metric: "대기 5분",
            status: "good",
            reason: "현재 이용객 20% 감소",
        },
        {
            id: 2,
            icon: "🍜",
            title: "포메인",
            metric: "테이블 2개",
            status: "warning",
            reason: "12:30까지 예약 추천",
        },
        {
            id: 3,
            icon: "🎪",
            title: "오로라 쇼",
            metric: "30분 후",
            status: "neutral",
            reason: "좋은 자리 확보 가능",
        },
    ];

    onMount(() => setTimeout(() => (showContent = true), 200));
</script>

<div class="recommend-scene">
    {#if showContent}
        <header class="header" in:fly={{ y: -20, duration: 400 }}>
            <div class="ai-thinking">
                <span class="dot"></span><span class="dot"></span><span
                    class="dot"
                ></span><span class="text">AI 분석 중</span>
            </div>
            <h2 class="title">지금 추천해요</h2>
        </header>

        <div class="list">
            {#each suggestions as s, i}
                <button
                    class="card"
                    class:selected={selectedSuggestion?.id === s.id}
                    on:click={() => (selectedSuggestion = s)}
                    in:fly={{ y: 20, delay: 200 + i * 100 }}
                >
                    <div class="card-main">
                        <span class="icon">{s.icon}</span>
                        <div class="info"><h3>{s.title}</h3></div>
                        <span
                            class="metric"
                            class:good={s.status === "good"}
                            class:warning={s.status === "warning"}
                            >{s.metric}</span
                        >
                    </div>
                    <div class="reason">💡 AI: {s.reason}</div>
                </button>
            {/each}
        </div>

        {#if selectedSuggestion}
            <div class="action" in:fly={{ y: 20 }}>
                <span>{selectedSuggestion.icon} {selectedSuggestion.title}</span
                >
                <button class="btn" on:click={() => dispatch("next")}
                    >길안내 →</button
                >
            </div>
        {/if}
    {/if}
</div>

<style>
    .recommend-scene {
        width: 100%;
        height: 100%;
        padding: 48px 20px;
        display: flex;
        flex-direction: column;
        gap: 20px;
        overflow-y: auto;
    }
    .header {
        display: flex;
        flex-direction: column;
        gap: 12px;
    }
    .ai-thinking {
        display: flex;
        align-items: center;
        gap: 8px;
    }
    .dot {
        width: 6px;
        height: 6px;
        background: var(--color-primary);
        border-radius: 50%;
        animation: pulse 1.4s infinite;
    }
    .dot:nth-child(2) {
        animation-delay: 0.2s;
    }
    .dot:nth-child(3) {
        animation-delay: 0.4s;
    }
    @keyframes pulse {
        0%,
        80%,
        100% {
            opacity: 0.3;
        }
        40% {
            opacity: 1;
        }
    }
    .text {
        font-size: 12px;
        color: var(--color-text-tertiary);
    }
    .title {
        font-size: 24px;
        font-weight: 700;
        color: var(--color-text-primary);
    }
    .list {
        display: flex;
        flex-direction: column;
        gap: 12px;
        flex: 1;
    }
    .card {
        display: flex;
        flex-direction: column;
        gap: 12px;
        padding: 16px;
        background: var(--color-surface);
        border: 1px solid var(--color-surface-border);
        border-radius: 16px;
        text-align: left;
        transition: all 0.25s;
    }
    .card:hover,
    .card.selected {
        border-color: var(--color-primary);
        background: var(--gradient-ai-soft);
    }
    .card-main {
        display: flex;
        align-items: center;
        gap: 12px;
    }
    .icon {
        font-size: 24px;
        width: 48px;
        height: 48px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: var(--color-bg-secondary);
        border-radius: 12px;
    }
    .info {
        flex: 1;
    }
    .info h3 {
        font-size: 16px;
        font-weight: 600;
        color: var(--color-text-primary);
    }
    .metric {
        padding: 8px 12px;
        background: var(--color-bg-secondary);
        border-radius: 8px;
        font-size: 12px;
        font-weight: 500;
    }
    .metric.good {
        background: rgba(34, 211, 187, 0.15);
        color: #22d3bb;
    }
    .metric.warning {
        background: rgba(251, 191, 36, 0.15);
        color: #fbbf24;
    }
    .reason {
        font-size: 12px;
        color: var(--color-text-secondary);
        padding: 8px 12px;
        background: var(--color-bg-secondary);
        border-radius: 8px;
    }
    .action {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 16px;
        background: var(--color-surface);
        border: 1px solid var(--color-surface-border);
        border-radius: 16px;
    }
    .btn {
        padding: 12px 20px;
        background: var(--gradient-ai);
        color: white;
        border-radius: 12px;
        font-weight: 600;
    }
</style>
