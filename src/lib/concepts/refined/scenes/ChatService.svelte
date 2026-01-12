<!--
  ChatService.svelte
  Refined Concept - Active Chat Service
  Displays AI response and structured service cards
-->
<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, fly, slide } from "svelte/transition";
    import { quintOut } from "svelte/easing";

    const dispatch = createEventDispatcher();

    export let initialQuery = "";

    let messages = [];
    let showOptions = false;

    // Simulate AI Processing
    onMount(() => {
        // User Message
        messages = [...messages, { id: 1, role: "user", text: initialQuery }];

        // AI Response (Simulated Delay)
        setTimeout(() => {
            messages = [
                ...messages,
                {
                    id: 2,
                    role: "ai",
                    text: `Certainly. I can assist you with ${initialQuery}. Here are some curated options for you.`,
                },
            ];
            showOptions = true;
        }, 1200);
    });

    const options = [
        {
            id: 1,
            title: "In-Room Dining",
            desc: "Chef's Special Course",
            icon: "🍽️",
        },
        {
            id: 2,
            title: "Spa Reservation",
            desc: "Aromatherapy Session",
            icon: "💆",
        },
        {
            id: 3,
            title: "Valet Service",
            desc: "Vehicle Retrieval",
            icon: "🚗",
        },
    ];

    function handleOptionClick(option) {
        // Further Logic or confirmation could go here
        messages = [
            ...messages,
            { id: 3, role: "user", text: `I'd like to book ${option.title}.` },
            {
                id: 4,
                role: "ai",
                text: "Excellent choice. I have confirmed your request.",
            },
        ];
    }
</script>

<div class="chat-service">
    <div class="chat-history">
        {#each messages as msg (msg.id)}
            <div
                class="message-row {msg.role}"
                in:fly={{ y: 20, duration: 400 }}
            >
                <div class="message-bubble">
                    {msg.text}
                </div>
            </div>
        {/each}

        {#if showOptions}
            <div
                class="options-container"
                in:fade={{ duration: 500, delay: 200 }}
            >
                <p class="options-label">Suggested Services</p>
                <div class="options-grid">
                    {#each options as option, i}
                        <button
                            class="option-card"
                            in:fly={{
                                y: 20,
                                duration: 500,
                                delay: 300 + i * 100,
                            }}
                            on:click={() => handleOptionClick(option)}
                        >
                            <span class="option-icon">{option.icon}</span>
                            <div class="option-info">
                                <span class="option-title">{option.title}</span>
                                <span class="option-desc">{option.desc}</span>
                            </div>
                        </button>
                    {/each}
                </div>
            </div>
        {/if}
    </div>
</div>

<style>
    .chat-service {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        padding-bottom: var(--space-10);
    }

    .chat-history {
        flex: 1;
        overflow-y: auto;
        padding: var(--space-6) var(--grid-margin);
        display: flex;
        flex-direction: column;
        gap: var(--space-4);
    }

    .message-row {
        display: flex;
        width: 100%;
    }

    .message-row.user {
        justify-content: flex-end;
    }

    .message-row.ai {
        justify-content: flex-start;
    }

    .message-bubble {
        max-width: 80%;
        padding: 16px 20px;
        border-radius: 20px;
        font-family: var(--font-sans);
        font-size: var(--font-size-md);
        line-height: 1.5;
    }

    .user .message-bubble {
        background: var(--color-primary);
        color: white;
        border-bottom-right-radius: 4px;
    }

    .ai .message-bubble {
        background: var(--color-surface);
        color: var(--color-text-primary);
        border: 1px solid var(--color-surface-border);
        border-bottom-left-radius: 4px;
    }

    /* Options */
    .options-container {
        margin-top: var(--space-4);
        display: flex;
        flex-direction: column;
        gap: var(--space-3);
    }

    .options-label {
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        color: var(--color-text-tertiary);
        letter-spacing: 0.1em;
        text-transform: uppercase;
        margin-left: 4px;
    }

    .options-grid {
        display: contents;
    }

    .option-card {
        display: flex;
        align-items: center;
        gap: var(--space-4);
        padding: var(--space-4);
        background: var(--color-surface);
        border: 1px solid var(--color-surface-border);
        border-radius: var(--radius-lg);
        text-align: left;
        transition: all 0.3s ease;
    }

    .option-card:hover {
        border-color: var(--color-primary);
        background: var(--color-bg-secondary);
    }

    .option-icon {
        font-size: 24px;
    }

    .option-info {
        display: flex;
        flex-direction: column;
        gap: 2px;
    }

    .option-title {
        font-family: var(--font-serif);
        font-size: var(--font-size-md);
        color: var(--color-text-primary);
    }

    .option-desc {
        font-family: var(--font-sans);
        font-size: var(--font-size-xs);
        color: var(--color-text-secondary);
    }
</style>
