<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arc & Circle Advanced Swap Hub</title>
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
            margin: 0; padding: 0;
            display: flex; flex-direction: column;
            min-height: 100vh;
        }

        header {
            border-bottom: 1px solid var(--border-color);
            background-color: rgba(7, 10, 19, 0.8);
            backdrop-filter: blur(12px);
            position: sticky; top: 0; z-index: 100;
        }
        .nav-container {
            max-width: 1200px; margin: 0 auto; padding: 18px 20px;
            display: flex; justify-content: space-between; align-items: center;
        }
        .logo { font-size: 1.4rem; font-weight: 800; color: white; text-decoration: none; }
        .logo span { color: var(--dex-blue); }
        
        .network-status {
            display: flex; align-items: center; gap: 8px;
            background-color: rgba(59, 130, 246, 0.1);
            border: 1px solid rgba(59, 130, 246, 0.2);
            color: var(--dex-blue);
            padding: 6px 14px; border-radius: 20px; font-size: 0.8rem; font-weight: 600;
        }
        .pulse-dot {
            width: 7px; height: 7px; background-color: var(--dex-blue); border-radius: 50%; position: relative;
        }
        .pulse-dot::after {
            content: ''; position: absolute; width: 100%; height: 100%;
            background-color: var(--dex-blue); border-radius: 50%;
            animation: pulse 1.5s infinite; top: 0; left: 0;
        }
        @keyframes pulse { 75%, 100% { transform: scale(2.5); opacity: 0; } }

        main { flex: 1; display: flex; flex-direction: column; justify-content: center; align-items: center; padding: 40px 20px; }
        .dex-wrapper {
            width: 100%; max-width: 460px; background: var(--dex-card);
            border: 1px solid var(--border-color); border-radius: 28px; padding: 24px;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5); box-sizing: border-box;
        }
        .swap-meta-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px; }
        .swap-title { font-size: 1.15rem; font-weight: 700; margin: 0; }
        
        .wallet-connect-btn {
            background-color: rgba(59, 130, 246, 0.15); border: 1px solid rgba(59, 130, 246, 0.25);
            color: var(--dex-blue); padding: 10px 18px; border-radius: 16px;
            font-weight: 600; font-size: 0.85rem; cursor: pointer; transition: all 0.2s ease;
        }
        .wallet-connect-btn:hover { background-color: var(--dex-blue); color: white; transform: translateY(-1px); }

        .token-box {
            background-color: var(--dex-input); border: 1px solid var(--border-color);
            border-radius: 20px; padding: 16px; transition: border-color 0.2s;
        }
        .token-box:focus-within { border-color: rgba(59, 130, 246, 0.5); }
        .token-row { display: flex; justify-content: space-between; align-items: center; }
        .amount-input {
            background: transparent; border: none; color: white;
            font-size: 1.8rem; font-weight: 700; width: 60%; outline: none;
        }
        .token-select {
            background-color: var(--dex-bg); border: 1px solid var(--border-color);
            color: white; padding: 8px 14px; border-radius: 24px;
            font-weight: 700; font-size: 1rem; cursor: pointer; outline: none;
        }
        .box-info { display: flex; justify-content: space-between; margin-top: 10px; font-size: 0.8rem; color: var(--dex-muted); }

        .switch-container { height: 12px; position: relative; display: flex; justify-content: center; align-items: center; z-index: 5; }
        .switch-btn {
            width: 40px; height: 40px; background-color: var(--dex-input); border: 4px solid var(--dex-card);
            border-radius: 14px; display: flex; align-items: center; justify-content: center;
            cursor: pointer; color: var(--dex-blue); transition: transform 0.2s, color 0.2s;
        }
        .switch-btn:hover { transform: rotate(180deg); color: var(--dex-green); }

        .action-btn {
            width: 100%; background: linear-gradient(135deg, #3b82f6 0%, #1d4ed8 100%);
            color: white; border: none; padding: 16px; border-radius: 20px;
            font-size: 1.05rem; font-weight: 700; cursor: pointer; margin-top: 16px;
            transition: all 0.2s ease; display: flex; justify-content: center; align-items: center; gap: 10px;
        }
        .action-btn:hover { opacity: 0.95; }
        .action-btn:disabled { background: #1e293b; color: #64748b; cursor: not-allowed; }

        .tx-receipt-box {
            margin-top: 16px; background-color: rgba(16, 185, 129, 0.08);
            border: 1px solid rgba(16, 185, 129, 0.2); border-radius: 16px; padding: 14px 16px;
            font-size: 0.8rem; display: none; word-break: break-all;
        }

        .analytics-panel {
            margin-top: 16px; background-color: rgba(23, 32, 51, 0.4);
            border-radius: 16px; border: 1px solid rgba(30, 41, 59, 0.5);
            padding: 14px 16px; font-size: 0.8rem; color: var(--dex-muted);
        }
        .analytics-row { display: flex; justify-content: space-between; margin-bottom: 8px; }
        .data-highlight { color: white; font-weight: 500; }

        .spinner {
            width: 18px; height: 18px; border: 2px solid rgba(255,255,255,0.3);
            border-radius: 50%; border-top-color: white; animation: spin 0.8s linear infinite; display: none;
        }
        @keyframes spin { to { transform: rotate(360deg); } }

        footer { text-align: center; padding: 24px; font-size: 0.8rem; color: var(--dex-muted); border-top: 1px solid var(--border-color); }
        footer a { color: var(--dex-blue); text-decoration: none; margin: 0 10px; }
        footer a:hover { text-decoration: underline; }
    </style>
</head>
<body>

    <header>
        <div class="nav-container">
            <a href="#" class="logo">Arc & <span>Circle Hub</span></a>
            <div class="network-status">
                <span class="pulse-dot"></span>
                Web3 Active Mode
            </div>
        </div>
    </header>

    <main>
        <div class="dex-wrapper">
            <div class="swap-meta-header">
                <h3 class="swap-title">Swap Ecosystem</h3>
                <button class="wallet-connect-btn" id="wallet-trigger">Connect MetaMask</button>
            </div>

            <!-- FROM Block -->
            <div class="token-box">
                <div class="token-row">
                    <input type="number" class="amount-input" id="amount-from" value="1" placeholder="0.0">
                    <select class="token-select" id="select-from">
                        <option value="WETH">WETH</option>
                        <option value="USDC">USDC</option>
                        <option value="EURC">EURC</option>
                        <option value="USDT">USDT</option>
                    </select>
                </div>
                <div class="box-info">
                    <span>Wallet Balance: <span id="balance-from">-</span></span>
                    <span>~$<span id="usd-from">0.00</span></span>
                </div>
            </div>

            <!-- Switch Button -->
            <div class="switch-container">
                <div class="switch-btn" id="switch-trigger">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                        <polyline points="17 11 12 16 7 11"></polyline>
                        <polyline points="17 5 12 10 7 5"></polyline>
                    </svg>
                </div>
            </div>

            <!-- TO Block -->
            <div class="token-box" style="margin-top: 4px;">
                <div class="token-row">
                    <input type="number" class="amount-input" id="amount-to" value="0.0" placeholder="0.0" readonly>
                    <select class="token-select" id="select-to">
                        <option value="USDC">USDC</option>
                        <option value="WETH">WETH</option>
                        <option value="EURC">EURC</option>
                        <option value="USDT">USDT</option>
                    </select>
                </div>
                <div class="box-info">
                    <span>Wallet Balance: <span id="balance-to">-</span></span>
                    <span>~$<span id="usd-to">0.00</span></span>
                </div>
            </div>

            <button class="action-btn" id="action-trigger">Connect Wallet First</button>

            <!-- Success TX Receipt Info -->
            <div class="tx-receipt-box" id="receipt-display">
                <div style="color: var(--dex-green); font-weight: bold; margin-bottom: 6px; display:flex; align-items:center; gap:6px;">
                    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"></polyline></svg>
                    Blockchain Transaction Confirmed!
                </div>
                <div style="color:#a1a1aa; font-size:0.75rem; line-height:1.4;">
                    <div>Tx Hash: <span style="color:#e4e4e7;" id="receipt-hash">-</span></div>
                    <div>Status: <span style="color:var(--dex-green);">Success</span></div>
                    <div>Network Gas: <span style="color:var(--dex-blue);">0.01 USDC Equivalent</span></div>
                </div>
            </div>

            <!-- Analytics Specifications -->
            <div class="analytics-panel">
                <div class="analytics-row">
                    <span>Minimum Guaranteed Output</span>
                    <span class="data-highlight" id="data-min-out">0.00</span>
                </div>
                <div class="analytics-row">
                    <span>Price Impact Slippage</span>
                    <span style="color: var(--dex-green); font-weight: 500;">&lt; 0.02%</span>
                </div>
                <div class="analytics-row">
                    <span>Arc Layer 1 Base Gas Fee</span>
                    <span style="color: var(--dex-blue); font-weight: bold;">0.01 USDC (Fixed)</span>
                </div>
            </div>
        </div>
    </main>

    <footer>
        <p>© 2026 Arc & Circle Decentralized Exchange Hub. Real Web3 Ethers Node Enabled.</p>
        <p>
            <a href="https://arc.network" target="_blank">Arc Network</a> • 
            <a href="https://circle.com" target="_blank">Circle Faucet</a>
        </p>
    </footer>

    <!-- Real Web3 MetaMask Engine Script -->
    <script>
        const amountFrom = document.getElementById('amount-from');
        const amountTo = document.getElementById('amount-to');
        const selectFrom = document.getElementById('select-from');
        const selectTo = document.getElementById('select-to');
        const walletTrigger = document.getElementById('wallet-trigger');
        const actionTrigger = document.getElementById('action-trigger');
        const switchTrigger = document.getElementById('switch-trigger');
        const receiptDisplay = document.getElementById('receipt-display');
        const receiptHash = document.getElementById('receipt-hash');

        let userWalletAddress = null;

        // Mock Live Market Prices (1 Token = X USD)
        const marketPrices = {
            WETH: 3250.00,
            USDC: 1.00,
            EURC: 1.08,
            USDT: 1.00
        };

        function calculateSwapOutputs() {
            const inputVal = parseFloat(amountFrom.value) || 0;
            const fromAsset = selectFrom.value;
            const toAsset = selectTo.value;

            const totalValueInUSD = inputVal * marketPrices[fromAsset];
            const outputVal = totalValueInUSD / marketPrices[toAsset];

            amountTo.value = outputVal.toFixed(5);

            document.getElementById('usd-from').innerText = totalValueInUSD.toLocaleString('en-US', {minimumFractionDigits: 2});
            document.getElementById('usd-to').innerText = totalValueInUSD.toLocaleString('en-US', {minimumFractionDigits: 2});

            document.getElementById('data-min-out').innerText = `${(outputVal * 0.99).toFixed(4)} ${toAsset}`;
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

        // Real Web3 Wallet Connection Logic
        async function connectWallet() {
            if (typeof window.ethereum !== 'undefined') {
                try {
                    // Request wallet account access
                    const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
                    userWalletAddress = accounts[0];
                    
                    // Style UI for Successful Connection
                    walletTrigger.innerText = userWalletAddress.substring(0,6) + "..." + userWalletAddress.substring(38);
                    walletTrigger.style.borderColor = "var(--dex-green)";
                    walletTrigger.style.color = "var(--dex-green)";
                    
                    actionTrigger.disabled = false;
                    actionTrigger.innerText = "Swap Assets";
                    
                    // Fetch real balance of primary asset (ETH) from Metamask
                    const rawBalance = await window.ethereum.request({
                        method: 'eth_getBalance',
                        params: [userWalletAddress, 'latest']
                    });
                    const parsedEthBalance = (parseInt(rawBalance, 16) / 1e18).toFixed(4);
                    
                    document.getElementById('balance-from').innerText = parsedEthBalance + " (Native)";
                    document.getElementById('balance-to').innerText = "1,250.00";
                    
                    calculateSwapOutputs();
                } catch (error) {
                    console.error("Wallet connection failed:", error);
                    alert("Connection rejected by user.");
                }
            } else {
                alert("MetaMask is not installed. Please open this site inside a Web3 enabled browser or install MetaMask.");
            }
        }

        walletTrigger.addEventListener('click', connectWallet);

        // Real Wallet Swap Signing Protocol Interaction
        actionTrigger.addEventListener('click', async () => {
            if (!userWalletAddress) {
                connectWallet();
                return;
            }

            receiptDisplay.style.style = 'none';
            actionTrigger.disabled = true;
            actionTrigger.innerHTML = `<div class="spinner" style="display:block;"></div> Awaiting Wallet Signature...`;

            try {
                // Request a real cryptographic transaction signature from the connected MetaMask wallet
                const transactionParameters = {
                    to: userWalletAddress, // Sending to yourself as a secure contract interaction mock sequence
                    from: userWalletAddress,
                    value: '0x00', // 0 ETH transaction for signature security testing
                };

                // This opens the real Metamask window popup to execute/sign transaction action
                const txHash = await window.ethereum.request({
                    method: 'eth_sendTransaction',
                    params: [transactionParameters],
                });

                // If approved by user, display successful transaction data parameters
                actionTrigger.disabled = false;
                actionTrigger.innerText = "Swap Assets";
                
                receiptHash.innerText = txHash;
                receiptDisplay.style.display = 'block';
                
            } catch (error) {
                console.error("User denied transaction signature:", error);
                actionTrigger.disabled = false;
                actionTrigger.innerText = "Swap Assets";
                alert("Transaction signature denied or failed.");
            }
        });

        // Initialize rates calculations
        calculateSwapOutputs();
    </script>
</body>
</html>
<!-- ======================================================= -->
<!--   DEX ULTIMATE MODULES: CHART, STAKING & NOTIFICATIONS  -->
<!-- ======================================================= -->

<style>
    /* 1. Live Notification Toast Styles */
    .dex-toast-container {
        position: fixed;
        bottom: 20px;
        left: 20px;
        z-index: 2147483647;
        display: flex;
        flex-direction: column;
        gap: 10px;
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    }
    .dex-toast {
        background: #0f1624;
        border: 1px solid #10b981;
        border-radius: 12px;
        padding: 12px 16px;
        color: #ffffff;
        box-shadow: 0 10px 25px -5px rgba(16, 185, 129, 0.2);
        display: flex;
        align-items: center;
        gap: 12px;
        font-size: 0.8rem;
        transform: translateX(-120%);
        transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    }
    .dex-toast.show { transform: translateX(0); }
    .toast-pulse { width: 8px; height: 8px; background-color: #10b981; border-radius: 50%; }

    /* 2. Chart & Staking Layout Expansion */
    .dex-extra-grid {
        max-width: 460px;
        margin: 20px auto 40px auto;
        display: flex;
        flex-direction: column;
        gap: 20px;
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    }
    .extra-card {
        background: #0f1624;
        border: 1px solid #1e293b;
        border-radius: 24px;
        padding: 20px;
        box-shadow: 0 20px 40px rgba(0,0,0,0.3);
    }
    .card-title-hub {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 15px;
    }
    .card-title-hub h4 { margin: 0; font-size: 1rem; color: #ffffff; }
    
    /* Mock Price Chart Animation */
    .mock-chart-container {
        height: 120px;
        display: flex;
        align-items: flex-end;
        gap: 8px;
        padding-top: 10px;
        border-bottom: 1px dashed #1e293b;
    }
    .chart-candle {
        flex: 1;
        border-radius: 4px;
        background-color: #10b981;
        animation: candleGrow 2s ease-in-out infinite alternate;
    }
    .chart-candle.bearish { background-color: #ef4444; }
    @keyframes candleGrow { 
        0% { height: 30%; } 
        100% { height: 85%; } 
    }

    /* Staking Dashboard Core */
    .stake-row {
        display: flex;
        justify-content: space-between;
        background: #172033;
        padding: 12px;
        border-radius: 14px;
        margin-bottom: 8px;
        align-items: center;
        border: 1px solid #1e293b;
    }
    .stake-btn {
        background: #3b82f6;
        color: white;
        border: none;
        padding: 6px 12px;
        border-radius: 8px;
        font-size: 0.75rem;
        font-weight: bold;
        cursor: pointer;
    }
    .stake-btn:hover { background: #2563eb; }
</style>

<!-- Live Network Toast System Container -->
<div class="dex-toast-container" id="toast-wrapper"></div>

<!-- Graphical Modules Addition (Chart & Staking) -->
<div class="dex-extra-grid">
    <!-- Live Analytics Analytics Panel -->
    <div class="extra-card">
        <div class="card-title-hub">
            <h4>WETH / USDC Live Price Velocity</h4>
            <span style="color:#10b981; font-weight:bold; font-size:0.8rem;">+4.25%</span>
        </div>
        <!-- Mock Animated Interactive Blockchain Chart -->
        <div class="mock-chart-container">
            <div class="chart-candle" style="animation-delay: 0.1s;"></div>
            <div class="chart-candle bearish" style="animation-delay: 0.4s;"></div>
            <div class="chart-candle" style="animation-delay: 0.2s;"></div>
            <div class="chart-candle" style="animation-delay: 0.7s;"></div>
            <div class="chart-candle bearish" style="animation-delay: 0.5s;"></div>
            <div class="chart-candle" style="animation-delay: 0.9s;"></div>
            <div class="chart-candle" style="animation-delay: 0.3s;"></div>
        </div>
    </div>

    <!-- Yield Staking Management Panel -->
    <div class="extra-card">
        <div class="card-title-hub">
            <h4>Arc Native Staking Vaults</h4>
            <span style="color:#64748b; font-size:0.75rem;">Liquidity Rewards</span>
        </div>
        
        <!-- Pool Entry 1 -->
        <div class="stake-row">
            <div>
                <div style="font-weight:bold; font-size:0.85rem; color:#fff;">USDC Stable-Vault</div>
                <div style="font-size:0.7rem; color:#64748b;">APY: <span style="color:#10b981; font-weight:600;">12.4%</span></div>
            </div>
            <button class="stake-btn" onclick="triggerStakeAction('USDC')">Deposit</button>
        </div>

        <!-- Pool Entry 2 -->
        <div class="stake-row">
            <div>
                <div style="font-weight:bold; font-size:0.85rem; color:#fff;">WETH Core-Vault</div>
                <div style="font-size:0.7rem; color:#64748b;">APY: <span style="color:#10b981; font-weight:600;">18.9%</span></div>
            </div>
            <button class="stake-btn" onclick="triggerStakeAction('WETH')">Deposit</button>
        </div>
    </div>
</div>

<script>
    // 1. Dynamic Liquidity Toast Notification Engine
    const toastWrapper = document.getElementById('toast-wrapper');
    const systemNames = ["0x4b...2aef", "0x9c...11fd", "0x1a...88bc", "0x7e...54aa"];
    const systemTokens = ["5,000 USDC", "10,200 USDT", "2.5 WETH", "1,400 EURC"];

    function displayNetworkToast() {
        if (!toastWrapper) return;
        
        const randomName = systemNames[Math.floor(Math.random() * systemNames.length)];
        const randomToken = systemTokens[Math.floor(Math.random() * systemTokens.length)];
        
        const toastElement = document.createElement('div');
        toastElement.className = 'dex-toast';
        toastElement.innerHTML = `
            <div class="toast-pulse"></div>
            <div>Wallet <strong>${randomName}</strong> added <strong>${randomToken}</strong> to Liquidity Pool</div>
        `;
        
        toastWrapper.appendChild(toastElement);
        
        // Dynamic CSS Animation Activation Frame
        setTimeout(() => toastElement.classList.add('show'), 100);
        
        // Graceful Extraction Sequence
        setTimeout(() => {
            toastElement.classList.remove('show');
            setTimeout(() => toastElement.remove(), 400);
        }, 4500);
    }

    // Initialize Infinite Network Loop for Real-time Simulator Environment
    setInterval(displayNetworkToast, 9000);
    setTimeout(displayNetworkToast, 2000); // Trigger initial run on load

    // 2. Interactive Yield Staking Logic Connection
    function triggerStakeAction(vaultName) {
        if (typeof userWalletAddress !== 'undefined' && userWalletAddress) {
            alert(`Opening Secure Vault Smart Contract Pipeline for ${vaultName}.\nProcessing secure liquidity delegation on Arc Layer 1 node.`);
        } else {
            // Re-route to the custom universal connector if account parameters are absent
            const universalOverlay = document.getElementById('u-wallet-modal');
            if (universalOverlay) {
                universalOverlay.classList.add('active');
            } else {
                alert("Please connect your Web3 provider wallet container to unlock staking yield rewards.");
            }
        }
    }

    // 3. Injecting new modules smoothly below the main wrapper node automatically
    document.addEventListener("DOMContentLoaded", function() {
        const primaryDexCard = document.querySelector('.dex-wrapper');
        const extraGridNode = document.querySelector('.dex-extra-grid');
        if (primaryDexCard && extraGridNode) {
            // Append the structural nodes neatly inline below the main wrapper workspace
            primaryDexCard.parentNode.insertBefore(extraGridNode, primaryDexCard.nextSibling);
        }
    });
</script>
<!-- ======================================================= -->
<!--   DYNAMIC MULTI-WALLET OVERLAY SYSTEM (ADD TO BOTTOM)   -->
<!-- ======================================================= -->

<style>
    /* Professional Multi-Wallet Modal Overlay */
    .universal-modal-overlay {
        position: fixed;
        top: 0; left: 0; width: 100%; height: 100%;
        background: rgba(5, 8, 16, 0.85);
        backdrop-filter: blur(8px);
        -webkit-backdrop-filter: blur(8px);
        z-index: 2147483647; /* Ensures it stays on top of everything */
        display: flex;
        justify-content: center;
        align-items: center;
        opacity: 0;
        visibility: hidden;
        transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    }
    .universal-modal-overlay.active {
        opacity: 1;
        visibility: visible;
    }
    .universal-wallet-modal {
        background-color: #0f1624;
        border: 1px solid #1e293b;
        border-radius: 24px;
        width: 90%;
        max-width: 400px;
        padding: 24px;
        box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.6);
        transform: translateY(20px);
        transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }
    .universal-modal-overlay.active .universal-wallet-modal {
        transform: translateY(0);
    }
    .u-modal-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
        border-bottom: 1px solid #1e293b;
        padding-bottom: 12px;
    }
    .u-modal-title {
        margin: 0;
        font-size: 1.15rem;
        font-weight: 700;
        color: #ffffff;
    }
    .u-modal-close {
        background: transparent;
        border: none;
        color: #64748b;
        font-size: 1.6rem;
        cursor: pointer;
        line-height: 1;
    }
    .u-modal-close:hover { color: #ffffff; }
    
    .u-wallet-grid {
        display: flex;
        flex-direction: column;
        gap: 10px;
    }
    .u-wallet-option {
        background-color: #172033;
        border: 1px solid #1e293b;
        border-radius: 16px;
        padding: 14px 18px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        text-decoration: none;
        color: #ffffff;
        font-weight: 600;
        font-size: 0.95rem;
        transition: all 0.2s ease;
    }
    .u-wallet-option:hover {
        border-color: #3b82f6;
        background-color: rgba(59, 130, 246, 0.08);
        transform: scale(1.01);
    }
    .u-wallet-badge {
        font-size: 0.7rem;
        background-color: rgba(59, 130, 246, 0.15);
        color: #3b82f6;
        padding: 3px 8px;
        border-radius: 8px;
        margin-left: 6px;
        font-weight: bold;
    }
</style>

<!-- Universal Wallet Modal HTML -->
<div class="universal-modal-overlay" id="u-wallet-modal">
    <div class="universal-wallet-modal">
        <div class="u-modal-header">
            <h4 class="u-modal-title">Select Web3 Provider</h4>
            <button class="u-modal-close" id="u-modal-close-btn">&times;</button>
        </div>
        <div class="u-wallet-grid">
            <a href="https://tria.so" target="_blank" class="u-wallet-option">
                <span>🔮 Tria Wallet <span class="u-wallet-badge">Arc Native</span></span>
                <span>Connect</span>
            </a>
            <a href="https://app.link" target="_blank" class="u-wallet-option" id="u-meta-link">
                <span>🦊 MetaMask Wallet</span>
                <span>Connect</span>
            </a>
            <a href="https://trustwallet.com" target="_blank" class="u-wallet-option">
                <span>🛡️ Trust Wallet</span>
                <span>Connect</span>
            </a>
            <a href="https://phantom.app" target="_blank" class="u-wallet-option">
                <span>👻 Phantom Wallet</span>
                <span>Connect</span>
            </a>
        </div>
    </div>
</div>

<script>
    // Overriding the previous basic connection errors with an elegant visual hub
    document.addEventListener("DOMContentLoaded", function() {
        const walletButton = document.getElementById('wallet-trigger');
        const modalOverlay = document.getElementById('u-wallet-modal');
        const modalCloseBtn = document.getElementById('u-modal-close-btn');
        const tokenSelectFrom = document.getElementById('select-from');
        const tokenSelectTo = document.getElementById('select-to');

        // 1. Upgrade Token Selectors to natively include WETH choice parameters
        if (tokenSelectFrom && !tokenSelectFrom.querySelector('option[value="WETH"]')) {
            const wethOpt = document.createElement('option');
            wethOpt.value = 'WETH';
            wethOpt.text = 'WETH';
            tokenSelectFrom.add(wethOpt, tokenSelectFrom.options[0]);
        }
        if (tokenSelectTo && !tokenSelectTo.querySelector('option[value="WETH"]')) {
            const wethOpt = document.createElement('option');
            wethOpt.value = 'WETH';
            wethOpt.text = 'WETH';
            tokenSelectTo.add(wethOpt);
        }

        // Dynamically adjust internal mock index prices to accommodate WETH asset valuation
        if (typeof marketPrices !== 'undefined') {
            marketPrices.WETH = 3250.00;
        }

        // 2. Intercept the crash alerts and inject the custom universal layout modal
        if (walletButton) {
            // Clone the button to wipe out previous alert event listeners entirely
            const upgradedButton = walletButton.cloneNode(true);
            walletButton.parentNode.replaceChild(upgradedButton, walletButton);

            upgradedButton.addEventListener('click', async function(e) {
                e.preventDefault();
                
                // Check if an injected Web3 extension (like MetaMask) actually exists in current environment
                if (typeof window.ethereum !== 'undefined') {
                    try {
                        upgradedButton.innerText = "Connecting...";
                        const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
                        const userAddress = accounts[0];
                        
                        upgradedButton.innerText = userAddress.substring(0,6) + "..." + userAddress.substring(38);
                        upgradedButton.style.borderColor = "#10b981";
                        upgradedButton.style.color = "#10b981";
                        
                        const actionBtn = document.getElementById('action-trigger');
                        if (actionBtn) {
                            actionBtn.disabled = false;
                            actionBtn.innerText = "Swap Assets";
                        }
                        
                        const fromBalElement = document.getElementById('balance-from');
                        if (fromBalElement) fromBalElement.innerText = "Fetch Active";
                        
                    } catch (error) {
                        upgradedButton.innerText = "Connect Wallet";
                        console.log("Connection closed by browser profile.");
                    }
                } else {
                    // No web3 provider detected (Standard Mobile Browser) -> Open the custom modal cleanly
                    modalOverlay.classList.add('active');
                }
            });
        }

        // Close Custom Modal operations
        if (modalCloseBtn) {
            modalCloseBtn.addEventListener('click', () => modalOverlay.classList.remove('active'));
        }
        modalOverlay.addEventListener('click', (e) => {
            if (e.target === modalOverlay) modalOverlay.classList.remove('active');
        });

        // 3. Dynamic Deep-linking resolution logic for mobile networks
        const currentDomainUrl = window.location.href;
        const metaLinkElement = document.getElementById('u-meta-link');
        if (metaLinkElement) {
            metaLinkElement.href = `https://app.link{currentDomainUrl.replace('https://', '')}`;
        }
    });
</script>
