<!--
  PhoneFrame.svelte
  iPhone 15/16 Pro frame wrapper (393x852px)
  Includes status bar and home indicator
-->
<script>
  export let showStatusBar = true;
  export let showHomeIndicator = true;
  export let theme = 'joyful'; // 'joyful' | 'refined' | 'ai'
</script>

<div class="phone-frame" data-theme={theme}>
  <div class="phone-screen">
    {#if showStatusBar}
      <div class="status-bar">
        <div class="status-left">
          <span class="time">13:17</span>
        </div>
        <div class="status-notch"></div>
        <div class="status-right">
          <svg class="icon" viewBox="0 0 24 24" fill="currentColor" width="16" height="16">
            <path d="M12 3C7.46 3 3.34 4.78.29 7.67l1.41 1.41C4.25 6.62 7.87 5 12 5s7.75 1.62 10.29 4.08l1.41-1.41C20.66 4.78 16.54 3 12 3z"/>
            <path d="M12 7c-3.31 0-6.31 1.16-8.7 3.08l1.41 1.41C6.78 9.85 9.25 9 12 9s5.22.85 7.29 2.49l1.41-1.41C18.31 8.16 15.31 7 12 7z"/>
            <path d="M12 11c-2.21 0-4.21.79-5.8 2.08l1.41 1.41C8.87 13.54 10.37 13 12 13s3.13.54 4.39 1.49l1.41-1.41C16.21 11.79 14.21 11 12 11z"/>
            <circle cx="12" cy="17" r="2"/>
          </svg>
          <svg class="icon" viewBox="0 0 24 24" fill="currentColor" width="18" height="18">
            <rect x="2" y="7" width="18" height="10" rx="2" stroke="currentColor" stroke-width="1.5" fill="none"/>
            <rect x="4" y="9" width="8" height="6" rx="1" fill="currentColor"/>
            <path d="M20 10v4a1 1 0 001 0v-4a1 1 0 00-1 0z" fill="currentColor"/>
          </svg>
        </div>
      </div>
    {/if}
    
    <div class="content-area" class:with-status-bar={showStatusBar} class:with-home-indicator={showHomeIndicator}>
      <slot />
    </div>
    
    {#if showHomeIndicator}
      <div class="home-indicator-area">
        <div class="home-indicator"></div>
      </div>
    {/if}
  </div>
</div>

<style>
  .phone-frame {
    width: var(--viewport-width);
    height: var(--viewport-height);
    background: var(--color-bg-primary, #0A0A0F);
    border-radius: 44px;
    overflow: hidden;
    position: relative;
    box-shadow: 
      0 0 0 2px #1A1A1A,
      0 0 0 4px #333,
      0 25px 50px -12px rgba(0, 0, 0, 0.5);
  }
  
  .phone-screen {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    position: relative;
    overflow: hidden;
  }
  
  /* Status Bar */
  .status-bar {
    height: var(--status-bar-height);
    padding: 12px 24px 0;
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    z-index: var(--z-fixed);
    color: var(--color-text-primary, #fff);
  }
  
  .status-left {
    display: flex;
    align-items: center;
    gap: 4px;
  }
  
  .time {
    font-size: var(--font-size-sm);
    font-weight: var(--font-weight-semibold);
  }
  
  .status-notch {
    width: 120px;
    height: 32px;
    background: #000;
    border-radius: 0 0 20px 20px;
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateX(-50%);
  }
  
  .status-right {
    display: flex;
    align-items: center;
    gap: 6px;
  }
  
  .icon {
    opacity: 0.9;
  }
  
  /* Content Area */
  .content-area {
    flex: 1;
    overflow: hidden;
    position: relative;
  }
  
  .content-area.with-status-bar {
    padding-top: var(--status-bar-height);
  }
  
  .content-area.with-home-indicator {
    padding-bottom: var(--home-indicator-height);
  }
  
  /* Home Indicator */
  .home-indicator-area {
    height: var(--home-indicator-height);
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: var(--z-fixed);
  }
  
  .home-indicator {
    width: 134px;
    height: 5px;
    background: var(--color-text-primary, #fff);
    border-radius: 3px;
    opacity: 0.5;
  }
</style>
