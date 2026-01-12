<!-- DMain.svelte -->
<script>
    import { fade, fly } from "svelte/transition";
    import QuickMenu from "../../components/QuickMenu.svelte";
    import {
        ChevronRight,
        Clock,
        Utensils,
        ShoppingBag,
        Landmark,
        BedDouble,
        Wifi,
    } from "lucide-svelte";

    export let active = false;

    const facilities = [
        { name: "다이닝", icon: Utensils, desc: "시그니처 키친" },
        { name: "쇼핑", icon: ShoppingBag, desc: "럭셔리 몰" },
        { name: "아레나", icon: Landmark, desc: "월드 클래스 공연장" },
        { name: "호텔", icon: BedDouble, desc: "프리미엄 스위트" },
        { name: "편의시설", icon: Wifi, desc: "익스클루시브 라운지" },
    ];
</script>

{#if active}
    <div class="d-main" in:fly={{ y: 20, duration: 400 }}>
        <!-- Hero Section -->
        <div class="hero">
            <h1 class="hero-title">INSPIRE<br />LUXURY</h1>
            <p class="hero-subtitle">The Premium Member's Lounge</p>

            <div class="card-gold-accent">
                <div class="card-content">
                    <span class="badge">Limited Offer</span>
                    <h2>시그니처 스위트</h2>
                    <p>최고의 럭셔리를 50% 할인된 가격으로 경험하세요</p>

                    <div class="time-sale-info">
                        <Clock size={16} />
                        <span>남은 시간 02:45:12</span>
                    </div>
                </div>
                <div class="card-action">
                    <span>자세히 보기</span>
                    <ChevronRight size={16} />
                </div>
                <div class="shimmer"></div>
            </div>
        </div>

        <!-- Quick Menu -->
        <div class="quick-menu-section">
            <QuickMenu on:quick />
        </div>

        <!-- Key Facilities -->
        <div class="section facilities">
            <div class="section-header">
                <h3>주요 시설</h3>
                <button class="view-all">전체 보기</button>
            </div>
            <div class="facilities-scroll">
                {#each facilities as fac}
                    <div class="facility-card">
                        <div class="fac-icon">
                            <svelte:component this={fac.icon} size={24} />
                        </div>
                        <h4>{fac.name}</h4>
                        <p>{fac.desc}</p>
                    </div>
                {/each}
            </div>
        </div>
    </div>
{/if}

<style>
    .d-main {
        height: 100%;
        overflow-y: auto;
        padding: var(--space-4) var(--space-5) 100px; /* Bottom padding for nav */
        color: var(--color-text-primary);
    }

    .hero {
        margin-bottom: var(--space-6);
    }

    .hero-title {
        font-size: var(--font-size-4xl);
        font-weight: 700;
        line-height: 1.1;
        background: var(--gradient-gold);
        -webkit-background-clip: text;
        background-clip: text;
        -webkit-text-fill-color: transparent;
        margin-bottom: var(--space-2);
    }

    .hero-subtitle {
        color: var(--color-text-secondary);
        margin-bottom: var(--space-4);
    }

    .card-gold-accent {
        background: linear-gradient(
            135deg,
            rgba(212, 175, 55, 0.1),
            rgba(0, 0, 0, 0.4)
        );
        border: 1px solid rgba(212, 175, 55, 0.3);
        border-radius: var(--radius-xl);
        padding: var(--space-5);
        position: relative;
        overflow: hidden;
        margin-bottom: var(--space-6);
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .card-gold-accent::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: linear-gradient(
            90deg,
            transparent,
            rgba(212, 175, 55, 0.1),
            transparent
        );
        transform: skewX(-20deg) translateX(-150%);
        animation: shine 6s infinite;
    }

    @keyframes shine {
        0% {
            transform: skewX(-20deg) translateX(-150%);
        }
        20% {
            transform: skewX(-20deg) translateX(150%);
        }
        100% {
            transform: skewX(-20deg) translateX(150%);
        }
    }

    h3 {
        margin: 0 0 var(--space-3);
        font-weight: 600;
    }

    .quick-menu-section {
        margin-bottom: var(--space-6);
        background: rgba(255, 255, 255, 0.03);
        border-radius: var(--radius-xl);
        padding: var(--space-2);
    }

    h4 {
        font-size: var(--font-size-sm);
        color: var(--color-text-secondary);
    }
</style>
