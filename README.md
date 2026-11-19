<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>XENOVA AI</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
        }

        body {
            background: #0a0a0f;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 8px;
        }

        .app {
            width: 100%;
            max-width: 900px;
            height: 100vh;
            max-height: 900px;
            background: #111118;
            border-radius: 32px;
            display: flex;
            flex-direction: column;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.8), inset 0 1px 2px rgba(255, 255, 255, 0.03);
            border: 1px solid #1e1e2a;
            overflow: hidden;
        }

        /* header */
        .header {
            padding: 16px 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 1px solid #1e1e2a;
            background: #111118;
            flex-shrink: 0;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .logo-icon {
            width: 32px;
            height: 32px;
            background: linear-gradient(135deg, #6d5dfc, #b25dfc);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            font-size: 18px;
            color: white;
            box-shadow: 0 4px 12px rgba(109, 93, 252, 0.3);
        }

        .logo-text {
            font-size: 20px;
            font-weight: 600;
            letter-spacing: -0.5px;
            color: #ffffff;
            background: linear-gradient(135deg, #e0e0ff, #b0a8ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .badge {
            font-size: 11px;
            font-weight: 500;
            padding: 4px 10px;
            border-radius: 20px;
            background: #1e1e2a;
            color: #a09ac0;
            border: 1px solid #2a2a3a;
            letter-spacing: 0.3px;
        }

        /* chat area */
        .chat-area {
            flex: 1;
            overflow-y: auto;
            padding: 20px 20px 8px 20px;
            display: flex;
            flex-direction: column;
            gap: 16px;
            scroll-behavior: smooth;
        }

        .chat-area::-webkit-scrollbar {
            width: 6px;
        }

        .chat-area::-webkit-scrollbar-track {
            background: #111118;
        }

        .chat-area::-webkit-scrollbar-thumb {
            background: #2d2d3d;
            border-radius: 10px;
        }

        .message {
            display: flex;
            gap: 12px;
            max-width: 100%;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(8px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .message.user {
            flex-direction: row-reverse;
        }

        .avatar {
            width: 36px;
            height: 36px;
            border-radius: 12px;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 16px;
            font-weight: 600;
        }

        .avatar.bot {
            background: linear-gradient(135deg, #2a2a3a, #1e1e2a);
            border: 1px solid #2d2d3d;
            color: #b0a8ff;
        }

        .avatar.user {
            background: #2a2a3a;
            border: 1px solid #3a3a4a;
            color: #d0d0e0;
        }

        .bubble {
            padding: 12px 16px;
            border-radius: 18px;
            font-size: 15px;
            line-height: 1.6;
            max-width: 80%;
            word-break: break-word;
            white-space: pre-wrap;
        }

        .bubble.user {
            background: #6d5dfc;
            color: white;
            border-bottom-right-radius: 6px;
            box-shadow: 0 4px 16px rgba(109, 93, 252, 0.2);
        }

        .bubble.bot {
            background: #1a1a24;
            color: #e4e4f0;
            border: 1px solid #2a2a3a;
            border-bottom-left-radius: 6px;
        }

        .bubble.bot .tip {
            margin-top: 10px;
            padding-top: 8px;
            border-top: 1px solid #2a2a3a;
            color: #9a94b0;
            font-size: 13px;
            font-style: italic;
        }

        /* typing indicator */
        .typing {
            display: flex;
            gap: 5px;
            padding: 8px 4px;
        }

        .typing span {
            width: 7px;
            height: 7px;
            background: #6d5dfc;
            border-radius: 50%;
            animation: bounce 1.4s infinite ease-in-out;
        }

        .typing span:nth-child(2) { animation-delay: 0.2s; }
        .typing span:nth-child(3) { animation-delay: 0.4s; }

        @keyframes bounce {
            0%, 60%, 100% { transform: translateY(0); opacity: 0.5; }
            30% { transform: translateY(-6px); opacity: 1; }
        }

        /* input area */
        .input-area {
            padding: 12px 16px 18px 16px;
            background: #111118;
            border-top: 1px solid #1e1e2a;
            flex-shrink: 0;
        }

        .input-wrap {
            display: flex;
            gap: 10px;
            align-items: flex-end;
            background: #1a1a24;
            border-radius: 28px;
            padding: 6px 6px 6px 20px;
            border: 1px solid #2a2a3a;
            transition: border-color 0.2s;
        }

        .input-wrap:focus-within {
            border-color: #6d5dfc;
            box-shadow: 0 0 0 3px rgba(109, 93, 252, 0.15);
        }

        .input-wrap textarea {
            flex: 1;
            background: transparent;
            border: none;
            outline: none;
            color: #e4e4f0;
            font-size: 15px;
            resize: none;
            padding: 8px 0;
            max-height: 120px;
            line-height: 1.5;
            font-family: inherit;
        }

        .input-wrap textarea::placeholder {
            color: #5a5a70;
        }

        .send-btn {
            width: 42px;
            height: 42px;
            border-radius: 50%;
            background: linear-gradient(135deg, #6d5dfc, #8a5dfc);
            border: none;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s;
            flex-shrink: 0;
        }

        .send-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 0 20px rgba(109, 93, 252, 0.4);
        }

        .send-btn:disabled {
            opacity: 0.4;
            transform: none;
            cursor: not-allowed;
            box-shadow: none;
        }

        .send-btn svg {
            width: 20px;
            height: 20px;
            fill: white;
        }

        .footer-note {
            text-align: center;
            font-size: 11px;
            color: #4a4a5a;
            margin-top: 8px;
            letter-spacing: 0.3px;
        }

        @media (max-width: 600px) {
            .app {
                border-radius: 0;
                max-height: 100vh;
                height: 100vh;
                border: none;
                box-shadow: none;
            }
            .header { padding: 14px 16px; }
            .logo-text { font-size: 18px; }
            .logo-icon { width: 28px; height: 28px; font-size: 16px; }
            .badge { font-size: 10px; padding: 3px 8px; }
            .bubble { font-size: 14px; max-width: 85%; }
            .chat-area { padding: 16px 14px 6px 14px; }
            .input-area { padding: 10px 12px 14px 12px; }
            .avatar { width: 32px; height: 32px; font-size: 14px; }
        }
    </style>
</head>
<body>
    <div class="app">
        <div class="header">
            <div class="logo">
                <div class="logo-icon">X</div>
                <span class="logo-text">XENOVA AI</span>
            </div>
            <div class="badge">⚡ Gemini + Backup</div>
        </div>

        <div class="chat-area" id="chatArea">
            <div class="message bot">
                <div class="avatar bot">X</div>
                <div class="bubble bot">
                    Hello, I am XENOVA AI. Ask me anything and I will give you a clean, accurate answer.
                    <div class="tip">Tip: Keep your questions short for the best answers.</div>
                </div>
            </div>
        </div>

        <div class="input-area">
            <div class="input-wrap">
                <textarea id="userInput" rows="1" placeholder="Ask anything..." autofocus></textarea>
                <button class="send-btn" id="sendBtn" aria-label="Send message">
                    <svg viewBox="0 0 24 24"><path d="M2.01 21L23 12 2.01 3 2 10l15 2-15 2z"/></svg>
                </button>
            </div>
            <div class="footer-note">XENOVA AI · fast · clean · reliable</div>
        </div>
    </div>

    <script>
        // ============================================================
        // 🔑 PASTE YOUR GEMINI API KEYS HERE
        // ============================================================
        const GEMINI_KEYS = [
            "AQ.Ab8RN6KnyHbFczsIS1sjt7r9wfug9TQz1tL3g2POlIHsjVr1Kg",
            "AQ.Ab8RN6JavdoIOx95fjoFljXRj-Qc8lqBB98aczL36L3UPTQzWQ",
            "AQ.Ab8RN6KySnGCx7yk0bi3DB8FXupFbNxbckGiola0VVKiiqfoxA",
            "AQ.Ab8RN6JR4QMPO3MZ2G-1yOiPMrL61FoF3uM5LIK33PMJdUatxA",
            "AQ.Ab8RN6JM-ifg5xTCnJGSP0JQtJy6DhkYhoKH4huEFMOmwHMY7w"
        ];

        // Models tried in order
        const MODELS = [
            "gemini-2.0-flash",
            "gemini-2.5-flash",
            "gemini-1.5-flash"
        ];

        // Backup endpoint
        const BACKUP_URL = "https://text.pollinations.ai/";

        // ============================================================
        // DOM
        // ============================================================
        const chatArea = document.getElementById('chatArea');
        const userInput = document.getElementById('userInput');
        const sendBtn = document.getElementById('sendBtn');

        // ============================================================
        // STATE
        // ============================================================
        let isBusy = false;

        // ============================================================
        // UI HELPERS
        // ============================================================
        function scrollToBottom() {
            chatArea.scrollTop = chatArea.scrollHeight;
        }

        function addMessage(text, sender) {
            const msgDiv = document.createElement('div');
            msgDiv.className = `message ${sender}`;

            const avatar = document.createElement('div');
            avatar.className = `avatar ${sender}`;
            avatar.textContent = sender === 'user' ? 'U' : 'X';

            const bubble = document.createElement('div');
            bubble.className = `bubble ${sender}`;

            // For bot, split tip line (if present) for styling
            if (sender === 'bot' && text.includes('\nTip:')) {
                const parts = text.split('\nTip:');
                bubble.textContent = parts[0].trim();
                if (parts[1]) {
                    const tipDiv = document.createElement('div');
                    tipDiv.className = 'tip';
                    tipDiv.textContent = 'Tip: ' + parts[1].trim();
                    bubble.appendChild(tipDiv);
                }
            } else {
                bubble.textContent = text;
            }

            msgDiv.appendChild(avatar);
            msgDiv.appendChild(bubble);
            chatArea.appendChild(msgDiv);
            scrollToBottom();
        }

        function addTypingIndicator() {
            const msgDiv = document.createElement('div');
            msgDiv.className = 'message bot';
            msgDiv.id = 'typingIndicator';

            const avatar = document.createElement('div');
            avatar.className = 'avatar bot';
            avatar.textContent = 'X';

            const bubble = document.createElement('div');
            bubble.className = 'bubble bot';
            bubble.style.padding = '12px 16px';

            const typing = document.createElement('div');
            typing.className = 'typing';
            typing.innerHTML = '<span></span><span></span><span></span>';

            bubble.appendChild(typing);
            msgDiv.appendChild(avatar);
            msgDiv.appendChild(bubble);
            chatArea.appendChild(msgDiv);
            scrollToBottom();
        }

        function removeTypingIndicator() {
            const el = document.getElementById('typingIndicator');
            if (el) el.remove();
        }

        function setBusy(state) {
            isBusy = state;
            sendBtn.disabled = state;
            userInput.disabled = state;
            if (!state) userInput.focus();
        }

        // ============================================================
        // POST-PROCESSING — clean answer rule set
        // ============================================================
        function cleanAnswer(raw) {
            if (!raw) return "I could not generate a response. Please try again.";

            let text = raw;

            // Remove code blocks
            text = text.replace(/```[\s\S]*?```/g, '');
            // Remove inline code backticks
            text = text.replace(/`([^`]+)`/g, '$1');
            // Remove markdown links [text](url) -> text
            text = text.replace(/\[([^\]]+)\]\([^)]+\)/g, '$1');
            // Remove raw URLs
            text = text.replace(/https?:\/\/\S+/gi, '');
            // Remove markdown headers
            text = text.replace(/^#{1,6}\s+/gm, '');
            // Remove bold/italic markers
            text = text.replace(/(\*\*|__)(.*?)\1/g, '$2');
            text = text.replace(/(\*|_)(.*?)\1/g, '$2');
            // Remove bullet markers and numbered list markers at line start
            text = text.replace(/^\s*[-*•–—]\s+/gm, '');
            text = text.replace(/^\s*\d+[.)]\s+/gm, '');
            // Remove emojis (broad unicode ranges)
            text = text.replace(/[\u{1F300}-\u{1FAFF}\u{1F600}-\u{1F64F}\u{1F680}-\u{1F6FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{1F900}-\u{1F9FF}\u{1FA70}-\u{1FAFF}\u{1F1E6}-\u{1F1FF}\u{2190}-\u{21FF}\u{2B00}-\u{2BFF}\u{FE00}-\u{FE0F}\u{200D}\u{20E3}\u{E0020}-\u{E007F}]/gu, '');
            // Remove hashtags
            text = text.replace(/#\w+/g, '');
            // Remove leftover asterisks and hashes
            text = text.replace(/[*#]/g, '');
            // Remove leading dashes
            text = text.replace(/^\s*[-–—]\s*/gm, '');
            // Collapse excessive whitespace
            text = text.replace(/[ \t]+/g, ' ');
            text = text.replace(/\n{3,}/g, '\n\n');
            // Trim each line
            text = text.split('\n').map(l => l.trim()).filter(l => l.length > 0).join('\n');

            // Ensure short points each on new line — split long sentences after periods if very long
            // We keep it simple: if the whole answer is one giant paragraph, split into short lines by sentence.
            const lines = text.split('\n');
            const processedLines = [];
            for (let line of lines) {
                if (line.length > 140) {
                    // Split into sentences roughly
                    const sentences = line.match(/[^.!?]+[.!?]*/g) || [line];
                    let buf = '';
                    for (let s of sentences) {
                        s = s.trim();
                        if (!s) continue;
                        if ((buf + ' ' + s).length > 120) {
                            if (buf) processedLines.push(buf.trim());
                            buf = s;
                        } else {
                            buf = buf ? buf + ' ' + s : s;
                        }
                    }
                    if (buf) processedLines.push(buf.trim());
                } else {
                    processedLines.push(line);
                }
            }
            text = processedLines.join('\n');

            // If result is empty, fallback
            if (!text.trim()) return "I could not generate a response. Please try again.";

            // Add tip if not already present
            if (!text.toLowerCase().includes('tip:')) {
                text += '\nTip: ' + generateTip();
            }

            return text;
        }

        function generateTip() {
            const tips = [
                'Ask follow up questions to go deeper.',
                'Short questions often give the clearest answers.',
                'You can ask for examples or comparisons.',
                'Try asking why or how to get better detail.',
                'If an answer is unclear, ask me to simplify it.',
                'Ask for a real world example to make it stick.',
                'You can ask me to explain like you are five.',
                'Request a summary if the answer feels long.'
            ];
            return tips[Math.floor(Math.random() * tips.length)];
        }

        // ============================================================
        // GEMINI CALL — tries all keys × models
        // ============================================================
        async function tryGemini(prompt) {
            for (const model of MODELS) {
                for (let i = 0; i < GEMINI_KEYS.length; i++) {
                    const key = GEMINI_KEYS[i];
                    if (!key || key.startsWith('YOUR_KEY')) continue; // skip placeholder

                    try {
                        const url = `https://generativelanguage.googleapis.com/v1beta/models/${model}:generateContent?key=${key}`;
                        const body = {
                            contents: [{
                                parts: [{ text: prompt }]
                            }],
                            generationConfig: {
                                temperature: 0.7,
                                maxOutputTokens: 1024
                            }
                        };

                        const res = await fetch(url, {
                            method: 'POST',
                            headers: { 'Content-Type': 'application/json' },
                            body: JSON.stringify(body)
                        });

                        // 429 or any failure → next key immediately
                        if (!res.ok) {
                            // Log lightly but continue instantly
                            console.warn(`Key ${i + 1} on ${model} failed: ${res.status}`);
                            continue;
                        }

                        const data = await res.json();
                        const text = data?.candidates?.[0]?.content?.parts?.[0]?.text;
                        if (text && text.trim()) {
                            return { ok: true, text };
                        } else {
                            console.warn(`Empty response from ${model} key ${i + 1}`);
                            continue;
                        }
                    } catch (e) {
                        // Network or other error → instantly try next
                        console.warn(`Key ${i + 1} on ${model} error:`, e.message);
                        continue;
                    }
                }
