<!--
  Concierge.svelte
  Refined Concept - Scene 3: 요청
  Elegant concierge service with slide gestures
-->
<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, fly, slide } from "svelte/transition";
    import { quintOut } from "svelte/easing";

    const dispatch = createEventDispatcher();

    let showContent = false;
    let selectedRequest = null;
    let requestSent = false;

    const requests = [
        {
            id: "wine",
            icon: "🍷",
            title: "와인 세팅",
            description: "객실에 샴페인 또는 와인을 준비해드립니다",
            options: ["Champagne", "Red Wine", "White Wine"],
        },
        {
            id: "turndown",
            icon: "🛏️",
            title: "턴다운 서비스",
            description: "편안한 취침을 위한 저녁 준비",
            options: ["6:00 PM", "8:00 PM", "10:00 PM"],
        },
        {
            id: "bath",
            icon: "🛁",
            title: "버블 바스",
            description: "아로마 에센셜 오일과 함께하는 프리미엄 바스",
            options: ["Lavender", "Rose", "Eucalyptus"],
        },
        {
            id: "breakfast",
            icon: "🥐",
            title: "조식 룸서비스",
            description: "객실에서 즐기는 시그니처 조식",
            options: ["7:00 AM", "8:00 AM", "9:00 AM"],
        },
    ];

    function selectRequest(request) {
        if (selectedRequest?.id === request.id) {
            selectedRequest = null;
        } else {
            selectedRequest = request;
        }
    }

    function sendRequest() {
        requestSent = true;
    }

    onMount(() => {
        setTimeout(() => (showContent = true), 200);
    });
</script>

<div class="concierge-scene">
    {#if showContent}
        <!-- Header -->
        <header class="header" in:fly={{ y: -20, duration: 500 }}>
            <p class="section-label">Personal Concierge</p>
            <h2 class="section-title">무엇을 도와드릴까요?</h2>
        </header>

        <!-- Request sent confirmation -->
        {#if requestSent}
            <div class="confirmation" in:fade={{ duration: 400 }}>
                <div class="confirmation-icon">✓</div>
                <h3 class="confirmation-title">요청이 접수되었습니다</h3>
                <p class="confirmation-text">
                    담당 컨시어지가 곧 연락드리겠습니다
                </p>
                <button
                    class="back-to-list"
                    on:click={() => {
                        requestSent = false;
                        selectedRequest = null;
                    }}
                >
                    다른 서비스 보기
                </button>
            </div>
        {:else}
            <!-- Request List -->
            <div class="request-list">
                {#each requests as request, index}
                    <div
                        class="request-item"
                        class:selected={selectedRequest?.id === request.id}
                        in:fly={{
                            y: 20,
                            duration: 400,
                            delay: 200 + index * 80,
                        }}
                    >
                        <button
                            class="request-header"
                            on:click={() => selectRequest(request)}
                        >
                            <div class="request-icon">{request.icon}</div>
                            <div class="request-info">
                                <h4 class="request-title">{request.title}</h4>
                                <p class="request-desc">
                                    {request.description}
                                </p>
                            </div>
                            <div class="request-toggle">
                                <svg
                                    viewBox="0 0 24 24"
                                    fill="none"
                                    stroke="currentColor"
                                    stroke-width="1.5"
                                >
                                    <path
                                        d={selectedRequest?.id === request.id
                                            ? "M18 15l-6-6-6 6"
                                            : "M6 9l6 6 6-6"}
                                    />
                                </svg>
                            </div>
                        </button>

                        {#if selectedRequest?.id === request.id}
                            <div
                                class="request-options"
                                transition:slide={{
                                    duration: 300,
                                    easing: quintOut,
                                }}
                            >
                                <p class="options-label">옵션 선택</p>
                                <div class="options-grid">
                                    {#each request.options as option}
                                        <button class="option-item">
                                            {option}
                                        </button>
                                    {/each}
                                </div>
                                <button
                                    class="send-request"
                                    on:click={sendRequest}
                                >
                                    요청하기
                                    <svg
                                        viewBox="0 0 24 24"
                                        fill="none"
                                        stroke="currentColor"
                                        stroke-width="1.5"
                                    >
                                        <path
                                            d="M22 2L11 13M22 2l-7 20-4-9-9-4 20-7z"
                                        />
                                    </svg>
                                </button>
                            </div>
                        {/if}
                    </div>
                {/each}
            </div>

            <!-- Quick Call -->
            <div class="quick-call" in:fade={{ duration: 400, delay: 600 }}>
                <div class="call-content">
                    <div class="call-icon">📞</div>
                    <div class="call-info">
                        <p class="call-label">직접 상담이 필요하신가요?</p>
                        <p class="call-number">컨시어지 연결</p>
                    </div>
                </div>
                <button class="call-button">
                    <svg
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="1.5"
                    >
                        <path
                            d="M22 16.92v3a2 2 0 01-2.18 2 19.79 19.79 0 01-8.63-3.07 19.5 19.5 0 01-6-6 19.79 19.79 0 01-3.07-8.67A2 2 0 014.11 2h3a2 2 0 012 1.72 12.84 12.84 0 00.7 2.81 2 2 0 01-.45 2.11L8.09 9.91a16 16 0 006 6l1.27-1.27a2 2 0 012.11-.45 12.84 12.84 0 002.81.7A2 2 0 0122 16.92z"
                        />
                    </svg>
                </button>
            </div>
        {/if}
    {/if}
</div>

<style>
    .concierge-scene {
        width: 100%;
        height: 100%;
        padding: var(--space-10) var(--grid-margin);
        display: flex;
        flex-direction: column;
        gap: var(--space-5);
        overflow-y: auto;
    }

    /* Header */
    .header {
        text-align: center;
    }

    .section-label {
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        font-weight: var(--font-weight-medium);
        color: var(--color-primary);
        letter-spacing: 0.15em;
        text-transform: uppercase;
        margin-bottom: var(--space-2);
    }

    .section-title {
        font-family: var(--font-serif);
        font-size: var(--font-size-2xl);
        font-weight: var(--font-weight-light);
        color: var(--color-text-primary);
    }

    /* Confirmation */
    .confirmation {
        flex: 1;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        text-align: center;
        gap: var(--space-4);
    }

    .confirmation-icon {
        width: 64px;
        height: 64px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: var(--gradient-gold);
        color: white;
        font-size: 28px;
        font-weight: var(--font-weight-bold);
        border-radius: 50%;
    }

    .confirmation-title {
        font-family: var(--font-serif);
        font-size: var(--font-size-xl);
        font-weight: var(--font-weight-regular);
        color: var(--color-text-primary);
    }

    .confirmation-text {
        font-family: var(--font-sans);
        font-size: var(--font-size-sm);
        color: var(--color-text-secondary);
    }

    .back-to-list {
        margin-top: var(--space-4);
        padding: var(--space-3) var(--space-6);
        background: transparent;
        border: 1px solid var(--color-surface-border);
        color: var(--color-text-secondary);
        font-family: var(--font-sans);
        font-size: var(--font-size-sm);
        border-radius: var(--radius-full);
        transition: all var(--transition-normal);
    }

    .back-to-list:hover {
        border-color: var(--color-primary);
        color: var(--color-primary);
    }

    /* Request List */
    .request-list {
        display: flex;
        flex-direction: column;
        gap: var(--space-3);
    }

    .request-item {
        background: var(--color-surface);
        border: 1px solid var(--color-surface-border);
        border-radius: var(--radius-xl);
        overflow: hidden;
        transition: all var(--transition-normal);
    }

    .request-item.selected {
        border-color: var(--color-primary);
        box-shadow: var(--shadow-refined);
    }

    .request-header {
        display: flex;
        align-items: center;
        gap: var(--space-4);
        padding: var(--space-4);
        width: 100%;
        text-align: left;
        background: transparent;
    }

    .request-icon {
        width: 48px;
        height: 48px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: var(--color-bg-secondary);
        border-radius: var(--radius-lg);
        font-size: 24px;
    }

    .request-info {
        flex: 1;
    }

    .request-title {
        font-family: var(--font-serif);
        font-size: var(--font-size-md);
        font-weight: var(--font-weight-regular);
        color: var(--color-text-primary);
        margin-bottom: 2px;
    }

    .request-desc {
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        color: var(--color-text-tertiary);
    }

    .request-toggle {
        color: var(--color-text-tertiary);
        transition: color var(--transition-fast);
    }

    .request-toggle svg {
        width: 20px;
        height: 20px;
    }

    .request-item.selected .request-toggle {
        color: var(--color-primary);
    }

    /* Request Options */
    .request-options {
        padding: 0 var(--space-4) var(--space-4);
        border-top: 1px solid var(--color-surface-border);
    }

    .options-label {
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        color: var(--color-text-tertiary);
        margin: var(--space-3) 0;
    }

    .options-grid {
        display: flex;
        gap: var(--space-2);
        flex-wrap: wrap;
        margin-bottom: var(--space-4);
    }

    .option-item {
        padding: var(--space-2) var(--space-4);
        background: var(--color-bg-secondary);
        border: 1px solid transparent;
        border-radius: var(--radius-full);
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        color: var(--color-text-secondary);
        transition: all var(--transition-normal);
    }

    .option-item:hover {
        border-color: var(--color-primary);
        color: var(--color-text-primary);
    }

    .send-request {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: var(--space-2);
        padding: var(--space-4);
        background: var(--color-secondary);
        color: var(--color-text-inverse);
        font-family: var(--font-sans);
        font-size: var(--font-size-sm);
        font-weight: var(--font-weight-medium);
        border-radius: var(--radius-lg);
        transition: all var(--transition-normal);
    }

    .send-request:hover {
        background: var(--color-secondary-light);
    }

    .send-request svg {
        width: 16px;
        height: 16px;
    }

    /* Quick Call */
    .quick-call {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: var(--space-4);
        background: linear-gradient(
            135deg,
            rgba(201, 169, 98, 0.1) 0%,
            rgba(217, 192, 138, 0.1) 100%
        );
        border: 1px solid rgba(201, 169, 98, 0.2);
        border-radius: var(--radius-xl);
        margin-top: auto;
    }

    .call-content {
        display: flex;
        align-items: center;
        gap: var(--space-3);
    }

    .call-icon {
        font-size: 24px;
    }

    .call-info {
        display: flex;
        flex-direction: column;
    }

    .call-label {
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        color: var(--color-text-tertiary);
    }

    .call-number {
        font-family: var(--font-serif);
        font-size: var(--font-size-sm);
        color: var(--color-text-primary);
    }

    .call-button {
        width: 44px;
        height: 44px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: var(--color-primary);
        color: white;
        border-radius: 50%;
        transition: all var(--transition-normal);
    }

    .call-button:hover {
        background: var(--color-primary-dark);
        transform: scale(1.05);
    }

    .call-button svg {
        width: 20px;
        height: 20px;
    }
</style>
