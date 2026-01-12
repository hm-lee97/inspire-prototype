<!--
  JoyfulApp.svelte
  Concept A: The Digital Playground
  Dark mode, neon aurora gradients, cinematic experience
-->
<script>
  import { createEventDispatcher, onMount } from "svelte";
  import { fade, fly, scale } from "svelte/transition";
  import { cubicOut, backOut } from "svelte/easing";

  // Scene Components
  import Entry from "./scenes/Entry.svelte";
  import Countdown from "./scenes/Countdown.svelte";
  import TicketActivation from "./scenes/TicketActivation.svelte";

  const dispatch = createEventDispatcher();

  let currentScene = "entry"; // 'entry' | 'countdown' | 'ticket'
  let isTransitioning = false;

  function goToScene(scene) {
    if (isTransitioning) return;
    isTransitioning = true;
    setTimeout(() => {
      currentScene = scene;
      isTransitioning = false;
    }, 300);
  }

  function handleBack() {
    dispatch("back");
  }
</script>

<div class="joyful-app" data-theme="joyful">
  <!-- Aurora Background -->
  <div class="aurora-background">
    <div class="aurora-layer aurora-1"></div>
    <div class="aurora-layer aurora-2"></div>
    <div class="aurora-layer aurora-3"></div>
    <div class="particles">
      {#each Array(20) as _, i}
        <div
          class="particle"
          style="--delay: {i * 0.5}s; --x: {Math.random() *
            100}%; --duration: {3 + Math.random() * 4}s;"
        ></div>
      {/each}
    </div>
  </div>

  <!-- Back Button -->
  <!-- svelte-ignore a11y_consider_explicit_label -->
  <button class="back-button" on:click={handleBack}>
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M19 12H5M12 19l-7-7 7-7" />
    </svg>
  </button>

  <!-- Scene Navigation -->
  <div class="scene-nav">
    <button
      class="nav-dot"
      class:active={currentScene === "entry"}
      on:click={() => goToScene("entry")}
    >
      <span class="nav-label">진입</span>
    </button>
    <button
      class="nav-dot"
      class:active={currentScene === "countdown"}
      on:click={() => goToScene("countdown")}
    >
      <span class="nav-label">인지</span>
    </button>
    <button
      class="nav-dot"
      class:active={currentScene === "ticket"}
      on:click={() => goToScene("ticket")}
    >
      <span class="nav-label">액션</span>
    </button>
  </div>

  <!-- Scenes -->
  <div class="scene-container">
    {#if currentScene === "entry"}
      <div
        class="scene"
        in:fade={{ duration: 400 }}
        out:fade={{ duration: 200 }}
      >
        <Entry on:next={() => goToScene("countdown")} />
      </div>
    {:else if currentScene === "countdown"}
      <div
        class="scene"
        in:fly={{ x: 50, duration: 400 }}
        out:fade={{ duration: 200 }}
      >
        <Countdown
          on:next={() => goToScene("ticket")}
          on:prev={() => goToScene("entry")}
        />
      </div>
    {:else if currentScene === "ticket"}
      <div
        class="scene"
        in:fly={{ x: 50, duration: 400 }}
        out:fade={{ duration: 200 }}
      >
        <TicketActivation on:prev={() => goToScene("countdown")} />
      </div>
    {/if}
  </div>
</div>

<style>
  .joyful-app {
    width: 100%;
    height: 100%;
    background: var(--color-bg-primary);
    position: relative;
    overflow: hidden;
  }

  /* Aurora Background */
  .aurora-background {
    position: absolute;
    inset: 0;
    overflow: hidden;
  }

  .aurora-layer {
    position: absolute;
    width: 150%;
    height: 150%;
    opacity: 0.6;
    filter: blur(80px);
  }

  .aurora-1 {
    background: radial-gradient(
      ellipse at 30% 20%,
      #ff6b9d 0%,
      transparent 50%
    );
    animation: aurora-move-1 12s ease-in-out infinite;
  }

  .aurora-2 {
    background: radial-gradient(
      ellipse at 70% 60%,
      #9b59ff 0%,
      transparent 50%
    );
    animation: aurora-move-2 15s ease-in-out infinite;
    animation-delay: -5s;
  }

  .aurora-3 {
    background: radial-gradient(
      ellipse at 50% 80%,
      #00d4ff 0%,
      transparent 50%
    );
    animation: aurora-move-3 18s ease-in-out infinite;
    animation-delay: -8s;
  }

  @keyframes aurora-move-1 {
    0%,
    100% {
      transform: translate(-10%, -10%) rotate(0deg);
    }
    50% {
      transform: translate(10%, 10%) rotate(10deg);
    }
  }

  @keyframes aurora-move-2 {
    0%,
    100% {
      transform: translate(10%, -5%) rotate(0deg);
    }
    50% {
      transform: translate(-15%, 15%) rotate(-10deg);
    }
  }

  @keyframes aurora-move-3 {
    0%,
    100% {
      transform: translate(-5%, 10%) rotate(0deg);
    }
    50% {
      transform: translate(10%, -10%) rotate(5deg);
    }
  }

  /* Particles */
  .particles {
    position: absolute;
    inset: 0;
    overflow: hidden;
  }

  .particle {
    position: absolute;
    width: 3px;
    height: 3px;
    background: rgba(255, 255, 255, 0.6);
    border-radius: 50%;
    left: var(--x);
    bottom: -10px;
    animation: particle-rise var(--duration) ease-out infinite;
    animation-delay: var(--delay);
  }

  @keyframes particle-rise {
    0% {
      opacity: 0;
      transform: translateY(0) scale(0);
    }
    10% {
      opacity: 1;
      transform: translateY(-50px) scale(1);
    }
    100% {
      opacity: 0;
      transform: translateY(-800px) scale(0.5);
    }
  }

  /* Back Button */
  .back-button {
    position: absolute;
    top: calc(var(--safe-area-top) + 8px);
    left: 16px;
    width: 36px;
    height: 36px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 100;
    transition: all var(--transition-fast);
  }

  .back-button:hover {
    background: rgba(255, 255, 255, 0.2);
    transform: scale(1.05);
  }

  .back-button svg {
    width: 20px;
    height: 20px;
  }

  /* Scene Navigation */
  .scene-nav {
    position: absolute;
    top: calc(var(--safe-area-top) + 12px);
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 16px;
    z-index: 100;
  }

  .nav-dot {
    position: relative;
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.3);
    transition: all var(--transition-normal);
  }

  .nav-dot.active {
    background: #ff6b9d;
    transform: scale(1.3);
    box-shadow: 0 0 12px rgba(255, 107, 157, 0.6);
  }

  .nav-label {
    position: absolute;
    top: 16px;
    left: 50%;
    transform: translateX(-50%);
    font-size: 10px;
    color: rgba(255, 255, 255, 0.5);
    white-space: nowrap;
    opacity: 0;
    transition: opacity var(--transition-fast);
  }

  .nav-dot:hover .nav-label,
  .nav-dot.active .nav-label {
    opacity: 1;
  }

  /* Scene Container */
  .scene-container {
    position: absolute;
    inset: 0;
    padding-top: var(--safe-area-top);
    padding-bottom: var(--safe-area-bottom);
  }

  .scene {
    width: 100%;
    height: 100%;
    position: absolute;
    inset: 0;
  }
</style>
