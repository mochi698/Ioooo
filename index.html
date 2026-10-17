<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>災害・地震・ミサイル情報 リアルタイムダッシュボード</title>
    <!-- Leaflet CSS -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background: #0f172a; color: #f8fafc; display: flex; flex-direction: column; height: 100vh; }
        header { background: #1e293b; padding: 12px 16px; font-size: 1.1rem; font-weight: bold; border-bottom: 1px solid #334155; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 8px; }
        .badges { display: flex; gap: 6px; font-size: 0.75rem; flex-wrap: wrap; }
        .badge { padding: 3px 8px; border-radius: 10px; color: #fff; }
        
        #container { display: flex; flex-direction: column; flex: 1; overflow: hidden; }
        #map { width: 100%; height: 45vh; }
        #sidebar { flex: 1; background: #1e293b; display: flex; flex-direction: column; border-top: 1px solid #334155; }
        
        .sidebar-header { padding: 10px 16px; background: #0f172a; border-bottom: 1px solid #334155; font-weight: bold; font-size: 0.9rem; display: flex; justify-content: space-between; align-items: center; }
        #log-container { flex: 1; overflow-y: auto; padding: 12px; display: flex; flex-direction: column; gap: 8px; }
        
        .log-card { background: #334155; padding: 10px; border-radius: 6px; border-left: 4px solid #38bdf8; font-size: 0.85rem; }
        .log-card.usgs { border-left-color: #3b82f6; }
        .log-card.wolfx { border-left-color: #f43f5e; }
        .log-card.tsunami { border-left-color: #06b6d4; background: #164e63; }
        .log-card.jalert { border-left-color: #eab308; background: #713f12; }
        .log-card.dmdata { border-left-color: #10b981; background: #064e3b; } /* DMdata用カラー */
        
        .log-meta { display: flex; justify-content: space-between; font-size: 0.75rem; color: #94a3b8; margin-bottom: 4px; }
        pre { margin: 0; white-space: pre-wrap; word-break: break-all; font-family: monospace; font-size: 0.75rem; color: #e2e8f0; }

        @media (min-width: 768px) {
            #container { flex-direction: row; }
            #map { height: 100%; flex: 2; }
            #sidebar { flex: 1; border-top: none; border-left: 1px solid #334155; }
        }
    </style>
</head>
<body>

<header>
    <div>🚨 災害・ミサイル・世界地震ダッシュボード</div>
    <div class="badges">
        <span class="badge" style="background: #3b82f6;">USGS</span>
        <span class="badge" style="background: #f43f5e;">Wolfx</span>
        <span class="badge" style="background: #06b6d4;">P2P 津波</span>
        <span class="badge" style="background: #10b981;">DMdata (EEW)</span>
        <span class="badge" style="background: #eab308; color: #000;">J-Alert</span>
    </div>
</header>

<div id="container">
    <div id="map"></div>
    <div id="sidebar">
        <div class="sidebar-header">
            <span>リアルタイム受信ストリーム</span>
            <span style="font-size: 0.75rem; color: #94a3b8;">USGS / Wolfx / P2P / DMdata</span>
        </div>
        <div id="log-container">
            <div class="log-card">システム初期化中... 各種APIに接続しています。</div>
        </div>
    </div>
</div>

<!-- Leaflet JS -->
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
    // 全世界マップ初期化
    const map = L.map('map').setView([20, 0], 2);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap contributors'
    }).addTo(map);

    const logContainer = document.getElementById('log-container');

    function addLog(source, text, type = 'normal') {
        const card = document.createElement('div');
        card.className = `log-card ${type}`;
        card.innerHTML = `
            <div class="log-meta">
                <span><strong>[${source}]</strong></span>
                <span>${new Date().toLocaleTimeString()}</span>
            </div>
            <pre>${text}</pre>
        `;
        logContainer.prepend(card);
    }

    // 1. USGS（全世界の地震情報）
    async function loadUsgsEarthquakes() {
        try {
            const response = await fetch('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/2.5_day.geojson');
            const data = await response.json();
            addLog('USGS', `世界地震データ取得: ${data.features.length}件 (M2.5+)`, 'usgs');

            data.features.forEach(quake => {
                const [lng, lat] = quake.geometry.coordinates;
                const mag = quake.properties.mag;
                const place = quake.properties.title;
                const url = quake.properties.url;

                const circleMarker = L.circleMarker([lat, lng], {
                    radius: Math.max(mag * 3, 4),
                    fillColor: '#3b82f6',
                    color: '#ffffff',
                    weight: 1,
                    fillOpacity: 0.7
                }).addTo(map);

                circleMarker.bindPopup(`<strong>${place}</strong><br>M: ${mag}<br><a href="${url}" target="_blank">詳細</a>`);
            });
        } catch (e) {
            addLog('USGS', '取得エラー: ' + e.message, 'usgs');
        }
    }

    // 2. Wolfx WebSocket（アジア・日本速報）
    function connectWolfx() {
        try {
            const ws = new WebSocket('wss://api.wolfx.jp/quake.websocket');
            ws.onopen = () => addLog('Wolfx', 'WebSocket接続成功', 'wolfx');
            ws.onmessage = (event) => addLog('Wolfx', event.data, 'wolfx');
            ws.onclose = () => {
                setTimeout(connectWolfx, 5000);
            };
        } catch(e) {
            addLog('Wolfx', '接続エラー: ' + e.message, 'wolfx');
        }
    }

    // 3. P2PQuake WebSocket（津波・J-Alert / ミサイル・地震情報）
    function connectP2PQuake() {
        try {
            const ws = new WebSocket('wss://api.p2pquake.net/v2/ws');
            ws.onopen = () => addLog('P2PQuake', 'P2PQuake 接続成功（津波・J-Alert監視中）');
            
            ws.onmessage = (event) => {
                try {
                    const data = JSON.parse(event.data);
                    if (data.code === 552) {
                        addLog('津波警報', JSON.stringify(data, null, 2), 'tsunami');
                    } else if (data.code === 56 || data.code === 561) {
                        addLog('🚨J-ALERT/国民保護', JSON.stringify(data, null, 2), 'jalert');
                    } else {
                        addLog('P2PQuake', `受信 [code: ${data.code}]`);
                    }
                } catch(err) {
                    addLog('P2PQuake', event.data);
                }
            };

            ws.onclose = () => {
                setTimeout(connectP2PQuake, 5000);
            };
        } catch(e) {
            addLog('P2PQuake', '接続エラー: ' + e.message);
        }
    }

    // 4. DMdata (eew-history / 連携モジュール)
    function initDMdataIntegration() {
        // DMdataのEEW履歴・配信サービスについての案内や、API連携口の初期化ログ
        addLog('DMdata', 'DMdata連携モジュール初期化完了 (eew-history.dmdata.ne.jp 連携待機)', 'dmdata');

        // 実運用でDMdataのWebSocket/APIを利用する場合は、発行されたAPIキーや認証トークンをヘッダーに付与してfetchします。
        // サンプルとして、公開エンドポイントやステータス確認のフェッチ処理を記述しておくことができます：
        /*
        fetch('https://api.dmdata.ne.jp/v2/...', {
            headers: { 'Authorization': 'Bearer YOUR_API_KEY' }
        })
        .then(res => res.json())
        .then(data => console.log(data));
        */
    }

    // 起動
    loadUsgsEarthquakes();
    connectWolfx();
    connectP2PQuake();
    initDMdataIntegration();
</script>

</body>
</html>
