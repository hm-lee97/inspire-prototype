<!--
  Entry.svelte
  Joyful Concept - Scene 1: 진입
  HERO COLLAGE + FACILITY SHOWCASE
  Long Scroll (8.5x height) + Imported Assets (Fix Path Issues)
-->
<script>
  import { createEventDispatcher, onMount, onDestroy } from "svelte";
  // Import image explicitly to ensure correct path resolution
  import exteriorImg from "../../../assets/joyful/exterior_v1.png";

  const dispatch = createEventDispatcher();

  let scrollY = 0;
  let innerHeight = 800;
  let tiltX = 0;
  let tiltY = 0;

  // Scroll Constants
  $: H = innerHeight;
  $: phase1Start = H * 1.5;
  $: phase2Start = H * 3.0;
  $: phase3Start = H * 4.5;
  $: phase4Start = H * 6.0;

  // --- PHASE 0: HERO EXPANSION ---
  $: heroProgress = Math.min(1, Math.max(0, scrollY / (H * 1.5)));
  $: heroOpacity = 1 - Math.max(0, (scrollY - H * 1.2) / (H * 0.3)); // Fade out before phase 1

  $: containerScale = 0.5 + heroProgress * 0.5;
  $: containerRadius = 30 - heroProgress * 30;
  $: heroImageScale = 1.1 + heroProgress * 0.1;

  // --- PHASE 1: ARENA ---
  $: arenaProgress = Math.min(
    1,
    Math.max(0, (scrollY - phase1Start) / (H * 1.5)),
  );
  $: arenaOpacity =
    scrollY > phase1Start && scrollY < phase2Start
      ? Math.min(1, (scrollY - phase1Start) / (H * 0.5)) -
        Math.max(0, (scrollY - (phase2Start - H * 0.5)) / (H * 0.5))
      : 0;
  $: arenaParallax = (scrollY - phase1Start) * 0.1;

  // --- PHASE 2: AURORA ---
  $: auroraProgress = Math.min(
    1,
    Math.max(0, (scrollY - phase2Start) / (H * 1.5)),
  );
  $: auroraOpacity =
    scrollY > phase2Start && scrollY < phase3Start
      ? Math.min(1, (scrollY - phase2Start) / (H * 0.5)) -
        Math.max(0, (scrollY - (phase3Start - H * 0.5)) / (H * 0.5))
      : 0;
  $: auroraParallax = (scrollY - phase2Start) * 0.1;

  // --- PHASE 3: SPLASH BAY ---
  $: splashProgress = Math.min(
    1,
    Math.max(0, (scrollY - phase3Start) / (H * 1.5)),
  );
  $: splashOpacity =
    scrollY > phase3Start && scrollY < phase4Start
      ? Math.min(1, (scrollY - phase3Start) / (H * 0.5)) -
        Math.max(0, (scrollY - (phase4Start - H * 0.5)) / (H * 0.5))
      : 0;
  $: splashParallax = (scrollY - phase3Start) * 0.1;

  // --- PHASE 4: FINAL CTA ---
  $: ctaOpacity = Math.min(1, Math.max(0, (scrollY - phase4Start) / (H * 0.5)));

  // Gyro
  $: gyroX = tiltX * 15;
  $: gyroY = tiltY * 15;

  function handleScroll(e) {
    scrollY = e.target.scrollTop;
  }
  function handleResize() {
    innerHeight = window.innerHeight;
  }

  function handleOrientation(e) {
    if (e.gamma !== null && e.beta !== null) {
      tiltX = Math.max(-1, Math.min(1, e.gamma / 30));
      tiltY = Math.max(-1, Math.min(1, (e.beta - 45) / 30));
    }
  }

  function handleMouseMove(e) {
    tiltX = (e.clientX / window.innerWidth) * 2 - 1;
    tiltY = (e.clientY / window.innerHeight) * 2 - 1;
  }

  onMount(() => {
    window.addEventListener("resize", handleResize);
    if (window.DeviceOrientationEvent)
      window.addEventListener("deviceorientation", handleOrientation);
    window.addEventListener("mousemove", handleMouseMove);
    handleResize();
  });

  onDestroy(() => {
    window.removeEventListener("resize", handleResize);
    window.removeEventListener("deviceorientation", handleOrientation);
    window.removeEventListener("mousemove", handleMouseMove);
  });
</script>

<div class="entry-scene" on:scroll={handleScroll}>
  <div class="sticky-wrapper">
    <!-- Base BG: Blurred Resort Exterior (v1) -->
    <div class="scene-bg">
      <img
        src={exteriorImg}
        alt="Resort Exterior"
        style="width: 100%; height: 100%; object-fit: cover; filter: blur(30px) brightness(0.3); transform: scale(1.1);"
      />
    </div>

    <!-- === PHASE 0: HERO (Socar Expansion) === -->
    <div
      class="phase-layer"
      style="opacity: {heroOpacity}; pointer-events: {heroOpacity > 0
        ? 'auto'
        : 'none'}"
    >
      <div
        class="expander-window"
        style="transform: scale({containerScale}); border-radius: {containerRadius}px;"
      >
        <!-- 1. Hero Content -->
        <img
          src="/images/hero_collage.png"
          alt="Hero"
          class="bg-image"
          style="transform: scale({heroImageScale}) translate({gyroX}px, {gyroY}px);"
        />

        <!-- 2. Exterior Texture Overlay (Imported Image) -->
        <div class="texture-overlay" style="z-index: 10;">
          <img
            src={exteriorImg}
            alt=""
            style="width: 100%; height: 100%; object-fit: cover; opacity: 0.6; mix-blend-mode: soft-light;"
          />
        </div>

        <!-- 3. Gradient Overlay -->
        <div class="gradient-overlay"></div>
      </div>

      <div
        class="intro-text"
        style="opacity: {1 - heroProgress * 2.5}; transform: scale({1 +
          heroProgress * 0.2});"
      >
        <div class="brand-tag">THE ULTIMATE DESTINATION</div>
        <h1 class="intro-title">INSPIRE<br />ENTERTAINMENT<br />RESORT</h1>
        <div class="scroll-wrapper">
          <span class="scroll-label">SCROLL TO TOUR</span>
          <div class="scroll-line"></div>
        </div>
      </div>
    </div>

    <!-- === PHASE 1: ARENA === -->
    <div class="phase-layer" style="opacity: {arenaOpacity}; z-index: 20;">
      <div class="full-bg">
        <img
          src="/images/arena.png"
          alt="Arena"
          style="transform: scale(1.1) translateY({-arenaParallax * 0.5}px);"
        />
        <div class="overlay-dark"></div>
      </div>
      <div class="center-content">
        <span class="phase-label">LIVE CONCERT HALL</span>
        <h2 class="phase-title">INSPIRE ARENA</h2>
        <p class="phase-desc">
          15,000석 규모의 압도적 사운드와<br />화려한 퍼포먼스
        </p>
      </div>
    </div>

    <!-- === PHASE 2: AURORA === -->
    <div class="phase-layer" style="opacity: {auroraOpacity}; z-index: 21;">
      <div class="full-bg">
        <img
          src="/images/aurora.png"
          alt="Aurora"
          style="transform: scale(1.1) translateY({-auroraParallax * 0.5}px);"
        />
        <div class="overlay-dark"></div>
      </div>
      <div class="center-content">
        <span class="phase-label">IMMERSIVE DIGITAL ART</span>
        <h2 class="phase-title">AURORA</h2>
        <p class="phase-desc">
          150m 길이의 초고화질 LED로<br />펼쳐지는 환상의 거리
        </p>
      </div>
    </div>

    <!-- === PHASE 3: SPLASH BAY === -->
    <div class="phase-layer" style="opacity: {splashOpacity}; z-index: 22;">
      <div class="full-bg">
        <img
          src="/images/splash.png"
          alt="Splash"
          style="transform: scale(1.1) translateY({-splashParallax * 0.5}px);"
        />
        <div class="overlay-dark"></div>
      </div>
      <div class="center-content">
        <span class="phase-label">FOREST POOL</span>
        <h2 class="phase-title">SPLASH BAY</h2>
        <p class="phase-desc">
          365일 여름을 만나는<br />유리 돔 아래 트로피컬 워터파크
        </p>
      </div>
    </div>

    <!-- === PHASE 4: FINAL CTA === -->
    <div
      class="phase-layer"
      style="opacity: {ctaOpacity}; z-index: 30; background: black;"
    >
      <div class="final-content">
        <h2 class="final-title">Are You Ready?</h2>
        <p class="final-desc">
          지금 바로 인스파이어의 세계로<br />당신을 초대합니다.
        </p>
        <button class="cta-button" on:click={() => dispatch("next")}>
          <span class="btn-text">입장하기</span>
        </button>
      </div>
    </div>
  </div>

  <div class="scroll-spacer"></div>
</div>

<style>
  .entry-scene {
    width: 100%;
    height: 100%;
    overflow-y: auto;
    overflow-x: hidden;
    background: #000;
    position: relative;
    font-family: "Sansation", sans-serif; /* Applied Sansation font */
    font-style: normal;
  }
  .scroll-spacer {
    height: 850vh;
  }

  .sticky-wrapper {
    position: sticky;
    top: 0;
    width: 100%;
    height: 100vh;
    overflow: hidden;
  }
  .scene-bg {
    position: absolute;
    inset: 0;
    overflow: hidden;
  }

  .phase-layer {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    pointer-events: none;
    transition: opacity 0.3s ease-out;
  }

  /* Expander */
  .expander-window {
    width: 100%;
    height: 100%;
    position: absolute;
    overflow: hidden;
    background: #222;
    will-change: transform, border-radius;
    box-shadow: 0 0 50px rgba(0, 0, 0, 0.5);
  }
  .bg-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  /* Texture & Gradient Overlays */
  .texture-overlay {
    position: absolute;
    inset: 0;
    pointer-events: none;
  }
  .gradient-overlay {
    position: absolute;
    inset: 0;
    pointer-events: none;
    background: radial-gradient(
      circle at center,
      transparent 0%,
      rgba(0, 0, 0, 0.4) 100%
    );
  }

  /* Full Screen BG */
  .full-bg {
    position: absolute;
    inset: 0;
    z-index: 0;
    overflow: hidden;
  }
  .full-bg img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .overlay-dark {
    position: absolute;
    inset: 0;
    background: rgba(0, 0, 0, 0.4);
  }

  /* Typography */
  .intro-text {
    position: relative;
    z-index: 10;
    text-align: center;
    color: white;
  }
  .brand-tag {
    font-size: 13px;
    letter-spacing: 0.3em;
    color: #ff2dcb;
    font-weight: 700;
    margin-bottom: 20px;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
  }
  .intro-title {
    font-size: clamp(32px, 8vw, 44px);
    font-weight: 900;
    line-height: 1.1;
    margin: 0;
    text-shadow: 0 4px 30px rgba(0, 0, 0, 0.5);
    word-break: keep-all;
    overflow-wrap: break-word;
    max-width: 100%;
    padding: 0 10px;
  }

  .center-content {
    position: relative;
    z-index: 10;
    text-align: center;
    color: white;
    transform: translateY(20px);
  }
  .phase-label {
    font-size: 12px;
    color: #ff2dcb;
    letter-spacing: 0.2em;
    font-weight: 700;
    display: block;
    margin-bottom: 12px;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
  }
  .phase-title {
    font-size: 48px;
    font-weight: 800;
    margin: 0 0 16px 0;
    text-shadow: 0 4px 20px rgba(0, 0, 0, 0.6);
  }
  .phase-desc {
    font-size: 16px;
    color: rgba(255, 255, 255, 0.9);
    line-height: 1.6;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
  }

  .final-content {
    text-align: center;
    color: white;
  }
  .final-title {
    font-size: 40px;
    font-weight: 800;
    margin-bottom: 16px;
    background: linear-gradient(to right, #fff, #ff2dcb);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .final-desc {
    font-size: 16px;
    color: rgba(255, 255, 255, 0.7);
    margin-bottom: 32px;
    line-height: 1.6;
  }

  .cta-button {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    padding: 14px 32px;
    background: linear-gradient(135deg, #9b59ff 0%, #ff2dcb 100%);
    border: none;
    font-size: 14px;
    font-weight: 700;
    letter-spacing: 0.05em;
    color: white;
    cursor: pointer;
    overflow: hidden;
    transition: all 0.3s ease;
    clip-path: polygon(
      8px 0,
      100% 0,
      100% calc(100% - 8px),
      calc(100% - 8px) 100%,
      0 100%,
      0 8px
    );
    margin: 0 auto;
    pointer-events: auto;
  }

  .cta-button:hover {
    transform: scale(1.05);
    box-shadow: 0 0 30px rgba(255, 45, 203, 0.5);
  }

  .cta-button:active {
    transform: scale(0.98);
  }

  .scroll-wrapper {
    position: absolute;
    bottom: -150px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
  }
  .scroll-label {
    font-size: 10px;
    letter-spacing: 0.2em;
    opacity: 0.6;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
  }
  .scroll-line {
    width: 1px;
    height: 40px;
    background: rgba(255, 255, 255, 0.3);
    position: relative;
    overflow: hidden;
  }
  .scroll-line::after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: #ff2dcb;
    animation: scroll-drop 2s infinite;
  }
  @keyframes scroll-drop {
    0% {
      transform: translateY(-100%);
    }
    100% {
      transform: translateY(100%);
    }
  }
</style>
