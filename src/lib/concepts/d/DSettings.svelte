<!-- DSettings.svelte - Full Page Settings -->
<script>
    import { fly } from "svelte/transition";
    import {
        ChevronLeft,
        ChevronRight,
        Bell,
        Shield,
        Globe,
        FileText,
        Lock,
    } from "lucide-svelte";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let active = false;

    let settings = {
        pushNotifications: true,
        emailNotifications: false,
        faceId: true,
        location: true,
    };

    function goBack() {
        dispatch("back");
    }
</script>

{#if active}
    <div class="d-settings" in:fly={{ x: 20, duration: 400 }}>
        <!-- Header -->
        <header class="header">
            <button class="back-btn" on:click={goBack}>
                <ChevronLeft size={24} />
            </button>
            <h1>Settings</h1>
            <div class="placeholder"></div>
        </header>

        <div class="settings-content">
            <!-- Account Section -->
            <section class="settings-group">
                <h2 class="group-title">ACCOUNT & SECURITY</h2>

                <div class="setting-item">
                    <div class="item-icon">
                        <Shield size={20} />
                    </div>
                    <div class="item-info">
                        <span class="item-title">Face ID</span>
                        <span class="item-desc">Secure biometric login</span>
                    </div>
                    <div class="toggle-switch">
                        <input
                            type="checkbox"
                            id="faceid"
                            bind:checked={settings.faceId}
                        />
                        <label for="faceid"></label>
                    </div>
                </div>

                <div class="setting-item">
                    <div class="item-icon">
                        <Lock size={20} />
                    </div>
                    <div class="item-info">
                        <span class="item-title">Change PIN</span>
                    </div>
                    <ChevronRight size={16} class="chevron" />
                </div>
            </section>

            <!-- Preferences Section -->
            <section class="settings-group">
                <h2 class="group-title">PREFERENCES</h2>

                <div class="setting-item">
                    <div class="item-icon">
                        <Bell size={20} />
                    </div>
                    <div class="item-info">
                        <span class="item-title">Push Notifications</span>
                    </div>
                    <div class="toggle-switch">
                        <input
                            type="checkbox"
                            id="push"
                            bind:checked={settings.pushNotifications}
                        />
                        <label for="push"></label>
                    </div>
                </div>

                <div class="setting-item">
                    <div class="item-icon">
                        <Globe size={20} />
                    </div>
                    <div class="item-info">
                        <span class="item-title">Language</span>
                        <span class="item-value">English</span>
                    </div>
                    <ChevronRight size={16} class="chevron" />
                </div>
            </section>

            <!-- Support Section -->
            <section class="settings-group">
                <h2 class="group-title">SUPPORT</h2>

                <div class="setting-item">
                    <div class="item-icon">
                        <FileText size={20} />
                    </div>
                    <div class="item-info">
                        <span class="item-title">Terms of Service</span>
                    </div>
                    <ChevronRight size={16} class="chevron" />
                </div>

                <div class="setting-item">
                    <div class="item-icon">
                        <Shield size={20} />
                    </div>
                    <div class="item-info">
                        <span class="item-title">Privacy Policy</span>
                    </div>
                    <ChevronRight size={16} class="chevron" />
                </div>
            </section>

            <div class="version">Version 2.4.0 (Build 302)</div>
        </div>
    </div>
{/if}

<style>
    .d-settings {
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

    .settings-content {
        flex: 1;
        overflow-y: auto;
        padding: var(--space-6);
        padding-bottom: calc(var(--safe-area-bottom) + 20px);
    }

    .settings-group {
        margin-bottom: var(--space-8);
    }

    .group-title {
        font-size: var(--font-size-xs);
        color: var(--color-text-secondary);
        margin-bottom: var(--space-3);
        padding-left: var(--space-2);
        letter-spacing: 0.1em;
    }

    .setting-item {
        display: flex;
        align-items: center;
        background: rgba(255, 255, 255, 0.05);
        padding: var(--space-4);
        border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    }

    .setting-item:first-of-type {
        border-top-left-radius: var(--radius-md);
        border-top-right-radius: var(--radius-md);
    }

    .setting-item:last-of-type {
        border-bottom-left-radius: var(--radius-md);
        border-bottom-right-radius: var(--radius-md);
        border-bottom: none;
    }

    .item-icon {
        color: var(--color-text-secondary);
        margin-right: var(--space-4);
        display: flex;
    }

    .item-info {
        flex: 1;
        display: flex;
        flex-direction: column;
        gap: 2px;
    }

    .item-title {
        font-size: var(--font-size-base);
        font-weight: 500;
    }

    .item-desc {
        font-size: var(--font-size-xs);
        color: var(--color-text-secondary);
    }

    .item-value {
        font-size: var(--font-size-sm);
        color: var(--color-text-secondary);
    }

    /* Toggle Switch */
    .toggle-switch {
        position: relative;
        width: 44px;
        height: 24px;
    }

    .toggle-switch input {
        opacity: 0;
        width: 0;
        height: 0;
    }

    .toggle-switch label {
        position: absolute;
        cursor: pointer;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(255, 255, 255, 0.2);
        transition: 0.4s;
        border-radius: 24px;
    }

    .toggle-switch label:before {
        position: absolute;
        content: "";
        height: 20px;
        width: 20px;
        left: 2px;
        bottom: 2px;
        background-color: white;
        transition: 0.4s;
        border-radius: 50%;
    }

    .toggle-switch input:checked + label {
        background-color: #d4af37;
    }

    .toggle-switch input:checked + label:before {
        transform: translateX(20px);
    }

    .version {
        text-align: center;
        font-size: var(--font-size-xs);
        color: var(--color-text-secondary);
        opacity: 0.5;
        margin-top: var(--space-8);
    }
</style>
