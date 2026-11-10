<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>REMOTE ANDROID - Control Panel</title>
    <style>
        :root {
            --bg-dark: #0a0a0a;
            --bg-card: #121212;
            --bg-input: #1e1e1e;
            --primary: #ff2a2a;
            --primary-hover: #ff4d4d;
            --primary-glow: rgba(255, 42, 42, 0.3);
            --text-main: #ffffff;
            --text-muted: #888888;
            --success: #00ff88;
            --border: #2a2a2a;
            --font-main: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--bg-dark);
            color: var(--text-main);
            font-family: var(--font-main);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background-image: radial-gradient(circle at 50% 0%, #1a0000 0%, var(--bg-dark) 60%);
        }

        .container {
            width: 100%;
            max-width: 900px;
            padding: 20px;
        }

        /* Header */
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid var(--border);
        }

        .logo {
            font-size: 1.5rem;
            font-weight: 800;
            letter-spacing: 2px;
            text-transform: uppercase;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .logo span {
            color: var(--primary);
            text-shadow: 0 0 10px var(--primary-glow);
        }

        .status-badge {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.85rem;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 1px;
            padding: 6px 12px;
            border-radius: 20px;
            background: var(--bg-input);
            border: 1px solid var(--border);
            transition: all 0.3s ease;
        }

        .status-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: var(--text-muted);
        }

        .status-connected {
            border-color: var(--success);
            color: var(--success);
        }
        .status-connected .status-dot {
            background: var(--success);
            box-shadow: 0 0 8px var(--success);
            animation: pulse 2s infinite;
        }

        .status-disconnected {
            border-color: var(--primary);
            color: var(--primary);
        }
        .status-disconnected .status-dot {
            background: var(--primary);
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        /* Grid Layout */
        .grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        @media (max-width: 768px) {
            .grid {
                grid-template-columns: 1fr;
            }
        }

        /* Cards */
        .card {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.5);
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            border-bottom: 1px solid var(--border);
            padding-bottom: 10px;
        }

        .card-title {
            font-size: 1rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: var(--text-main);
        }

        .master-toggle {
            position: relative;
            width: 50px;
            height: 26px;
            background: var(--bg-input);
            border-radius: 13px;
            cursor: pointer;
            border: 1px solid var(--border);
            transition: all 0.3s ease;
        }

        .master-toggle::after {
            content: '';
            position: absolute;
            top: 2px;
            left: 2px;
            width: 20px;
            height: 20px;
            background: var(--text-muted);
            border-radius: 50%;
            transition: all 0.3s ease;
        }

        .master-toggle.active {
            background: rgba(255, 42, 42, 0.2);
            border-color: var(--primary);
            box-shadow: 0 0 10px var(--primary-glow);
        }

        .master-toggle.active::after {
            left: 26px;
            background: var(--primary);
        }

        /* Sliders */
        .control-group {
            margin-bottom: 20px;
        }
        .control-group:last-child {
            margin-bottom: 0;
        }

        .control-label {
            display: flex;
            justify-content: space-between;
            font-size: 0.85rem;
            margin-bottom: 8px;
            color: var(--text-muted);
            text-transform: uppercase;
            font-weight: 600;
        }

        .control-value {
            color: var(--primary);
            font-weight: 700;
        }

        input[type=range] {
            -webkit-appearance: none;
            width: 100%;
            background: transparent;
        }

        input[type=range]:focus {
            outline: none;
        }

        input[type=range]::-webkit-slider-runnable-track {
            width: 100%;
            height: 6px;
            cursor: pointer;
            background: var(--bg-input);
            border-radius: 3px;
            border: 1px solid var(--border);
        }

        input[type=range]::-webkit-slider-thumb {
            height: 18px;
            width: 18px;
            border-radius: 50%;
            background: var(--primary);
            cursor: pointer;
            -webkit-appearance: none;
            margin-top: -7px;
            box-shadow: 0 0 5px var(--primary-glow);
            transition: transform 0.1s;
        }

        input[type=range]::-webkit-slider-thumb:hover {
            transform: scale(1.2);
        }

        /* Pairing Section */
        .pairing-box {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .qr-placeholder {
            width: 100%;
            aspect-ratio: 1;
            background: var(--bg-input);
            border: 2px dashed var(--border);
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            color: var(--text-muted);
            font-size: 0.9rem;
            margin-bottom: 10px;
            position: relative;
            overflow: hidden;
        }
        
        .qr-placeholder img {
            width: 100%;
            height: 100%;
            object-fit: contain;
            padding: 10px;
        }

        .code-display {
            background: var(--bg-input);
            padding: 12px;
            text-align: center;
            font-family: 'Courier New', monospace;
            font-size: 1.2rem;
            font-weight: bold;
            letter-spacing: 2px;
            color: var(--primary);
            border-radius: 8px;
            border: 1px solid var(--border);
            text-transform: uppercase;
        }

        .btn {
            background: var(--bg-input);
            color: var(--text-main);
            border: 1px solid var(--border);
            padding: 12px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 1px;
            transition: all 0.2s ease;
            width: 100%;
            font-size: 0.9rem;
        }

        .btn:hover {
            background: var(--border);
            border-color: var(--text-muted);
        }

        .btn-primary {
            background: var(--primary);
            border-color: var(--primary);
            color: white;
        }

        .btn-primary:hover {
            background: var(--primary-hover);
            box-shadow: 0 0 15px var(--primary-glow);
        }

        /* Log Console */
        .log-console {
            background: #000;
            height: 100px;
            overflow-y: auto;
            font-family: 'Courier New', monospace;
            font-size: 0.75rem;
            padding: 10px;
            border-radius: 6px;
            border: 1px solid var(--border);
            color: var(--text-muted);
        }
        .log-entry { margin-bottom: 4px; }
        .log-time { color: var(--primary); margin-right: 8px; }
        .log-success { color: var(--success); }
        .log-error { color: #ff4d4d; }

        /* Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: var(--bg-dark);
        }
        ::-webkit-scrollbar-thumb {
            background: var(--border);
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: var(--text-muted);
        }

    </style>
</head>
<body>

<div class="container">
    <header>
        <div class="logo">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="var(--primary)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="5" y="2" width="14" height="20" rx="2" ry="2"></rect>
                <line x1="12" y1="18" x2="12.01" y2="18"></line>
            </svg>
            <span>REMOTE ANDROID</span>
        </div>
        <div id="statusBadge" class="status-badge status-disconnected">
            <div class="status-dot"></div>
            <span id="statusText">OFFLINE</span>
        </div>
    </header>

    <div class="grid">
        <!-- Left Column: Controls -->
        <div class="card">
            <div class="card-header">
                <div class="card-title">Assistência de Mira</div>
                <div class="master-toggle active" id="masterToggle" title="Master ON/OFF"></div>
            </div>

            <div class="control-group">
                <div class="control-label">
                    <span>Intensidade</span>
                    <span class="control-value" id="val-intensity">75%</span>
                </div>
                <input type="range" min="0" max="100" value="75" id="slider-intensity" class="slider-control" data-key="intensity">
            </div>

            <div class="control-group">
                <div class="control-label">
                    <span>Precisão</span>
                    <span class="control-value" id="val-precision">80%</span>
                </div>
                <input type="range" min="0" max="100" value="80" id="slider-precision" class="slider-control" data-key="precision">
            </div>
            
             <div class="control-group">
                <div class="control-label">
                    <span>FOV (Campo de Visão)</span>
                    <span class="control-value" id="val-fov">65%</span>
                </div>
                <input type="range" min="0" max="100" value="65" id="slider-fov" class="slider-control" data-key="fov">
            </div>

            <div class="control-group">
                <div class="control-label">
                    <span>Suavização (Smooth)</span>
                    <span class="control-value" id="val-smooth">50%</span>
                </div>
                <input type="range" min="0" max="100" value="50" id="slider-smooth" class="slider-control" data-key="smooth">
            </div>

            <div class="control-group">
                <div class="control-label">
                    <span>Sensibilidade Geral</span>
                    <span class="control-value" id="val-sens">100%</span>
                </div>
                <input type="range" min="0" max="200" value="100" id="slider-sens" class="slider-control" data-key="sens">
            </div>
        </div>

        <!-- Right Column: Pairing & Status -->
        <div class="card">
            <div class="card-header">
                <div class="card-title">Pareamento & Conexão</div>
            </div>
            
            <div class="pairing-box">
                <div class="qr-placeholder" id="qrArea">
                    <span>Escaneando... ou Gere Código</span>
                </div>
                
                <div class="code-display" id="pairingCode">--:--:--:--</div>
                
                <button class="btn btn-primary" id="btnGenerate">Gerar Novo Código</button>
                <button class="btn" id="btnReconnect">Reconectar</button>
            </div>

            <div style="margin-top: 20px;">
                <div class="card-title" style="font-size: 0.8rem; margin-bottom: 10px;">Log de Atividade</div>
                <div class="log-console" id="logConsole"></div>
            </div>
        </div>
    </div>
</div>

<script>
    // ─── Estado Local ─────────────────────────────────────────────────
    const state = {
        connected: false,
        masterActive: true,
        settings: {
            intensity: 75,
            precision: 80,
            fov: 65,
            smooth: 50,
            sens: 100
        },
        ws: null,
        sessionId: null,
        pairingCode: null,
        reconnectTimer: null
    };

    const WS_URL = 'ws://localhost:3000/ws'; // Ajuste para seu IP se necessário

    // ─── DOM Elements ─────────────────────────────────────────────────
    const els = {
        statusBadge: document.getElementById('statusBadge'),
        statusText: document.getElementById('statusText'),
        masterToggle: document.getElementById('masterToggle'),
        qrArea: document.getElementById('qrArea'),
        pairingCode: document.getElementById('pairingCode'),
        btnGenerate: document.getElementById('btnGenerate'),
        btnReconnect: document.getElementById('btnReconnect'),
        logConsole: document.getElementById('logConsole'),
        sliders: document.querySelectorAll('.slider-control')
    };

    // ─── Logging ──────────────────────────────────────────────────────
    function log(message, type = 'info') {
        const now = new Date
