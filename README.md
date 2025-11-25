
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>数独游戏 - 手机优化版</title>
    <style>
        :root {
            --primary-color: #3498db;
            --secondary-color: #2c3e50;
            --accent-color: #e74c3c;
            --success-color: #2ecc71;
            --light-bg: #f5f5f5;
            --dark-text: #333;
            --light-text: #fff;
            --cell-size: min(9vw, 45px);
            --button-size: min(10vw, 50px);
            --font-size-normal: min(4vw, 18px);
            --font-size-large: min(5vw, 22px);
            --padding-normal: min(3vw, 15px);
            --padding-small: min(2vw, 10px);
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            -webkit-tap-highlight-color: transparent;
            -webkit-touch-callout: none;
            -webkit-user-select: none;
            user-select: none;
        }
        
        body {
            background-color: var(--light-bg);
            color: var(--dark-text);
            line-height: 1.6;
            padding: var(--padding-small);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        
        .container {
            width: 100%;
            max-width: 500px;
            background-color: white;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            margin: 0 auto;
            display: flex;
            flex-direction: column;
        }
        
        /* 开始页面样式 */
        .screen {
            display: flex;
            flex-direction: column;
            padding: var(--padding-normal);
        }
        
        .start-screen {
            text-align: center;
        }
        
        .start-screen h1 {
            color: var(--secondary-color);
            font-size: var(--font-size-large);
            margin-bottom: 15px;
        }
        
        .rules {
            text-align: left;
            margin: 15px 0;
            padding: 12px;
            background-color: #f8f9fa;
            border-radius: 8px;
            font-size: var(--font-size-normal);
            max-height: 50vh;
            overflow-y: auto;
        }
        
        .rules h2 {
            color: var(--primary-color);
            margin-bottom: 8px;
            text-align: center;
            font-size: var(--font-size-normal);
        }
        
        .rules ul {
            padding-left: 18px;
            margin-bottom: 12px;
        }
        
        .rules li {
            margin-bottom: 6px;
        }
        
        .btn {
            padding: 10px 20px;
            background-color: var(--primary-color);
            color: var(--light-text);
            border: none;
            border-radius: 8px;
            font-size: var(--font-size-normal);
            cursor: pointer;
            transition: all 0.3s;
            margin: 5px;
            min-height: 44px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            text-decoration: none;
            flex: 1;
        }
        
        .btn:hover {
            background-color: #2980b9;
        }
        
        .btn-secondary {
            background-color: #9b59b6;
        }
        
        .btn-secondary:hover {
            background-color: #8e44ad;
        }
        
        .btn-container {
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
            gap: 10px;
        }
        
        /* 游戏页面样式 */
        .game-screen {
            display: none;
        }
        
        .game-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 10px;
        }
        
        .home-btn {
            background: none;
            border: none;
            font-size: var(--font-size-normal);
            cursor: pointer;
            color: var(--primary-color);
            padding: 8px 12px;
            border-radius: 5px;
            transition: background-color 0.2s;
            min-height: 44px;
            min-width: 44px;
        }
        
        .home-btn:hover {
            background-color: #f0f0f0;
        }
        
        .game-info {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            font-size: var(--font-size-normal);
        }
        
        .difficulty-selector {
            margin-bottom: 10px;
            display: flex;
            gap: 6px;
        }
        
        .difficulty-btn {
            flex: 1;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background-color: #f8f9fa;
            color: #495057;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            min-height: 40px;
            font-size: var(--font-size-normal);
        }
        
        .difficulty-btn.active {
            background-color: var(--primary-color);
            color: white;
            border-color: var(--primary-color);
        }
        
        .board-container {
            display: flex;
            justify-content: center;
            margin-bottom: 15px;
        }
        
        .sudoku-board {
            display: grid;
            grid-template-columns: repeat(9, 1fr);
            grid-gap: 1px;
            background-color: #333;
            border: 2px solid #333;
            border-radius: 5px;
            overflow: hidden;
            width: 100%;
            max-width: 95vw;
            aspect-ratio: 1 / 1;
        }
        
        .cell {
            background-color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: var(--font-size-large);
            font-weight: bold;
            cursor: pointer;
            position: relative;
            min-height: var(--cell-size);
        }
        
        .cell.prefilled {
            background-color: #f0f0f0;
            color: #000;
        }
        
        .cell.selected {
            background-color: #d6eaf8;
        }
        
        .cell.same-number {
            background-color: #e8f4fc;
        }
        
        .cell.error {
            background-color: #fadbd8;
            color: #c0392b;
        }
        
        .cell.border-right-thick {
            border-right: 2px solid #333;
        }
        
        .cell.border-bottom-thick {
            border-bottom: 2px solid #333;
        }
        
        /* 标记样式 - 小数字显示在左上角 */
        .pencil-marks {
            position: absolute;
            top: 2px;
            left: 2px;
            display: flex;
            flex-wrap: wrap;
            max-width: 100%;
            pointer-events: none;
            gap: 1px;
            width: calc(100% - 4px);
        }
        
        .pencil-mark {
            font-size: calc(var(--font-size-normal) * 0.6);
            color: #6c757d;
            line-height: 1;
            padding: 0 1px;
            flex: 1 0 30%;
            text-align: center;
        }
        
        .controls-container {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        
        .number-pad {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            grid-gap: 6px;
        }
        
        .number-btn, .action-btn {
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: var(--primary-color);
            color: var(--light-text);
            border: none;
            border-radius: 8px;
            font-size: var(--font-size-large);
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            min-height: var(--button-size);
        }
        
        .number-btn:hover, .action-btn:hover {
            background-color: #2980b9;
        }
        
        .action-btn {
            background-color: var(--accent-color);
        }
        
        .action-btn:hover {
            background-color: #c0392b;
        }
        
        .action-btn:disabled {
            background-color: #95a5a6;
            cursor: not-allowed;
        }
        
        .action-btn:disabled:hover {
            background-color: #95a5a6;
        }
        
        .game-controls {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            grid-gap: 8px;
        }
        
        .control-btn {
            padding: 10px;
            background-color: var(--secondary-color);
            color: var(--light-text);
            border: none;
            border-radius: 8px;
            font-size: var(--font-size-normal);
            cursor: pointer;
            transition: all 0.3s;
            min-height: 50px;
        }
        
        .control-btn:hover {
            background-color: #1a252f;
        }
        
        .control-btn.hint {
            background-color: #f39c12;
        }
        
        .control-btn.hint:hover {
            background-color: #d35400;
        }
        
        .control-btn.mark {
            background-color: #9b59b6;
        }
        
        .control-btn.mark:hover {
            background-color: #8e44ad;
        }
        
        .control-btn.mark.active {
            background-color: #8e44ad;
            box-shadow: 0 0 0 2px white, 0 0 0 4px #9b59b6;
        }
        
        .control-btn:disabled {
            background-color: #95a5a6;
            cursor: not-allowed;
        }
        
        .control-btn:disabled:hover {
            background-color: #95a5a6;
        }
        
        .game-stats {
            display: flex;
            justify-content: space-between;
            margin-top: 10px;
            font-size: calc(var(--font-size-normal) * 0.8);
            color: #6c757d;
        }
        
        /* 排行榜样式 */
        .leaderboard-screen {
            display: none;
        }
        
        .leaderboard-screen h1 {
            color: var(--secondary-color);
            font-size: var(--font-size-large);
            margin-bottom: 15px;
            text-align: center;
        }
        
        .leaderboard-tabs {
            display: flex;
            margin-bottom: 15px;
            border-bottom: 1px solid #ddd;
        }
        
        .leaderboard-tab {
            flex: 1;
            padding: 10px;
            background: none;
            border: none;
            cursor: pointer;
            font-size: var(--font-size-normal);
            transition: all 0.2s;
            min-height: 44px;
        }
        
        .leaderboard-tab.active {
            border-bottom: 3px solid var(--primary-color);
            color: var(--primary-color);
            font-weight: bold;
        }
        
        .leaderboard-list {
            text-align: left;
            margin-bottom: 15px;
            max-height: 50vh;
            overflow-y: auto;
        }
        
        .leaderboard-item {
            padding: 10px;
            border-bottom: 1px solid #eee;
            display: flex;
            justify-content: space-between;
        }
        
        .leaderboard-item:nth-child(odd) {
            background-color: #f8f9fa;
        }
        
        /* 完成弹窗样式 */
        .completion-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 1000;
            align-items: center;
            justify-content: center;
            padding: var(--padding-normal);
        }
        
        .completion-content {
            background-color: white;
            border-radius: 12px;
            padding: 20px;
            text-align: center;
            width: 100%;
            max-width: 350px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
        }
        
        .completion-content h2 {
            color: var(--secondary-color);
            margin-bottom: 12px;
            font-size: var(--font-size-large);
        }
        
        .completion-content p {
            margin-bottom: 15px;
            font-size: var(--font-size-normal);
            color: #555;
        }
        
        .completion-time {
            font-size: calc(var(--font-size-large) * 1.2);
            font-weight: bold;
            color: var(--primary-color);
            margin: 12px 0;
        }
        
        .completion-buttons {
            display: flex;
            gap: 12px;
            margin-top: 20px;
        }
        
        /* 响应式调整 */
        @media (min-width: 768px) {
            :root {
                --cell-size: 45px;
                --button-size: 50px;
                --font-size-normal: 18px;
                --font-size-large: 22px;
                --padding-normal: 20px;
                --padding-small: 10px;
            }
            
            .container {
                max-width: 600px;
            }
            
            .btn {
                min-width: 150px;
            }
            
            .btn-container {
                justify-content: center;
            }
            
            .game-content {
                flex-direction: row;
                gap: 20px;
            }
            
            .board-container {
                flex: 1;
                margin-bottom: 0;
            }
            
            .controls-container {
                flex: 0 0 250px;
                justify-content: center;
            }
            
            .sudoku-board {
                max-width: 100%;
            }
        }
        
        @media (max-width: 480px) {
            .completion-buttons {
                flex-direction: column;
            }
            
            .container {
                height: auto;
                min-height: 100vh;
            }
        }
        
        @media (max-height: 700px) and (orientation: landscape) {
            .screen {
                padding: var(--padding-small);
            }
            
            .sudoku-board {
                margin-bottom: 10px;
            }
            
            .rules {
                max-height: 40vh;
            }
            
            .leaderboard-list {
                max-height: 30vh;
            }
        }
        
        /* 防止iOS Safari缩放 */
        @supports (-webkit-touch-callout: none) {
            body {
                -webkit-text-size-adjust: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 开始页面 -->
        <div class="screen start-screen" id="start-screen">
            <h1>数独游戏</h1>
            <div class="rules">
                <h2>游戏规则</h2>
                <ul>
                    <li>数独是一个9x9的网格，分为9个3x3的小宫格</li>
                    <li>目标是用数字1-9填满整个网格</li>
                    <li>每行、每列和每个3x3小宫格都必须包含数字1-9，且不能重复</li>
                    <li>游戏开始时，部分格子已经填好数字，这些是固定数字不能更改</li>
                </ul>
                <h2>操作说明</h2>
                <ul>
                    <li>点击空白格子选中它</li>
                    <li>使用数字键盘输入数字</li>
                    <li>使用"标记"功能可以在格子中添加候选数字</li>
                    <li>使用"提示"功能可以显示当前格子的正确答案（最多3次）</li>
                    <li>使用"新游戏"可以开始新的游戏</li>
                    <li>使用"解题"可以显示完整答案</li>
                </ul>
            </div>
            <div class="btn-container">
                <button class="btn" id="start-btn">开始游戏</button>
                <button class="btn btn-secondary" id="leaderboard-btn">排行榜</button>
            </div>
        </div>

        <!-- 游戏页面 -->
        <div class="screen game-screen" id="game-screen">
            <div class="game-header">
                <button class="home-btn" id="home-btn">← 返回</button>
                <h1>数独游戏</h1>
                <div style="width: 60px;"></div> <!-- 占位符，保持标题居中 -->
            </div>
            
            <div class="game-info">
                <div class="timer">时间: <span id="timer">00:00</span></div>
                <div class="mistakes">错误: <span id="mistakes">0</span>/3</div>
            </div>
            
            <div class="difficulty-selector">
                <button class="difficulty-btn active" data-difficulty="easy">简单</button>
                <button class="difficulty-btn" data-difficulty="medium">中等</button>
                <button class="difficulty-btn" data-difficulty="hard">困难</button>
            </div>
            
            <div class="board-container">
                <div class="sudoku-board" id="board">
                    <!-- 数独棋盘将通过JavaScript生成 -->
                </div>
            </div>
            
            <div class="controls-container">
                <div class="number-pad">
                    <button class="number-btn" data-number="1">1</button>
                    <button class="number-btn" data-number="2">2</button>
                    <button class="number-btn" data-number="3">3</button>
                    <button class="number-btn" data-number="4">4</button>
                    <button class="action-btn" id="delete-btn">删除</button>
                    <button class="number-btn" data-number="5">5</button>
                    <button class="number-btn" data-number="6">6</button>
                    <button class="number-btn" data-number="7">7</button>
                    <button class="number-btn" data-number="8">8</button>
                    <button class="number-btn" data-number="9">9</button>
                </div>
                
                <div class="game-controls">
                    <button class="control-btn hint" id="hint-btn">提示 (3/3)</button>
                    <button class="control-btn mark" id="mark-btn">标记</button>
                    <button class="control-btn" id="new-game-btn">新游戏</button>
                    <button class="control-btn" id="solve-btn">解题</button>
                </div>
                
                <div class="game-stats">
                    <div>已用提示: <span id="hints-used">0</span>/3</div>
                    <div>标记模式: <span id="mark-status">关闭</span></div>
                </div>
            </div>
        </div>

        <!-- 排行榜页面 -->
        <div class="screen leaderboard-screen" id="leaderboard-screen">
            <h1>排行榜</h1>
            <div class="leaderboard-tabs">
                <button class="leaderboard-tab active" data-difficulty="easy">简单</button>
                <button class="leaderboard-tab" data-difficulty="medium">中等</button>
                <button class="leaderboard-tab" data-difficulty="hard">困难</button>
            </div>
            <div class="leaderboard-list" id="leaderboard-list">
                <!-- 排行榜内容将通过JavaScript生成 -->
            </div>
            <div class="btn-container">
                <button class="btn" id="back-btn">返回</button>
            </div>
        </div>
    </div>

    <!-- 完成弹窗 -->
    <div class="completion-modal" id="completion-modal">
        <div class="completion-content">
            <h2>恭喜完成！</h2>
            <p>你成功解决了这个数独谜题</p>
            <div class="completion-time">完成时间: <span id="completion-time">00:00</span></div>
            <div class="completion-buttons">
                <button class="btn completion-new-game" id="completion-new-game">新游戏</button>
                <button class="btn btn-secondary completion-home" id="completion-home">返回主页</button>
            </div>
        </div>
    </div>

    <script>
        // 数独游戏逻辑
        document.addEventListener('DOMContentLoaded', function() {
            // 获取页面元素
            const startScreen = document.getElementById('start-screen');
            const gameScreen = document.getElementById('game-screen');
            const leaderboardScreen = document.getElementById('leaderboard-screen');
            const startBtn = document.getElementById('start-btn');
            const leaderboardBtn = document.getElementById('leaderboard-btn');
            const backBtn = document.getElementById('back-btn');
            const homeBtn = document.getElementById('home-btn');
            
            // 游戏状态变量
            let selectedCell = null;
            let mistakes = 0;
            let startTime = null;
            let timerInterval = null;
            let solution = null;
            let initialBoard = null;
            let currentDifficulty = 'easy';
            let hintsUsed = 0;
            let maxHints = 3;
            let markMode = false;
            let pencilMarks = {}; // 存储标记 {cellIndex: [marked numbers]}
            let leaderboard = {
                easy: [],
                medium: [],
                hard: []
            };
            
            // 获取DOM元素
            const boardElement = document.getElementById('board');
            const timerElement = document.getElementById('timer');
            const mistakesElement = document.getElementById('mistakes');
            const difficultyButtons = document.querySelectorAll('.difficulty-btn');
            const numberButtons = document.querySelectorAll('.number-btn');
            const deleteButton = document.getElementById('delete-btn');
            const hintButton = document.getElementById('hint-btn');
            const newGameButton = document.getElementById('new-game-btn');
            const solveButton = document.getElementById('solve-btn');
            const markButton = document.getElementById('mark-btn');
            const hintsUsedElement = document.getElementById('hints-used');
            const markStatusElement = document.getElementById('mark-status');
            const leaderboardTabs = document.querySelectorAll('.leaderboard-tab');
            const leaderboardList = document.getElementById('leaderboard-list');
            
            // 完成弹窗相关元素
            const completionModal = document.getElementById('completion-modal');
            const completionTimeElement = document.getElementById('completion-time');
            const completionNewGameBtn = document.getElementById('completion-new-game');
            const completionHomeBtn = document.getElementById('completion-home');
            
            // 预定义的数独题目和答案
            const puzzles = {
                easy: {
                    puzzle: [
                        [5, 3, 0, 0, 7, 0, 0, 0, 0],
                        [6, 0, 0, 1, 9, 5, 0, 0, 0],
                        [0, 9, 8, 0, 0, 0, 0, 6, 0],
                        [8, 0, 0, 0, 6, 0, 0, 0, 3],
                        [4, 0, 0, 8, 0, 3, 0, 0, 1],
                        [7, 0, 0, 0, 2, 0, 0, 0, 6],
                        [0, 6, 0, 0, 0, 0, 2, 8, 0],
                        [0, 0, 0, 4, 1, 9, 0, 0, 5],
                        [0, 0, 0, 0, 8, 0, 0, 7, 9]
                    ],
                    solution: [
                        [5, 3, 4, 6, 7, 8, 9, 1, 2],
                        [6, 7, 2, 1, 9, 5, 3, 4, 8],
                        [1, 9, 8, 3, 4, 2, 5, 6, 7],
                        [8, 5, 9, 7, 6, 1, 4, 2, 3],
                        [4, 2, 6, 8, 5, 3, 7, 9, 1],
                        [7, 1, 3, 9, 2, 4, 8, 5, 6],
                        [9, 6, 1, 5, 3, 7, 2, 8, 4],
                        [2, 8, 7, 4, 1, 9, 6, 3, 5],
                        [3, 4, 5, 2, 8, 6, 1, 7, 9]
                    ]
                },
                medium: {
                    puzzle: [
                        [0, 2, 0, 6, 0, 8, 0, 0, 0],
                        [5, 8, 0, 0, 0, 9, 7, 0, 0],
                        [0, 0, 0, 0, 4, 0, 0, 0, 0],
                        [3, 7, 0, 0, 0, 0, 5, 0, 0],
                        [6, 0, 0, 0, 0, 0, 0, 0, 4],
                        [0, 0, 8, 0, 0, 0, 0, 1, 3],
                        [0, 0, 0, 0, 2, 0, 0, 0, 0],
                        [0, 0, 9, 8, 0, 0, 0, 3, 6],
                        [0, 0, 0, 3, 0, 6, 0, 9, 0]
                    ],
                    solution: [
                        [1, 2, 3, 6, 7, 8, 9, 4, 5],
                        [5, 8, 4, 2, 3, 9, 7, 6, 1],
                        [9, 6, 7, 1, 4, 5, 3, 2, 8],
                        [3, 7, 2, 4, 6, 1, 5, 8, 9],
                        [6, 9, 1, 5, 8, 3, 2, 7, 4],
                        [4, 5, 8, 7, 9, 2, 6, 1, 3],
                        [8, 3, 6, 9, 2, 4, 1, 5, 7],
                        [2, 1, 9, 8, 5, 7, 4, 3, 6],
                        [7, 4, 5, 3, 1, 6, 8, 9, 2]
                    ]
                },
                hard: {
                    puzzle: [
                        [8, 0, 0, 0, 0, 0, 0, 0, 0],
                        [0, 0, 3, 6, 0, 0, 0, 0, 0],
                        [0, 7, 0, 0, 9, 0, 2, 0, 0],
                        [0, 5, 0, 0, 0, 7, 0, 0, 0],
                        [0, 0, 0, 0, 4, 5, 7, 0, 0],
                        [0, 0, 0, 1, 0, 0, 0, 3, 0],
                        [0, 0, 1, 0, 0, 0, 0, 6, 8],
                        [0, 0, 8, 5, 0, 0, 0, 1, 0],
                        [0, 9, 0, 0, 0, 0, 4, 0, 0]
                    ],
                    solution: [
                        [8, 1, 2, 7, 5, 3, 6, 4, 9],
                        [9, 4, 3, 6, 8, 2, 1, 7, 5],
                        [6, 7, 5, 4, 9, 1, 2, 8, 3],
                        [1, 5, 4, 2, 3, 7, 8, 9, 6],
                        [3, 6, 9, 8, 4, 5, 7, 2, 1],
                        [2, 8, 7, 1, 6, 9, 5, 3, 4],
                        [5, 2, 1, 9, 7, 4, 3, 6, 8],
                        [4, 3, 8, 5, 2, 6, 9, 1, 7],
                        [7, 9, 6, 3, 1, 8, 4, 5, 2]
                    ]
                }
            };
            
            // 加载排行榜数据
            loadLeaderboard();
            
            // 开始游戏按钮事件
            startBtn.addEventListener('click', function() {
                startScreen.style.display = 'none';
                gameScreen.style.display = 'flex';
                initGame();
            });
            
            // 排行榜按钮事件
            leaderboardBtn.addEventListener('click', function() {
                startScreen.style.display = 'none';
                leaderboardScreen.style.display = 'flex';
                updateLeaderboardDisplay('easy');
            });
            
            // 返回按钮事件
            backBtn.addEventListener('click', function() {
                leaderboardScreen.style.display = 'none';
                startScreen.style.display = 'flex';
            });
            
            // 主页按钮事件
            homeBtn.addEventListener('click', function() {
                gameScreen.style.display = 'none';
                startScreen.style.display = 'flex';
                clearInterval(timerInterval);
            });
            
            // 排行榜标签切换事件
            leaderboardTabs.forEach(tab => {
                tab.addEventListener('click', function() {
                    leaderboardTabs.forEach(t => t.classList.remove('active'));
                    this.classList.add('active');
                    updateLeaderboardDisplay(this.dataset.difficulty);
                });
            });
            
            // 完成弹窗按钮事件
            completionNewGameBtn.addEventListener('click', function() {
                completionModal.style.display = 'none';
                resetGame();
            });
            
            completionHomeBtn.addEventListener('click', function() {
                completionModal.style.display = 'none';
                gameScreen.style.display = 'none';
                startScreen.style.display = 'flex';
                clearInterval(timerInterval);
            });
            
            // 新游戏按钮事件
            newGameButton.addEventListener('click', function() {
                resetGame();
            });
            
            // 初始化游戏函数
            function initGame() {
                generateBoard();
                setupEventListeners();
                generateSudoku();
                startTimer();
            }
            
            // 生成数独棋盘
            function generateBoard() {
                // 清空棋盘
                boardElement.innerHTML = '';
                pencilMarks = {};
                
                // 创建81个单元格
                for (let i = 0; i < 81; i++) {
                    const cell = document.createElement('div');
                    cell.className = 'cell';
                    cell.dataset.index = i;
                    
                    // 创建数字显示区域
                    const numberElement = document.createElement('div');
                    numberElement.className = 'cell-number';
                    cell.appendChild(numberElement);
                    
                    // 创建标记区域
                    const pencilMarkContainer = document.createElement('div');
                    pencilMarkContainer.className = 'pencil-marks';
                    cell.appendChild(pencilMarkContainer);
                    
                    // 添加粗边框
                    const col = i % 9;
                    const row = Math.floor(i / 9);
                    
                    if (col === 2 || col === 5) {
                        cell.classList.add('border-right-thick');
                    }
                    
                    if (row === 2 || row === 5) {
                        cell.classList.add('border-bottom-thick');
                    }
                    
                    boardElement.appendChild(cell);
                }
            }
            
            // 生成数独游戏
            function generateSudoku() {
                // 根据难度选择题目
                const puzzleData = puzzles[currentDifficulty];
                initialBoard = JSON.parse(JSON.stringify(puzzleData.puzzle));
                solution = JSON.parse(JSON.stringify(puzzleData.solution));
                
                // 显示初始棋盘
                displayBoard(initialBoard);
            }
            
            // 显示棋盘
            function displayBoard(board) {
                const cells = document.querySelectorAll('.cell');
                
                for (let i = 0; i < 81; i++) {
                    const row = Math.floor(i / 9);
                    const col = i % 9;
                    const value = board[row][col];
                    
                    // 清除所有选中状态
                    cells[i].classList.remove('selected', 'same-number', 'error');
                    
                    // 设置单元格内容
                    const numberElement = cells[i].querySelector('.cell-number');
                    numberElement.textContent = value === 0 ? '' : value;
                    
                    // 重置单元格样式
                    cells[i].classList.remove('prefilled');
                    
                    // 如果是初始棋盘中的非零数字，标记为预填充
                    if (initialBoard && initialBoard[row][col] !== 0) {
                        cells[i].classList.add('prefilled');
                    }
                    
                    // 更新标记显示
                    updatePencilMarks(i);
                }
                
                // 清除选中的单元格
                selectedCell = null;
            }
            
            // 更新标记显示
            function updatePencilMarks(cellIndex) {
                const cell = document.querySelector(`.cell[data-index="${cellIndex}"]`);
                const pencilMarkContainer = cell.querySelector('.pencil-marks');
                
                // 清空标记容器
                pencilMarkContainer.innerHTML = '';
                
                // 如果有标记，显示它们
                if (pencilMarks[cellIndex] && pencilMarks[cellIndex].length > 0) {
                    // 对标记数字进行排序
                    const sortedMarks = [...pencilMarks[cellIndex]].sort((a, b) => a - b);
                    
                    // 创建标记元素
                    sortedMarks.forEach(number => {
                        const markElement = document.createElement('div');
                        markElement.className = 'pencil-mark';
                        markElement.textContent = number;
                        pencilMarkContainer.appendChild(markElement);
                    });
                }
            }
            
            // 设置事件监听器
            function setupEventListeners() {
                // 单元格点击事件
                boardElement.addEventListener('click', function(e) {
                    const cell = e.target.closest('.cell');
                    if (cell) {
                        // 只有当不是预填充的单元格才能被选中
                        if (!cell.classList.contains('prefilled')) {
                            selectCell(cell);
                        }
                    }
                });
                
                // 难度按钮点击事件
                difficultyButtons.forEach(button => {
                    button.addEventListener('click', function() {
                        // 更新活动按钮
                        difficultyButtons.forEach(btn => btn.classList.remove('active'));
                        this.classList.add('active');
                        
                        // 更新难度
                        currentDifficulty = this.dataset.difficulty;
                        
                        // 重置游戏
                        resetGame();
                    });
                });
                
                // 数字按钮点击事件
                numberButtons.forEach(button => {
                    button.addEventListener('click', function() {
                        if (selectedCell) {
                            const number = parseInt(this.dataset.number);
                            if (markMode) {
                                togglePencilMark(selectedCell, number);
                            } else {
                                inputNumber(number);
                            }
                        }
                    });
                });
                
                // 删除按钮点击事件
                deleteButton.addEventListener('click', function() {
                    if (selectedCell && !selectedCell.classList.contains('prefilled')) {
                        if (markMode) {
                            // 清除所有标记
                            clearPencilMarks(selectedCell);
                        } else {
                            // 清除数字
                            const numberElement = selectedCell.querySelector('.cell-number');
                            numberElement.textContent = '';
                            selectedCell.classList.remove('error');
                        }
                    }
                });
                
                // 提示按钮点击事件
                hintButton.addEventListener('click', function() {
                    if (selectedCell && hintsUsed < maxHints) {
                        const index = parseInt(selectedCell.dataset.index);
                        const row = Math.floor(index / 9);
                        const col = index % 9;
                        
                        // 直接显示答案，不标记为预填充
                        const numberElement = selectedCell.querySelector('.cell-number');
                        numberElement.textContent = solution[row][col];
                        selectedCell.classList.remove('error');
                        hintsUsed++;
                        updateHintButton();
                        
                        // 检查游戏是否完成
                        if (isGameComplete()) {
                            setTimeout(() => {
                                showCompletionModal();
                            }, 500);
                        }
                    } else if (hintsUsed >= maxHints) {
                        alert('提示次数已用完！');
                    }
                });
                
                // 解题按钮点击事件
                solveButton.addEventListener('click', function() {
                    // 直接显示答案，但保留预填充样式
                    const cells = document.querySelectorAll('.cell');
                    for (let i = 0; i < 81; i++) {
                        const row = Math.floor(i / 9);
                        const col = i % 9;
                        const numberElement = cells[i].querySelector('.cell-number');
                        numberElement.textContent = solution[row][col];
                        cells[i].classList.remove('error');
                    }
                });
                
                // 标记按钮点击事件
                markButton.addEventListener('click', function() {
                    toggleMarkMode();
                });
            }
            
            // 显示完成弹窗
            function showCompletionModal() {
                // 停止计时器
                clearInterval(timerInterval);
                
                // 设置完成时间
                completionTimeElement.textContent = timerElement.textContent;
                
                // 保存到排行榜
                saveToLeaderboard();
                
                // 显示弹窗
                completionModal.style.display = 'flex';
            }
            
            // 更新提示按钮状态
            function updateHintButton() {
                hintsUsedElement.textContent = `${hintsUsed}/${maxHints}`;
                hintButton.textContent = `提示 (${maxHints - hintsUsed}/${maxHints})`;
                
                if (hintsUsed >= maxHints) {
                    hintButton.disabled = true;
                } else {
                    hintButton.disabled = false;
                }
            }
            
            // 选择单元格
            function selectCell(cell) {
                // 清除之前的选择
                if (selectedCell) {
                    selectedCell.classList.remove('selected');
                    // 清除相同数字高亮
                    clearHighlights();
                }
                
                // 选择新单元格
                selectedCell = cell;
                cell.classList.add('selected');
                
                // 高亮相同数字
                const numberElement = cell.querySelector('.cell-number');
                highlightSameNumbers(numberElement.textContent);
            }
            
            // 高亮相同数字
            function highlightSameNumbers(number) {
                if (!number) return;
                
                const cells = document.querySelectorAll('.cell');
                cells.forEach(cell => {
                    const numberElement = cell.querySelector('.cell-number');
                    if (numberElement.textContent === number) {
                        cell.classList.add('same-number');
                    }
                });
            }
            
            // 清除高亮
            function clearHighlights() {
                const cells = document.querySelectorAll('.cell');
                cells.forEach(cell => {
                    cell.classList.remove('same-number');
                });
            }
            
            // 输入数字
            function inputNumber(number) {
                if (!selectedCell || selectedCell.classList.contains('prefilled')) return;
                
                const numberElement = selectedCell.querySelector('.cell-number');
                numberElement.textContent = number;
                
                // 检查输入是否正确
                const index = parseInt(selectedCell.dataset.index);
                const row = Math.floor(index / 9);
                const col = index % 9;
                
                if (number !== solution[row][col]) {
                    selectedCell.classList.add('error');
                    mistakes++;
                    mistakesElement.textContent = mistakes;
                    
                    if (mistakes >= 3) {
                        setTimeout(() => {
                            alert('游戏结束！错误次数已达到3次。');
                            resetGame();
                        }, 500);
                    }
                } else {
                    selectedCell.classList.remove('error');
                    
                    // 清除该单元格的标记
                    clearPencilMarks(selectedCell);
                }
                
                // 高亮相同数字
                clearHighlights();
                highlightSameNumbers(number.toString());
                
                // 检查游戏是否完成
                if (isGameComplete()) {
                    setTimeout(() => {
                        showCompletionModal();
                    }, 500);
                }
            }
            
            // 切换标记模式
            function toggleMarkMode() {
                markMode = !markMode;
                markStatusElement.textContent = markMode ? '开启' : '关闭';
                markButton.classList.toggle('active', markMode);
            }
            
            // 切换标记
            function togglePencilMark(cell, number) {
                const index = parseInt(cell.dataset.index);
                
                // 初始化该单元格的标记数组
                if (!pencilMarks[index]) {
                    pencilMarks[index] = [];
                }
                
                // 切换标记
                const markIndex = pencilMarks[index].indexOf(number);
                if (markIndex === -1) {
                    // 添加标记
                    pencilMarks[index].push(number);
                } else {
                    // 移除标记
                    pencilMarks[index].splice(markIndex, 1);
                }
                
                // 更新显示
                updatePencilMarks(index);
            }
            
            // 清除标记
            function clearPencilMarks(cell) {
                const index = parseInt(cell.dataset.index);
                pencilMarks[index] = [];
                updatePencilMarks(index);
            }
            
            // 检查游戏是否完成
            function isGameComplete() {
                const cells = document.querySelectorAll('.cell');
                
                for (let i = 0; i < cells.length; i++) {
                    const numberElement = cells[i].querySelector('.cell-number');
                    if (!numberElement.textContent || cells[i].classList.contains('error')) {
                        return false;
                    }
                }
                
                return true;
            }
            
            // 重置游戏
            function resetGame() {
                clearInterval(timerInterval);
                mistakes = 0;
                mistakesElement.textContent = mistakes;
                hintsUsed = 0;
                updateHintButton();
                
                // 清除选中状态
                if (selectedCell) {
                    selectedCell.classList.remove('selected');
                    selectedCell = null;
                }
                
                markMode = false;
                markStatusElement.textContent = '关闭';
                markButton.classList.remove('active');
                pencilMarks = {}; // 清除所有标记
                generateSudoku();
                startTimer();
            }
            
            // 开始计时器
            function startTimer() {
                startTime = new Date();
                timerInterval = setInterval(updateTimer, 1000);
            }
            
            // 更新计时器
            function updateTimer() {
                const currentTime = new Date();
                const elapsedTime = Math.floor((currentTime - startTime) / 1000);
                const minutes = Math.floor(elapsedTime / 60);
                const seconds = elapsedTime % 60;
                
                timerElement.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            }
            
            // 加载排行榜数据
            function loadLeaderboard() {
                const savedLeaderboard = localStorage.getItem('sudokuLeaderboard');
                if (savedLeaderboard) {
                    leaderboard = JSON.parse(savedLeaderboard);
                }
            }
            
            // 保存排行榜数据
            function saveLeaderboard() {
                localStorage.setItem('sudokuLeaderboard', JSON.stringify(leaderboard));
            }
            
            // 保存到排行榜
            function saveToLeaderboard() {
                const currentTime = timerElement.textContent;
                const timeInSeconds = convertTimeToSeconds(currentTime);
                
                // 添加到排行榜
                leaderboard[currentDifficulty].push({
                    time: currentTime,
                    seconds: timeInSeconds,
                    date: new Date().toLocaleDateString()
                });
                
                // 按时间排序
                leaderboard[currentDifficulty].sort((a, b) => a.seconds - b.seconds);
                
                // 只保留前10名
                if (leaderboard[currentDifficulty].length > 10) {
                    leaderboard[currentDifficulty] = leaderboard[currentDifficulty].slice(0, 10);
                }
                
                // 保存到本地存储
                saveLeaderboard();
            }
            
            // 将时间字符串转换为秒数
            function convertTimeToSeconds(timeStr) {
                const [minutes, seconds] = timeStr.split(':').map(Number);
                return minutes * 60 + seconds;
            }
            
            // 更新排行榜显示
            function updateLeaderboardDisplay(difficulty) {
                leaderboardList.innerHTML = '';
                
                if (leaderboard[difficulty].length === 0) {
                    leaderboardList.innerHTML = '<div class="leaderboard-item">暂无记录</div>';
                    return;
                }
                
                leaderboard[difficulty].forEach((record, index) => {
                    const item = document.createElement('div');
                    item.className = 'leaderboard-item';
                    item.innerHTML = `
                        <div>${index + 1}. ${record.time}</div>
                        <div>${record.date}</div>
                    `;
                    leaderboardList.appendChild(item);
                });
            }
        });
    </script>
</body>
</html>
