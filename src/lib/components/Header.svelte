<!-- Header.svelte -->
<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
</script>

<div class="header">
    <div class="logo">
        <span class="logo-text">INSPIRE</span>
    </div>
    <p class="tagline">Experience Design Concepts</p>
</div>

<style>
    .header {
        text-align: center;
        padding: var(--space-10) 0 var(--space-4);
        position: relative;
        z-index: 1;
    }
    .logo {
        margin-bottom: var(--space-2);
    }
    .logo-text {
        font-size: var(--font-size-2xl);
        font-weight: var(--font-weight-bold);
        letter-spacing: 0.15em;
        background: linear-gradient(
            135deg,
            #fff 0%,
            rgba(255, 255, 255, 0.7) 100%
        );
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
    }
    .tagline {
        font-size: var(--font-size-sm);
        color: rgba(255, 255, 255, 0.5);
        letter-spacing: 0.05em;
    }
</style>
