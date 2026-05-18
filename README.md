<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stark Labs Mainframe</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #02070e;
            font-family: monospace;
        }

        /* Direct Connect to background.png in the same folder */
        .mainframe-bg {
            position: relative;
            width: 100%;
            height: 100%;
            background: url('background.png') no-repeat center center;
            background-size: cover;
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1;
        }

        /* Tech monitor overlay effect */
        .mainframe-bg::before {
            content: " ";
            display: block;
            position: absolute;
            top: 0; left: 0; bottom: 0; right: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.18) 50%);
            background-size: 100% 4px;
            z-index: 2;
            pointer-events: none;
            opacity: 0.5;
        }

        /* Clickable Ring Exactly Over Arc Reactor Core */
        .reactor-core-touch {
            position: absolute;
            width: 320px;
            height: 320px;
            border-radius: 50%;
            z-index: 10;
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            border: 2px dashed rgba(0, 255, 204, 0.2);
            animation: slowRingSpin 40s linear infinite;
            transition: all 0.4s ease-in-out;
        }

        .reactor-core-touch:hover {
            border: 2px solid #00ffcc;
            box-shadow: 0 0 50px rgba(0, 255, 204, 0.4), inset 0 0 30px rgba(0, 255, 204, 0.2);
            transform: scale(1.02);
        }

        /* HUD Text Layer */
        .hud-text-layer {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            z-index: 5;
            pointer-events: none;
        }

        .monitor-logs {
            position: absolute;
            color: #52bccc;
            font-size: 11px;
            letter-spacing: 1px;
            line-height: 1.8;
            text-shadow: 0 0 8px rgba(82, 188, 204, 0.6);
        }

        .left-screen-data {
            left: 4%;
            bottom: 6%;
            border-left: 3px solid #00f2fe;
            padding-left: 12px;
        }

        .right-screen-data {
            right: 4%;
            bottom: 6%;
            text-align: right;
            border-right: 3px solid #00ffcc;
            padding-right: 12px;
        }

        .center-status-panel {
            position: absolute;
            bottom: 12%;
            width: 100%;
            text-align: center;
        }

        .status-alert-text {
            color: #00ffcc;
            font-size: 16px;
            letter-spacing: 5px;
            font-weight: bold;
            text-transform: uppercase;
            text-shadow: 0 0 15px rgba(0, 255, 204, 0.8);
            margin-bottom: 5px;
        }

        .status-sub-prompt {
            color: #ffffff;
            font-size: 11px;
            letter-spacing: 2px;
            opacity: 0.5;
            animation: alphaPulse 2.5s infinite ease-in-out;
        }

        @keyframes slowRingSpin {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        @keyframes alphaPulse {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 0.1; }
        }

        /* Popup Box Style */
        .hologram-modal-shield {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(3, 11, 22, 0.85);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            z-index: 1000;
            display: flex;
            justify-content: center;
            align-items: center;
            opacity: 0;
            pointer-events: none;
            transition: all 0.5s ease;
        }

        .hologram-modal-shield.active {
            opacity: 1;
            pointer-events: auto;
        }

        .cyber-panel-card {
            background: rgba(4, 15, 30, 0.9);
            border: 1px solid rgba(0, 242, 254, 0.4);
            box-shadow: 0 0 40px rgba(0, 242, 254, 0.3);
            width: 400px;
            padding: 40px;
            text-align: center;
        }

        .cyber-panel-card h3 {
            color: #00f2fe;
            letter-spacing: 4px;
            font-size: 16px;
            margin-bottom: 25px;
            text-shadow: 0 0 10px rgba(0, 242, 254, 0.4);
        }

        .secure-gate-input {
            width: 100%;
            background: rgba(1, 5, 11, 0.9);
            border: 1px solid #143f49;
            padding: 12px;
            color: #fff;
            font-size: 16px;
            letter-spacing: 6px;
            text-align: center;
            outline: none;
            margin-bottom: 25px;
        }

        .secure-gate-input:focus {
            border-color: #00ffcc;
            box-shadow: 0 0 15px rgba(0, 255, 204, 0.3);
        }

        .access-trigger-btn {
            background: linear-gradient(135deg, #00f2fe 0%, #4facfe 100%);
            border: none;
            color: #02070e;
            padding: 12px 35px;
            font-weight: bold;
            letter-spacing: 2px;
            cursor: pointer;
            box-shadow: 0 0 15px rgba(0, 242, 254, 0.4);
            transition: 0.3s;
        }

        .access-trigger-btn:hover {
            background: #fff;
            box-shadow: 0 0 25px rgba(0, 242, 254, 0.8);
        }
    </style>
</head>
<body>

    <div class="mainframe-bg">
        <div class="reactor-core-touch" onclick="triggerMainframeVerification()"></div>

        <div class="hud-text-layer">
            <div class="monitor-logs left-screen-data" id="dynamicLogFeed">
                HACKER PROTOCOLS: ACTIVE<br>
                NETWORK STATUS: SECURE<br>
                ARC POWER LOAD: 98%<br>
                STARK CORE LINK: ESTABLISHED
            </div>

            <div class="monitor-logs right-screen-data">
                SYSTEM_DIAGNOSTICS: STABLE<br>
                INTERFACE_BUS: TERMINAL OK<br>
                AI_INITIALIZE_NODE: STANDBY<br>
                ROUTING_TRACE: ENCRYPTED
            </div>

            <div class="center-status-panel">
                <div class="status-alert-text">SYSTEM ACCESS POINT</div>
                <div class="status-sub-prompt">[ TOUCH CENTRAL ARC REACTOR TO CONNECT AGENT ]</div>
            </div>
        </div>
    </div>

    <div class="hologram-modal-shield" id="securityGateway">
        <div class="cyber-panel-card">
            <h3>CRITICAL AUTHORIZATION</h3>
            <input type="password" placeholder="••••••••" class="secure-gate-input">
            <button class="access-trigger-btn" onclick="shutdownGatewayNode()">BYPASS SECURITY</button>
        </div>
    </div>

    <script>
        function triggerMainframeVerification() {
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel(); 
                let coreSpeechNode = new SpeechSynthesisUtterance("System authorization required.");
                coreSpeechNode.lang = "en-US";
                coreSpeechNode.pitch = 0.95;
                coreSpeechNode.rate = 1.0;
                window.speechSynthesis.speak(coreSpeechNode);
            }
            document.getElementById('securityGateway').classList.add('active');
        }

        function shutdownGatewayNode() {
            document.getElementById('securityGateway').classList.remove('active');
        }

        setInterval(() => {
            const staticHexBufferId = Math.floor(Math.random() * 256).toString(16).toUpperCase();
            document.getElementById('dynamicLogFeed').innerHTML = `
                HACKER PROTOCOLS: ACTIVE<br>
                NETWORK STATUS: SECURE<br>
                ARC POWER LOAD: 98%<br>
                STARK CORE LINK: ESTABLISHED<br>
                <span style="color: #00ffcc;">STREAM_NODE_ID: 0x${staticHexBufferId}...</span>
            `;
        }, 1500);
    </script>
</body>
</html>
