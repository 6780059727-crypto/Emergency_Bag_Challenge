<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>緊急時バッグパッキングゲーム | Emergency Bag Packing Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 50%, #fecfef 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(45deg, #ff6b6b, #ff8e8e);
            color: white;
            text-align: center;
            padding: 20px;
            position: relative;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
        }

        .game-controls {
            display: flex;
            justify-content: center;
            gap: 20px;
            padding: 20px;
            background: #f8f9fa;
            border-bottom: 3px solid #dee2e6;
        }

        .difficulty-btn {
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            font-size: 1.1em;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: bold;
            text-transform: uppercase;
        }

        .easy { background: #4CAF50; color: white; }
        .medium { background: #FF9800; color: white; }
        .hard { background: #F44336; color: white; }

        .difficulty-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }

        .game-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px;
            background: #e3f2fd;
            border-bottom: 3px solid #2196f3;
        }

        .timer, .score {
            font-size: 1.5em;
            font-weight: bold;
            padding: 10px 20px;
            border-radius: 15px;
            background: white;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
        }

        .timer {
            color: #ff5722;
        }

        .score {
            color: #4caf50;
        }

        .game-area {
            display: grid;
            grid-template-columns: 1fr 300px;
            gap: 20px;
            padding: 20px;
            min-height: 500px;
        }

        .items-section {
            background: #fff3e0;
            border-radius: 15px;
            padding: 20px;
            border: 3px dashed #ff9800;
        }

        .items-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .item {
            width: 100px;
            height: 120px;
            border-radius: 15px;
            cursor: grab;
            transition: all 0.3s ease;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background: white;
            border: 3px solid #e0e0e0;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
            padding: 5px;
        }

        .item-emoji {
            font-size: 2.5em;
            margin-bottom: 5px;
        }

        .item-label {
            font-size: 0.7em;
            font-weight: bold;
            color: #333;
            text-align: center;
            line-height: 1.2;
        }

        .item:hover {
            transform: scale(1.1);
            box-shadow: 0 5px 20px rgba(0,0,0,0.2);
        }

        .item.dragging {
            opacity: 0.5;
            transform: rotate(5deg);
        }

        .bag-section {
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .backpack {
            width: 280px;
            height: 380px;
            background: linear-gradient(145deg, #2196f3, #1976d2);
            border-radius: 30px 30px 20px 20px;
            position: relative;
            border: 4px solid #0d47a1;
            box-shadow: 0 15px 40px rgba(0,0,0,0.3);
            margin: 0 auto;
        }

        /* Top handle */
        .backpack::before {
            content: '';
            position: absolute;
            top: -25px;
            left: 50%;
            transform: translateX(-50%);
            width: 120px;
            height: 25px;
            background: #0d47a1;
            border-radius: 15px 15px 5px 5px;
            border: 3px solid #424242;
        }

        /* Front pocket */
        .backpack::after {
            content: '';
            position: absolute;
            top: 30px;
            left: 50%;
            transform: translateX(-50%);
            width: 150px;
            height: 80px;
            background: rgba(13, 71, 161, 0.3);
            border-radius: 15px;
            border: 2px solid rgba(255,255,255,0.2);
        }

        /* Left strap */
        .backpack-strap-left {
            position: absolute;
            left: -15px;
            top: 40px;
            width: 20px;
            height: 200px;
            background: linear-gradient(to bottom, #424242, #616161);
            border-radius: 10px;
            box-shadow: inset 0 0 10px rgba(0,0,0,0.3);
            z-index: -1;
        }

        /* Right strap */
        .backpack-strap-right {
            position: absolute;
            right: -15px;
            top: 40px;
            width: 20px;
            height: 200px;
            background: linear-gradient(to bottom, #424242, #616161);
            border-radius: 10px;
            box-shadow: inset 0 0 10px rgba(0,0,0,0.3);
            z-index: -1;
        }

        /* Zipper */
        .backpack-zipper {
            position: absolute;
            top: 120px;
            left: 50%;
            transform: translateX(-50%);
            width: 80%;
            height: 3px;
            background: #ffc107;
            border-radius: 2px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.3);
        }

        .backpack-zipper::after {
            content: '';
            position: absolute;
            right: -8px;
            top: -4px;
            width: 10px;
            height: 10px;
            background: #ff9800;
            border-radius: 2px;
            border: 1px solid #e65100;
        }

        .bag-interior {
            width: 85%;
            height: 75%;
            margin: 15% auto 0;
            background: rgba(255,255,255,0.15);
            border-radius: 20px;
            border: 3px dashed rgba(255,255,255,0.6);
            display: flex;
            flex-wrap: wrap;
            align-content: flex-start;
            padding: 15px;
            gap: 8px;
            min-height: 220px;
            position: relative;
            z-index: 10;
        }

        .bag-interior.drag-over {
            background: rgba(255,255,0,0.3);
            border-color: #ffeb3b;
        }

        .bag-item {
            width: 45px;
            height: 45px;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: white;
            font-size: 1.8em;
            border: 2px solid #ccc;
            box-shadow: 0 3px 8px rgba(0,0,0,0.15);
            margin: 2px;
        }

        .vocabulary-section {
            background: #f3e5f5;
            border-top: 5px solid #9c27b0;
            padding: 20px;
        }

        .vocab-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .vocab-card {
            background: white;
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
            text-align: center;
            border: 3px solid #e1bee7;
            transition: all 0.3s ease;
        }

        .vocab-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 20px rgba(0,0,0,0.2);
        }

        .vocab-japanese {
            font-size: 1.5em;
            font-weight: bold;
            color: #4a148c;
            margin-bottom: 5px;
        }

        .vocab-reading {
            font-size: 1em;
            color: #7b1fa2;
            margin-bottom: 5px;
        }

        .vocab-english {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 10px;
        }

        .play-button {
            background: #ff4081;
            color: white;
            border: none;
            border-radius: 20px;
            padding: 8px 16px;
            cursor: pointer;
            font-size: 0.9em;
            transition: all 0.3s ease;
        }

        .play-button:hover {
            background: #f50057;
            transform: scale(1.1);
        }

        .game-over {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }

        .feedback-message {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 25px;
            border-radius: 10px;
            font-weight: bold;
            font-size: 1em;
            z-index: 2000;
            transform: translateX(400px);
            transition: transform 0.3s ease;
            max-width: 300px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }

        .feedback-message.show {
            transform: translateX(0);
        }

        .feedback-message.success {
            background: #4caf50;
            color: white;
            border-left: 5px solid #388e3c;
        }

        .feedback-message.error {
            background: #f44336;
            color: white;
            border-left: 5px solid #d32f2f;
        }

        .game-over-content {
            background: white;
            padding: 40px;
            border-radius: 20px;
            text-align: center;
            max-width: 500px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            max-height: 80vh;
            overflow-y: auto;
        }

        .stats-section {
            background: #f5f5f5;
            border-radius: 15px;
            padding: 20px;
            margin: 20px 0;
            text-align: left;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin: 15px 0;
        }

        .stat-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 10px;
            background: white;
            border-radius: 8px;
            border-left: 4px solid #2196f3;
        }

        .stat-value {
            font-weight: bold;
            font-size: 1.2em;
            color: #1976d2;
        }

        .ai-feedback {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 15px;
            margin: 20px 0;
            text-align: left;
            position: relative;
        }

        .ai-feedback::before {
            content: '🤖';
            position: absolute;
            top: -10px;
            left: 20px;
            background: inherit;
            padding: 10px;
            border-radius: 50%;
            font-size: 1.5em;
        }

        .ai-feedback h3 {
            margin-top: 0;
            margin-bottom: 15px;
            color: #fff;
        }

        .missed-items {
            background: #ffebee;
            border-radius: 10px;
            padding: 15px;
            margin: 10px 0;
        }

        .missed-item {
            display: flex;
            align-items: center;
            margin: 8px 0;
            padding: 8px;
            background: white;
            border-radius: 6px;
            border-left: 3px solid #f44336;
        }

        .performance-badge {
            display: inline-block;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: bold;
            margin: 5px;
        }

        .excellent { background: #4caf50; color: white; }
        .good { background: #8bc34a; color: white; }
        .fair { background: #ff9800; color: white; }
        .needs-improvement { background: #f44336; color: white; }

        .game-over h2 {
            color: #ff5722;
            font-size: 2.5em;
            margin-bottom: 20px;
        }

        .game-over p {
            font-size: 1.2em;
            color: #666;
            margin-bottom: 30px;
        }

        .restart-btn {
            background: #4caf50;
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            font-size: 1.1em;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .restart-btn:hover {
            background: #45a049;
            transform: translateY(-2px);
        }

        @keyframes bounce {
            0%, 20%, 53%, 80%, 100% { transform: translate3d(0,0,0); }
            40%, 43% { transform: translate3d(0,-30px,0); }
            70% { transform: translate3d(0,-15px,0); }
            90% { transform: translate3d(0,-4px,0); }
        }

        .bounce {
            animation: bounce 1s ease infinite;
        }

        .correct {
            border-color: #4caf50 !important;
            background: #e8f5e8 !important;
        }

        .incorrect {
            border-color: #f44336 !important;
            background: #ffebee !important;
        }

        @media (max-width: 768px) {
            .game-area {
                grid-template-columns: 1fr;
                gap: 15px;
            }
            
            .items-grid {
                grid-template-columns: repeat(auto-fill, minmax(90px, 1fr));
            }
            
            .item {
                width: 90px;
                height: 110px;
                font-size: 0.8em;
            }
            
            .item-emoji {
                font-size: 2.2em;
            }
            
            .item-label {
                font-size: 0.65em;
            }
            
            .backpack {
                width: 250px;
                height: 320px;
            }
            
            .bag-interior {
                height: 70%;
                margin: 18% auto 0;
            }
            
            .vocab-grid {
                grid-template-columns: 1fr;
            }
            
            .feedback-message {
                top: 10px;
                right: 10px;
                left: 10px;
                max-width: none;
                transform: translateY(-100px);
                font-size: 0.9em;
            }
            
            .feedback-message.show {
                transform: translateY(0);
            }
            
            .game-over-content {
                padding: 20px;
                margin: 10px;
                max-width: 90vw;
                max-height: 90vh;
            }
            
            .stats-grid {
                grid-template-columns: 1fr;
                gap: 10px;
            }
            
            .ai-feedback {
                padding: 15px;
                font-size: 0.9em;
            }
            
            .game-controls {
                flex-direction: column;
                gap: 10px;
            }
            
            .difficulty-btn {
                padding: 15px 20px;
                font-size: 1em;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header class="header">
            <h1>🎒 緊急時バッグゲーム</h1>
            <p>災害に備えて正しいアイテムを選ぼう！</p>
        </header>

        <div class="game-controls">
            <button class="difficulty-btn easy" onclick="startGame('easy')">
                かんたん<br>60秒
            </button>
            <button class="difficulty-btn medium" onclick="startGame('medium')">
                ふつう<br>30秒
            </button>
            <button class="difficulty-btn hard" onclick="startGame('hard')">
                むずかしい<br>20秒
            </button>
        </div>

        <div class="game-info">
            <div class="timer">
                時間: <span id="timeLeft">--</span>秒
            </div>
            <div class="score">
                スコア: <span id="score">0</span>
            </div>
        </div>

        <div class="game-area">
            <div class="items-section">
                <h3>🛍️ アイテムを選んでね</h3>
                <div class="items-grid" id="itemsGrid">
                    <!-- Items will be populated by JavaScript -->
                </div>
            </div>

            <div class="bag-section">
                <h3>🎒 緊急バッグ</h3>
                <div class="backpack">
                    <div class="backpack-strap-left"></div>
                    <div class="backpack-strap-right"></div>
                    <div class="backpack-zipper"></div>
                    <div class="bag-interior" id="bagInterior"></div>
                </div>
            </div>
        </div>

        <div class="vocabulary-section">
            <h3>📚 ことば辞典</h3>
            <div class="vocab-grid" id="vocabGrid">
                <!-- Vocabulary cards will be populated by JavaScript -->
            </div>
        </div>
    </div>

    <div class="game-over" id="gameOver">
        <div class="game-over-content">
            <h2>ゲーム終了！</h2>
            <p id="finalScore"></p>
            <p id="resultMessage"></p>
            <button class="restart-btn" onclick="location.reload()">เล่นใหม่</button>
        </div>
    </div>

    <script>
        // Game data
        const emergencyItems = [
            { emoji: '💊', japanese: '薬', reading: 'くすり', english: 'Medicine', essential: true },
            { emoji: '🔦', japanese: '懐中電灯', reading: 'かいちゅうでんとう', english: 'Flashlight', essential: true },
            { emoji: '🥤', japanese: '水', reading: 'みず', english: 'Water', essential: true },
            { emoji: '🍞', japanese: 'パン', reading: 'パン', english: 'Bread', essential: true },
            { emoji: '🩹', japanese: '絆創膏', reading: 'ばんそうこう', english: 'Band-aid', essential: true },
            { emoji: '🔋', japanese: '電池', reading: 'でんち', english: 'Battery', essential: true },
            { emoji: '📱', japanese: '携帯電話', reading: 'けいたいでんわ', english: 'Mobile phone', essential: true },
            { emoji: '🧥', japanese: '服', reading: 'ふく', english: 'Clothes', essential: true },
            { emoji: '💰', japanese: 'お金', reading: 'おかね', english: 'Money', essential: true },
            { emoji: '📄', japanese: '書類', reading: 'しょるい', english: 'Documents', essential: true },
            // Non-essential items
            { emoji: '🎮', japanese: 'ゲーム', reading: 'ゲーム', english: 'Game', essential: false },
            { emoji: '🍰', japanese: 'ケーキ', reading: 'ケーキ', english: 'Cake', essential: false },
            { emoji: '👠', japanese: 'ハイヒール', reading: 'ハイヒール', english: 'High heels', essential: false },
            { emoji: '🎸', japanese: 'ギター', reading: 'ギター', english: 'Guitar', essential: false },
            { emoji: '🧸', japanese: 'ぬいぐるみ', reading: 'ぬいぐるみ', english: 'Stuffed animal', essential: false },
            { emoji: '💄', japanese: '口紅', reading: 'くちべに', english: 'Lipstick', essential: false },
            { emoji: '🎭', japanese: 'マスク', reading: 'マスク', english: 'Theater mask', essential: false },
            { emoji: '🏀', japanese: 'ボール', reading: 'ボール', english: 'Ball', essential: false }
        ];

        let gameTimer;
        let timeLeft;
        let score = 0;
        let gameActive = false;
        let currentDifficulty;
        let gameStats = {
            correctItems: 0,
            incorrectItems: 0,
            totalItems: 0,
            timeUsed: 0,
            missedEssentials: [],
            incorrectChoices: []
        };

        // Initialize game
        function initGame() {
            createItemsGrid();
            createVocabularyCards();
            setupDragAndDrop();
        }

        function createItemsGrid() {
            const grid = document.getElementById('itemsGrid');
            grid.innerHTML = '';
            
            // Shuffle items
            const shuffledItems = [...emergencyItems].sort(() => Math.random() - 0.5);
            
            shuffledItems.forEach((item, index) => {
                const itemEl = document.createElement('div');
                itemEl.className = 'item';
                itemEl.innerHTML = `
                    <div class="item-emoji">${item.emoji}</div>
                    <div class="item-label">${item.japanese}<br><small>${item.reading}</small></div>
                `;
                itemEl.draggable = true;
                itemEl.dataset.essential = item.essential;
                itemEl.dataset.japanese = item.japanese;
                itemEl.dataset.emoji = item.emoji;
                itemEl.id = `item-${index}`;
                grid.appendChild(itemEl);
            });
        }

        function createVocabularyCards() {
            const grid = document.getElementById('vocabGrid');
            grid.innerHTML = '';

            emergencyItems.forEach(item => {
                const card = document.createElement('div');
                card.className = 'vocab-card';
                card.innerHTML = `
                    <div class="vocab-japanese">${item.emoji} ${item.japanese}</div>
                    <div class="vocab-reading">${item.reading}</div>
                    <div class="vocab-english">${item.english}</div>
                    <button class="play-button" onclick="speakWord('${item.japanese}', '${item.reading}')">
                        🔊 音声
                    </button>
                `;
                grid.appendChild(card);
            });
        }

        function speakWord(japanese, reading) {
            if ('speechSynthesis' in window) {
                const utterance = new SpeechSynthesisUtterance(japanese);
                utterance.lang = 'ja-JP';
                utterance.rate = 0.8;
                speechSynthesis.speak(utterance);
            } else {
                alert(`発音: ${reading}`);
            }
        }

        function setupDragAndDrop() {
            const items = document.querySelectorAll('.item');
            const bagInterior = document.getElementById('bagInterior');

            items.forEach(item => {
                item.addEventListener('dragstart', handleDragStart);
                item.addEventListener('dragend', handleDragEnd);
                
                // Touch support for mobile
                item.addEventListener('touchstart', handleTouchStart, {passive: false});
                item.addEventListener('touchmove', handleTouchMove, {passive: false});
                item.addEventListener('touchend', handleTouchEnd, {passive: false});
            });

            bagInterior.addEventListener('dragover', handleDragOver);
            bagInterior.addEventListener('drop', handleDrop);
            bagInterior.addEventListener('dragenter', handleDragEnter);
            bagInterior.addEventListener('dragleave', handleDragLeave);
        }

        // Touch handling for mobile
        let draggedElement = null;

        function handleTouchStart(e) {
            if (!gameActive) return;
            draggedElement = e.target.closest('.item');
            if (draggedElement) {
                draggedElement.classList.add('dragging');
            }
        }

        function handleTouchMove(e) {
            e.preventDefault();
            if (!draggedElement) return;
            
            const touch = e.touches[0];
            const elementBelow = document.elementFromPoint(touch.clientX, touch.clientY);
            
            if (elementBelow && (elementBelow.id === 'bagInterior' || elementBelow.closest('#bagInterior'))) {
                document.getElementById('bagInterior').classList.add('drag-over');
            } else {
                document.getElementById('bagInterior').classList.remove('drag-over');
            }
        }

        function handleTouchEnd(e) {
            if (!draggedElement) return;
            
            const touch = e.changedTouches[0];
            const elementBelow = document.elementFromPoint(touch.clientX, touch.clientY);
            const bagInterior = document.getElementById('bagInterior');
            
            bagInterior.classList.remove('drag-over');
            
            if (elementBelow && (elementBelow.id === 'bagInterior' || elementBelow.closest('#bagInterior'))) {
                // Simulate drop event
                const isEssential = draggedElement.dataset.essential === 'true';
                const emoji = draggedElement.dataset.emoji;
                const japanese = draggedElement.dataset.japanese;
                
                const bagItem = document.createElement('div');
                bagItem.className = 'bag-item';
                bagItem.innerHTML = emoji;
                
                // Update statistics
                gameStats.totalItems++;
                
                if (isEssential) {
                    bagItem.classList.add('correct');
                    score += 10;
                    gameStats.correctItems++;
                    bagItem.classList.add('bounce');
                    setTimeout(() => bagItem.classList.remove('bounce'), 1000);
                    showFeedbackMessage(`✅ ถูกต้อง! ${japanese} จำเป็นในยามฉุกเฉิน`, 'success');
                } else {
                    bagItem.classList.add('incorrect');
                    score -= 5;
                    gameStats.incorrectItems++;
                    gameStats.incorrectChoices.push({
                        item: japanese,
                        emoji: emoji,
                        reason: getIncorrectReasonThai(japanese)
                    });
                    showFeedbackMessage(`❌ ${japanese} ไม่จำเป็นในยามฉุกเฉิน ${getIncorrectReasonThai(japanese)}`, 'error');
                }
                
                bagInterior.appendChild(bagItem);
                draggedElement.remove();
                
                document.getElementById('score').textContent = score;
                checkWinCondition();
            }
            
            if (draggedElement) {
                draggedElement.classList.remove('dragging');
            }
            draggedElement = null;
        }

        function handleDragStart(e) {
            if (!gameActive) return;
            e.dataTransfer.setData('text/plain', e.target.id);
            e.target.classList.add('dragging');
        }

        function handleDragEnd(e) {
            e.target.classList.remove('dragging');
        }

        function handleDragOver(e) {
            e.preventDefault();
        }

        function handleDragEnter(e) {
            e.preventDefault();
            const bagInterior = document.getElementById('bagInterior');
            if (e.target === bagInterior || e.target.closest('#bagInterior')) {
                bagInterior.classList.add('drag-over');
            }
        }

        function handleDragLeave(e) {
            const bagInterior = document.getElementById('bagInterior');
            if (!bagInterior.contains(e.relatedTarget)) {
                bagInterior.classList.remove('drag-over');
            }
        }

        function handleDrop(e) {
            e.preventDefault();
            e.target.classList.remove('drag-over');
            
            if (!gameActive) return;

            const itemId = e.dataTransfer.getData('text/plain');
            const item = document.getElementById(itemId);
            
            // Make sure we're dropping on the bag interior
            const bagInterior = document.getElementById('bagInterior');
            const dropTarget = e.target.id === 'bagInterior' ? e.target : 
                             e.target.closest('#bagInterior');
            
            if (item && dropTarget) {
                const isEssential = item.dataset.essential === 'true';
                const emoji = item.dataset.emoji;
                const japanese = item.dataset.japanese;
                
                const bagItem = document.createElement('div');
                bagItem.className = 'bag-item';
                bagItem.innerHTML = emoji;
                
                // Update statistics
                gameStats.totalItems++;
                
                if (isEssential) {
                    bagItem.classList.add('correct');
                    score += 10;
                    gameStats.correctItems++;
                    
                    // Add bounce animation for correct items
                    bagItem.classList.add('bounce');
                    setTimeout(() => bagItem.classList.remove('bounce'), 1000);
                    
                    // Show success message in Thai
                    showFeedbackMessage(`✅ ถูกต้อง! ${japanese} จำเป็นในยาวโชุกเฉิน`, 'success');
                } else {
                    bagItem.classList.add('incorrect');
                    score -= 5;
                    gameStats.incorrectItems++;
                    gameStats.incorrectChoices.push({
                        item: japanese,
                        emoji: emoji,
                        reason: getIncorrectReasonThai(japanese)
                    });
                    
                    // Show incorrect message with explanation in Thai
                    showFeedbackMessage(`❌ ${japanese} ไม่จำเป็นในยามฉุกเฉิน ${getIncorrectReasonThai(japanese)}`, 'error');
                }
                
                dropTarget.appendChild(bagItem);
                item.remove();
                
                document.getElementById('score').textContent = score;
                
                // Check if all essential items are packed
                checkWinCondition();
            }
        }

        function getIncorrectReasonThai(itemName) {
            const reasons = {
                'ゲーム': 'ในยามฉุกเฉินต้องให้ความสำคัญกับความปลอดภัยก่อน',
                'ケーキ': 'อาหารที่เน่าเสียง่ายไม่เหมาะในยามฉุกเฉิน',
                'ハイヒール': 'ต้องใส่รองเท้าที่เดินง่ายเมื่อหนีภัย',
                'ギター': 'หนักและใหญ่ไม่เหมาะสมในยามฉุกเฉิน',
                'ぬいぐるみ': 'ใช้พื้นที่มากและมีความสำคัญน้อย',
                '口紅': 'ในยามฉุกเฉินความปลอดภัยสำคัญกว่าการแต่งตัว',
                'マスク': 'หน้ากากละครไม่จำเป็นในยามฉุกเฉิน',
                'ボール': 'ควรให้ความสำคัญกับของใช้จำเป็นก่อน'
            };
            return reasons[itemName] || 'ในยามฉุกเฉินควรให้ความสำคัญกับสิ่งอื่นก่อน';
        }

        function showFeedbackMessage(message, type) {
            const feedback = document.createElement('div');
            feedback.className = `feedback-message ${type}`;
            feedback.textContent = message;
            document.body.appendChild(feedback);
            
            setTimeout(() => {
                feedback.classList.add('show');
            }, 100);
            
            setTimeout(() => {
                feedback.remove();
            }, 3000);
        }

        function startGame(difficulty) {
            currentDifficulty = difficulty;
            gameActive = true;
            score = 0;
            
            // Reset statistics
            gameStats = {
                correctItems: 0,
                incorrectItems: 0,
                totalItems: 0,
                timeUsed: 0,
                startTime: Date.now(),
                missedEssentials: [],
                incorrectChoices: []
            };
            
            // Set timer based on difficulty
            switch(difficulty) {
                case 'easy': timeLeft = 60; break;
                case 'medium': timeLeft = 30; break;
                case 'hard': timeLeft = 20; break;
            }
            
            gameStats.initialTime = timeLeft;
            
            document.getElementById('score').textContent = score;
            document.getElementById('timeLeft').textContent = timeLeft;
            
            // Reset game
            const bagInterior = document.getElementById('bagInterior');
            bagInterior.innerHTML = '';
            bagInterior.classList.remove('drag-over');
            
            createItemsGrid();
            setupDragAndDrop();
            
            // Start timer
            gameTimer = setInterval(() => {
                timeLeft--;
                document.getElementById('timeLeft').textContent = timeLeft;
                
                if (timeLeft <= 10 && timeLeft > 0) {
                    // Add urgency effect for last 10 seconds
                    document.getElementById('timeLeft').style.color = '#ff1744';
                    document.getElementById('timeLeft').style.fontWeight = 'bold';
                }
                
                if (timeLeft <= 0) {
                    endGame();
                }
            }, 1000);
        }

        function checkWinCondition() {
            const essentialItems = emergencyItems.filter(item => item.essential);
            const bagItems = document.querySelectorAll('.bag-item.correct');
            
            // Check if all essential items are packed (allowing for some mistakes)
            if (bagItems.length >= essentialItems.length) {
                endGame(true);
            }
            
            // Check if all items are processed (win or lose)
            const remainingItems = document.querySelectorAll('.item');
            if (remainingItems.length === 0) {
                // Game finished with all items processed
                setTimeout(() => {
                    if (gameActive) { // Only end if still active
                        endGame(bagItems.length >= essentialItems.length);
                    }
                }, 500);
            }
        }

        function endGame(won = false) {
            gameActive = false;
            clearInterval(gameTimer);
            
            // Calculate final statistics
            gameStats.timeUsed = gameStats.initialTime - timeLeft;
            gameStats.missedEssentials = findMissedEssentials();
            
            const gameOver = document.getElementById('gameOver');
            const finalScore = document.getElementById('finalScore');
            const resultMessage = document.getElementById('resultMessage');
            
            // Create comprehensive results display
            const resultsHTML = createGameResults(won);
            
            finalScore.innerHTML = resultsHTML;
            
            if (won) {
                resultMessage.innerHTML = '🎉 <strong>ยินดีด้วย!</strong><br>เก็บของจำเป็นครบทุกอย่างแล้ว!';
            } else {
                resultMessage.innerHTML = '⏰ <strong>หมดเวลาแล้ว!</strong><br>ลองเล่นใหม่อีกครั้งนะ!';
            }
            
            gameOver.style.display = 'flex';
        }

        function findMissedEssentials() {
            const essentialItems = emergencyItems.filter(item => item.essential);
            const packedCorrectItems = document.querySelectorAll('.bag-item.correct');
            const packedEmojis = Array.from(packedCorrectItems).map(item => item.textContent);
            
            return essentialItems.filter(item => !packedEmojis.includes(item.emoji));
        }

        function createGameResults(won) {
            const accuracy = gameStats.totalItems > 0 ? 
                Math.round((gameStats.correctItems / gameStats.totalItems) * 100) : 0;
            
            const essentialCount = emergencyItems.filter(item => item.essential).length;
            const completionRate = Math.round((gameStats.correctItems / essentialCount) * 100);
            
            return `
                <div class="stats-section">
                    <h3>📊 ผลการเล่น</h3>
                    <div class="stats-grid">
                        <div class="stat-item">
                            <span>คะแนนรวม</span>
                            <span class="stat-value">${score} คะแนน</span>
                        </div>
                        <div class="stat-item">
                            <span>เวลาที่ใช้</span>
                            <span class="stat-value">${gameStats.timeUsed} วินาทีี</span>
                        </div>
                        <div class="stat-item">
                            <span>เลือกถูก</span>
                            <span class="stat-value">${gameStats.correctItems} ชิ้น</span>
                        </div>
                        <div class="stat-item">
                            <span>เลือกผิด</span>
                            <span class="stat-value">${gameStats.incorrectItems} ชิ้น</span>
                        </div>
                        <div class="stat-item">
                            <span>ความแม่นยำ</span>
                            <span class="stat-value">${accuracy}%</span>
                        </div>
                        <div class="stat-item">
                            <span>ความสำเร็จ</span>
                            <span class="stat-value">${completionRate}%</span>
                        </div>
                    </div>
                    ${getPerformanceBadgeThai(accuracy, completionRate)}
                </div>
                
                ${createMissedItemsSectionThai()}
                ${createIncorrectChoicesSectionThai()}
                ${createAIFeedbackThai(won, accuracy, completionRate)}
            `;
        }

        function getPerformanceBadgeThai(accuracy, completion) {
            let badge = '';
            if (accuracy >= 90 && completion >= 90) {
                badge = '<div class="performance-badge excellent">🏆 ยอดเยี่ยม!</div>';
            } else if (accuracy >= 75 && completion >= 75) {
                badge = '<div class="performance-badge good">👍 ดีมาก</div>';
            } else if (accuracy >= 50 && completion >= 50) {
                badge = '<div class="performance-badge fair">📈 พอใช้</div>';
            } else {
                badge = '<div class="performance-badge needs-improvement">💪 ต้องฝึกเพิ่ม</div>';
            }
            return badge;
        }

        function createMissedItemsSectionThai() {
            if (gameStats.missedEssentials.length === 0) return '';
            
            let html = `
                <div class="missed-items">
                    <h4>⚠️ ของสำคัญที่ยังขาด</h4>
                    <p>สิ่งของเหล่านี้จำเป็นในยามฉุกเฉิน:</p>
            `;
            
            gameStats.missedEssentials.forEach(item => {
                html += `
                    <div class="missed-item">
                        <span style="font-size: 1.5em; margin-right: 10px;">${item.emoji}</span>
                        <div>
                            <strong>${item.japanese}</strong> (${item.reading})<br>
                            <small style="color: #666;">${item.english}</small>
                        </div>
                    </div>
                `;
            });
            
            html += '</div>';
            return html;
        }

        function createIncorrectChoicesSectionThai() {
            if (gameStats.incorrectChoices.length === 0) return '';
            
            let html = `
                <div class="missed-items">
                    <h4>❌ ของที่เลือกผิด</h4>
                    <p>สิ่งของเหล่านี้ไม่จำเป็นในยามฉุกเฉิน:</p>
            `;
            
            gameStats.incorrectChoices.forEach(choice => {
                html += `
                    <div class="missed-item">
                        <span style="font-size: 1.5em; margin-right: 10px;">${choice.emoji}</span>
                        <div>
                            <strong>${choice.item}</strong><br>
                            <small style="color: #666;">${choice.reason}</small>
                        </div>
                    </div>
                `;
            });
            
            html += '</div>';
            return html;
        }

        function createAIFeedbackThai(won, accuracy, completion) {
            let feedback = '';
            let tips = [];
            
            // AI Assessment based on performance - tailored for Thai middle school students learning Japanese
            if (accuracy >= 90 && completion >= 90) {
                feedback = 'ยอดเยี่ยมมาก! นักเรียนเข้าใจเรื่องการเตรียมความพร้อมภัยพิบัติเป็นอย่างดี และยังจำคำศัพท์ภาษาญี่ปุ่นได้แม่นยำอีกด้วย';
                tips.push('ลองฝึกใช้คำศัพท์เหล่านี้ในประโยคภาษาญี่ปุ่นดูนะ');
                tips.push('แนะนำให้เตรียมกระเป๋าฉุกเฉินจริงที่บ้าน และคุยกับครอบครัวเรื่องแผนหนีภัย');
                tips.push('ลองเล่นในโหมดยากเพื่อฝึกความเร็วในการตัดสินใจ');
            } else if (accuracy >= 75) {
                feedback = 'ดีมาก! นักเรียนมีความเข้าใจพื้นฐานที่ดี แต่ยังมีจุดที่ปรับปรุงได้';
                if (gameStats.missedEssentials.length > 0) {
                    tips.push('จำไว้นะว่า น้ำ (みず) อาหาร (パン) และไฟฉาย (かいちゅうでんとう) เป็นของสำคัญที่สุด');
                    tips.push('ลองทำบัตรคำศัพท์ (flashcard) เพื่อจำคำเหล่านี้ให้ได้');
                }
                if (gameStats.incorrectItems > 0) {
                    tips.push('ในยามฉุกเฉิน เราต้องเลือกของที่เบาและใช้ประโยชน์ได้จริง');
                    tips.push('ลองนึกภาพว่าถ้าต้องแบกกระเป๋าวิ่งหนีภัย ของไหนที่จำเป็นที่สุด');
                }
            } else if (accuracy >= 50) {
                feedback = 'เข้าใจพื้นฐานแล้ว แต่ต้องฝึกเพิ่มเติมนะ ทั้งเรื่องภาษาญี่ปุ่นและการเตรียมความพร้อมภัยพิบัติ';
                tips.push('เริ่มจำ 4 สิ่งพื้นฐาน: น้ำ (みず) อาหาร (パン) ไฟฉาย (かいちゅうでんとう) และยา (くすり)');
                tips.push('ในยามฉุกเฉิน สิ่งที่ช่วยชีวิตได้เป็นสิ่งสำคัญที่สุด');
                tips.push('ลองอ่านข่าวเกี่ยวกับภัยพิบัติเป็นภาษาญี่ปุ่นระดับง่ายๆ');
            } else {
                feedback = 'ยังต้องเรียนรู้เพิ่มเติมนะ แต่ไม่เป็นไร เริ่มจากพื้นฐานกันก่อน';
                tips.push('เรียนรู้คำศัพท์ 3 กลุ่มพื้นฐาน: อาหาร (たべもの) เครื่องใช้ (どうぐ) และเสื้อผ้า (ふく)');
                tips.push('ในยามฉุกเฉิน สิ่งที่สำคัญคือ ความปลอดภัย อาหาร และที่พักพิง');
                tips.push('ลองดูภาพสื่อการสอนเกี่ยวกับภัยพิบัติในญี่ปุ่น เพื่อเข้าใจวัฒนธรรมการเตรียมความพร้อม');
            }

            // Additional tips based on time and difficulty
            if (gameStats.timeUsed < gameStats.initialTime / 3) {
                tips.push('เร็วมาก! การตัดสินใจเร็วแบบนี้ดีมากสำหรับสถานการณ์ฉุกเฉิน');
            } else if (gameStats.timeUsed > gameStats.initialTime * 0.8) {
                tips.push('ลองฝึกอ่านคำภาษาญี่ปุ่นให้เร็วขึ้น จะช่วยให้ตัดสินใจเร็วขึ้น');
            }

            if (currentDifficulty === 'hard' && accuracy >= 70) {
                tips.push('เล่นในระดับยากได้ดีแล้ว! แสดงว่าพร้อมเรียนคำศัพท์ JLPT N4 ต่อแล้ว');
            }

            // Add Japanese study tips since students are N5 level
            if (accuracy >= 80) {
                tips.push('ลองศึกษาคำศัพท์ภาษาญี่ปุ่นเกี่ยวกับ "ぼうさい (การป้องกันภัย)" เพิ่มเติม');
            }

            tips.push('💡 เทคนิค: ลองใช้คำศัพท์ที่เรียนในเกมนี้เขียนประโยคภาษาญี่ปุ่นดูนะ');

            return `
                <div class="ai-feedback">
                    <h3>🤖 ความเห็นจากครูผู้สอน AI</h3>
                    <p><strong>ประเมินผลรวม:</strong> ${feedback}</p>
                    <div style="margin-top: 15px;">
                        <strong>คำแนะนำสำหรับการพัฒนา:</strong>
                        <ul style="text-align: left; margin: 10px 0; padding-left: 20px;">
                            ${tips.map(tip => `<li style="margin: 8px 0;">${tip}</li>`).join('')}
                        </ul>
                    </div>
                    <div style="margin-top: 15px; padding: 10px; background: rgba(255,255,255,0.1); border-radius: 8px;">
                        <strong>🎌 เรียนรู้เพิ่มเติม:</strong><br>
                        ในญี่ปุ่น การเตรียมความพร้อมภัยพิบัติเรียกว่า "防災 (ぼうさい)" เป็นเรื่องสำคัญมากเพราะเป็นประเทศที่มีแผ่นดินไหวบ่อย นักเรียนที่เก่งทั้งภาษาญี่ปุ่นและเข้าใจวัฒนธรรมแบบนี้ จะเป็นคนที่มีคุณค่าในอนาคต!
                    </div>
                    <p style="margin-top: 15px; font-style: italic;">
                        การเล่นเกมนี้ไม่เพียงฝึกภาษาญี่ปุ่น แต่ยังเป็นการเรียนรู้ทักษะชีวิตที่สำคัญ กำลังใจให้นักเรียน! 🌟
                    </p>
                </div>
            `;
        }

        // Initialize the game when page loads
        window.addEventListener('DOMContentLoaded', initGame);
    </script>
</body>
</html>
