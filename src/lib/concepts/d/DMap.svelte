<!-- DMap.svelte -->
<script>
    import { fly } from "svelte/transition";
    export let active = false;

    const categories = ["전체", "다이닝", "쇼핑", "아레나", "호텔", "시설"];
    let selectedCategory = "전체";
</script>

{#if active}
    <div class="d-map" in:fly={{ y: 20, duration: 400 }}>
        <h2 class="section-title">MAP & FACILITIES</h2>

        <!-- Categories -->
        <div class="categories">
            {#each categories as cat}
                <button
                    class="cat-chip"
                    class:selected={selectedCategory === cat}
                    on:click={() => (selectedCategory = cat)}
                >
                    {cat}
                </button>
            {/each}
        </div>

        <!-- Map Placeholder -->
        <div class="map-view">
            <div class="map-placeholder">
                <span>Interactive Map Loading...</span>
            </div>
        </div>

        <!-- Venue List -->
        <div class="venue-list">
            <div class="venue-card">
                <div class="venue-img"></div>
                <div class="venue-info">
                    <h4>셰프스 키친</h4>
                    <p>인터내셔널 뷔페 • 2F</p>
                    <span class="status open">영업중</span>
                </div>
            </div>
            <div class="venue-card">
                <div class="venue-img"></div>
                <div class="venue-info">
                    <h4>오로라 바</h4>
                    <p>라운지 & 바 • 1F</p>
                    <span class="status closed">영업종료</span>
                </div>
            </div>
            <div class="venue-card">
                <div class="venue-img"></div>
                <div class="venue-info">
                    <h4>Inspire Shop</h4>
                    <p>Souvenirs & Gifts</p>
                    <span class="status closed">Closed</span>
                </div>
            </div>
        </div>
    </div>
{/if}

<style>
    .d-map {
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

    .categories {
        display: flex;
        gap: var(--space-2);
        overflow-x: auto;
        padding-bottom: var(--space-2);
        margin-bottom: var(--space-4);
        scrollbar-width: none;
    }

    .categories::-webkit-scrollbar {
        display: none;
    }

    .cat-chip {
        background: var(--color-bg-secondary);
        border: 1px solid var(--color-surface-border);
        color: var(--color-text-secondary);
        padding: var(--space-2) var(--space-4);
        border-radius: var(--radius-full);
        white-space: nowrap;
        cursor: pointer;
        transition: all 0.2s;
    }

    .cat-chip.selected {
        background: var(--color-primary);
        color: #000;
        border-color: var(--color-primary);
        font-weight: 600;
    }

    .map-view {
        height: 200px;
        background: var(--color-bg-secondary);
        border-radius: var(--radius-lg);
        margin-bottom: var(--space-6);
        overflow: hidden;
        border: 1px solid var(--color-surface-border);
    }

    .map-placeholder {
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        background: repeating-linear-gradient(
            45deg,
            rgba(255, 255, 255, 0.03),
            rgba(255, 255, 255, 0.03) 10px,
            rgba(255, 255, 255, 0.05) 10px,
            rgba(255, 255, 255, 0.05) 20px
        );
        color: var(--color-text-tertiary);
        font-size: var(--font-size-sm);
        border: 1px solid rgba(255, 255, 255, 0.1);
    }

    .venue-list {
        display: flex;
        flex-direction: column;
        gap: var(--space-3);
    }

    .venue-card {
        display: flex;
        gap: var(--space-3);
        background: var(--color-surface);
        padding: var(--space-3);
        border-radius: var(--radius-lg);
        align-items: center;
    }

    .venue-img {
        width: 60px;
        height: 60px;
        background: var(--color-bg-tertiary);
        border-radius: var(--radius-md);
    }

    .venue-info {
        flex: 1;
    }

    .venue-info h4 {
        margin: 0 0 var(--space-1);
        font-size: var(--font-size-base);
    }

    .venue-info p {
        margin: 0 0 var(--space-1);
        font-size: var(--font-size-xs);
        color: var(--color-text-secondary);
    }

    .status {
        font-size: 10px;
        padding: 2px 6px;
        border-radius: 4px;
        font-weight: 600;
        text-transform: uppercase;
    }

    .status.open {
        background: rgba(76, 175, 80, 0.2);
        color: #81c784;
    }
    .status.closed {
        background: rgba(244, 67, 54, 0.2);
        color: #e57373;
    }
</style>
