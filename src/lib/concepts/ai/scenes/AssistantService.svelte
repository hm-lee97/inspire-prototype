<!--
  AssistantService.svelte
  AI Concept - Active Chat Service
  Glassmorphism style chat interface
-->
<script>
    import { createEventDispatcher, onMount } from "svelte";
    import { fade, fly, slide } from "svelte/transition";
    import { quintOut } from "svelte/easing";

    const dispatch = createEventDispatcher();

    export let initialQuery = "";

    let messages = [];
    let curationStep = 0; // 0: Idle, 1: Purpose, 2: Companion, 3: Duration, 4: Result
    let showOptions = false;

    // Simulate AI Processing
    onMount(() => {
        // User Message
        messages = [...messages, { id: 1, role: "user", text: initialQuery }];

        // Check if initial query triggers curation
        if (initialQuery.includes("체크인")) {
            setTimeout(() => {
                startCuration();
            }, 1000);
        } else {
            // Generic AI Response
            setTimeout(() => {
                messages = [
                    ...messages,
                    {
                        id: 2,
                        role: "ai",
                        text: `요청하신 "${initialQuery}" 내용을 확인하고 있습니다... 고객님을 위한 맞춤 서비스를 제안해 드립니다.`,
                    },
                ];
                showOptions = true;
                followUpOptions = [
                    "스마트 체크인 시작해줘",
                    "오늘 공연 뭐 있어?",
                ];
            }, 1200);
        }
    });

    // Initial Menu
    const options = [
        {
            id: 1,
            title: "스마트 체크인",
            desc: "빠른 입장을 도와드립니다",
            icon: "✨",
        },
        {
            id: 2,
            title: "오늘의 일정",
            desc: "오늘 밤 예정된 이벤트",
            icon: "📅",
        },
        {
            id: 3,
            title: "길 찾기",
            desc: "목적지까지 안내해 드립니다",
            icon: "🗺️",
        },
    ];

    // Curation Data
    const curationQuestions = {
        1: {
            text: "이번 방문의 주된 목적은 무엇인가요?",
            options: [
                { title: "호캉스 & 휴식", icon: "🌿" },
                { title: "엔터테인먼트", icon: "🎉" },
                { title: "다이닝 & 쇼핑", icon: "🛍️" },
            ],
        },
        2: {
            text: "누구와 함께 방문하시나요?",
            options: [
                { title: "연인과 함께", icon: "❤️" },
                { title: "가족과 함께", icon: "👨‍👩‍👧‍👦" },
                { title: "친구들과", icon: "👯" },
            ],
        },
        3: {
            text: "이번 여정은 얼마나 계획하고 계신가요?",
            options: [
                { title: "당일 치기", icon: "☀️" },
                { title: "1박 2일", icon: "🌙" },
                { title: "2박 3일 이상", icon: "✨" },
            ],
        },
    };

    function handleOptionClick(option) {
        // User selection message
        messages = [
            ...messages,
            { id: Date.now(), role: "user", text: option.title },
        ];

        if (curationStep > 0 && curationStep < 3) {
            // Advance Curation
            curationStep++;
            setTimeout(() => {
                askCurationQuestion(curationStep);
            }, 800);
        } else if (curationStep === 3) {
            // Finish Curation
            curationStep = 4;
            setTimeout(() => {
                showRecommendation();
            }, 1000);
        } else {
            // Initial Menu Selection
            if (option.title === "스마트 체크인") {
                startCuration();
            } else {
                // Other existing logic (Map, Schedule)
                messages = [
                    ...messages,
                    {
                        id: Date.now() + 1,
                        role: "ai",
                        text: `${option.title} 모듈을 실행합니다...`,
                    },
                ];
                setTimeout(() => {
                    let widgetType = "default";
                    if (option.title.includes("길")) widgetType = "map";
                    if (
                        option.title.includes("일정") ||
                        option.title.includes("공연")
                    )
                        widgetType = "ticket";

                    if (widgetType !== "default") {
                        messages = [
                            ...messages,
                            {
                                id: Date.now() + 2,
                                role: "ai",
                                text: "",
                                type: "widget",
                                widgetType: widgetType,
                                data: option,
                            },
                        ];
                        scrollToBottom();
                    }
                }, 1000);
            }
        }
    }

    function startCuration() {
        curationStep = 1;
        setTimeout(() => {
            askCurationQuestion(1);
        }, 800);
    }

    function askCurationQuestion(step) {
        messages = [
            ...messages,
            { id: Date.now(), role: "ai", text: curationQuestions[step].text },
        ];
        currentOptions = curationQuestions[step].options;
        scrollToBottom();
    }

    let currentOptions = options; // Default main menu
    let followUpOptions = []; // Contextual quick replies

    $: if (curationStep === 0) {
        currentOptions = options;
        followUpOptions = [];
    }

    /* Logic */
    function scrollToBottom() {
        setTimeout(() => {
            const historyEl = document.querySelector(".chat-history");
            if (historyEl) historyEl.scrollTop = historyEl.scrollHeight;
        }, 100);
    }

    function showRecommendation() {
        messages = [
            ...messages,
            {
                id: Date.now(),
                role: "ai",
                text: "고객님의 취향을 분석하여 완벽한 여정을 설계했습니다.",
                type: "widget",
                widgetType: "recommendation",
            },
        ];
        currentOptions = []; // Clear card options

        // precise-ui: Set follow-up chips
        setTimeout(() => {
            followUpOptions = [
                "이 코스로 예약해줘",
                "다른 추천 볼래",
                "상세 정보 보여줘",
            ];
            scrollToBottom();
        }, 500);

        scrollToBottom();
    }

    /* Continuous Chat Logic */
    let chatInput = "";

    function handleSendMessage() {
        if (!chatInput.trim()) return;

        const text = chatInput;
        // Add user message
        messages = [...messages, { id: Date.now(), role: "user", text: text }];
        chatInput = ""; // Clear input
        followUpOptions = []; // Clear chips on input
        scrollToBottom();

        // Simulate AI Processing
        setTimeout(() => {
            processMessage(text);
        }, 1000);
    }

    function handleQuickReply(text) {
        chatInput = text;
        handleSendMessage();
    }

    function processMessage(text) {
        let responseText = "";
        let widgetType = null;
        let widgetData = null;

        // Simple Rule-based Intent Matching
        if (text.includes("예약")) {
            responseText =
                "네, 제안드린 코스로 예약을 진행하겠습니다. 잠시만 기다려주세요.";
            setTimeout(() => {
                messages = [
                    ...messages,
                    {
                        id: Date.now(),
                        role: "ai",
                        text: "예약이 성공적으로 완료되었습니다.",
                        type: "widget",
                        widgetType: "booking_confirm",
                    },
                ];
                // Add Post-Booking Chips
                followUpOptions = [
                    "처음으로 돌아가기",
                    "친구에게 동선 공유하기",
                ];
                scrollToBottom();
            }, 1500);
        } else if (text.includes("처음") || text.includes("초기화")) {
            responseText = "초기 화면으로 돌아갑니다. 무엇을 도와드릴까요?";
            // Reset State
            curationStep = 0;
            showOptions = true;
            currentOptions = options;
            followUpOptions = ["스마트 체크인", "오늘의 일정", "길 찾기"];
        } else if (text.includes("공유")) {
            responseText =
                "동선이 친구분께 공유되었습니다! 친구분의 입장을 기다리고 있습니다.";
            followUpOptions = ["처음으로 돌아가기"];
        } else if (text.includes("다른") || text.includes("변경")) {
            responseText =
                "다른 스타일의 코스를 찾아보겠습니다. '엔터테인먼트' 중심의 활기찬 코스는 어떠신가요?";
            followUpOptions = ["좋아, 보여줘", "아니, 그냥 쉴래"];
        } else if (text.includes("상세") || text.includes("정보")) {
            responseText =
                "Forest Tower Suite는 45평형 스위트룸으로, 전면 통유리창을 통해 숲을 조망할 수 있습니다. 킹 사이즈 베드와 최고급 어메니티가 준비되어 있습니다.";
            followUpOptions = ["예약할래", "다른 방은 없어?"];
        } else if (text.includes("좋아") || text.includes("보여줘")) {
            responseText = "네, 여기 새로운 추천 코스입니다.";
            setTimeout(() => {
                messages = [
                    ...messages,
                    {
                        id: Date.now(),
                        role: "ai",
                        text: "엔터테인먼트 중심의 코스입니다.",
                        type: "widget",
                        widgetType: "recommendation_2",
                    },
                ];
                scrollToBottom();
                followUpOptions = ["이걸로 예약할래", "처음 추천이 더 나아"];
            }, 800);
            return; // Skip default add
        } else {
            responseText =
                "죄송합니다. 제가 이해하기 어려운 문장입니다. 위 버튼을 눌러보시거나, '예약', '추천' 등의 단어를 사용해 주세요.";
            followUpOptions = ["스마트 체크인", "길 찾기", "오늘의 일정"];
        }

        if (responseText) {
            messages = [
                ...messages,
                { id: Date.now(), role: "ai", text: responseText },
            ];
        }
        scrollToBottom();
    }
</script>

<div class="assistant-service">
    <div class="chat-history">
        {#each messages as msg (msg.id)}
            {#if msg.type === "widget"}
                <div
                    class="message-row ai widget-row"
                    in:fly={{ y: 20, duration: 500 }}
                >
                    <!-- Service Widgets -->
                    {#if msg.widgetType === "recommendation"}
                        <div class="service-widget rec-widget">
                            <div class="widget-header">
                                <span class="widget-title"
                                    >Recommended Journey</span
                                >
                            </div>
                            <!-- Room Recommendation -->
                            <div class="rec-card room-card">
                                <div class="rec-image room-image"></div>
                                <div class="rec-info">
                                    <span class="rec-label">SUITE ROOM</span>
                                    <span class="rec-name"
                                        >Forest Tower Lake View</span
                                    >
                                    <p class="rec-desc">
                                        채광이 가득한 숲 속의 휴식
                                    </p>
                                </div>
                            </div>
                            <!-- Course Recommendation -->
                            <div class="rec-card course-card">
                                <div class="rec-image course-image"></div>
                                <div class="rec-info">
                                    <span class="rec-label">CURATED COURSE</span
                                    >
                                    <span class="rec-name">Relaxation Walk</span
                                    >
                                    <p class="rec-desc">
                                        디스커버리 파크 → 오로라 바
                                    </p>
                                </div>
                            </div>
                            <div class="widget-footer">
                                <button
                                    class="action-btn"
                                    on:click={() =>
                                        handleQuickReply("이 코스로 예약해줘")}
                                    >이 코스로 예약하기</button
                                >
                            </div>
                        </div>
                    {:else if msg.widgetType === "recommendation_2"}
                        <div class="service-widget rec-widget">
                            <div class="widget-header">
                                <span class="widget-title"
                                    >Alternative Choice</span
                                >
                            </div>
                            <!-- Room Recommendation -->
                            <div class="rec-card room-card">
                                <div
                                    class="rec-image room-image"
                                    style="background-image: url('https://images.unsplash.com/photo-1566073771259-6a8506099945?auto=format&fit=crop&q=80&w=400')"
                                ></div>
                                <div class="rec-info">
                                    <span class="rec-label">DELUXE ROOM</span>
                                    <span class="rec-name"
                                        >Ocean Tower City View</span
                                    >
                                    <p class="rec-desc">
                                        화려한 야경과 함께하는 밤
                                    </p>
                                </div>
                            </div>
                            <!-- Course Recommendation -->
                            <div class="rec-card course-card">
                                <div
                                    class="rec-image course-image"
                                    style="background-image: url('https://images.unsplash.com/photo-1514525253440-b393452e8d26?auto=format&fit=crop&q=80&w=400')"
                                ></div>
                                <div class="rec-info">
                                    <span class="rec-label">FUN COURSE</span>
                                    <span class="rec-name">Splashing Bay</span>
                                    <p class="rec-desc">
                                        스플래시 베이 → 하이파이 치킨
                                    </p>
                                </div>
                            </div>
                            <div class="widget-footer">
                                <button
                                    class="action-btn"
                                    on:click={() =>
                                        handleQuickReply("이걸로 예약할래")}
                                    >이 코스로 예약하기</button
                                >
                            </div>
                        </div>
                    {:else if msg.widgetType === "booking_confirm"}
                        <div class="service-widget booking-widget">
                            <div class="widget-header">
                                <span class="widget-icon" style="color:#4CAF50"
                                    >✅</span
                                >
                                <span class="widget-title"
                                    >Booking Confirmed</span
                                >
                            </div>
                            <div class="booking-details">
                                <div class="booking-row">
                                    <span>예약 번호</span>
                                    <span class="val">#INS-2024-8821</span>
                                </div>
                                <div class="booking-row">
                                    <span>날짜</span>
                                    <span class="val">2026. 01. 10</span>
                                </div>
                            </div>
                            <div class="widget-footer">
                                <button
                                    class="action-btn"
                                    style="background: #252830; color: #8A8F98;"
                                    >내 예약 확인하기</button
                                >
                            </div>
                        </div>
                    {:else if msg.widgetType === "checkin"}
                        <!-- Legacy checkin (kept for fallback but unused in main flow now) -->
                        <div class="service-widget checkin-widget">
                            <!-- ... content ... -->
                        </div>
                    {:else if msg.widgetType === "map"}
                        <div class="service-widget map-widget">
                            <div class="widget-header">
                                <span class="widget-icon">🗺️</span>
                                <span class="widget-title"
                                    >Navigation Started</span
                                >
                            </div>
                            <div class="map-placeholder">
                                <div class="path-line"></div>
                                <div class="location-dot"></div>
                            </div>
                            <div class="widget-footer">
                                <p>예상 소요 시간: 5분</p>
                            </div>
                        </div>
                    {:else if msg.widgetType === "ticket"}
                        <div class="service-widget ticket-widget">
                            <div class="widget-header">
                                <span class="widget-icon">🎟️</span>
                                <span class="widget-title">Tonight's Show</span>
                            </div>
                            <div class="ticket-content">
                                <div class="ticket-time">19:30</div>
                                <div class="ticket-name">
                                    Aurora Performance
                                </div>
                            </div>
                            <div class="widget-footer">
                                <button class="action-btn">티켓 보기</button>
                            </div>
                        </div>
                    {/if}
                </div>
            {:else}
                <div
                    class="message-row {msg.role}"
                    in:fly={{ y: 20, duration: 400 }}
                >
                    <div class="message-bubble">
                        {msg.text}
                    </div>
                </div>
            {/if}
        {/each}

        {#if followUpOptions.length > 0}
            <div class="quick-replies" in:fade={{ duration: 300 }}>
                {#each followUpOptions as reply}
                    <button
                        class="reply-chip"
                        on:click={() => handleQuickReply(reply)}
                    >
                        {reply}
                    </button>
                {/each}
            </div>
        {/if}

        {#if showOptions && currentOptions.length > 0 && !messages.find((m) => m.type === "widget" && m.widgetType === "recommendation")}
            <div
                class="options-container"
                in:fade={{ duration: 500, delay: 200 }}
            >
                <p class="options-label">
                    {curationStep > 0
                        ? "답변을 선택해주세요"
                        : "AI 추천 서비스"}
                </p>
                <div class="options-grid">
                    {#each currentOptions as option, i (option.title)}
                        <button
                            class="option-card"
                            in:fly={{
                                y: 20,
                                duration: 500,
                                delay: i * 50,
                            }}
                            on:click={() => handleOptionClick(option)}
                        >
                            <span class="option-icon">{option.icon}</span>
                            <div class="option-info">
                                <span class="option-title">{option.title}</span>
                                {#if option.desc}<span class="option-desc"
                                        >{option.desc}</span
                                    >{/if}
                            </div>
                        </button>
                    {/each}
                </div>
            </div>
        {/if}
    </div>

    <!-- Persistent Input -->
    <div class="input-area">
        <form on:submit|preventDefault={handleSendMessage}>
            <input
                type="text"
                bind:value={chatInput}
                placeholder="메시지를 입력하세요..."
                class="chat-input"
            />
            <button type="submit" class="send-btn" disabled={!chatInput.trim()}>
                <svg
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                >
                    <path d="M22 2L11 13M22 2l-7 20-4-9-9-4 20-7z"></path>
                </svg>
            </button>
        </form>
    </div>
</div>

<style>
    .assistant-service {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        padding-bottom: 0; /* Changed from var(--space-8) to 0 to accommodate input area */
        font-family: var(--font-ai);
    }

    .chat-history {
        flex: 1;
        overflow-y: auto;
        padding: 24px 20px;
        display: flex;
        flex-direction: column;
        gap: 24px;
        scroll-behavior: smooth;
        padding-bottom: 80px; /* Space for input area */
    }

    /* Input Area */
    .input-area {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        padding: 16px;
        background: rgba(15, 17, 21, 0.95);
        border-top: 1px solid rgba(255, 255, 255, 0.08); /* Linear border */
        backdrop-filter: blur(10px);
    }

    .input-area form {
        display: flex;
        gap: 8px;
        position: relative;
    }

    .chat-input {
        flex: 1;
        height: 44px;
        background: #0f1115;
        border: 1px solid rgba(255, 255, 255, 0.1);
        border-radius: 8px;
        padding: 0 44px 0 16px;
        color: white;
        font-size: 14px;
        outline: none;
        transition: border-color 0.2s;
    }

    .chat-input:focus {
        border-color: rgba(255, 255, 255, 0.2);
    }

    .send-btn {
        position: absolute;
        right: 8px;
        top: 50%;
        transform: translateY(-50%);
        width: 28px;
        height: 28px;
        background: transparent;
        border: none;
        color: #5e6ad2;
        cursor: pointer;
        padding: 4px;
        opacity: 0.8;
        transition: opacity 0.2s;
        border-radius: 4px;
    }

    .send-btn:hover:not(:disabled) {
        opacity: 1;
        background: rgba(94, 106, 210, 0.1);
    }

    .send-btn:disabled {
        opacity: 0.3;
        cursor: not-allowed;
    }

    .send-btn svg {
        width: 100%;
        height: 100%;
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
        max-width: 85%;
        padding: 12px 16px;
        border-radius: 12px;
        font-size: 15px;
        line-height: 1.5;
        font-weight: 400;
    }

    .user .message-bubble {
        background: #5e6ad2; /* Linear Purple/Blue Accent */
        color: #ffffff;
        border-bottom-right-radius: 4px;
        box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
    }

    .ai .message-bubble {
        background: transparent;
        color: #f7f8f8;
        padding-left: 0; /* Align with avatar conceptually */
    }

    /* Options */
    .options-container {
        margin-top: 8px;
        display: flex;
        flex-direction: column;
        gap: 12px;
    }

    .options-label {
        font-size: 11px;
        font-weight: 600;
        color: #8a8f98;
        letter-spacing: 0.05em;
        text-transform: uppercase;
        margin-left: 0;
        margin-bottom: 4px;
    }

    .options-grid {
        display: contents;
    }

    .option-card {
        display: flex;
        align-items: center;
        gap: 16px;
        padding: 12px 16px;
        background: rgba(255, 255, 255, 0.03);
        border: 1px solid rgba(255, 255, 255, 0.08);
        border-radius: 8px;
        text-align: left;
        transition: all 0.2s cubic-bezier(0.2, 0, 0, 1);
        cursor: pointer;
    }

    .option-card:hover {
        background: rgba(255, 255, 255, 0.06);
        border-color: rgba(255, 255, 255, 0.15);
        transform: translateY(-1px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    }

    .option-icon {
        font-size: 20px;
        filter: grayscale(100%);
        opacity: 0.8;
        transition: all 0.2s;
    }

    .option-card:hover .option-icon {
        filter: grayscale(0%);
        opacity: 1;
        transform: scale(1.1);
    }

    .option-info {
        display: flex;
        flex-direction: column;
        gap: 2px;
    }

    .option-title {
        font-size: 14px;
        font-weight: 500;
        color: #f7f8f8;
    }

    .option-desc {
        font-size: 12px;
        color: #8a8f98;
    }

    /* Service Widgets */
    .service-widget {
        width: 100%;
        max-width: 320px;
        background: #191b21;
        border: 1px solid rgba(255, 255, 255, 0.1);
        border-radius: 12px;
        padding: 16px;
        display: flex;
        flex-direction: column;
        gap: 16px;
        box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4);
    }

    .widget-header {
        display: flex;
        align-items: center;
        gap: 10px;
        border-bottom: 1px solid rgba(255, 255, 255, 0.05);
        padding-bottom: 12px;
    }

    .widget-title {
        font-size: 14px;
        font-weight: 600;
        color: #f7f8f8;
    }

    /* Check-in Widget */
    .qr-code-placeholder {
        width: 100%;
        height: 180px;
        background: #0f1115;
        border-radius: 8px;
        position: relative;
        overflow: hidden;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .qr-pattern {
        width: 120px;
        height: 120px;
        background-image: radial-gradient(#5e6ad2 2px, transparent 2px);
        background-size: 10px 10px;
        opacity: 0.8;
    }

    .scan-line {
        position: absolute;
        width: 100%;
        height: 2px;
        background: #5e6ad2;
        box-shadow: 0 0 10px #5e6ad2;
        top: 0;
        animation: scan 2s linear infinite;
    }

    @keyframes scan {
        0% {
            top: 0;
        }
        100% {
            top: 100%;
        }
    }

    .widget-footer p {
        font-size: 13px;
        color: #8a8f98;
        text-align: center;
        margin: 0;
    }

    /* Map Widget */
    .map-placeholder {
        width: 100%;
        height: 140px;
        background: #252830;
        border-radius: 8px;
        position: relative;
        overflow: hidden;
    }

    .path-line {
        position: absolute;
        width: 80%;
        height: 80%;
        top: 10%;
        left: 10%;
        border-left: 2px dashed #5e6ad2;
        border-bottom: 2px dashed #5e6ad2;
    }

    .location-dot {
        position: absolute;
        bottom: 10%;
        right: 10%;
        width: 12px;
        height: 12px;
        background: #5e6ad2;
        border-radius: 50%;
        box-shadow: 0 0 10px #5e6ad2;
        animation: pulse-dot 1.5s infinite;
    }

    @keyframes pulse-dot {
        0% {
            transform: scale(1);
            opacity: 1;
        }
        100% {
            transform: scale(1.5);
            opacity: 0;
        }
    }

    /* Ticket Widget */
    .ticket-content {
        background: linear-gradient(135deg, #1c1e23 0%, #0f1115 100%);
        border: 1px dashed rgba(255, 255, 255, 0.1);
        padding: 16px;
        border-radius: 8px;
        text-align: center;
    }

    .ticket-time {
        font-size: 24px;
        font-weight: 700;
        color: #5e6ad2;
        margin-bottom: 4px;
    }

    .ticket-name {
        font-size: 14px;
        color: #f7f8f8;
    }

    .action-btn {
        width: 100%;
        padding: 12px;
        background: #5e6ad2;
        color: white;
        border: none;
        border-radius: 8px;
        font-size: 14px;
        font-weight: 600;
        cursor: pointer;
        transition: all 0.2s;
        margin-top: 8px;
    }

    .action-btn:hover {
        background: #6e7be4;
        transform: translateY(-1px);
        box-shadow: 0 4px 12px rgba(94, 106, 210, 0.3);
    }

    /* Recommendation Widget */
    .rec-widget {
        max-width: 360px; /* Slightly wider */
    }

    .rec-card {
        background: rgba(255, 255, 255, 0.05);
        border: 1px solid rgba(255, 255, 255, 0.1);
        border-radius: 8px;
        overflow: hidden;
        display: flex;
        flex-direction: column;
        margin-bottom: 8px;
    }

    /* ... rec styles ... */

    /* Booking Widget */
    .booking-details {
        background: rgba(0, 0, 0, 0.2);
        border-radius: 8px;
        padding: 12px;
        display: flex;
        flex-direction: column;
        gap: 8px;
    }

    .booking-row {
        display: flex;
        justify-content: space-between;
        font-size: 13px;
        color: #8a8f98;
    }

    .booking-row .val {
        color: #f7f8f8;
        font-weight: 600;
    }

    /* Quick Replies */
    .quick-replies {
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
        padding-left: 16px; /* Align with bubbles somewhat */
        margin-bottom: 8px;
    }

    .reply-chip {
        background: transparent;
        border: 1px solid rgba(94, 106, 210, 0.3);
        color: #5e6ad2;
        padding: 6px 12px;
        border-radius: 16px;
        font-size: 12px;
        font-weight: 500;
        cursor: pointer;
        transition: all 0.2s;
    }

    .reply-chip:hover {
        background: rgba(94, 106, 210, 0.1);
        border-color: #5e6ad2;
        transform: translateY(-1px);
    }

    .rec-image {
        width: 100%;
        height: 120px;
        background-color: #252830;
        background-size: cover;
        background-position: center;
    }

    .room-image {
        background-image: url("https://images.unsplash.com/photo-1618773928121-c32242e63f39?auto=format&fit=crop&q=80&w=400");
    }
    .course-image {
        background-image: url("https://images.unsplash.com/photo-1544965836-822d86fb5d39?auto=format&fit=crop&q=80&w=400");
    }

    .rec-info {
        padding: 12px 16px;
        display: flex;
        flex-direction: column;
        gap: 4px;
    }

    .rec-label {
        font-size: 10px;
        font-weight: 700;
        letter-spacing: 0.1em;
        color: #5e6ad2;
        text-transform: uppercase;
    }

    .rec-name {
        font-size: 15px;
        font-weight: 600;
        color: #f7f8f8;
    }

    .rec-desc {
        font-size: 13px;
        color: #8a8f98;
        margin: 0;
    }
</style>
