<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CYBER_SCRIPT // TERMINAL</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Courier New', Courier, monospace;
        }
        body {
            background-color: #050505;
            color: #00ff66;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            padding: 20px;
            overflow-x: hidden;
        }

        /* 배경 스캔라인 효과 */
        body::before {
            content: " ";
            display: block;
            position: fixed;
            top: 0; left: 0; bottom: 0; right: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));
            z-index: 999;
            background-size: 100% 4px, 6px 100%;
            pointer-events: none;
        }

        /* 라이선스 인증 창 */
        .license-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(5, 5, 5, 0.98);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 10000;
            padding: 20px;
        }
        .license-card {
            background-color: #0a0a0a;
            border: 2px solid #ff0055;
            border-radius: 6px;
            padding: 35px;
            max-width: 480px;
            width: 100%;
            text-align: center;
            box-shadow: 0 0 30px rgba(255, 0, 85, 0.3);
        }
        .license-card h2 {
            color: #ff0055;
            margin-bottom: 15px;
            font-size: 1.5rem;
            text-shadow: 0 0 10px rgba(255, 0, 85, 0.5);
            letter-spacing: 2px;
        }
        .license-card p {
            color: #888;
            font-size: 0.9rem;
            margin-bottom: 20px;
        }
        .license-card input {
            width: 100%;
            padding: 14px;
            border-radius: 4px;
            border: 1px solid #ff0055;
            background-color: #000;
            color: #00ff66;
            font-size: 1rem;
            margin-bottom: 15px;
            outline: none;
            text-align: center;
            letter-spacing: 1px;
        }
        .license-card button {
            width: 100%;
            padding: 14px;
            background-color: #ff0055;
            color: #fff;
            border: none;
            border-radius: 4px;
            font-size: 1.05rem;
            font-weight: bold;
            cursor: pointer;
            transition: 0.2s;
            text-shadow: 0 0 5px rgba(0,0,0,0.5);
        }
        .license-card button:hover {
            background-color: #ff3366;
            box-shadow: 0 0 15px #ff0055;
        }

        /* 메인 콘텐츠 스타일 */
        .main-content {
            display: none;
            width: 100%;
            max-width: 850px;
            flex-direction: column;
            align-items: center;
        }
        .header-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            margin-bottom: 25px;
            width: 100%;
        }
        h1 {
            font-size: 2.2rem;
            color: #00ff66;
            text-shadow: 0 0 12px rgba(0, 255, 102, 0.4);
            text-align: center;
            letter-spacing: 3px;
        }
        .reset-btn {
            background-color: transparent;
            border: 1px solid #ff0055;
            color: #ff0055;
            padding: 6px 12px;
            font-size: 0.8rem;
            font-weight: bold;
            border-radius: 4px;
            cursor: pointer;
            transition: 0.2s;
            letter-spacing: 1px;
        }
        .reset-btn:hover {
            background-color: #ff0055;
            color: #fff;
            box-shadow: 0 0 10px rgba(255, 0, 85, 0.5);
        }

        .search-box {
            width: 100%;
            display: flex;
            flex-direction: column;
            gap: 16px;
        }
        input[type="text"].search-input {
            width: 100%;
            padding: 16px 20px;
            font-size: 1.1rem;
            border: 2px solid #00ff66;
            border-radius: 6px;
            background-color: #0a0a0a;
            color: #00ff66;
            outline: none;
            box-shadow: inset 0 0 10px rgba(0, 255, 102, 0.1);
            transition: 0.2s;
        }
        input[type="text"].search-input:focus {
            box-shadow: 0 0 15px rgba(0, 255, 102, 0.4), inset 0 0 10px rgba(0, 255, 102, 0.2);
        }
        .action-btn {
            padding: 14px 20px;
            font-size: 1.1rem;
            font-weight: bold;
            background-color: #00ff66;
            color: #050505;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            transition: 0.2s;
            letter-spacing: 1px;
        }
        .action-btn:hover {
            background-color: #00cc52;
            box-shadow: 0 0 15px rgba(0, 255, 102, 0.6);
        }
        .result-container {
            margin-top: 5px;
            background-color: #0a0a0a;
            border: 1px solid #00ff66;
            border-radius: 6px;
            padding: 20px;
            display: none;
            flex-direction: column;
            gap: 12px;
            box-shadow: 0 0 15px rgba(0, 255, 102, 0.1);
        }
        .english-text-box {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background-color: #000000;
            padding: 12px 16px;
            border-radius: 4px;
            border: 1px solid #333;
        }
        .query-text {
            font-size: 1.2rem;
            color: #00ff66;
            text-shadow: 0 0 8px rgba(0, 255, 102, 0.5);
            user-select: all;
        }
        .copy-btn {
            padding: 8px 14px;
            background-color: #111;
            color: #00ff66;
            border: 1px solid #00ff66;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.9rem;
            font-weight: bold;
            transition: 0.2s;
        }
        .copy-btn:hover {
            background-color: #00ff66;
            color: #050505;
        }
        .guide-msg {
            color: #888;
            font-size: 0.9rem;
        }
        .site-section {
            margin-top: 15px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .section-title {
            font-size: 1rem;
            color: #ff0055;
            font-weight: bold;
            letter-spacing: 1px;
        }
        .site-buttons {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }
        .site-btn {
            padding: 12px 14px;
            font-size: 0.95rem;
            font-weight: bold;
            border: 1px solid #333;
            border-radius: 6px;
            cursor: pointer;
            transition: 0.2s;
            color: #ffffff;
            text-align: center;
            background-color: #111;
        }
        .site-btn:hover {
            border-color: #00ff66;
            color: #00ff66;
            box-shadow: 0 0 10px rgba(0, 255, 102, 0.3);
            transform: translateY(-2px);
        }
    </style>
</head>
<body>

    <!-- 라이선스 인증 창 -->
    <div id="licenseModal" class="license-modal">
        <div class="license-card">
            <h2>[ ACCESS RESTRICTED ]</h2>
            <p>SYSTEM SECURED. ENTER VALID LICENSE KEY TO PROCEED.</p>
            <input type="text" id="licenseInput" placeholder="ENTER_LICENSE_KEY">
            <button onclick="verifyLicense()">INITIALIZE_ACCESS</button>
        </div>
    </div>

    <!-- 메인 사이트 콘텐츠 -->
    <div id="mainContent" class="main-content">
        <div class="header-container">
            <h1>ROBLOX // SCRIPT_TERMINAL</h1>
            <button class="reset-btn" onclick="resetLogin()">[ [RESET_AUTH] 로그인 초기화 ]</button>
        </div>
        
        <div class="search-box">
            <input type="text" id="searchInput" class="search-input" placeholder="게임 이름 입력 (예: 포세이큰, Blox Fruits)" onkeypress="handleKeyPress(event)">
            <button class="action-btn" onclick="processSearch()">EXECUTE // TRANSLATE & SEARCH</button>
            
            <div id="resultContainer" class="result-container">
                <div class="english-text-box">
                    <span id="englishQueryText" class="query-text"></span>
                    <button class="copy-btn" onclick="copyText()">COPY</button>
                </div>
                <div class="guide-msg">> TARGET QUERY GENERATED. PASTE INTO TARGET REPOSITORY.</div>

                <div class="site-section">
                    <div class="section-title">// ACTIVE REPOSITORIES (13 TARGETS)</div>
                    <div class="site-buttons">
                        <button class="site-btn" onclick="goToSite('scriptblox')">ScriptBlox</button>
                        <button class="site-btn" onclick="goToSite('rscripts')">Rscripts</button>
                        <button class="site-btn" onclick="goToSite('robscript')">Robscript</button>
                        <button class="site-btn" onclick="goToSite('nokeyscript')">NoKeyScript</button>
                        <button class="site-btn" onclick="goToSite('pastebin')">Pastebin</button>
                        <button class="site-btn" onclick="goToSite('rbxscript')">RBXScript</button>
                        <button class="site-btn" onclick="goToSite('v3rmillion')">V3rmillion (Archive)</button>
                        <button class="site-btn" onclick="goToSite('robloxscripts')">RobloxScripts.com</button>
                        <button class="site-btn" onclick="goToSite('rbxvault')">RBXVault</button>
                        <button class="site-btn" onclick="goToSite('scripthub')">ScriptHub</button>
                        <button class="site-btn" onclick="goToSite('wearedevs')">WeAreDevs</button>
                        <button class="site-btn" onclick="goToSite('bloxsource')">BloxSource</button>
                        <button class="site-btn" onclick="goToSite('rbxhub')">RBX Hub</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // 허용된 5개의 라이선스 키
        const VALID_LICENSE_KEYS = [
            "op.oawpaiwanwjwiamwdo",
            "cyber.x992_alpha_key",
            "roblox.hack.node.01",
            "root_access_secure_77",
            "terminal.override.key"
        ];

        window.onload = function() {
            checkLicenseStatus();
        };

        function checkLicenseStatus() {
            const isActivated = localStorage.getItem("license_activated");

            if (isActivated === "true") {
                document.getElementById("licenseModal").style.display = "none";
                document.getElementById("mainContent").style.display = "flex";
            } else {
                document.getElementById("licenseModal").style.display = "flex";
                document.getElementById("mainContent").style.display = "none";
            }
        };

        function verifyLicense() {
            const inputKey = document.getElementById("licenseInput").value.trim();
            const isUsed = localStorage.getItem("license_key_used");

            if (isUsed === "true") {
                alert("[ERROR] 이 브라우저/기기에서는 이미 라이선스가 사용되었습니다. 재사용 불가!");
                return;
            }

            if (VALID_LICENSE_KEYS.includes(inputKey)) {
                localStorage.setItem("license_key_used", "true");
                localStorage.setItem("license_activated", "true");

                alert("[SUCCESS] 인증 완료. 시스템에 진입합니다.");
                document.getElementById("licenseModal").style.display = "none";
                document.getElementById("mainContent").style.display = "flex";
            } else {
                alert("[ACCESS DENIED] 유효하지 않은 라이선스 키입니다.");
            }
        }

        // 로그인 초기화 함수 (저장된 인증 정보 및 사용 기록을 모두 삭제)
        function resetLogin() {
            if (confirm("정말로 로그인 및 라이선스 사용 기록을 초기화하시겠습니까?")) {
                localStorage.removeItem("license_activated");
                localStorage.removeItem("license_key_used");
                alert("[SYSTEM] 인증 정보가 초기화되었습니다.");
                location.reload(); // 페이지 새로고침하여 인증창 재출력
            }
        }

        const gameTranslationMap = {
            "블록스열매": "Blox Fruits",
            "블록스 열매": "Blox Fruits",
            "입양하세요": "Adopt Me",
            "킹피스": "King Legacy",
            "킹 리거시": "King Legacy",
            "킹리거시": "King Legacy",
            "포세이큰": "Forsaken",
            "타워오브헬": "Tower of Hell",
            "타워 오브 헬": "Tower of Hell",
            "타오헬": "Tower of Hell",
            "도어즈": "DOORS",
            "문들": "DOORS",
            "베드워즈": "BedWars",
            "펫시뮬레이터": "Pet Simulator",
            "펫시뮬": "Pet Simulator",
            "살인마VS생존자": "Murder Mystery 2",
            "머더": "Murder Mystery 2",
            "머더미스테리": "Murder Mystery 2",
            "원피스": "Blox Fruits"
        };

        function handleKeyPress(event) {
            if (event.key === 'Enter') {
                processSearch();
            }
        }

        function hasKorean(text) {
            return /[ㄱ-ㅎ|ㅏ-ㅣ|가-힣]/.test(text);
        }

        async function processSearch() {
            let query = document.getElementById('searchInput').value.trim();
            const resultContainer = document.getElementById('resultContainer');
            const englishQueryText = document.getElementById('englishQueryText');

            if (!query) {
                alert('검색어를 입력하세요.');
                resultContainer.style.display = 'none';
                return;
            }

            resultContainer.style.display = 'flex';
            englishQueryText.textContent = 'TRANSLATING...';

            const lowerQuery = query.toLowerCase().replace(/\s+/g, '');
            let englishResult = '';

            if (gameTranslationMap[query]) {
                englishResult = gameTranslationMap[query];
            } else if (gameTranslationMap[lowerQuery]) {
                englishResult = gameTranslationMap[lowerQuery];
            } else if (hasKorean(query)) {
                try {
                    const res = await fetch(`https://api.mymemory.translated.net/get?q=${encodeURIComponent(query)}&langpair=ko|en`);
                    const data = await res.json();
                    if (data && data.responseData && data.responseData.translatedText) {
                        englishResult = data.responseData.translatedText;
                    } else {
                        englishResult = query;
                    }
                } catch (err) {
                    console.error("번역 에러:", err);
                    englishResult = query;
                }
            } else {
                englishResult = query;
            }

            englishQueryText.textContent = englishResult;
        }

        function copyText() {
            const text = document.getElementById('englishQueryText').textContent;
            if (text === 'TRANSLATING...') return;
            navigator.clipboard.writeText(text).then(() => {
                alert('[OK] 클립보드에 복사되었습니다.');
            }).catch(err => {
                alert('복사 실패: ' + err);
            });
        }

        function getQuery() {
            return document.getElementById('englishQueryText').textContent.trim();
        }

        function goToSite(site) {
            const query = getQuery();
            if (!query || query === 'TRANSLATING...') return;

            let url = '';
            if (site === 'scriptblox') {
                url = `https://scriptblox.com/?q=${encodeURIComponent(query)}`;
            } else if (site === 'rscripts') {
                url = `https://rscripts.net/scripts?q=${encodeURIComponent(query)}`;
            } else if (site === 'robscript') {
                url = `https://robscript.com/?q=${encodeURIComponent(query)}`;
            } else if (site === 'nokeyscript') {
                url = `https://nokeyscript.com/?s=${encodeURIComponent(query)}`;
            } else if (site === 'pastebin') {
                url = `https://pastebin.com/search?q=${encodeURIComponent('roblox ' + query + ' script')}`;
            } else if (site === 'rbxscript') {
                url = `https://rbxscript.com/?s=${encodeURIComponent(query)}`;
            } else if (site === 'v3rmillion') {
                url = `https://v3rm.net/search/1/?q=${encodeURIComponent(query)}`;
            } else if (site === 'robloxscripts') {
                url = `https://robloxscripts.com/?s=${encodeURIComponent(query)}`;
            } else if (site === 'rbxvault') {
                url = `https://rbxvault.com/?s=${encodeURIComponent(query)}`;
            } else if (site === 'scripthub') {
                url = `https://scripthub.org/?s=${encodeURIComponent(query)}`;
            } else if (site === 'wearedevs') {
                url = `https://wearedevs.net/scripts?q=${encodeURIComponent(query)}`;
            } else if (site === 'bloxsource') {
                url = `https://bloxsource.com/?s=${encodeURIComponent(query)}`;
            } else if (site === 'rbxhub') {
                url = `https://rbxhub.com/?s=${encodeURIComponent(query)}`;
            }

            if (url) {
                window.open(url, '_blank');
            }
        }
    </script>
</body>
</html>
