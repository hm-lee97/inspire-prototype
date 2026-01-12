<!--
  Curation.svelte
  Refined Concept - Scene 2: 탐색
  Editorial-style dining curation
-->
<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, fly, slide } from "svelte/transition";
    import { quintOut } from "svelte/easing";

    const dispatch = createEventDispatcher();

    let showContent = false;
    let activeCategory = "signature";

    const categories = [
        { id: "signature", label: "Signature" },
        { id: "casual", label: "Casual" },
        { id: "bar", label: "Bar & Lounge" },
    ];

    const restaurants = [
        {
            id: 1,
            name: "AZUR",
            type: "French Fine Dining",
            description: "미쉐린 스타 셰프가 선보이는 프렌치 정통 요리",
            rating: 4.9,
            priceLevel: "₩₩₩₩",
            image: "🇫🇷",
            featured: true,
        },
        {
            id: 2,
            name: "HINOKI",
            type: "Japanese Omakase",
            description: "계절의 정수를 담은 오마카세 경험",
            rating: 4.8,
            priceLevel: "₩₩₩₩",
            image: "🍣",
        },
        {
            id: 3,
            name: "THE GRILL",
            type: "Steakhouse",
            description: "프라임 등급 드라이에이징 스테이크",
            rating: 4.7,
            priceLevel: "₩₩₩",
            image: "🥩",
        },
    ];

    onMount(() => {
        setTimeout(() => (showContent = true), 200);
    });
</script>

<div class="curation-scene">
    {#if showContent}
        <!-- Header -->
        <header class="header" in:fly={{ y: -20, duration: 500 }}>
            <p class="section-label">Curated For You</p>
            <h2 class="section-title">Signature Dining</h2>
        </header>

        <!-- Category Tabs -->
        <div class="category-tabs" in:fade={{ duration: 400, delay: 200 }}>
            {#each categories as category}
                <button
                    class="tab-item"
                    class:active={activeCategory === category.id}
                    on:click={() => (activeCategory = category.id)}
                >
                    {category.label}
                </button>
            {/each}
        </div>

        <!-- Featured Card -->
        <div
            class="featured-card"
            in:fly={{ y: 20, duration: 500, delay: 300 }}
        >
            <div class="featured-image">
                <span class="featured-emoji">🍷</span>
                <div class="featured-overlay">
                    <span class="featured-badge">Editor's Pick</span>
                </div>
            </div>
            <div class="featured-content">
                <div class="featured-info">
                    <h3 class="featured-name">AZUR Restaurant</h3>
                    <p class="featured-type">French Fine Dining</p>
                    <p class="featured-desc">
                        지중해의 영감을 담은 시그니처 코스
                    </p>
                </div>
                <button
                    class="reserve-button"
                    on:click={() => dispatch("next")}
                >
                    예약하기
                </button>
            </div>
        </div>

        <!-- Restaurant List -->
        <div class="restaurant-list" in:fade={{ duration: 400, delay: 500 }}>
            {#each restaurants as restaurant, index}
                <button
                    class="restaurant-item"
                    in:fly={{ y: 20, duration: 400, delay: 600 + index * 100 }}
                >
                    <div class="item-image">
                        <span>{restaurant.image}</span>
                    </div>
                    <div class="item-content">
                        <div class="item-header">
                            <h4 class="item-name">{restaurant.name}</h4>
                            <span class="item-rating"
                                >★ {restaurant.rating}</span
                            >
                        </div>
                        <p class="item-type">{restaurant.type}</p>
                        <p class="item-price">{restaurant.priceLevel}</p>
                    </div>
                    <div class="item-arrow">
                        <svg
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="1.5"
                        >
                            <path d="M9 18l6-6-6-6" />
                        </svg>
                    </div>
                </button>
            {/each}
        </div>
    {/if}
</div>

<style>
    .curation-scene {
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

    /* Category Tabs */
    .category-tabs {
        display: flex;
        justify-content: center;
        gap: var(--space-1);
        padding: var(--space-1);
        background: var(--color-bg-secondary);
        border-radius: var(--radius-full);
    }

    .tab-item {
        flex: 1;
        padding: var(--space-2) var(--space-3);
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        font-weight: var(--font-weight-medium);
        color: var(--color-text-tertiary);
        background: transparent;
        border-radius: var(--radius-full);
        transition: all var(--transition-normal);
    }

    .tab-item.active {
        background: var(--color-surface);
        color: var(--color-text-primary);
        box-shadow: var(--shadow-refined);
    }

    /* Featured Card */
    .featured-card {
        background: var(--color-surface);
        border: 1px solid var(--color-surface-border);
        border-radius: var(--radius-2xl);
        overflow: hidden;
        box-shadow: var(--shadow-refined);
    }

    .featured-image {
        height: 140px;
        background: linear-gradient(135deg, #1a2642 0%, #2a3a5c 100%);
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
    }

    .featured-emoji {
        font-size: 48px;
    }

    .featured-overlay {
        position: absolute;
        top: var(--space-3);
        left: var(--space-3);
    }

    .featured-badge {
        padding: 4px 12px;
        background: var(--color-primary);
        color: white;
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        font-weight: var(--font-weight-medium);
        border-radius: var(--radius-full);
    }

    .featured-content {
        padding: var(--space-5);
        display: flex;
        justify-content: space-between;
        align-items: flex-end;
        gap: var(--space-4);
    }

    .featured-info {
        flex: 1;
    }

    .featured-name {
        font-family: var(--font-serif);
        font-size: var(--font-size-lg);
        font-weight: var(--font-weight-regular);
        color: var(--color-text-primary);
        margin-bottom: 2px;
    }

    .featured-type {
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        color: var(--color-primary);
        margin-bottom: var(--space-2);
    }

    .featured-desc {
        font-family: var(--font-serif);
        font-size: var(--font-size-sm);
        color: var(--color-text-secondary);
        line-height: var(--line-height-normal);
    }

    .reserve-button {
        padding: var(--space-3) var(--space-5);
        background: var(--color-secondary);
        color: var(--color-text-inverse);
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        font-weight: var(--font-weight-medium);
        border-radius: var(--radius-md);
        white-space: nowrap;
        transition: all var(--transition-normal);
    }

    .reserve-button:hover {
        background: var(--color-secondary-light);
    }

    /* Restaurant List */
    .restaurant-list {
        display: flex;
        flex-direction: column;
        gap: var(--space-3);
    }

    .restaurant-item {
        display: flex;
        align-items: center;
        gap: var(--space-4);
        padding: var(--space-4);
        background: var(--color-surface);
        border: 1px solid var(--color-surface-border);
        border-radius: var(--radius-xl);
        text-align: left;
        transition: all var(--transition-normal);
    }

    .restaurant-item:hover {
        border-color: var(--color-primary);
        box-shadow: var(--shadow-refined);
    }

    .item-image {
        width: 56px;
        height: 56px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: var(--color-bg-secondary);
        border-radius: var(--radius-lg);
        font-size: 28px;
    }

    .item-content {
        flex: 1;
    }

    .item-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 2px;
    }

    .item-name {
        font-family: var(--font-serif);
        font-size: var(--font-size-md);
        font-weight: var(--font-weight-regular);
        color: var(--color-text-primary);
    }

    .item-rating {
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        color: var(--color-primary);
    }

    .item-type {
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        color: var(--color-text-secondary);
        margin-bottom: 2px;
    }

    .item-price {
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        color: var(--color-text-tertiary);
    }

    .item-arrow {
        color: var(--color-text-tertiary);
        transition: all var(--transition-fast);
    }

    .item-arrow svg {
        width: 20px;
        height: 20px;
    }

    .restaurant-item:hover .item-arrow {
        color: var(--color-primary);
        transform: translateX(4px);
    }
</style>
