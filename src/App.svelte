<!--
  App.svelte
  Main application entry point
-->
<script>
  import { fade, fly } from "svelte/transition";
  import Header from "./lib/components/Header.svelte";
  import BottomNav from "./lib/components/BottomNav.svelte";
  import PhoneFrame from "./lib/components/PhoneFrame.svelte";
  import ConceptSelector from "./lib/components/ConceptSelector.svelte";

  // Concept Apps
  import JoyfulApp from "./lib/concepts/joyful/JoyfulApp.svelte";
  import RefinedApp from "./lib/concepts/refined/RefinedApp.svelte";
  import AIApp from "./lib/concepts/ai/AIApp.svelte";
  import DApp from "./lib/concepts/d/DApp.svelte";

  let currentView = "d"; // 'selector' | 'joyful' | 'refined' | 'ai'
  let currentTheme = "d";

  function handleConceptSelect(event) {
    const concept = event.detail.concept;
    currentTheme = concept;
    currentView = concept;
  }

  function handleNav(event) {
    const view = event.detail.view;
    currentView = view;
  }

  function goBack() {
    currentView = "selector";
    currentTheme = "joyful";
  }
</script>

<div class="app-container">
  <PhoneFrame
    theme={currentTheme}
    showStatusBar={true}
    showHomeIndicator={true}
  >
    {#if currentView === "selector"}
      <div class="view" in:fade={{ duration: 300 }}>
        <ConceptSelector on:select={handleConceptSelect} />
      </div>
    {:else if currentView === "joyful"}
      <div class="view" in:fly={{ x: 100, duration: 400 }}>
        <JoyfulApp on:back={goBack} />
      </div>
    {:else if currentView === "refined"}
      <div class="view" in:fly={{ x: 100, duration: 400 }}>
        <RefinedApp on:back={goBack} />
      </div>
    {:else if currentView === "ai"}
      <div class="view" in:fly={{ x: 100, duration: 400 }}>
        <AIApp on:back={goBack} />
      </div>
    {:else if ["d", "main", "wallet", "pass", "casino", "my", "home", "reservations", "settings"].includes(currentView)}
      <div class="view" in:fly={{ x: 100, duration: 400 }}>
        <DApp view={currentView} on:back={goBack} on:navigate={handleNav} />
      </div>
    {/if}
  </PhoneFrame>
</div>

<style>
  .app-container {
    width: 100%;
    max-height: 100svh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
  }

  .view {
    width: 100%;
    height: 100%;
    position: absolute;
    inset: 0;
  }
</style>
