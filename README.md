# -ômega-core-ia
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ômega Core AI | Rede Ômega Quântica</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;600;700;800;900&family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-oled: #000000;
            --bg-surface: #060606;
            --bg-card: #0d0d0d;
            --bg-input: #101010;
            --gold-primary: #D4AF37;
            --gold-bright: #FFD700;
            --gold-light: #F0E68C;
            --gold-dark: #B8860B;
            --gold-glow: #FFC107;
            --text-primary: #e8e8e8;
            --text-secondary: #b0b0b0;
            --text-dim: #707070;
            --border-gold: rgba(212, 175, 55, 0.25);
            --accent-gold-gradient: linear-gradient(135deg, #B8860B, #D4AF37, #FFD700, #D4AF37, #B8860B);
            --glow-gold: 0 0 20px rgba(212, 175, 55, 0.4), 0 0 60px rgba(212, 175, 55, 0.15);
            --glow-intense: 0 0 30px rgba(255, 215, 0, 0.6), 0 0 80px rgba(255, 215, 0, 0.3);
            --font-display: 'Orbitron', sans-serif;
            --font-body: 'Inter', sans-serif;
            --font-mono: 'JetBrains Mono', monospace;
            --sidebar-width: 290px;
            --transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        html,
        body {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background: var(--bg-oled);
            font-family: var(--font-body);
            color: var(--text-primary);
            -webkit-font-smoothing: antialiased;
        }
        #neural-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
            pointer-events: none;
            opacity: 0.65;
        }
        #app-container {
            position: relative;
            z-index: 1;
            display: flex;
            width: 100%;
            height: 100%;
        }

        /* SIDEBAR */
        #sidebar {
            width: var(--sidebar-width);
            min-width: var(--sidebar-width);
            height: 100%;
            background: rgba(6, 6, 6, 0.88);
            border-right: 1px solid var(--border-gold);
            display: flex;
            flex-direction: column;
            z-index: 10;
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            transition: transform var(--transition);
        }
        #sidebar-header {
            padding: 18px 14px 10px;
            border-bottom: 1px solid rgba(212, 175, 55, 0.12);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        #sidebar-logo {
            width: 40px;
            height: 40px;
            border-radius: 10px;
            background: var(--accent-gold-gradient);
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: var(--font-display);
            font-weight: 900;
            font-size: 17px;
            color: #000;
            letter-spacing: -1px;
            box-shadow: var(--glow-gold);
            flex-shrink: 0;
            animation: logoGlow 3s ease-in-out infinite;
        }
        @keyframes logoGlow {
            0%,
            100% {
                box-shadow: 0 0 20px rgba(212, 175, 55, 0.4), 0 0 60px rgba(212, 175, 55, 0.15);
            }
            50% {
                box-shadow: 0 0 35px rgba(255, 215, 0, 0.7), 0 0 90px rgba(255, 215, 0, 0.3);
            }
        }
        #sidebar-title {
            font-family: var(--font-display);
            font-weight: 700;
            font-size: 12px;
            letter-spacing: 1.5px;
            background: var(--accent-gold-gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-transform: uppercase;
            line-height: 1.2;
        }
        #sidebar-title span {
            font-size: 9px;
            letter-spacing: 2px;
            font-weight: 500;
            display: block;
        }
        #new-chat-btn {
            margin: 12px;
            padding: 11px 16px;
            background: rgba(212, 175, 55, 0.07);
            border: 1px solid rgba(212, 175, 55, 0.35);
            border-radius: 12px;
            color: var(--gold-bright);
            cursor: pointer;
            font-family: var(--font-body);
            font-weight: 500;
            font-size: 12px;
            letter-spacing: 0.5px;
            transition: all var(--transition);
            display: flex;
            align-items: center;
            gap: 8px;
            width: calc(100% - 24px);
        }
        #new-chat-btn:hover {
            background: rgba(212, 175, 55, 0.16);
            border-color: rgba(212, 175, 55, 0.6);
            box-shadow: 0 0 22px rgba(212, 175, 55, 0.2);
        }
        #conversations-list {
            flex: 1;
            overflow-y: auto;
            padding: 4px 6px;
            display: flex;
            flex-direction: column;
            gap: 1px;
        }
        #conversations-list::-webkit-scrollbar {
            width: 3px;
        }
        #conversations-list::-webkit-scrollbar-thumb {
            background: rgba(212, 175, 55, 0.2);
            border-radius: 10px;
        }
        .conv-item {
            padding: 10px 12px;
            border-radius: 10px;
            cursor: pointer;
            font-size: 11px;
            color: var(--text-secondary);
            transition: all var(--transition);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            border: 1px solid transparent;
        }
        .conv-item:hover {
            background: rgba(212, 175, 55, 0.05);
            color: var(--text-primary);
            border-color: rgba(212, 175, 55, 0.2);
        }
        .conv-item.active {
            background: rgba(212, 175, 55, 0.12);
            color: var(--gold-bright);
            border-color: rgba(212, 175, 55, 0.4);
            font-weight: 500;
            box-shadow: 0 0 12px rgba(212, 175, 55, 0.07);
        }
        .conv-dot {
            display: inline-block;
            width: 5px;
            height: 5px;
            border-radius: 50%;
            background: var(--gold-bright);
            margin-right: 7px;
            box-shadow: 0 0 6px var(--gold-glow);
            flex-shrink: 0;
            vertical-align: middle;
        }
        #sidebar-footer {
            padding: 10px 14px;
            border-top: 1px solid rgba(212, 175, 55, 0.12);
            font-size: 9px;
            color: var(--text-dim);
            text-align: center;
            letter-spacing: 1px;
            font-family: var(--font-mono);
            display: flex;
            flex-direction: column;
            gap: 4px;
        }
        .status-dot {
            display: inline-block;
            width: 7px;
            height: 7px;
            border-radius: 50%;
            background: #00ff88;
            box-shadow: 0 0 10px #00ff88, 0 0 25px #00ff8877;
            animation: statusPulse 2s ease-in-out infinite;
            vertical-align: middle;
        }
        @keyframes statusPulse {
            0%,
            100% {
                box-shadow: 0 0 8px #00ff88, 0 0 20px #00ff8877;
            }
            50% {
                box-shadow: 0 0 18px #00ff88, 0 0 40px #00ff88aa;
            }
        }
        #hf-token-input {
            width: 100%;
            background: rgba(20, 20, 20, 0.9);
            border: 1px solid rgba(212, 175, 55, 0.2);
            border-radius: 6px;
            color: #ccc;
            font-size: 9px;
            padding: 5px 8px;
            font-family: var(--font-mono);
            text-align: center;
            outline: none;
            transition: all var(--transition);
        }
        #hf-token-input:focus {
            border-color: rgba(212, 175, 55, 0.5);
            box-shadow: 0 0 10px rgba(212, 175, 55, 0.1);
        }

        /* MAIN AREA */
        #main-area {
            flex: 1;
            display: flex;
            flex-direction: column;
            height: 100%;
            min-width: 0;
            position: relative;
        }
        #main-header {
            height: 56px;
            min-height: 56px;
            display: flex;
            align-items: center;
            padding: 0 20px;
            border-bottom: 1px solid var(--border-gold);
            background: rgba(6, 6, 6, 0.7);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            gap: 10px;
            z-index: 5;
            flex-wrap: wrap;
        }
        #main-header .model-badge {
            font-family: var(--font-display);
            font-weight: 700;
            font-size: 10px;
            letter-spacing: 2px;
            background: var(--accent-gold-gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-transform: uppercase;
            padding: 5px 12px;
            border: 1px solid rgba(212, 175, 55, 0.4);
            border-radius: 20px;
        }
        .mode-selector {
            display: flex;
            gap: 3px;
            background: rgba(20, 20, 20, 0.8);
            border-radius: 20px;
            padding: 3px;
            border: 1px solid rgba(212, 175, 55, 0.25);
            margin-left: auto;
        }
        .mode-btn {
            padding: 6px 13px;
            border-radius: 18px;
            border: none;
            cursor: pointer;
            font-size: 10px;
            font-family: var(--font-display);
            font-weight: 600;
            letter-spacing: 1px;
            transition: all var(--transition);
            background: transparent;
            color: var(--text-dim);
            white-space: nowrap;
        }
        .mode-btn.active {
            background: var(--accent-gold-gradient);
            color: #000;
            box-shadow: 0 0 15px rgba(212, 175, 55, 0.4);
        }
        .mode-btn:hover:not(.active) {
            color: var(--gold-light);
            background: rgba(212, 175, 55, 0.1);
        }
        #nucleus-indicator {
            display: flex;
            align-items: center;
            gap: 5px;
            font-size: 9px;
            font-family: var(--font-mono);
            color: var(--gold-light);
            letter-spacing: 1px;
            animation: nucleusPulse 2s ease-in-out infinite;
        }
        @keyframes nucleusPulse {
            0%,
            100% {
                text-shadow: 0 0 5px rgba(212, 175, 55, 0.5);
            }
            50% {
                text-shadow: 0 0 20px rgba(255, 215, 0, 0.9), 0 0 40px rgba(212, 175, 55, 0.5);
            }
        }

        /* CHAT */
        #chat-container {
            flex: 1;
            overflow-y: auto;
            padding: 18px 20px;
            display: flex;
            flex-direction: column;
            gap: 14px;
            scroll-behavior: smooth;
        }
        #chat-container::-webkit-scrollbar {
            width: 4px;
        }
        #chat-container::-webkit-scrollbar-thumb {
            background: rgba(212, 175, 55, 0.2);
            border-radius: 10px;
        }
        .message {
            display: flex;
            gap: 10px;
            animation: fadeInUp 0.4s ease-out;
            max-width: 88%;
        }
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(14px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        .message.user {
            align-self: flex-end;
            flex-direction: row-reverse;
        }
        .message.assistant {
            align-self: flex-start;
        }
        .msg-avatar {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            font-size: 13px;
        }
        .message.user .msg-avatar {
            background: #1a1a1a;
            border: 2px solid rgba(255, 255, 255, 0.3);
            color: #fff;
        }
        .message.assistant .msg-avatar {
            background: var(--accent-gold-gradient);
            border: 2px solid rgba(212, 175, 55, 0.6);
            color: #000;
            font-family: var(--font-display);
            font-weight: 900;
            font-size: 10px;
            box-shadow: var(--glow-gold);
        }
        .msg-content {
            padding: 12px 16px;
            border-radius: 14px;
            font-size: 13px;
            line-height: 1.6;
            letter-spacing: 0.2px;
            word-wrap: break-word;
            overflow-wrap: break-word;
            position: relative;
        }
        .message.user .msg-content {
            background: rgba(40, 40, 40, 0.85);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-bottom-right-radius: 4px;
        }
        .message.assistant .msg-content {
            background: rgba(16, 14, 8, 0.75);
            border: 1px solid rgba(212, 175, 55, 0.22);
            border-bottom-left-radius: 4px;
            box-shadow: 0 0 18px rgba(212, 175, 55, 0.04);
        }
        .msg-time {
            font-size: 9px;
            color: var(--text-dim);
            margin-top: 3px;
            font-family: var(--font-mono);
            letter-spacing: 0.5px;
        }
        .message.user .msg-time {
            text-align: right;
        }
        .thinking-block {
            background: rgba(212, 175, 55, 0.06);
            border-left: 2px solid var(--gold-primary);
            padding: 8px 12px;
            margin: 6px 0;
            border-radius: 0 8px 8px 0;
            font-style: italic;
            color: var(--gold-light);
            font-size: 11px;
            letter-spacing: 0.3px;
            animation: thinkFade 0.5s ease-out;
        }
        @keyframes thinkFade {
            from {
                opacity: 0;
                border-left-color: transparent;
            }
            to {
                opacity: 1;
                border-left-color: var(--gold-primary);
            }
        }
        .file-preview {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 8px 12px;
            background: rgba(20, 20, 20, 0.7);
            border-radius: 10px;
            border: 1px solid rgba(212, 175, 55, 0.2);
            margin-top: 6px;
            max-width: 300px;
        }
        .file-preview img {
            width: 48px;
            height: 48px;
            object-fit: cover;
            border-radius: 6px;
        }
        .file-preview .file-icon {
            font-size: 24px;
        }
        .file-preview .file-info {
            font-size: 10px;
            color: var(--text-secondary);
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .typing-cursor {
            display: inline-block;
            width: 2px;
            height: 15px;
            background: var(--gold-bright);
            margin-left: 2px;
            vertical-align: text-bottom;
            animation: blink 0.8s step-end infinite;
            box-shadow: 0 0 8px var(--gold-glow);
        }
        @keyframes blink {
            0%,
            100% {
                opacity: 1;
            }
            50% {
                opacity: 0;
            }
        }
        .generated-image {
            max-width: 300px;
            border-radius: 12px;
            border: 2px solid rgba(212, 175, 55, 0.3);
            box-shadow: 0 0 25px rgba(212, 175, 55, 0.2);
            margin-top: 8px;
            cursor: pointer;
            transition: transform var(--transition);
        }
        .generated-image:hover {
            transform: scale(1.03);
            box-shadow: 0 0 40px rgba(212, 175, 55, 0.4);
        }

        /* INPUT AREA */
        #input-area {
            padding: 10px 16px 14px;
            border-top: 1px solid var(--border-gold);
            background: rgba(6, 6, 6, 0.75);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            z-index: 5;
        }
        #upload-preview-row {
            display: flex;
            flex-wrap: wrap;
            gap: 6px;
            margin-bottom: 8px;
            min-height: 0;
        }
        .upload-thumb {
            position: relative;
            width: 44px;
            height: 44px;
            border-radius: 8px;
            overflow: hidden;
            border: 1px solid rgba(212, 175, 55, 0.3);
            flex-shrink: 0;
        }
        .upload-thumb img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .upload-thumb .remove-thumb {
            position: absolute;
            top: 2px;
            right: 2px;
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: rgba(0, 0, 0, 0.8);
            border: 1px solid rgba(255, 255, 255, 0.3);
            color: #fff;
            font-size: 10px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            line-height: 1;
        }
        #input-wrapper {
            display: flex;
            align-items: center;
            gap: 8px;
            background: var(--bg-input);
            border: 1px solid rgba(212, 175, 55, 0.3);
            border-radius: 16px;
            padding: 5px 6px 5px 14px;
            transition: all var(--transition);
        }
        #input-wrapper:focus-within {
            border-color: rgba(212, 175, 55, 0.7);
            box-shadow: 0 0 28px rgba(212, 175, 55, 0.2);
            background: #131313;
        }
        #user-input {
            flex: 1;
            background: transparent;
            border: none;
            outline: none;
            color: #f0f0f0;
            font-size: 13px;
            font-family: var(--font-body);
            letter-spacing: 0.3px;
            padding: 7px 0;
            resize: none;
            min-height: 20px;
            max-height: 100px;
            line-height: 1.4;
        }
        #user-input::placeholder {
            color: #555;
            letter-spacing: 0.5px;
            font-size: 12px;
        }
        .input-icon-btn {
            width: 34px;
            height: 34px;
            border-radius: 10px;
            border: 1px solid rgba(212, 175, 55, 0.25);
            background: rgba(20, 20, 20, 0.8);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--gold-bright);
            font-size: 15px;
            transition: all var(--transition);
            flex-shrink: 0;
            position: relative;
        }
        .input-icon-btn:hover {
            background: rgba(212, 175, 55, 0.15);
            border-color: rgba(212, 175, 55, 0.5);
            box-shadow: 0 0 15px rgba(212, 175, 55, 0.2);
        }
        .input-icon-btn.recording {
            background: #ff3333 !important;
            border-color: #ff3333 !important;
            color: #fff !important;
            animation: recPulse 0.8s ease-in-out infinite;
        }
        @keyframes recPulse {
            0%,
            100% {
                box-shadow: 0 0 5px #ff3333;
            }
            50% {
                box-shadow: 0 0 25px #ff3333, 0 0 50px #ff333388;
            }
        }
        #send-btn {
            width: 40px;
            height: 40px;
            border-radius: 12px;
            border: none;
            background: var(--accent-gold-gradient);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all var(--transition);
            flex-shrink: 0;
            box-shadow: 0 0 15px rgba(212, 175, 55, 0.3);
        }
        #send-btn:hover {
            box-shadow: var(--glow-intense);
            transform: scale(1.04);
        }
        #send-btn:active {
            transform: scale(0.94);
        }
        #send-btn:disabled {
            opacity: 0.35;
            cursor: not-allowed;
            box-shadow: none;
            filter: grayscale(30%);
        }
        #send-btn svg {
            width: 19px;
            height: 19px;
            fill: #000;
        }

        /* MOBILE */
        @media (max-width: 768px) {
            #sidebar {
                position: fixed;
                left: 0;
                top: 0;
                height: 100%;
                z-index: 100;
                transform: translateX(-100%);
                box-shadow: 10px 0 40px rgba(0, 0, 0, 0.8);
            }
            #sidebar.open {
                transform: translateX(0);
            }
            #sidebar-toggle {
                display: flex !important;
            }
            .message {
                max-width: 95%;
            }
            #main-header {
                flex-wrap: wrap;
                gap: 4px;
                padding: 6px 10px;
                height: auto;
                min-height: 48px;
            }
            .mode-selector {
                margin-left: 0;
                order: 10;
            }
            .mode-btn {
                padding: 4px 8px;
                font-size: 8px;
                letter-spacing: 0.5px;
            }
        }
        @media (min-width: 769px) {
            #sidebar-toggle {
                display: none !important;
            }
        }
        #sidebar-toggle {
            display: none;
            position: fixed;
            top: 10px;
            left: 10px;
            z-index: 110;
            width: 34px;
            height: 34px;
            border-radius: 8px;
            background: rgba(10, 10, 10, 0.9);
            border: 1px solid rgba(212, 175, 55, 0.4);
            cursor: pointer;
            align-items: center;
            justify-content: center;
            color: var(--gold-bright);
            font-size: 17px;
            backdrop-filter: blur(10px);
        }
        #sidebar-overlay {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.7);
            z-index: 99;
        }
        #sidebar-overlay.show {
            display: block;
        }
        .toast {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(20, 15, 5, 0.95);
            border: 1px solid var(--gold-primary);
            color: var(--gold-bright);
            padding: 10px 20px;
            border-radius: 20px;
            font-size: 12px;
            z-index: 200;
            letter-spacing: 0.5px;
            box-shadow: 0 0 30px rgba(212, 175, 55, 0.3);
            animation: toastIn 0.4s ease-out, toastOut 0.4s 2.5s ease-in forwards;
            pointer-events: none;
        }
        @keyframes toastIn {
            from {
                opacity: 0;
                transform: translateX(-50%) translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateX(-50%) translateY(0);
            }
        }
        @keyframes toastOut {
            from {
                opacity: 1;
            }
            to {
                opacity: 0;
                transform: translateX(-50%) translateY(-10px);
            }
        }
    </style>
</head>
<body>

    <canvas id="neural-canvas"></canvas>
    <div id="sidebar-overlay" onclick="toggleSidebar()"></div>
    <button id="sidebar-toggle" onclick="toggleSidebar()" aria-label="Menu">☰</button>

    <div id="app-container">
        <!-- Sidebar -->
        <aside id="sidebar">
            <div id="sidebar-header">
                <div id="sidebar-logo">Ω</div>
                <div id="sidebar-title">Ômega Core<span>Rede Ômega</span></div>
            </div>
            <button id="new-chat-btn" onclick="newConversation()">+ Nova Conversa</button>
            <div id="conversations-list"></div>
            <div id="sidebar-footer">
                <div><span class="status-dot"></span> Núcleo Ômega Ativo</div>
                <div id="node-count">70 nós quânticos</div>
                <input type="password" id="hf-token-input" placeholder="HF Token (opcional)" onchange="updateHFToken(this.value)" title="Token Hugging Face para modelos avançados de visão/áudio">
            </div>
        </aside>

        <!-- Main -->
        <main id="main-area">
            <header id="main-header">
                <span class="model-badge">ÔMEGA IA</span>
                <span id="nucleus-indicator">⚡ Núcleo Ω</span>
                <div class="mode-selector" id="mode-selector">
                    <button class="mode-btn" data-mode="basic" onclick="setMode('basic')">💬 Básico</button>
                    <button class="mode-btn active" data-mode="quantum" onclick="setMode('quantum')">⚛️ Quântico</button>
                    <button class="mode-btn" data-mode="performance" onclick="setMode('performance')">⚡ Performance</button>
                </div>
            </header>
            <div id="chat-container"></div>
            <div id="input-area">
                <div id="upload-preview-row"></div>
                <div id="input-wrapper">
                    <input type="file" id="file-upload-input" multiple accept="image/*,video/*,audio/*,.pdf,.txt,.doc,.docx,.json,.csv,.zip" style="display:none" onchange="handleFileUpload(event)">
                    <button class="input-icon-btn" onclick="document.getElementById('file-upload-input').click()" title="Anexar arquivos" id="upload-btn">📎</button>
                    <button class="input-icon-btn" id="mic-btn" onclick="toggleMicrophone()" title="Gravar áudio">🎤</button>
                    <textarea id="user-input" rows="1" placeholder="Mensagem para Ômega IA... (/imagem, /audio, /modo)" onkeydown="handleKeyDown(event)" oninput="autoResize(this)"></textarea>
                    <button id="send-btn" onclick="sendMessage()" aria-label="Enviar">
                        <svg viewBox="0 0 24 24"><path d="M2.01 21L23 12 2.01 3 2 10l15 2-15 2z"/></svg>
                    </button>
                </div>
            </div>
        </main>
    </div>

    <script>
        // ============ CANVAS DE PARTÍCULAS ============
        const canvas = document.getElementById('neural-canvas');
        const ctx = canvas.getContext('2d');
        let particles = [];
        const PARTICLE_COUNT = 70;
        const CONNECTION_DIST = 130;
        const MOUSE_INFLUENCE_DIST = 160;
        let mouseX = -1000,
            mouseY = -1000;
        let canvasWidth, canvasHeight;

        function resizeCanvas() {
            canvasWidth = window.innerWidth;
            canvasHeight = window.innerHeight;
            canvas.width = canvasWidth;
            canvas.height = canvasHeight;
        }
        resizeCanvas();
        window.addEventListener('resize', () => { resizeCanvas();
            initParticles(); });
        document.addEventListener('mousemove', (e) => { mouseX = e.clientX;
            mouseY = e.clientY; });
        document.addEventListener('touchmove', (e) => { if (e.touches.length > 0) { mouseX = e.touches[0].clientX;
                mouseY = e.touches[0].clientY; } }, { passive: true });
        document.addEventListener('mouseleave', () => { mouseX = -1000;
            mouseY = -1000; });

        function initParticles() {
            particles = [];
            for (let i = 0; i < PARTICLE_COUNT; i++) {
                particles.push({
                    x: Math.random() * canvasWidth,
                    y: Math.random() * canvasHeight,
                    vx: (Math.random() - 0.5) * 0.55,
                    vy: (Math.random() - 0.5) * 0.55,
                    radius: Math.random() * 2.5 + 1.5,
                    baseRadius: Math.random() * 2.5 + 1.5,
                    pulseOffset: Math.random() * Math.PI * 2,
                    pulseSpeed: 0.02 + Math.random() * 0.03,
                    alpha: 0.5 + Math.random() * 0.5,
                });
            }
            document.getElementById('node-count').textContent = PARTICLE_COUNT + ' nós quânticos';
        }
        initParticles();

        function animateParticles() {
            ctx.clearRect(0, 0, canvasWidth, canvasHeight);
            for (let p of particles) {
                p.x += p.vx;
                p.y += p.vy;
                if (p.x < -20) p.x = canvasWidth + 20;
                if (p.x > canvasWidth + 20) p.x = -20;
                if (p.y < -20) p.y = canvasHeight + 20;
                if (p.y > canvasHeight + 20) p.y = -20;
                p.radius = p.baseRadius + Math.sin(Date.now() * p.pulseSpeed + p.pulseOffset) * 0.7;
            }
            for (let i = 0; i < particles.length; i++) {
                for (let j = i + 1; j < particles.length; j++) {
                    const dx = particles[i].x - particles[j].x;
                    const dy = particles[i].y - particles[j].y;
                    const dist = Math.sqrt(dx * dx + dy * dy);
                    if (dist < CONNECTION_DIST) {
                        const alpha = (1 - dist / CONNECTION_DIST) * 0.32;
                        ctx.strokeStyle = `rgba(212,175,55,${alpha})`;
                        ctx.lineWidth = 0.5;
                        ctx.beginPath();
                        ctx.moveTo(particles[i].x, particles[i].y);
                        ctx.lineTo(particles[j].x, particles[j].y);
                        ctx.stroke();
                    }
                }
            }
            for (let p of particles) {
                const glow = ctx.createRadialGradient(p.x, p.y, 0, p.x, p.y, p.radius * 3);
                glow.addColorStop(0, `rgba(255,215,0,${p.alpha*0.55})`);
                glow.addColorStop(0.4, `rgba(212,175,55,${p.alpha*0.2})`);
                glow.addColorStop(1, 'rgba(212,175,55,0)');
                ctx.fillStyle = glow;
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.radius * 3, 0, Math.PI * 2);
                ctx.fill();
                ctx.fillStyle = `rgba(255,215,0,${p.alpha})`;
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
                ctx.fill();
            }
            if (mouseX > -500 && mouseY > -500) {
                for (let p of particles) {
                    const dx = mouseX - p.x;
                    const dy = mouseY - p.y;
                    const dist = Math.sqrt(dx * dx + dy * dy);
                    if (dist < MOUSE_INFLUENCE_DIST && dist > 1) {
                        const force = (1 - dist / MOUSE_INFLUENCE_DIST) * 0.035;
                        p.vx += (dx / dist) * force;
                        p.vy += (dy / dist) * force;
                        const speed = Math.sqrt(p.vx * p.vx + p.vy * p.vy);
                        if (speed > 1.4) { p.vx = (p.vx / speed) * 1.4;
                            p.vy = (p.vy / speed) * 1.4; }
                    }
                }
            }
            requestAnimationFrame(animateParticles);
        }
        animateParticles();

        // ============ NÚCLEO ÔMEGA ============
        const OmegaCore = {
            currentMode: 'quantum',
            hfToken: localStorage.getItem('omega_hf_token') || '',
            isProcessing: false,
            pendingFiles: [],
            conversations: [],
            currentConvId: null,
            recognition: null,
            isRecording: false,
            synth: window.speechSynthesis,

            init() {
                this.loadConversations();
                this.setupSpeechRecognition();
                this.renderAll();
                document.getElementById('hf-token-input').value = this.hfToken;
                if (this.hfToken) this.logNucleus('Token HF carregado — modelos avançados disponíveis');
                this.logNucleus('Núcleo Ômega inicializado — ' + PARTICLE_COUNT + ' nós ativos');
            },

            logNucleus(msg) {
                console.log('%c⚡ Núcleo Ω:%c ' + msg,
                    'color:#FFD700;font-weight:bold;', 'color:#D4AF37;');
            },

            getModeConfig() {
                const configs = {
                    basic: { temp: 0.5, maxTokens: 300, systemAdd: ' Seja conciso e direto. Respostas curtas.', label: 'Básico' },
                    quantum: { temp: 0.9, maxTokens: 2000,
                        systemAdd: ' Pense passo a passo. Seja detalhado, profundo e analítico. Mostre seu raciocínio.',
                        label: 'Quântico', showThinking: true },
                    performance: { temp: 0.2, maxTokens: 120,
                        systemAdd: ' Responda com extrema brevidade. Apenas o essencial.', label: 'Performance' },
                };
                return configs[this.currentMode] || configs.quantum;
            },

            async callTextAPI(messages, modeConfig) {
                const body = {
                    messages: messages,
                    model: 'openai',
                    temperature: modeConfig.temp,
                    max_tokens: modeConfig.maxTokens,
                };
                try {
                    const resp = await fetch('https://text.pollinations.ai/', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(body),
                    });
                    if (!resp.ok) throw new Error('API status: ' + resp.status);
                    const text = await resp.text();
                    return text.trim();
                } catch (e) {
                    this.logNucleus('Erro API texto: ' + e.message);
                    throw e;
                }
            },

            async generateImage(prompt) {
                const encoded = encodeURIComponent(prompt);
                const url = `https://image.pollinations.ai/prompt/${encoded}?width=1024&height=1024&nologo=true&seed=${Math.floor(Math.random()*100000)}`;
                this.logNucleus('Gerando imagem: ' + prompt.substring(0, 60) + '...');
                return url;
            },

            async describeImage(base64Data) {
                if (!this.hfToken) return '(Token Hugging Face necessário para análise de imagem. Adicione no painel lateral.)';
                try {
                    const resp = await fetch(
                        'https://api-inference.huggingface.co/models/Salesforce/blip-image-captioning-large', {
                            method: 'POST',
                            headers: {
                                'Authorization': 'Bearer ' + this.hfToken,
                                'Content-Type': 'application/json',
                            },
                            body: JSON.stringify({ inputs: base64Data.split(',')[1] || base64Data }),
                        });
                    const result = await resp.json();
                    if (Array.isArray(result) && result[0]?.generated_text) return result[0].generated_text;
                    if (result?.generated_text) return result.generated_text;
                    return '(Não foi possível analisar a imagem)';
                } catch (e) {
                    return '(Erro na análise de imagem: ' + e.message + ')';
                }
            },

            setupSpeechRecognition() {
                const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
                if (!SpeechRecognition) {
                    this.logNucleus('SpeechRecognition não suportado neste navegador');
                    return;
                }
                this.recognition = new SpeechRecognition();
                this.recognition.continuous = false;
                this.recognition.interimResults = false;
                this.recognition.lang = 'pt-BR';
                this.recognition.onresult = (event) => {
                    const transcript = event.results[0][0].transcript;
                    document.getElementById('user-input').value = transcript;
                    autoResize(document.getElementById('user-input'));
                    this.stopRecording();
                    this.logNucleus('Áudio capturado: ' + transcript.substring(0, 40) + '...');
                };
                this.recognition.onerror = (e) => {
                    this.stopRecording();
                    this.logNucleus('Erro microfone: ' + e.error);
                    showToast('Erro no microfone: ' + e.error);
                };
                this.recognition.onend = () => this.stopRecording();
            },

            startRecording() {
                if (!this.recognition) { showToast('Reconhecimento de voz não suportado');
                    return; }
                try {
                    this.recognition.start();
                    this.isRecording = true;
                    document.getElementById('mic-btn').classList.add('recording');
                    document.getElementById('mic-btn').textContent = '⏹';
                    this.logNucleus('Gravação iniciada...');
                } catch (e) {
                    this.isRecording = false;
                    this.logNucleus('Erro ao iniciar gravação: ' + e.message);
                }
            },

            stopRecording() {
                if (this.recognition && this.isRecording) {
                    try { this.recognition.stop(); } catch (e) {}
                }
                this.isRecording = false;
                document.getElementById('mic-btn').classList.remove('recording');
                document.getElementById('mic-btn').textContent = '🎤';
            },

            speakText(text) {
                if (!this.synth) return;
                this.synth.cancel();
                const cleanText = text.replace(/<[^>]*>/g, '').replace(/\*\*/g, '').replace(/\*/g, '').replace(/`/g, '')
                    .substring(0, 800);
                const utterance = new SpeechSynthesisUtterance(cleanText);
                utterance.lang = 'pt-BR';
                utterance.rate = 1.05;
                utterance.pitch = 1.0;
                const voices = this.synth.getVoices();
                const ptVoice = voices.find(v => v.lang.startsWith('pt')) || voices.find(v => v.lang.startsWith('en')) ||
                    voices[0];
                if (ptVoice) utterance.voice = ptVoice;
                this.synth.speak(utterance);
                this.logNucleus('TTS iniciado');
            },

            loadConversations() {
                try {
                    const saved = localStorage.getItem('omega_core_conversations_v2');
                    if (saved) this.conversations = JSON.parse(saved);
                } catch (e) { this.conversations = []; }
                if (this.conversations.length === 0) {
                    const initConv = {
                        id: 'conv_' + Date.now(),
                        title: 'Nova Conversa',
                        messages: [{
                            role: 'assistant',
                            content: '🟡 **Ômega IA iniciada.**\n\nSou a **Rede Ômega** — inteligência artificial quântica com ' +
                                PARTICLE_COUNT +
                                ' nós neurais. Processo texto, imagens, áudio e arquivos.\n\n**Comandos:**\n• `/imagem [descrição]` — Gero imagens\n• `/audio [texto]` — Leio em voz alta\n• `/modo [basico|quantico|performance]` — Mudo o modo\n\nComo posso ajudar?',
                            timestamp: new Date().toISOString(),
                        }],
                        createdAt: new Date().toISOString(),
                    };
                    this.conversations.push(initConv);
                }
                this.currentConvId = this.conversations[0].id;
                this.saveConversations();
            },

            saveConversations() {
                try {
                    const trimmed = this.conversations.slice(-30);
                    localStorage.setItem('omega_core_conversations_v2', JSON.stringify(trimmed));
                } catch (e) {}
            },

            getCurrentConv() {
                return this.conversations.find(c => c.id === this.currentConvId) || this.conversations[0];
            },

            renderAll() {
                this.renderConvList();
                this.renderChat();
                document.getElementById('hf-token-input').value = this.hfToken;
            },

            renderConvList() {
                const list = document.getElementById('conversations-list');
                list.innerHTML = '';
                this.conversations.forEach(conv => {
                    const div = document.createElement('div');
                    div.className = 'conv-item' + (conv.id === this.currentConvId ? ' active' : '');
                    div.innerHTML = '<span class="conv-dot"></span>' + escapeHtml(conv.title);
                    div.onclick = () => this.switchConv(conv.id);
                    list.appendChild(div);
                });
            },

            switchConv(id) {
                this.currentConvId = id;
                this.renderAll();
                closeSidebar();
            },

            renderChat() {
                const conv = this.getCurrentConv();
                const container = document.getElementById('chat-container');
                container.innerHTML = '';
                if (!conv) return;
                conv.messages.forEach(msg => appendMessageToDOM(msg.role, msg.content, msg.timestamp, false, msg
                    .thinking, msg.fileData));
                container.scrollTop = container.scrollHeight;
            },
        };

        // ============ FUNÇÕES AUXILIARES ============
        function escapeHtml(str) {
            const div = document.createElement('div');
            div.textContent = str;
            return div.innerHTML;
        }

        function formatTime(iso) {
            const d = new Date(iso);
            const now = new Date();
            const diffMin = Math.floor((now - d) / 60000);
            if (diffMin < 1) return 'Agora';
            if (diffMin < 60) return diffMin + 'min';
            const diffH = Math.floor(diffMin / 60);
            if (diffH < 24) return diffH + 'h';
            return d.toLocaleDateString('pt-BR', { day: '2-digit', month: 'short', hour: '2-digit', minute: '2-digit' });
        }

        function showToast(msg) {
            const existing = document.querySelector('.toast');
            if (existing) existing.remove();
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.textContent = msg;
            document.body.appendChild(toast);
            setTimeout(() => toast.remove(), 3000);
        }

        function autoResize(ta) {
            ta.style.height = 'auto';
            ta.style.height = Math.min(ta.scrollHeight, 100) + 'px';
        }

        function handleKeyDown(e) {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                sendMessage();
            }
        }

        function appendMessageToDOM(role, content, timestamp, animate = true, thinking = null, fileData = null) {
            const container = document.getElementById('chat-container');
            const msgDiv = document.createElement('div');
            msgDiv.className = 'message ' + role;
            if (animate) { msgDiv.style.animation = 'none';
                msgDiv.offsetHeight;
                msgDiv.style.animation = 'fadeInUp 0.4s ease-out'; }

            const avatar = document.createElement('div');
            avatar.className = 'msg-avatar';
            avatar.textContent = role === 'user' ? 'U' : 'Ω';

            const wrapper = document.createElement('div');
            const contentDiv = document.createElement('div');
            contentDiv.className = 'msg-content';

            if (thinking) {
                const thinkDiv = document.createElement('div');
                thinkDiv.className = 'thinking-block';
                thinkDiv.textContent = '🧠 Pensamento: ' + thinking;
                contentDiv.appendChild(thinkDiv);
            }
            contentDiv.innerHTML += formatMessage(content);

            if (fileData && fileData.type === 'image') {
                const img = document.createElement('img');
                img.src = fileData.dataUrl;
                img.className = 'generated-image';
                img.onclick = () => window.open(fileData.dataUrl, '_blank');
                contentDiv.appendChild(img);
            }

            const timeDiv = document.createElement('div');
            timeDiv.className = 'msg-time';
            timeDiv.textContent = formatTime(timestamp);

            wrapper.appendChild(contentDiv);
            wrapper.appendChild(timeDiv);
            if (role === 'user') { msgDiv.appendChild(wrapper);
                msgDiv.appendChild(avatar); } else { msgDiv.appendChild(avatar);
                msgDiv.appendChild(wrapper); }
            container.appendChild(msgDiv);
            container.scrollTop = container.scrollHeight;
            return msgDiv;
        }

        function formatMessage(text) {
            return text
                .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
                .replace(/\*(.*?)\*/g, '<em>$1</em>')
                .replace(/`(.*?)`/g,
                    '<code style="background:rgba(212,175,55,0.13);padding:2px 6px;border-radius:4px;font-family:var(--font-mono);font-size:12px;">$1</code>'
                    )
                .replace(/\n/g, '<br>');
        }

        async function typeWriter(element, htmlContent, speed = 8) {
            element.innerHTML = '';
            const cursor = document.createElement('span');
            cursor.className = 'typing-cursor';
            element.appendChild(cursor);
            const tempDiv = document.createElement('div');
            tempDiv.innerHTML = htmlContent;
            const fullText = tempDiv.textContent || '';
            let charIndex = 0;
            const totalChars = fullText.length;
            for (let i = 0; i < totalChars; i++) {
                const delay = fullText[i] === '\n' ? 20 : fullText[i] === '.' || fullText[i] === '!' ? 22 : speed + Math
                    .random() * 4;
                await new Promise(r => setTimeout(r, delay));
                charIndex++;
                let partial = '';
                let count = 0;
                const walker = document.createTreeWalker(tempDiv, NodeFilter.SHOW_TEXT, null, false);
                let node;
                while ((node = walker.nextNode())) {
                    if (count + node.textContent.length <= charIndex) {
                        partial += node.textContent;
                        count += node.textContent.length;
                    } else {
                        partial += node.textContent.substring(0, charIndex - count);
                        break;
                    }
                }
                element.innerHTML = partial.replace(/\n/g, '<br>');
                element.appendChild(cursor);
                document.getElementById('chat-container').scrollTop = document.getElementById('chat-container')
                    .scrollHeight;
            }
            element.innerHTML = htmlContent;
        }

        // ============ UPLOAD DE ARQUIVOS ============
        function handleFileUpload(event) {
            const files = event.target.files;
            if (!files.length) return;
            Array.from(files).forEach(file => {
                const reader = new FileReader();
                reader.onload = (e) => {
                    const fileObj = {
                        name: file.name,
                        type: file.type.startsWith('image/') ? 'image' :
                            file.type.startsWith('video/') ? 'video' :
                            file.type.startsWith('audio/') ? 'audio' : 'file',
                        mimeType: file.type,
                        dataUrl: e.target.result,
                        size: file.size,
                    };
                    OmegaCore.pendingFiles.push(fileObj);
                    renderUploadPreviews();
                    OmegaCore.logNucleus('Arquivo anexado: ' + file.name + ' (' + formatSize(file.size) + ')');
                };
                reader.readAsDataURL(file);
            });
            event.target.value = '';
        }

        function formatSize(bytes) {
            if (bytes < 1024) return bytes + ' B';
            if (bytes < 1048576) return (bytes / 1024).toFixed(1) + ' KB';
            return (bytes / 1048576).toFixed(1) + ' MB';
        }

        function renderUploadPreviews() {
            const row = document.getElementById('upload-preview-row');
            row.innerHTML = '';
            OmegaCore.pendingFiles.forEach((f, i) => {
                const thumb = document.createElement('div');
                thumb.className = 'upload-thumb';
                if (f.type === 'image') {
                    thumb.innerHTML =
                        `<img src="${f.dataUrl}" alt="preview"><span class="remove-thumb" onclick="removePendingFile(${i})">×</span>`;
                } else {
                    const icon = f.type === 'video' ? '🎬' : f.type === 'audio' ? '🎵' : '📄';
                    thumb.innerHTML =
                        `<span style="font-size:22px;display:flex;align-items:center;justify-content:center;width:100%;height:100%;">${icon}</span><span class="remove-thumb" onclick="removePendingFile(${i})">×</span>`;
                }
                row.appendChild(thumb);
            });
        }

        function removePendingFile(index) {
            OmegaCore.pendingFiles.splice(index, 1);
            renderUploadPreviews();
        }

        // ============ MODO ============
        function setMode(mode) {
            OmegaCore.currentMode = mode;
            document.querySelectorAll('.mode-btn').forEach(b => b.classList.remove('active'));
            document.querySelector(`.mode-btn[data-mode="${mode}"]`)?.classList.add('active');
            const config = OmegaCore.getModeConfig();
            OmegaCore.logNucleus('Modo alterado para: ' + config.label);
            showToast('Modo: ' + config.label + ' ativado');
        }

        function updateHFToken(token) {
            OmegaCore.hfToken = token.trim();
            localStorage.setItem('omega_hf_token', OmegaCore.hfToken);
            if (OmegaCore.hfToken) {
                OmegaCore.logNucleus('Token HF atualizado — recursos avançados desbloqueados');
                showToast('Token HF salvo! Modelos avançados disponíveis.');
            }
        }

        // ============ MICROFONE ============
        function toggleMicrophone() {
            if (OmegaCore.isRecording) {
                OmegaCore.stopRecording();
            } else {
                OmegaCore.startRecording();
            }
        }

        // ============ ENVIO DE MENSAGEM ============
        async function sendMessage() {
            if (OmegaCore.isProcessing) return;
            const inputEl = document.getElementById('user-input');
            const text = inputEl.value.trim();
            const hasFiles = OmegaCore.pendingFiles.length > 0;
            if (!text && !hasFiles) return;

            const conv = OmegaCore.getCurrentConv();
            if (!conv) return;

            OmegaCore.isProcessing = true;
            document.getElementById('send-btn').disabled = true;

            // Constrói conteúdo da mensagem do usuário
            let userContent = text || '(Arquivo enviado)';
            const fileDataForMsg = hasFiles ? OmegaCore.pendingFiles[0] : null;

            // Se tem arquivo de imagem e pouco texto, prepara para análise
            let imageAnalysisPrompt = '';
            if (hasFiles && OmegaCore.pendingFiles.some(f => f.type === 'image') && OmegaCore.hfToken) {
                imageAnalysisPrompt = '\n[Analisando imagem anexada...]';
            }

            const userMsg = {
                role: 'user',
                content: userContent + imageAnalysisPrompt,
                timestamp: new Date().toISOString(),
                fileData: fileDataForMsg ? { type: fileDataForMsg.type, dataUrl: fileDataForMsg.dataUrl, name: fileDataForMsg
                        .name } : null,
            };
            conv.messages.push(userMsg);

            // Atualiza título
            const userMsgs = conv.messages.filter(m => m.role === 'user');
            if (userMsgs.length === 1 && text) {
                conv.title = text.length > 30 ? text.substring(0, 30) + '...' : text;
            }

            OmegaCore.saveConversations();
            OmegaCore.renderConvList();
            appendMessageToDOM('user', userContent, userMsg.timestamp, true, null, userMsg.fileData);
            inputEl.value = '';
            inputEl.style.height = 'auto';

            // Processa arquivos pendentes
            const pendingFiles = [...OmegaCore.pendingFiles];
            OmegaCore.pendingFiles = [];
            renderUploadPreviews();

            // Verifica comandos especiais
            const lowerText = text.toLowerCase().trim();

            // Comando /imagem
            if (lowerText.startsWith('/imagem')) {
                const prompt = text.replace('/imagem', '').trim() || 'paisagem futurista dourada abstrata';
                try {
                    const imgUrl = await OmegaCore.generateImage(prompt);
                    const assistantMsg = {
                        role: 'assistant',
                        content: '🖼️ **Imagem gerada:**\n\n*' + prompt + '*\n\nClique na imagem para ampliar.',
                        timestamp: new Date().toISOString(),
                        fileData: { type: 'image', dataUrl: imgUrl, name: 'imagem_gerada.png' },
                    };
                    conv.messages.push(assistantMsg);
                    OmegaCore.saveConversations();
                    appendMessageToDOM('assistant', assistantMsg.content, assistantMsg.timestamp, true, null, assistantMsg
                        .fileData);
                } catch (e) {
                    const errMsg = {
                        role: 'assistant',
                        content: '❌ Erro ao gerar imagem: ' + e.message,
                        timestamp: new Date().toISOString(),
                    };
                    conv.messages.push(errMsg);
                    OmegaCore.saveConversations();
                    appendMessageToDOM('assistant', errMsg.content, errMsg.timestamp, true);
                }
                OmegaCore.isProcessing = false;
                document.getElementById('send-btn').disabled = false;
                document.getElementById('user-input').focus();
                return;
            }

            // Comando /audio
            if (lowerText.startsWith('/audio')) {
                const speechText = text.replace('/audio', '').trim() || 'Olá, sou a Ômega IA.';
                OmegaCore.speakText(speechText);
                const assistantMsg = {
                    role: 'assistant',
                    content: '🔊 **Falando:** *' + speechText.substring(0, 100) + '*',
                    timestamp: new Date().toISOString(),
                };
                conv.messages.push(assistantMsg);
                OmegaCore.saveConversations();
                appendMessageToDOM('assistant', assistantMsg.content, assistantMsg.timestamp, true);
                OmegaCore.isProcessing = false;
                document.getElementById('send-btn').disabled = false;
                document.getElementById('user-input').focus();
                return;
            }

            // Comando /modo
            if (lowerText.startsWith('/modo')) {
                const mode = lowerText.replace('/modo', '').trim();
                if (['basico', 'basic', 'quantico', 'quantum', 'performance'].includes(mode)) {
                    const mapped = mode === 'basic' ? 'basic' : mode === 'quantum' ? 'quantum' : 'performance';
                    setMode(mapped);
                }
                const assistantMsg = {
                    role: 'assistant',
                    content: '⚙️ Modo alterado para: **' + OmegaCore.getModeConfig().label + '**',
                    timestamp: new Date().toISOString(),
                };
                conv.messages.push(assistantMsg);
                OmegaCore.saveConversations();
                appendMessageToDOM('assistant', assistantMsg.content, assistantMsg.timestamp, true);
                OmegaCore.isProcessing = false;
                document.getElementById('send-btn').disabled = false;
                document.getElementById('user-input').focus();
                return;
            }

            // Processamento normal com API
            const modeConfig = OmegaCore.getModeConfig();
            const chatContainer = document.getElementById('chat-container');

            // Mostra indicador de processamento
            const procDiv = document.createElement('div');
            procDiv.className = 'processing-indicator';
            procDiv.innerHTML =
                '<span style="font-family:var(--font-display);font-size:10px;letter-spacing:2px;color:var(--gold-bright);">ÔMEGA IA</span> <span style="color:#aaa;">processando</span> <span class="processing-dots"><span></span><span></span><span></span></span>';
            procDiv.style.cssText =
                'display:flex;align-items:center;gap:8px;padding:8px 14px;font-size:11px;color:var(--gold-light);letter-spacing:1px;font-family:var(--font-mono);animation:fadeInUp 0.3s ease-out;';
            chatContainer.appendChild(procDiv);
            chatContainer.scrollTop = chatContainer.scrollHeight;

            try {
                // Constrói histórico de mensagens
                const messages = [
                    { role: 'system',
                        content: 'Você é a Ômega IA, uma inteligência artificial quântica avançada da Rede Ômega. Você é amigável, inteligente e responde em português. Use markdown para formatação.' +
                            modeConfig.systemAdd },
                ];
                // Adiciona últimas 10 mensagens
                const recentMsgs = conv.messages.slice(-10);
                recentMsgs.forEach(m => {
                    if (m.role === 'user' || m.role === 'assistant') {
                        messages.push({ role: m.role, content: m.content.replace(/<[^>]*>/g, '') });
                    }
                });

                // Se tem arquivo de imagem, adiciona descrição
                if (pendingFiles.some(f => f.type === 'image')) {
                    const imgFile = pendingFiles.find(f => f.type === 'image');
                    let imgDesc = '';
                    if (OmegaCore.hfToken && imgFile) {
                        imgDesc = await OmegaCore.describeImage(imgFile.dataUrl);
                        messages.push({ role: 'user',
                            content: '[O usuário enviou uma imagem. Descrição automática: ' + imgDesc +
                                '] Por favor, comente sobre esta imagem.' });
                    } else if (imgFile) {
                        messages.push({ role: 'user',
                            content: '[O usuário enviou uma imagem. Para análise detalhada, adicione um Token Hugging Face no painel lateral.]' });
                    }
                }

                // Se tem outros arquivos
                if (pendingFiles.some(f => f.type !== 'image')) {
                    const otherFiles = pendingFiles.filter(f => f.type !== 'image');
                    const fileNames = otherFiles.map(f => f.name).join(', ');
                    messages.push({ role: 'user', content: '[Arquivos anexados: ' + fileNames +
                            '] Processe esses arquivos conforme necessário.' });
                }

                let responseText = '';
                let thinkingText = null;

                if (modeConfig.showThinking) {
                    // Modo quântico: gera pensamento primeiro
                    const thinkMessages = [...messages];
                    thinkMessages.push({ role: 'user',
                        content: 'Antes de responder, faça uma análise interna de 1-2 frases sobre esta consulta. Comece com "Análise interna:"' });
                    try {
                        thinkingText = await OmegaCore.callTextAPI(thinkMessages, { temp: 0.7, maxTokens: 200,
                            systemAdd: '' });
                        thinkingText = thinkingText.replace(/^Análise interna:?\s*/i, '').trim();
                        if (thinkingText.length > 300) thinkingText = thinkingText.substring(0, 300) + '...';
                    } catch (e) { thinkingText = null; }
                }

                responseText = await OmegaCore.callTextAPI(messages, modeConfig);

                procDiv.remove();

                const assistantMsg = {
                    role: 'assistant',
                    content: responseText,
                    timestamp: new Date().toISOString(),
                    thinking: thinkingText,
                    fileData: null,
                };
                conv.messages.push(assistantMsg);
                OmegaCore.saveConversations();

                const msgEl = appendMessageToDOM('assistant', '', assistantMsg.timestamp, true, thinkingText, null);
                const contentDiv = msgEl.querySelector('.msg-content');
                if (thinkingText) {
                    // Já foi adicionado via appendMessageToDOM
                }
                const mainContentSpan = contentDiv.querySelector(':scope > :not(.thinking-block)') || contentDiv;
                // Limpa e aplica typewriter no conteúdo principal
                const thinkBlock = contentDiv.querySelector('.thinking-block');
                let mainHTML = formatMessage(responseText);
                if (thinkBlock) {
                    // Mantém o thinking block e adiciona o resto
                    const wrapper = document.createElement('span');
                    wrapper.innerHTML = mainHTML;
                    contentDiv.appendChild(wrapper);
                    await typeWriter(wrapper, mainHTML, 6);
                } else {
                    await typeWriter(contentDiv, mainHTML, 6);
                }

            } catch (e) {
                procDiv.remove();
                const errMsg = {
                    role: 'assistant',
                    content: '❌ **Erro de conexão com a API.**\n\nDetalhes: ' + e.message +
                        '\n\nVerifique sua internet. A Rede Ômega tentará novamente na próxima mensagem.',
                    timestamp: new Date().toISOString(),
                };
                conv.messages.push(errMsg);
                OmegaCore.saveConversations();
                appendMessageToDOM('assistant', errMsg.content, errMsg.timestamp, true);
                OmegaCore.logNucleus('Erro: ' + e.message);
            }

            OmegaCore.isProcessing = false;
            document.getElementById('send-btn').disabled = false;
            document.getElementById('user-input').focus();
            OmegaCore.logNucleus('Resposta concluída no modo ' + modeConfig.label);
        }

        function newConversation() {
            const newConv = {
                id: 'conv_' + Date.now(),
                title: 'Nova Conversa',
                messages: [{
                    role: 'assistant',
                    content: '🟡 **Nova sessão Ômega IA.**\n\nRede Ômega online — ' + PARTICLE_COUNT +
                        ' nós quânticos ativos. Modo: **' + OmegaCore.getModeConfig().label +
                        '**.\n\nComandos disponíveis: `/imagem`, `/audio`, `/modo`\n\nComo posso ajudar?',
                    timestamp: new Date().toISOString(),
                }],
                createdAt: new Date().toISOString(),
            };
            OmegaCore.conversations.unshift(newConv);
            OmegaCore.currentConvId = newConv.id;
            OmegaCore.saveConversations();
            OmegaCore.renderAll();
            closeSidebar();
        }

        // ============ SIDEBAR MOBILE ============
        function toggleSidebar() {
            document.getElementById('sidebar').classList.toggle('open');
            document.getElementById('sidebar-overlay').classList.toggle('show');
        }

        function closeSidebar() {
            document.getElementById('sidebar').classList.remove('open');
            document.getElementById('sidebar-overlay').classList.remove('show');
        }
        document.getElementById('sidebar-overlay').addEventListener('click', closeSidebar);

        // ============ DRAG & DROP ============
        document.addEventListener('dragover', (e) => { e.preventDefault(); });
        document.addEventListener('drop', (e) => {
            e.preventDefault();
            if (e.dataTransfer.files.length) {
                const dt = new DataTransfer();
                Array.from(e.dataTransfer.files).forEach(f => dt.items.add(f));
                document.getElementById('file-upload-input').files = dt.files;
                handleFileUpload({ target: { files: dt.files } });
            }
        });

        // ============ ATALHOS ============
        document.addEventListener('keydown', (e) => {
            if ((e.ctrlKey || e.metaKey) && e.key === 'k') {
                e.preventDefault();
                document.getElementById('user-input').focus();
            }
            if (e.key === 'Escape') {
                closeSidebar();
                document.getElementById('user-input').blur();
                OmegaCore.stopRecording();
            }
        });

        // ============ INICIALIZAÇÃO ============
        window.addEventListener('resize', () => {
            if (window.innerWidth > 768) closeSidebar();
        });

        OmegaCore.init();
        document.getElementById('user-input').focus();

        console.log('%c🚀 Ômega Core AI %cv3.7 %ciniciada.',
            'color:#FFD700;font-size:16px;font-weight:bold;',
            'color:#D4AF37;',
            'color:#e0e0e0;');
        console.log('%c⚡ Núcleo Ômega %cgerenciando todos os módulos.',
            'color:#FFD700;font-weight:bold;', 'color:#b0b0b0;');
        console.log('%c🟡 Modos: Básico | Quântico | Performance', 'color:#D4AF37;');
        console.log('%c📡 API: Pollinations.ai (texto+imagem) | Web Speech (voz) | HF (visão opcional)',
            'color:#888;font-size:10px;');
    </script>
</body>
</html>
