<!-- DReservations.svelte - Full Page Reservations List -->
<script>
    import { fly, fade } from "svelte/transition";
    import {
        ChevronLeft,
        Calendar,
        Clock,
        MapPin,
        CheckCircle,
        AlertCircle,
    } from "lucide-svelte";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let active = false;

    const reservations = [
        {
            id: 1,
            type: "stay",
            title: "Ocean View Suite",
            date: "2025.01.13",
            time: "3:00 PM",
            location: "Main Tower",
            status: "confirmed",
            image: "https://images.unsplash.com/photo-1590490360182-c33d57733427?q=80&w=800&auto=format&fit=crop",
        },
        {
            id: 2,
            type: "dining",
            title: "Chef's Kitchen",
            date: "2025.01.13",
            time: "7:30 PM",
            location: "Lobby Level",
            status: "confirmed",
            image: "https://images.unsplash.com/photo-1517248135467-4c7edcad34c4?q=80&w=800&auto=format&fit=crop",
        },
        {
            id: 3,
            type: "spa",
            title: "Aromatherapy Massage",
            date: "2025.01.13",
            time: "10:00 AM",
            location: "Inspire Spa",
            status: "pending",
            image: "https://images.unsplash.com/photo-1600334089648-b0d9d3028eb2?q=80&w=800&auto=format&fit=crop",
        },
    ];

    function goBack() {
        dispatch("back");
    }
</script>

{#if active}
    <div class="d-reservations" in:fly={{ x: 20, duration: 400 }}>
        <!-- Header -->
        <header class="header">
            <button class="back-btn" on:click={goBack}>
                <ChevronLeft size={24} />
            </button>
            <h1>My Reservations</h1>
            <div class="placeholder"></div>
        </header>

        <!-- List -->
        <div class="reservation-list">
            {#each reservations as item}
                <div class="reservation-card">
                    <div
                        class="card-image"
                        style="background-image: url({item.image})"
                    >
                        <div class="status-badge {item.status}">
                            {#if item.status === "confirmed"}
                                <CheckCircle size={12} />
                                <span>Confirmed</span>
                            {:else}
                                <AlertCircle size={12} />
                                <span>Pending</span>
                            {/if}
                        </div>
                    </div>
                    <div class="card-content">
                        <h2>{item.title}</h2>

                        <div class="info-row">
                            <Calendar size={14} />
                            <span>{item.date}</span>
                        </div>

                        <div class="info-row">
                            <Clock size={14} />
                            <span>{item.time}</span>
                        </div>

                        <div class="info-row">
                            <MapPin size={14} />
                            <span>{item.location}</span>
                        </div>
                    </div>
                </div>
            {/each}
        </div>
    </div>
{/if}

<style>
    .d-reservations {
        height: 100%;
        display: flex;
        flex-direction: column;
        background: var(--color-bg-primary);
        color: var(--color-text-primary);
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        z-index: 20;
    }

    .header {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: calc(var(--safe-area-top) + 10px) 20px 20px;
        background: rgba(14, 28, 48, 0.95);
        backdrop-filter: blur(10px);
        border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    }

    .header h1 {
        font-size: var(--font-size-lg);
        font-weight: 500;
        letter-spacing: 0.05em;
    }

    .back-btn {
        background: none;
        border: none;
        color: var(--color-text-primary);
        cursor: pointer;
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .placeholder {
        width: 40px;
    }

    .reservation-list {
        flex: 1;
        overflow-y: scroll;
        min-height: 0;
        padding: var(--space-5);
        display: flex;
        flex-direction: column;
        gap: var(--space-5);
        padding-bottom: calc(var(--safe-area-bottom) + 20px);
    }

    .reservation-card {
        background: rgba(255, 255, 255, 0.05);
        border-radius: var(--radius-lg);
        overflow: hidden;
        border: 1px solid rgba(255, 255, 255, 0.05);
    }

    .card-image {
        height: 140px;
        background-size: cover;
        background-position: center;
        position: relative;
    }

    .status-badge {
        position: absolute;
        top: 12px;
        right: 12px;
        display: flex;
        align-items: center;
        gap: 4px;
        padding: 4px 10px;
        border-radius: 20px;
        font-size: 10px;
        font-weight: 400;
        text-transform: uppercase;
        backdrop-filter: blur(12px);
    }

    .status-badge.confirmed {
        background: rgba(76, 175, 80, 0.3);
        color: white;
    }

    .status-badge.pending {
        background: rgba(212, 175, 55, 0.3);
        color: white;
    }

    .card-content {
        padding: var(--space-3) var(--space-4);
        display: flex;
        flex-direction: column;
        gap: var(--space-2);
    }

    .card-content h2 {
        font-size: var(--font-size-lg);
        font-weight: 600;
        margin-bottom: var(--space-1);
    }

    .info-row {
        display: flex;
        align-items: center;
        gap: var(--space-2);
        color: var(--color-text-secondary);
        font-size: var(--font-size-sm);
    }
</style>
