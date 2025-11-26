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

        .game-instructions {
            background: linear-gradient(135deg, #e8f5e8, #f0f8f0);
            border: 3px solid #4caf50;
            border-radius: 15px;
            margin: 20px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }

        .instruction-thai {
            margin-bottom: 15px;
            padding: 15px;
            background: white;
            border-radius: 10px;
            border-left: 5px solid #2196f3;
        }

        .instruction-thai h3 {
            color: #1976d2;
            margin-bottom: 10px;
            font-size: 1.3em;
        }

        .instruction-thai p {
            font-size: 1.1em;
            color: #333;
            line-height: 1.4;
        }

        .instruction-japanese {
            padding: 15px;
            background: white;
            border-radius: 10px;
            border-left: 5px solid #ff5722;
        }

        .instruction-japanese h3 {
            color: #d84315;
            margin-bottom: 10px;
            font-size: 1.3em;
        }

        .instruction-japanese p {
            font-size: 1.1em;
            color: #333;
            line-height: 1.4;
            margin-bottom: 8px;
        }

        .reading {
            font-size: 0.9em !important;
            color: #666 !important;
            font-style: italic;
            margin-top: 5px;
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

        .section-instruction {
            background: rgba(255, 152, 0, 0.1);
            padding: 12px;
            border-radius: 8px;
            margin: 10px 0 15px 0;
            font-size: 0.9em;
            line-height: 1.4;
            border: 1px solid rgba(255, 152, 0, 0.3);
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

        .bag-instruction {
            background: rgba(33, 150, 243, 0.1);
            padding: 10px;
            border-radius: 8px;
            margin: 10px 0 15px 0;
            font-size: 0.85em;
            line-height: 1.4;
            border: 1px solid rgba(33, 150, 243, 0.3);
            text-align: center;
            max-width: 280px;
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

        .vocab-instruction {
            background: rgba(156, 39, 176, 0.1);
            padding: 12px;
            border-radius: 8px;
            margin: 10px 0 15px 0;
            font-size: 0.9em;
            line-height: 1.4;
            border: 1px solid rgba(156, 39, 176, 0.3);
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
            margin-bottom: 8px;
            line-height: 1.4;
        }

        .vocab-japanese ruby {
            ruby-align: center;
        }

        .vocab-japanese rt {
            font-size: 0.5em;
            color: #7b1fa2;
            font-weight: normal;
        }

        .vocab-romaji {
            font-size: 1em;
            color: #8e24aa;
            margin-bottom: 5px;
            font-style: italic;
            font-weight: 500;
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

        .speech-status {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%) translateY(-100px);
            padding: 12px 25px;
            border-radius: 25px;
            font-weight: bold;
            font-size: 0.9em;
            z-index: 3000;
            transition: transform 0.3s ease;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            min-width: 250px;
            text-align: center;
        }

        .speech-status.show {
            transform: translateX(-50%) translateY(0);
        }

        .speech-status.success {
            background: #4caf50;
            color: white;
            border: 2px solid #388e3c;
        }

        .speech-status.warning {
            background: #ff9800;
            color: white;
            border: 2px solid #f57c00;
        }

        .speech-status.info {
            background: #2196f3;
            color: white;
            border: 2px solid #1976d2;
        }

        .reading-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 4000;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .reading-modal.show {
            opacity: 1;
        }

        .reading-content {
            background: white;
            padding: 40px;
            border-radius: 20px;
            text-align: center;
            max-width: 400px;
            width: 90%;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            transform: scale(0.8);
            transition: transform 0.3s ease;
        }

        .reading-modal.show .reading-content {
            transform: scale(1);
        }

        .reading-content h3 {
            color: #1976d2;
            font-size: 1.8em;
            margin-bottom: 20px;
        }

        .reading-japanese {
            font-size: 2.5em;
            font-weight: bold;
            color: #4a148c;
            margin-bottom: 15px;
        }

        .reading-phonetic {
            font-size: 1.5em;
            color: #7b1fa2;
            margin-bottom: 25px;
            font-style: italic;
        }

        .reading-content button {
            background: #2196f3;
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            font-size: 1.1em;
            cursor: pointer;
            transition: background 0.3s ease;
        }

        .player-modal, .difficulty-modal, .leaderboard-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 5000;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .player-modal.show, .difficulty-modal.show, .leaderboard-modal.show {
            opacity: 1;
        }

        .player-modal-content, .difficulty-modal-content {
            background: white;
            padding: 40px;
            border-radius: 20px;
            text-align: center;
            max-width: 500px;
            width: 90%;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            transform: scale(0.8);
            transition: transform 0.3s ease;
        }

        .player-modal.show .player-modal-content,
        .difficulty-modal.show .difficulty-modal-content {
            transform: scale(1);
        }

        .player-modal-content h3, .difficulty-modal-content h3 {
            color: #1976d2;
            font-size: 2em;
            margin-bottom: 15px;
        }

        .player-modal-content p {
            color: #666;
            font-size: 1.1em;
            margin-bottom: 25px;
        }

        #playerNameInput {
            width: 80%;
            padding: 15px;
            font-size: 1.2em;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            margin-bottom: 25px;
            text-align: center;
            transition: border-color 0.3s ease;
        }

        #playerNameInput:focus {
            outline: none;
            border-color: #2196f3;
            box-shadow: 0 0 10px rgba(33, 150, 243, 0.2);
        }

        .player-modal-buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
        }

        .player-modal-buttons button {
            padding: 12px 25px;
            border: none;
            border-radius: 25px;
            font-size: 1.1em;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: bold;
        }

        #startGameBtn {
            background: #4caf50;
            color: white;
        }

        #startGameBtn:hover {
            background: #45a049;
            transform: translateY(-2px);
        }

        #skipNameBtn {
            background: #9e9e9e;
            color: white;
        }

        #skipNameBtn:hover {
            background: #757575;
            transform: translateY(-2px);
        }

        .difficulty-options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }

        .difficulty-option {
            background: white;
            border: 3px solid #e0e0e0;
            border-radius: 15px;
            padding: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
        }

        .difficulty-option:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }

        .difficulty-option.easy {
            border-color: #4caf50;
        }

        .difficulty-option.easy:hover {
            background: #e8f5e8;
        }

        .difficulty-option.medium {
            border-color: #ff9800;
        }

        .difficulty-option.medium:hover {
            background: #fff3e0;
        }

        .difficulty-option.hard {
            border-color: #f44336;
        }

        .difficulty-option.hard:hover {
            background: #ffebee;
        }

        .diff-icon {
            font-size: 3em;
            margin-bottom: 10px;
        }

        .diff-title {
            font-size: 1.3em;
            font-weight: bold;
            color: #333;
            margin-bottom: 5px;
        }

        .diff-desc {
            color: #666;
            font-size: 0.9em;
        }

        .view-scores-btn {
            background: linear-gradient(45deg, #ff6b6b, #ffd93d);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            font-size: 1.2em;
            font-weight: bold;
            cursor: pointer;
            margin-top: 20px;
            transition: all 0.3s ease;
        }

        .view-scores-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.2);
        }

        .leaderboard-content {
            background: white;
            border-radius: 20px;
            max-width: 800px;
            width: 95%;
            max-height: 90vh;
            overflow: hidden;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            transform: scale(0.8);
            transition: transform 0.3s ease;
        }

        .leaderboard-modal.show .leaderboard-content {
            transform: scale(1);
        }

        .leaderboard-header {
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
            color: white;
            padding: 25px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .leaderboard-header h3 {
            font-size: 2em;
            margin: 0;
        }

        .close-btn {
            background: none;
            border: none;
            color: white;
            font-size: 1.5em;
            cursor: pointer;
            padding: 5px;
            border-radius: 50%;
            width: 35px;
            height: 35px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background 0.3s ease;
        }

        .close-btn:hover {
            background: rgba(255,255,255,0.2);
        }

        .leaderboard-tabs {
            display: flex;
            background: #f5f5f5;
            border-bottom: 1px solid #e0e0e0;
        }

        .tab-btn {
            flex: 1;
            padding: 15px;
            border: none;
            background: transparent;
            cursor: pointer;
            font-size: 1em;
            font-weight: bold;
            color: #666;
            transition: all 0.3s ease;
        }

        .tab-btn.active {
            background: white;
            color: #1976d2;
            border-bottom: 3px solid #2196f3;
        }

        .tab-btn:hover {
            background: #eeeeee;
        }

        .leaderboard-section {
            padding: 25px 30px;
            max-height: 400px;
            overflow-y: auto;
        }

        .leaderboard-section.hidden {
            display: none;
        }

        .leaderboard-section h4 {
            color: #1976d2;
            font-size: 1.5em;
            margin-bottom: 20px;
            text-align: center;
        }

        .score-table {
            width: 100%;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }

        .score-header {
            display: grid;
            grid-template-columns: 60px 1fr 80px 70px 100px;
            background: #1976d2;
            color: white;
            font-weight: bold;
            padding: 15px 10px;
        }

        .score-row {
            display: grid;
            grid-template-columns: 60px 1fr 80px 70px 100px;
            padding: 12px 10px;
            border-bottom: 1px solid #f0f0f0;
            align-items: center;
            transition: background 0.3s ease;
        }

        .score-row:hover {
            background: #f8f9fa;
        }

        .score-row.top-three {
            background: linear-gradient(135deg, #fffde7, #fff3e0);
            font-weight: bold;
        }

        .rank {
            font-size: 1.2em;
            text-align: center;
        }

        .player-name {
            font-weight: 600;
            color: #333;
        }

        .score-points {
            font-weight: bold;
            color: #4caf50;
            text-align: center;
        }

        .completion-time {
            color: #ff9800;
            text-align: center;
        }

        .score-date {
            font-size: 0.9em;
            color: #666;
            text-align: center;
        }

        .no-scores {
            text-align: center;
            color: #666;
            font-style: italic;
            padding: 50px 0;
        }

        .leaderboard-footer {
            background: #f8f9fa;
            padding: 20px 30px;
            display: flex;
            gap: 15px;
            justify-content: center;
            border-top: 1px solid #e0e0e0;
        }

        .leaderboard-footer button {
            padding: 10px 20px;
            border: none;
            border-radius: 20px;
            cursor: pointer;
            font-size: 0.9em;
            font-weight: bold;
            transition: all 0.3s ease;
        }

        .leaderboard-footer button:first-child {
            background: #f44336;
            color: white;
        }

        .leaderboard-footer button:first-child:hover {
            background: #d32f2f;
        }

        .leaderboard-footer button:nth-child(2) {
            background: #2196f3;
            color: white;
        }

        .leaderboard-footer button:nth-child(2):hover {
            background: #1976d2;
        }

        .leaderboard-footer button:last-child {
            background: #9e9e9e;
            color: white;
        }

        .leaderboard-footer button:last-child:hover {
            background: #757575;
        }

        .score-achievement {
            background: linear-gradient(135deg, #e8f5e8, #f0f8f0);
            border: 2px solid #4caf50;
            border-radius: 15px;
            padding: 25px;
            margin: 20px 0;
            text-align: center;
        }

        .score-achievement h4 {
            color: #2e7d32;
            font-size: 1.5em;
            margin-bottom: 15px;
        }

        .achievement-badge {
            background: white;
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 20px;
        }

        .rank-display {
            font-size: 2.5em;
            line-height: 1;
        }

        .achievement-text {
            text-align: left;
            color: #333;
            font-size: 1.1em;
        }

        .achievement-text strong {
            color: #1976d2;
        }

        .feedback-message.info {
            background: #2196f3;
            color: white;
            border-left: 5px solid #1976d2;
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
            
            .vocab-japanese {
                font-size: 1.3em;
            }
            
            .vocab-japanese rt {
                font-size: 0.45em;
            }
            
            .vocab-romaji {
                font-size: 0.9em;
            }
            
            .missed-item {
                flex-direction: column;
                align-items: flex-start;
                text-align: left;
            }
            
            .missed-item span {
                margin-bottom: 8px;
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
            
            .game-instructions {
                margin: 15px;
                padding: 15px;
            }
            
            .instruction-thai, .instruction-japanese {
                padding: 12px;
            }
            
            .instruction-thai h3, .instruction-japanese h3 {
                font-size: 1.2em;
            }
            
            .instruction-thai p, .instruction-japanese p {
                font-size: 1em;
            }
            
            .section-instruction, .bag-instruction, .vocab-instruction {
                font-size: 0.8em;
                padding: 10px;
            }
            
            .speech-status {
                top: 10px;
                left: 10px;
                right: 10px;
                transform: none;
                min-width: auto;
                font-size: 0.8em;
                padding: 10px 15px;
            }
            
            .speech-status.show {
                transform: none;
            }
            
            .reading-content {
                padding: 25px;
                width: 95%;
            }
            
            .reading-content h3 {
                font-size: 1.5em;
            }
            
            .reading-japanese {
                font-size: 2em;
            }
            
            .reading-phonetic {
                font-size: 1.2em;
            }
            
            .player-modal-content, .difficulty-modal-content {
                padding: 25px;
                width: 95%;
            }
            
            .player-modal-content h3, .difficulty-modal-content h3 {
                font-size: 1.6em;
            }
            
            #playerNameInput {
                width: 90%;
                padding: 12px;
                font-size: 1.1em;
            }
            
            .player-modal-buttons {
                flex-direction: column;
                gap: 10px;
            }
            
            .player-modal-buttons button {
                width: 100%;
                padding: 15px;
            }
            
            .difficulty-options {
                grid-template-columns: 1fr;
                gap: 15px;
            }
            
            .difficulty-option {
                padding: 15px;
            }
            
            .diff-icon {
                font-size: 2.5em;
            }
            
            .diff-title {
                font-size: 1.2em;
            }
            
            .view-scores-btn {
                padding: 12px 20px;
                font-size: 1.1em;
            }
            
            .leaderboard-content {
                width: 98%;
                max-height: 95vh;
            }
            
            .leaderboard-header {
                padding: 20px 15px;
            }
            
            .leaderboard-header h3 {
                font-size: 1.5em;
            }
            
            .leaderboard-tabs {
                flex-wrap: wrap;
            }
            
            .tab-btn {
                padding: 12px 10px;
                font-size: 0.9em;
                min-width: 25%;
            }
            
            .leaderboard-section {
                padding: 15px 10px;
                max-height: 300px;
            }
            
            .score-header {
                grid-template-columns: 40px 1fr 60px 50px 70px;
                padding: 10px 5px;
                font-size: 0.8em;
            }
            
            .score-row {
                grid-template-columns: 40px 1fr 60px 50px 70px;
                padding: 8px 5px;
                font-size: 0.8em;
            }
            
            .rank {
                font-size: 1em;
            }
            
            .player-name {
                font-size: 0.8em;
            }
            
            .leaderboard-footer {
                padding: 15px 10px;
                flex-direction: column;
                gap: 10px;
            }
            
            .leaderboard-footer button {
                width: 100%;
                padding: 12px;
            }
            
            .score-achievement {
                margin: 15px 0;
                padding: 20px;
            }
            
            .score-achievement h4 {
                font-size: 1.3em;
            }
            
            .achievement-badge {
                flex-direction: column;
                gap: 15px;
                text-align: center;
            }
            
            .achievement-text {
                text-align: center;
                font-size: 1em;
            }
            
            .rank-display {
                font-size: 2em;
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

        <div class="game-instructions">
            <div class="instruction-thai">
                <h3>📋 คำสั่ง</h3>
                <p><strong>จงนำสิ่งของที่จำเป็นในการใช้ยามเกิดภัยพิบัติ ใส่ลงในกระเป๋าให้ถูกต้อง</strong></p>
            </div>
            <div class="instruction-japanese">
                <h3>📋 指示 (しじ)</h3>
                <p><strong>緊急時に必要なものを正しくかばんに入れましょう</strong></p>
                <p class="reading">(きんきゅうじ に ひつよう な もの を ただしく かばん に いれましょう)</p>
            </div>
        </div>

        <div class="game-controls">
            <button class="difficulty-btn easy" onclick="showPlayerNameInput()">
                🎮 เริ่มเล่น
            </button>
            <button class="difficulty-btn medium" onclick="showLeaderboard()">
                🏆 คะแนนสูงสุด
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
                <p class="section-instruction">
                    <strong>วิธีเล่น:</strong> ลากสิ่งของที่คิดว่าจำเป็นในยามฉุกเฉินใส่กระเป๋า<br>
                    <strong>遊び方:</strong> 緊急時に必要だと思うものをかばんにドラッグしてください
                </p>
                <div class="items-grid" id="itemsGrid">
                    <!-- Items will be populated by JavaScript -->
                </div>
            </div>

            <div class="bag-section">
                <h3>🎒 緊急バッグ</h3>
                <p class="bag-instruction">
                    <strong>ใส่ที่นี่:</strong> ลากสิ่งของมาใส่ในกระเป๋า<br>
                    <strong>ここに入れて:</strong> ものをかばんにドラッグしてください
                </p>
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
            <p class="vocab-instruction">
                <strong>เรียนรู้คำศัพท์:</strong> กดปุ่ม 🔊 เพื่อฟังการออกเสียงภาษาญี่ปุ่น<br>
                <strong>言葉を学ぼう:</strong> 🔊ボタンを押して日本語の発音を聞きましょう
            </p>
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
            <button class="restart-btn" onclick="restartGame()">เล่นใหม่</button>
        </div>
    </div>

    <script>
        // Game data
        const emergencyItems = [
            { emoji: '💊', japanese: '薬', furigana: 'くすり', romaji: 'kusuri', english: 'Medicine', essential: true },
            { emoji: '🔦', japanese: '懐中電灯', furigana: 'かいちゅうでんとう', romaji: 'kaichū dentō', english: 'Flashlight', essential: true },
            { emoji: '🥤', japanese: '水', furigana: 'みず', romaji: 'mizu', english: 'Water', essential: true },
            { emoji: '🍞', japanese: 'パン', furigana: '', romaji: 'pan', english: 'Bread', essential: true },
            { emoji: '🩹', japanese: '絆創膏', furigana: 'ばんそうこう', romaji: 'bansōkō', english: 'Band-aid', essential: true },
            { emoji: '🔋', japanese: '電池', furigana: 'でんち', romaji: 'denchi', english: 'Battery', essential: true },
            { emoji: '📱', japanese: '携帯電話', furigana: 'けいたいでんわ', romaji: 'keitai denwa', english: 'Mobile phone', essential: true },
            { emoji: '🧥', japanese: '服', furigana: 'ふく', romaji: 'fuku', english: 'Clothes', essential: true },
            { emoji: '💰', japanese: 'お金', furigana: 'おかね', romaji: 'okane', english: 'Money', essential: true },
            { emoji: '📄', japanese: '書類', furigana: 'しょるい', romaji: 'shorui', english: 'Documents', essential: true },
            // Non-essential items
            { emoji: '🎮', japanese: 'ゲーム', furigana: '', romaji: 'gēmu', english: 'Game', essential: false },
            { emoji: '🍰', japanese: 'ケーキ', furigana: '', romaji: 'kēki', english: 'Cake', essential: false },
            { emoji: '👠', japanese: 'ハイヒール', furigana: '', romaji: 'hai hīru', english: 'High heels', essential: false },
            { emoji: '🎸', japanese: 'ギター', furigana: '', romaji: 'gitā', english: 'Guitar', essential: false },
            { emoji: '🧸', japanese: 'ぬいぐるみ', furigana: '', romaji: 'nuigurumi', english: 'Stuffed animal', essential: false },
            { emoji: '💄', japanese: '口紅', furigana: 'くちべに', romaji: 'kuchibeni', english: 'Lipstick', essential: false },
            { emoji: '🎭', japanese: 'マスク', furigana: '', romaji: 'masuku', english: 'Theater mask', essential: false },
            { emoji: '🏀', japanese: 'ボール', furigana: '', romaji: 'bōru', english: 'Ball', essential: false }
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
        let playerName = '';

        // Scoring system
        function initScoreSystem() {
            if (!localStorage.getItem('emergencyGameScores')) {
                localStorage.setItem('emergencyGameScores', JSON.stringify([]));
            }
        }

        function saveScore(playerName, score, difficulty, accuracy, completionTime) {
            const scores = JSON.parse(localStorage.getItem('emergencyGameScores') || '[]');
            const newScore = {
                id: Date.now(),
                playerName: playerName,
                score: score,
                difficulty: difficulty,
                accuracy: accuracy,
                completionTime: completionTime,
                date: new Date().toLocaleDateString('th-TH'),
                timestamp: Date.now()
            };
            
            scores.push(newScore);
            
            // Sort by score (high to low), then by time (low to high)
            scores.sort((a, b) => {
                if (b.score === a.score) {
                    return a.completionTime - b.completionTime;
                }
                return b.score - a.score;
            });
            
            // Keep only top 50 scores
            const topScores = scores.slice(0, 50);
            localStorage.setItem('emergencyGameScores', JSON.stringify(topScores));
            
            return newScore;
        }

        function getTopScores(difficulty = 'all', limit = 10) {
            const scores = JSON.parse(localStorage.getItem('emergencyGameScores') || '[]');
            
            if (difficulty !== 'all') {
                return scores.filter(s => s.difficulty === difficulty).slice(0, limit);
            }
            
            return scores.slice(0, limit);
        }

        function showPlayerNameInput() {
            const modal = document.createElement('div');
            modal.className = 'player-modal';
            modal.innerHTML = `
                <div class="player-modal-content">
                    <h3>🎮 ใส่ชื่อผู้เล่น</h3>
                    <p>ใส่ชื่อของคุณเพื่อบันทึกคะแนน</p>
                    <input type="text" id="playerNameInput" placeholder="ชื่อผู้เล่น" maxlength="20" />
                    <div class="player-modal-buttons">
                        <button id="startGameBtn" onclick="startGameWithName()">เริ่มเล่น</button>
                        <button id="skipNameBtn" onclick="startGameWithoutName()">เล่นโดยไม่บันทึก</button>
                    </div>
                </div>
            `;
            document.body.appendChild(modal);
            
            setTimeout(() => {
                modal.classList.add('show');
                document.getElementById('playerNameInput').focus();
            }, 100);
            
            // Handle Enter key
            document.getElementById('playerNameInput').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    startGameWithName();
                }
            });
        }

        function startGameWithName() {
            const nameInput = document.getElementById('playerNameInput');
            playerName = nameInput.value.trim() || `ผู้เล่น${Date.now().toString().slice(-4)}`;
            document.querySelector('.player-modal').remove();
            showDifficultySelection();
        }

        function startGameWithoutName() {
            playerName = '';
            document.querySelector('.player-modal').remove();
            showDifficultySelection();
        }

        function showDifficultySelection() {
            const modal = document.createElement('div');
            modal.className = 'difficulty-modal';
            modal.innerHTML = `
                <div class="difficulty-modal-content">
                    <h3>⚡ เลือกระดับความยาก</h3>
                    <div class="difficulty-options">
                        <button class="difficulty-option easy" onclick="startGameFromModal('easy')">
                            <div class="diff-icon">😊</div>
                            <div class="diff-title">ง่าย</div>
                            <div class="diff-desc">60 วินาที</div>
                        </button>
                        <button class="difficulty-option medium" onclick="startGameFromModal('medium')">
                            <div class="diff-icon">😐</div>
                            <div class="diff-title">ปานกลาง</div>
                            <div class="diff-desc">30 วินาที</div>
                        </button>
                        <button class="difficulty-option hard" onclick="startGameFromModal('hard')">
                            <div class="diff-icon">😤</div>
                            <div class="diff-title">ยาก</div>
                            <div class="diff-desc">20 วินาที</div>
                        </button>
                    </div>
                    <button class="view-scores-btn" onclick="showLeaderboard()">🏆 ดูคะแนนสูงสุด</button>
                </div>
            `;
            document.body.appendChild(modal);
            
            setTimeout(() => {
                modal.classList.add('show');
            }, 100);
        }

        function startGameFromModal(difficulty) {
            document.querySelector('.difficulty-modal').remove();
            startGame(difficulty);
        }

        function showLeaderboard() {
            const modal = document.createElement('div');
            modal.className = 'leaderboard-modal';
            modal.innerHTML = createLeaderboardHTML();
            document.body.appendChild(modal);
            
            setTimeout(() => {
                modal.classList.add('show');
            }, 100);
        }

        function createLeaderboardHTML() {
            const allScores = getTopScores('all', 20);
            const easyScores = getTopScores('easy', 5);
            const mediumScores = getTopScores('medium', 5);
            const hardScores = getTopScores('hard', 5);
            
            return `
                <div class="leaderboard-content">
                    <div class="leaderboard-header">
                        <h3>🏆 กระดานคะแนน</h3>
                        <button class="close-btn" onclick="closeLeaderboard()">✕</button>
                    </div>
                    
                    <div class="leaderboard-tabs">
                        <button class="tab-btn active" onclick="showTab('all')">ทั้งหมด</button>
                        <button class="tab-btn" onclick="showTab('easy')">ง่าย</button>
                        <button class="tab-btn" onclick="showTab('medium')">ปานกลาง</button>
                        <button class="tab-btn" onclick="showTab('hard')">ยาก</button>
                    </div>
                    
                    <div class="leaderboard-section" id="tab-all">
                        <h4>🏆 คะแนนสูงสุด (ทุกระดับ)</h4>
                        ${createScoreTable(allScores)}
                    </div>
                    
                    <div class="leaderboard-section hidden" id="tab-easy">
                        <h4>😊 ระดับง่าย</h4>
                        ${createScoreTable(easyScores)}
                    </div>
                    
                    <div class="leaderboard-section hidden" id="tab-medium">
                        <h4>😐 ระดับปานกลาง</h4>
                        ${createScoreTable(mediumScores)}
                    </div>
                    
                    <div class="leaderboard-section hidden" id="tab-hard">
                        <h4>😤 ระดับยาก</h4>
                        ${createScoreTable(hardScores)}
                    </div>
                    
                    <div class="leaderboard-footer">
                        <button onclick="clearAllScores()">🗑️ ล้างคะแนนทั้งหมด</button>
                        <button onclick="exportScores()">📤 ส่งออกคะแนน</button>
                        <button onclick="closeLeaderboard()">ปิด</button>
                    </div>
                </div>
            `;
        }

        function createScoreTable(scores) {
            if (scores.length === 0) {
                return '<p class="no-scores">ยังไม่มีคะแนนในระดับนี้</p>';
            }
            
            let html = `
                <div class="score-table">
                    <div class="score-header">
                        <div>อันดับ</div>
                        <div>ผู้เล่น</div>
                        <div>คะแนน</div>
                        <div>เวลา</div>
                        <div>วันที่</div>
                    </div>
            `;
            
            scores.forEach((score, index) => {
                const medal = index === 0 ? '🥇' : index === 1 ? '🥈' : index === 2 ? '🥉' : `${index + 1}`;
                const difficultyIcon = score.difficulty === 'easy' ? '😊' : 
                                     score.difficulty === 'medium' ? '😐' : '😤';
                
                html += `
                    <div class="score-row ${index < 3 ? 'top-three' : ''}">
                        <div class="rank">${medal}</div>
                        <div class="player-name">${score.playerName} ${difficultyIcon}</div>
                        <div class="score-points">${score.score}</div>
                        <div class="completion-time">${score.completionTime}วิ</div>
                        <div class="score-date">${score.date}</div>
                    </div>
                `;
            });
            
            html += '</div>';
            return html;
        }

        function showTab(tabName) {
            // Remove active class from all tabs
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            document.querySelectorAll('.leaderboard-section').forEach(section => section.classList.add('hidden'));
            
            // Add active class to clicked tab
            event.target.classList.add('active');
            document.getElementById(`tab-${tabName}`).classList.remove('hidden');
        }

        function closeLeaderboard() {
            const modal = document.querySelector('.leaderboard-modal');
            if (modal) modal.remove();
        }

        function clearAllScores() {
            if (confirm('คุณแน่ใจหรือไม่ที่จะล้างคะแนนทั้งหมด?')) {
                localStorage.removeItem('emergencyGameScores');
                showFeedbackMessage('🗑️ ล้างคะแนนเรียบร้อยแล้ว', 'info');
                closeLeaderboard();
            }
        }

        function exportScores() {
            const scores = JSON.parse(localStorage.getItem('emergencyGameScores') || '[]');
            const dataStr = JSON.stringify(scores, null, 2);
            const dataBlob = new Blob([dataStr], {type: 'application/json'});
            const url = URL.createObjectURL(dataBlob);
            
            const link = document.createElement('a');
            link.href = url;
            link.download = `emergency-game-scores-${new Date().toISOString().split('T')[0]}.json`;
            link.click();
            
            URL.revokeObjectURL(url);
            showFeedbackMessage('📤 ส่งออกคะแนนเรียบร้อย', 'success');
        }

        function restartGame() {
            // Close game over modal
            document.getElementById('gameOver').style.display = 'none';
            
            // Reset all game variables
            score = 0;
            gameActive = false;
            currentDifficulty = '';
            playerName = '';
            gameStats = {
                correctItems: 0,
                incorrectItems: 0,
                totalItems: 0,
                timeUsed: 0,
                missedEssentials: [],
                incorrectChoices: []
            };
            
            // Clear timer if exists
            if (gameTimer) {
                clearInterval(gameTimer);
            }
            
            // Reset UI
            document.getElementById('score').textContent = '0';
            document.getElementById('timeLeft').textContent = '--';
            document.getElementById('timeLeft').style.color = '';
            document.getElementById('timeLeft').style.fontWeight = '';
            
            // Clear bag
            document.getElementById('bagInterior').innerHTML = '';
            
            // Recreate items
            createItemsGrid();
            setupDragAndDrop();
            
            // Show player name input to start new game
            showPlayerNameInput();
        }

        // Initialize game
        function initGame() {
            createItemsGrid();
            createVocabularyCards();
            setupDragAndDrop();
            
            // Initialize speech synthesis
            initSpeechSynthesis();
            
            // Initialize scoring system
            initScoreSystem();
            
            // Add first touch handler for mobile speech synthesis
            addFirstInteractionHandler();
        }

        function addFirstInteractionHandler() {
            let firstInteraction = true;
            
            function handleFirstInteraction() {
                if (firstInteraction && speechSupported) {
                    firstInteraction = false;
                    
                    // Re-initialize speech synthesis on first user interaction
                    try {
                        const utterance = new SpeechSynthesisUtterance('');
                        utterance.volume = 0;
                        speechSynthesis.speak(utterance);
                        speechSynthesis.cancel();
                        
                        speechEnabled = true;
                        showSpeechStatus('🔊 เสียงเปิดใช้งานแล้ว!', 'success');
                    } catch (error) {
                        console.error('Error initializing speech on first interaction:', error);
                        speechEnabled = false;
                    }
                    
                    // Remove event listeners after first interaction
                    document.removeEventListener('click', handleFirstInteraction);
                    document.removeEventListener('touchstart', handleFirstInteraction);
                }
            }
            
            // Add event listeners for first user interaction
            document.addEventListener('click', handleFirstInteraction, { once: true });
            document.addEventListener('touchstart', handleFirstInteraction, { once: true });
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
                    <div class="item-label">${item.japanese}<br><small>${item.romaji}</small></div>
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
                
                // Create Japanese text with furigana
                let japaneseDisplay = '';
                if (item.furigana) {
                    // Has kanji, show furigana above
                    japaneseDisplay = `<ruby>${item.japanese}<rt>${item.furigana}</rt></ruby>`;
                } else {
                    // No kanji, just show the text
                    japaneseDisplay = item.japanese;
                }
                
                card.innerHTML = `
                    <div class="vocab-japanese">${item.emoji} ${japaneseDisplay}</div>
                    <div class="vocab-romaji">${item.romaji}</div>
                    <div class="vocab-english">${item.english}</div>
                    <button class="play-button" onclick="speakWord('${item.japanese}', '${item.furigana || item.japanese}')">
                        🔊 音声
                    </button>
                `;
                grid.appendChild(card);
            });
        }

        // Speech synthesis setup
        let speechEnabled = false;
        let speechSupported = false;

        // Initialize speech synthesis
        function initSpeechSynthesis() {
            if ('speechSynthesis' in window) {
                speechSupported = true;
                
                // For mobile devices, we need to trigger speechSynthesis on user interaction
                if (/Android|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)) {
                    // Mobile device detected
                    const utterance = new SpeechSynthesisUtterance('');
                    utterance.volume = 0;
                    speechSynthesis.speak(utterance);
                    speechSynthesis.cancel();
                }
                
                speechEnabled = true;
                showSpeechStatus('🔊 เสียงพร้อมใช้งาน', 'success');
            } else {
                speechSupported = false;
                speechEnabled = false;
                showSpeechStatus('⚠️ อุปกรณ์นี้ไม่รองรับเสียงออกเสียง', 'warning');
            }
        }

        function showSpeechStatus(message, type) {
            const statusEl = document.createElement('div');
            statusEl.className = `speech-status ${type}`;
            statusEl.textContent = message;
            document.body.appendChild(statusEl);
            
            setTimeout(() => {
                statusEl.classList.add('show');
            }, 100);
            
            setTimeout(() => {
                statusEl.remove();
            }, 4000);
        }

        function speakWord(japanese, reading) {
            if (!speechSupported) {
                // Fallback: show reading in popup
                showReadingFallback(japanese, reading);
                return;
            }

            if (!speechEnabled) {
                // Try to re-initialize
                initSpeechSynthesis();
                if (!speechEnabled) {
                    showReadingFallback(japanese, reading);
                    return;
                }
            }

            try {
                // Stop any currently speaking utterance
                speechSynthesis.cancel();
                
                const utterance = new SpeechSynthesisUtterance(japanese);
                utterance.lang = 'ja-JP';
                utterance.rate = 0.8;
                utterance.pitch = 1;
                utterance.volume = 1;
                
                utterance.onerror = function(event) {
                    console.error('Speech synthesis error:', event.error);
                    showReadingFallback(japanese, reading);
                };
                
                utterance.onend = function() {
                    console.log('Speech finished successfully');
                };
                
                speechSynthesis.speak(utterance);
                
                // Visual feedback that speech is working
                showFeedbackMessage(`🔊 ${japanese} (${reading})`, 'info');
                
            } catch (error) {
                console.error('Error in speech synthesis:', error);
                showReadingFallback(japanese, reading);
            }
        }

        function showReadingFallback(japanese, reading) {
            const modal = document.createElement('div');
            modal.className = 'reading-modal';
            modal.innerHTML = `
                <div class="reading-content">
                    <h3>📖 การอ่าน</h3>
                    <div class="reading-japanese">${japanese}</div>
                    <div class="reading-phonetic">${reading || japanese}</div>
                    <button onclick="this.parentElement.parentElement.remove()">ปิด</button>
                </div>
            `;
            document.body.appendChild(modal);
            
            setTimeout(() => {
                modal.classList.add('show');
            }, 100);
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
            
            // Save score if player name exists
            let savedScore = null;
            let rank = null;
            if (playerName) {
                const accuracy = gameStats.totalItems > 0 ? 
                    Math.round((gameStats.correctItems / gameStats.totalItems) * 100) : 0;
                
                savedScore = saveScore(playerName, score, currentDifficulty, accuracy, gameStats.timeUsed);
                
                // Find player's rank
                const topScores = getTopScores(currentDifficulty, 50);
                rank = topScores.findIndex(s => s.id === savedScore.id) + 1;
            }
            
            // Create comprehensive results display
            const resultsHTML = createGameResults(won, savedScore, rank);
            
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

        function createGameResults(won, savedScore = null, rank = null) {
            const accuracy = gameStats.totalItems > 0 ? 
                Math.round((gameStats.correctItems / gameStats.totalItems) * 100) : 0;
            
            const essentialCount = emergencyItems.filter(item => item.essential).length;
            const completionRate = Math.round((gameStats.correctItems / essentialCount) * 100);
            
            let scoreSection = '';
            if (savedScore && rank) {
                const rankDisplay = rank <= 3 ? 
                    (rank === 1 ? '🥇 อันดับ 1' : rank === 2 ? '🥈 อันดับ 2' : '🥉 อันดับ 3') :
                    `🏆 อันดับ ${rank}`;
                
                scoreSection = `
                    <div class="score-achievement">
                        <h4>🎊 บันทึกคะแนนแล้ว!</h4>
                        <div class="achievement-badge">
                            <div class="rank-display">${rankDisplay}</div>
                            <div class="achievement-text">
                                ผู้เล่น: <strong>${savedScore.playerName}</strong><br>
                                ระดับ: <strong>${getDifficultyName(currentDifficulty)}</strong>
                            </div>
                        </div>
                    </div>
                `;
            } else if (playerName === '') {
                scoreSection = `
                    <div class="score-achievement">
                        <h4>💡 เคล็ดลับ</h4>
                        <p>ใส่ชื่อก่อนเล่นเพื่อบันทึกคะแนนในกระดานคะแนน!</p>
                    </div>
                `;
            }
            
            return `
                ${scoreSection}
                
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

        function getDifficultyName(difficulty) {
            const names = {
                'easy': 'ง่าย (60วิ)',
                'medium': 'ปานกลาง (30วิ)', 
                'hard': 'ยาก (20วิ)'
            };
            return names[difficulty] || difficulty;
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
                const displayReading = item.furigana ? `(${item.furigana})` : '';
                html += `
                    <div class="missed-item">
                        <span style="font-size: 1.5em; margin-right: 10px;">${item.emoji}</span>
                        <div>
                            <strong>${item.japanese}</strong> ${displayReading}<br>
                            <small style="color: #8e24aa; font-style: italic;">${item.romaji}</small><br>
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
