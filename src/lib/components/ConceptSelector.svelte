<!--
  ConceptSelector.svelte
  Landing page for selecting between 3 concepts
-->
<script>
  import Header from "./Header.svelte";
  import { createEventDispatcher } from "svelte";
  import { fade, fly, scale } from "svelte/transition";
  import { cubicOut, elasticOut } from "svelte/easing";

  const dispatch = createEventDispatcher();

  let hoveredConcept = null;
  let selectedConcept = null;

  const concepts = [
    {
      id: "joyful",
      label: "A",
      title: "Joyful",
      subtitle: "The Digital Playground",
      description: "축제의 시작점",
      gradient:
        "linear-gradient(135deg, #FF6B9D 0%, #9B59FF 50%, #00D4FF 100%)",
      bgColor: "#0A0A0F",
    },
    {
      id: "refined",
      label: "B",
      title: "Refined",
      subtitle: "The Signature Collection",
      description: "품격 있는 휴식",
      gradient:
        "linear-gradient(135deg, #C9A962 0%, #D9C08A 50%, #C9A962 100%)",
      bgColor: "#FDFCFB",
      light: true,
    },
    /*
    {
      id: "ai",
      label: "C",
      title: "AI",
      subtitle: "The Smart Navigator",
      description: "똑똑한 동행",
      gradient: "linear-gradient(135deg, #4F8CFF 0%, #6366F1 100%)",
      bgColor: "#F8FAFC",
      light: true,
    },
    */
    {
      id: "d",
      label: "D",
      title: "Luxury",
      subtitle: "The Premium Experience",
      description: "블랙 앤 화이트 + 미니멀리즘",
      gradient: "linear-gradient(135deg, #FFFFFF 0%, #0A0A14 100%)",
      bgColor: "#FFFFFF",
      light: false,
    },
  ];

  function selectConcept(concept) {
    selectedConcept = concept.id;
    setTimeout(() => {
      dispatch("select", { concept: concept.id });
    }, 400);
  }
</script>

<div class="concept-selector">
  <!-- Animated Background -->
  <div class="animated-bg">
    <div class="gradient-orb orb-1"></div>
    <div class="gradient-orb orb-2"></div>
    <div class="gradient-orb orb-3"></div>
  </div>

  <!-- Header -->
  <Header />

  <!-- Concept Cards -->
  <div class="cards-container">
    {#each concepts as concept, index}
      <button
        class="concept-card"
        class:hovered={hoveredConcept === concept.id}
        class:selected={selectedConcept === concept.id}
        class:light={concept.light}
        style="--card-gradient: {concept.gradient}; --card-bg: {concept.bgColor};"
        on:mouseenter={() => (hoveredConcept = concept.id)}
        on:mouseleave={() => (hoveredConcept = null)}
        on:click={() => selectConcept(concept)}
        in:fly={{
          y: 30,
          duration: 500,
          delay: 300 + index * 100,
          easing: cubicOut,
        }}
      >
        <div class="card-gradient-border"></div>
        <div class="card-content">
          <span class="concept-label">{concept.label}</span>
          <h2 class="concept-title">{concept.title}</h2>
          <p class="concept-subtitle">{concept.subtitle}</p>
          <p class="concept-description">{concept.description}</p>
        </div>
        <div class="card-arrow">
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <path d="M5 12h14M12 5l7 7-7 7" />
          </svg>
        </div>
      </button>
    {/each}
  </div>

  <!-- Footer -->
  <footer class="footer" in:fade={{ duration: 400, delay: 700 }}>
    <p>Tap to explore</p>
  </footer>
</div>

<style>
  .concept-selector {
    width: 100%;
    height: 100%;
    background: linear-gradient(180deg, #0a0a0f 0%, #15151d 100%);
    display: flex;
    flex-direction: column;
    padding: var(--space-5);
    position: relative;
    overflow: scroll;
  }

  /* Animated Background */
  .animated-bg {
    position: absolute;
    inset: 0;
    overflow: hidden;
    pointer-events: none;
  }

  .gradient-orb {
    position: absolute;
    border-radius: 50%;
    filter: blur(60px);
    opacity: 0.4;
    animation: float 8s ease-in-out infinite;
  }

  .orb-1 {
    width: 200px;
    height: 200px;
    background: linear-gradient(135deg, #ff6b9d, #9b59ff);
    top: -50px;
    right: -50px;
    animation-delay: 0s;
  }

  .orb-2 {
    width: 150px;
    height: 150px;
    background: linear-gradient(135deg, #c9a962, #d9c08a);
    bottom: 30%;
    left: -40px;
    animation-delay: -3s;
  }

  .orb-3 {
    width: 180px;
    height: 180px;
    background: linear-gradient(135deg, #4f8cff, #6366f1);
    bottom: -30px;
    right: 20%;
    animation-delay: -5s;
  }

  @keyframes float {
    0%,
    100% {
      transform: translateY(0) scale(1);
    }
    50% {
      transform: translateY(-20px) scale(1.05);
    }
  }

  /* Cards Container */
  .cards-container {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: var(--space-3);
    justify-content: center;
    position: relative;
    z-index: 1;
  }

  /* Concept Card */
  .concept-card {
    position: relative;
    padding: var(--space-5);
    border-radius: var(--radius-2xl);
    background: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.1);
    text-align: left;
    color: #fff;
    transition: all var(--transition-normal);
    overflow: hidden;
    min-height: 100px;
  }

  .concept-card:hover,
  .concept-card.hovered {
    transform: translateX(4px);
    background: rgba(255, 255, 255, 0.08);
    border-color: rgba(255, 255, 255, 0.2);
  }

  .concept-card.selected {
    transform: scale(0.98);
    opacity: 0.7;
  }

  .concept-card.light {
    color: var(--card-bg);
  }

  /* Gradient Border */
  .card-gradient-border {
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    width: 4px;
    background: var(--card-gradient);
    border-radius: 2px;
    opacity: 0.8;
    transition: width var(--transition-normal);
  }

  .concept-card:hover .card-gradient-border,
  .concept-card.hovered .card-gradient-border {
    width: 6px;
  }

  /* Card Content */
  .card-content {
    position: relative;
    z-index: 1;
  }

  .concept-label {
    display: inline-block;
    font-size: var(--font-size-xs);
    font-weight: var(--font-weight-bold);
    letter-spacing: 0.1em;
    padding: 2px 8px;
    background: rgba(255, 255, 255, 0.1);
    border-radius: var(--radius-sm);
    margin-bottom: var(--space-2);
  }

  .concept-title {
    font-size: var(--font-size-xl);
    font-weight: var(--font-weight-bold);
    margin-bottom: 2px;
    background: var(--card-gradient);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .concept-subtitle {
    font-size: var(--font-size-sm);
    color: rgba(255, 255, 255, 0.7);
    margin-bottom: var(--space-1);
  }

  .concept-description {
    font-size: var(--font-size-xs);
    color: rgba(255, 255, 255, 0.5);
  }

  /* Arrow */
  .card-arrow {
    position: absolute;
    right: var(--space-5);
    top: 50%;
    transform: translateY(-50%) translateX(-4px);
    width: 24px;
    height: 24px;
    color: rgba(255, 255, 255, 0.4);
    transition: all var(--transition-normal);
  }

  .concept-card:hover .card-arrow,
  .concept-card.hovered .card-arrow {
    transform: translateY(-50%) translateX(0);
    color: rgba(255, 255, 255, 0.8);
  }

  /* Footer */
  .footer {
    text-align: center;
    padding: var(--space-4) 0;
    position: relative;
    z-index: 1;
  }

  .footer p {
    font-size: var(--font-size-xs);
    color: rgba(255, 255, 255, 0.3);
    animation: pulse 2s ease-in-out infinite;
  }

  @keyframes pulse {
    0%,
    100% {
      opacity: 0.3;
    }
    50% {
      opacity: 0.6;
    }
  }
</style>
