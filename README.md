<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ArcFlow DEX - Native L1 Liquidity & Swap Hub</title>
    <style>
        :root {
            --dex-bg: #070a13;
            --dex-card: #0f1624;
            --dex-input: #172033;
            --dex-blue: #3b82f6;
            --dex-green: #10b981;
            --dex-text: #ffffff;
            --dex-muted: #64748b;
            --border-color: #1e293b;
        }

        body {
            background-color: var(--dex-bg);
            color: var(--dex-text);
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }

        /* Top Header Navigation */
        header {
            border-bottom: 1px solid var(--border-color);
            background-color: rgba(7, 10, 19, 0.8);
            backdrop-filter: blur(12px);
            position: sticky;
            top: 0;
            z-index: 100;
        }
        .nav-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 18px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .logo {
            font-size: 1.4rem;
            font-weight: 800;
            color: white;
            text-decoration: none;
            letter-spacing: -0.5px;
        }
        .logo span {
            color: var(--dex-blue);
        }
        .network-status {
            display: flex;
            align-items: center;
            gap: 8px;
            background-color: rgba(16, 185, 129, 0.1);
            border: 1px solid rgba(16, 185, 129, 0.2);
            color: var(--dex-green);
            padding: 6px 14px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
        }
        .pulse-dot {
            width: 7px;
            height: 7px;
            background-color: var(--dex-green);
            border-radius: 50%;
            position: relative;
        }
        .pulse-dot::after {
            content: '';
            position: absolute;
            width: 100%;
            height: 100%;
            background-color: var(--dex-green);
            border-radius: 50%;
            animation: pulse 1.5s infinite;
            top: 0; left: 0;
        }
        @keyframes pulse {
            75%, 100% { transform: scale(2.5); opacity: 0; }
        }

        /* Main Swap Layout */
        main {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 40px 20px;
        }
        .dex-wrapper {
            width: 100%;
            max-width: 460px;
            background: var(--dex-card);
            border: 1px solid var(--border-color);
            border-radius: 28px;
            padding: 24px;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
            box-sizing: border-box;
        }
        .swap-meta-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 16px;
        }
        .swap-title {
            font-size: 1.15rem;
            font-weight: 700;
            margin: 0;
        }
        .wallet-connect-btn {
            background-color: rgba(59, 130, 246, 0.15);
            border: 1px solid rgba(59, 130, 246, 0.25);
            color: var(--dex-blue);
            padding: 10px 18px;
            border-radius: 16px;
            font-weight: 600;
            font-size: 0.85rem;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        .wallet-connect-btn:hover {
            background-color: var(--dex-blue);
            color: white;
            transform: translateY(-1px);
        }

        /* Input Cards */
        .token-box {
            background-color: var(--dex-input);
            border: 1px solid var(--border-color);
            border-radius: 20px;
            padding: 16px;
            transition: border-color 0.2s;
        }
        .token-box:focus-within {
            border-color: rgba(59, 130, 246, 0.5);
        }
        .token-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .amount-input {
            background: transparent;
            border: none;
            color: white;
            font-size: 1.8rem;
            font-weight: 700;
            width: 60%;
            outline: none;
        }
        .token-select {
            background-color: var(--dex-bg);
            border: 1px solid var(--border-color);
            color: white;
            padding: 8px 14px;
            border-radius: 24px;
            font-weight: 700;
            font-size: 1rem;
            cursor: pointer;
            outline: none;
        }
        .box-info {
            display: flex;
            justify-content: space-between;
            margin-top: 10px;
            font-size: 0.8rem;
            color: var(--dex-muted);
        }

        /* Switch Middle Button */
        .switch-container {
            height: 12px;
            position: relative;
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 5;
        }
        .switch-btn {
            width: 40px;
            height: 40px;
            background-color: var(--dex-input);
            border: 4px solid var(--dex-card);
            border-radius: 14px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: var(--dex-blue);
            transition: transform 0.2s, color 0.2s;
        }
        .switch-btn:hover {
            transform: rotate(180deg);
            color: var(--dex-green);
        }

        /* Main Action Button */
        .action-btn {
            width: 100%;
            background: linear-gradient(135deg, #3b82f6 0%, #1d4ed8 100%);
            color: white;
            border: none;
            padding: 16px;
            border-radius: 20px;
            font-size: 1.05rem;
            font-weight: 700;
            cursor: pointer;
            margin-top: 16px;
            transition: opacity 0.2s, transform 0.1s;
        }
        .action-btn:hover { opacity: 0.95; }
        .action-btn:active { transform: scale(0.99); }

        /* Tech Specs Analytics Panel */
        .analytics-panel {
            margin-top: 16px;
            background-color: rgba(23, 32, 51, 0.4);
            border-radius: 16px;
            border: 1px solid rgba(30, 41, 59, 0.5);
            padding: 14px 16px;
            font-size: 0.8rem;
            color: var(--dex-muted);
        }
        .analytics-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
        }
        .analytics-row:last-child { margin-bottom: 0; }
        .data-highlight { color: white; font-weight: 500; }

        /* Footer Links */
        footer {
            text-align: center;
            padding: 24px;
            font-size: 0.8rem;
            color: var(--dex-muted);
            border-top: 1px solid var(--border-color);
        }
        footer a {
            color: var(--dex-blue);
            text-decoration: none;
            margin: 0 10px;
        }
        footer a:hover { text-decoration: underline; }
    </style>
</head>
<body>

    <!-- Header Block -->
    <header>
        <div class="nav-container">
            <a href="#" class="logo">Arc<span>Flow</span></a>
            <div class="network-status">
                <span class="pulse-dot"></span>
                Arc L1 Native
            </div>
        </div>
    </header>

    <!-- Swap Main Core Interface -->
    <main>
        <div class="dex-wrapper">
            <div class="swap-meta-header">
                <h3 class="swap-title">Swap Ecosystem</h3>
                <button class="wallet-connect-btn" id="wallet-trigger">Connect Wallet</button>
            </div>

            <!-- Asset Source Block (FROM) -->
            <div class="token-box">
                <div class="token-row">
                    <input type="number" class="amount-input" id="amount-from" value="100" placeholder="0.0">
                    <select class="token-select" id="select-from">
                        <option value="USDC">USDC</option>
                        <option value="EURC">EURC</option>
                        <option value="ARC">ARC</option>
                    </select>
                </div>
                <div class="box-info">
                    <span>Available: <span id="balance-from">1,500.00</span></span>
                    <span>~$<span id="usd-from">100.00</span></span>
                </div>
            </div>

            <!-- Middle Switch Node -->
            <div class="switch-container">
                <div class="switch-btn" id="switch-trigger">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                        <polyline points="17 11 12 16 7 11"></polyline>
                        <polyline points="17 5 12 10 7 5"></polyline>
                    </svg>
                </div>
            </div>

            <!-- Asset Destination Block (TO) -->
            <div class="token-box" style="margin-top: 4px;">
                <div class="token-row">
                    <input type="number" class="amount-input" id="amount-to" value="33.3333" placeholder="0.0" readonly>
                    <select class="token-select" id="select-to">
                        <option value="ARC">ARC</option>
                        <option value="USDC">USDC</option>
                        <option value="EURC">EURC</option>
                    </select>
                </div>
                <div class="box-info">
                    <span>Available: <span id="balance-to">0.00</span></span>
                    <span>~$<span id="usd-to">100.00</span></span>
                </div>
            </div>

            <!-- Action Button -->
            <button class="action-btn" id="action-trigger">Swap Assets</button>

            <!-- Metadata Transaction Details -->
            <div class="analytics-panel">
                <div class="analytics-row">
                    <span>Minimum Guaranteed Output</span>
                    <span class="data-highlight" id="data-min-out">33.16 ARC</span>
                </div>
                <div class="analytics-row">
                    <span>Price Deviation Slips</span>
                    <span style="color: var(--dex-green); font-weight: 500;">&lt; 0.01%</span>
                </div>
                <div class="analytics-row">
                    <span>Ecosystem Liquidity Provision Fee</span>
                    <span class="data-highlight" id="data-lp-fee">0.05 USDC</span>
                </div>
                <div class="analytics-row">
                    <span>Arc Layer 1 Network Fee</span>
                    <span style="color: var(--dex-blue); font-weight: bold;">0.01 USDC (Fixed Architecture)</span>
                </div>
            </div>
        </div>
    </main>

    <!-- Footer Area Links -->
    <footer>
        <p>© 2026 ArcFlow Hub. Powered independently by Circle L1 Technology.</p>
        <p>
            <a href="https://arc.network" target="_blank">Arc Network</a> • 
            <a href="https://circle.com" target="_blank">Circle Faucet</a> • 
            <a href="https://circle.com" target="_blank">Support Center</a>
        </p>
    </footer>

    <!-- Interactive Math Core Script -->
    <script>
        const amountFrom = document.getElementById('amount-from');
        const amountTo = document.getElementById('amount-to');
        const selectFrom = document.getElementById('select-from');
        const selectTo = document.getElementById('select-to');
        const walletTrigger = document.getElementById('wallet-trigger');
        const actionTrigger = document.getElementById('action-trigger');
        const switchTrigger = document.getElementById('switch-trigger');

        const marketRates = { USDC: 1.0, EURC: 1.08, ARC: 3.0 };

        function calculateSwapOutputs() {
            const inputVal = parseFloat(amountFrom.value) || 0;
            const fromAsset = selectFrom.value;
            const toAsset = selectTo.value;

            // Engine Math logic Conversion
            const valueInUSD = inputVal * marketRates[fromAsset];
            const outputVal = valueInUSD / marketRates[toAsset];

            amountTo.value = outputVal.toFixed(4);

            document.getElementById('usd-from').innerText = valueInUSD.toFixed(2);
            document.getElementById('usd-to').innerText = valueInUSD.toFixed(2);

            document.getElementById('data-min-out').innerText = `${(outputVal * 0.995).toFixed(2)} ${toAsset}`;
            document.getElementById('data-lp-fee').innerText = `${(inputVal * 0.0005).toFixed(4)} ${fromAsset}`;
        }

        amountFrom.addEventListener('input', calculateSwapOutputs);
        selectFrom.addEventListener('change', calculateSwapOutputs);
        selectTo.addEventListener('change', calculateSwapOutputs);

        switchTrigger.addEventListener('click', () => {
            const temp = selectFrom.value;
            selectFrom.value = selectTo.value;
            selectTo.value = temp;
            calculateSwapOutputs();
        });

        walletTrigger.addEventListener('click', () => {
            if(walletTrigger.innerText === "Connect Wallet") {
                walletTrigger.innerText = "0x4f...a89d";
                walletTrigger.style.borderColor = "rgba(16, 185, 129, 0.3)";
                walletTrigger.style.color = "var(--dex-green)";
                walletTrigger.style.backgroundColor = "rgba(16, 185, 129, 0.15)";
                document.getElementById('balance-from').innerText = "2,850.00";
            } else {
                walletTrigger.innerText = "Connect Wallet";
                walletTrigger.style.borderColor = "rgba(59, 130, 246, 0.25)";
                walletTrigger.style.color = "var(--dex-blue)";
                walletTrigger.style.backgroundColor = "rgba(59, 130, 246, 0.15)";
                document.getElementById('balance-from').innerText = "1,500.00";
            }
        });

        actionTrigger.addEventListener('click', () => {
            alert(`Execution sequence routing to Arc Ecosystem nodes successful!\nSwapped ${amountFrom.value} ${selectFrom.value} into ${amountTo.value} ${selectTo.value}.\nGas architecture covered natively with USDC.`);
        });

        calculateSwapOutputs();
    </script>
</body>
</html>

# Jarc-swap
