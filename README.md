
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>数独游戏</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Arial', sans-serif;
            -webkit-tap-highlight-color: transparent;
            -webkit-touch-callout: none;
            -webkit-user-select: none;
            user-select: none;
        }
        
        body {
            background-color: #f5f5f5;
            color: #333;
            line-height: 1.6;
            padding: 3px;
            max-width: 100%;
            margin: 0 auto;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            overflow-x: hidden;
        }
        
        /* 开始页面样式 */
        .start-screen {
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            padding: 12px;
            text-align: center;
            width: 100%;
            margin-top: 3px;
        }
        
        .start-screen h1 {
            color: #2c3e50;
            font-size: 1.8rem;
            margin-bottom: 12px;
        }
        
        .rules {
            text-align: left;
            margin: 12px 0;
            padding: 10px;
            background-color: #f8f9fa;
            border-radius: 8px;
            font-size: 0.85rem;
        }
        
        .rules h2 {
            color: #3498db;
            margin-bottom: 6px;
            text-align: center;
            font-size: 1rem;
        }
        
        .rules ul {
            padding-left: 16px;
            margin-bottom: 8px;
        }
        
        .rules li {
            margin-bottom: 5px;
        }
        
        .difficulty-selector {
            margin: 12px 0;
            display: flex;
            gap: 6px;
        }
        
        .difficulty-btn {
            flex: 1;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background-color: #f8f9fa;
            color: #495057;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            min-height: 45px;
            font-size: 0.95rem;
        }
        
        .difficulty-btn.active {
            background-color: #3498db;
            color: white;
            border-color: #3498db;
        }
        
        .start-btn {
            padding: 12px 20px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 1rem;
            cursor: pointer;
            transition: background-color 0.2s;
            margin-top: 8px;
            min-height: 45px;
            width: 100%;
            max-width: 180px;
        }
        
        .start-btn:hover {
            background-color: #2980b9;
        }
        
        /* 游戏页面样式 - 进一步增大尺寸 */
        .game-screen {
            display: none;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            padding: 5px;
            margin-bottom: 5px;
            width: 100%;
            min-height: 95vh;
            justify-content: space-between;
            flex-direction: column;
        }
        
        .game-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 5px;
            padding: 0 2px;
        }
        
        .home-btn {
            background: none;
            border: none;
            font-size: 0.95rem;
            cursor: pointer;
            color: #3498db;
            padding: 6px 10px;
            border-radius: 5px;
            transition: background-color 0.2s;
            min-height: 40px;
            min-width: 40px;
        }
        
        .home-btn:hover {
            background-color: #f0f0f0;
        }
        
        .game-info {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
            font-size: 0.95rem;
            width: 100%;
            padding: 0 2px;
        }
        
        .sudoku-board {
            display: grid;
            grid-template-columns: repeat(9, 1fr);
            grid-gap: 1px;
            background-color: #333;
            border: 2px solid #333;
            margin-bottom: 8px;
            border-radius: 5px;
            overflow: hidden;
            width: 100%;
            aspect-ratio: 1 / 1;
            max-width: 100%;
            margin-left: auto;
            margin-right: auto;
        }
        
        .cell {
            background-color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.2rem;
            font-weight: bold;
            cursor: pointer;
            position: relative;
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
            font-size: 0.8rem;
            color: #6c757d;
            line-height: 1;
            padding: 0 1px;
            flex: 1 0 30%;
            text-align: center;
        }
        
        .number-pad {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            grid-gap: 5px;
            margin-bottom: 8px;
            padding: 0 2px;
        }
        
        .number-btn, .action-btn {
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 1.5rem;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.2s;
            min-height: 55px;
        }
        
        .number-btn:hover, .action-btn:hover {
            background-color: #2980b9;
        }
        
        .action-btn {
            background-color: #e74c3c;
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
            grid-gap: 6px;
            margin-bottom: 8px;
            padding: 0 2px;
        }
        
        .control-btn {
            padding: 12px;
            background-color: #34495e;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 0.95rem;
            cursor: pointer;
            transition: background-color 0.2s;
            min-height: 55px;
        }
        
        .control-btn:hover {
            background-color: #2c3e50;
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
            margin-top: 5px;
            font-size: 0.85rem;
            color: #6c757d;
            padding: 0 2px;
        }
        
        /* 游戏记录样式 */
        .records-screen {
            display: none;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            padding: 12px;
            text-align: center;
            width: 100%;
            margin-top: 3px;
        }
        
        .records-screen h1 {
            color: #2c3e50;
            font-size: 1.8rem;
            margin-bottom: 12px;
        }
        
        .records-tabs {
            display: flex;
            margin-bottom: 12px;
            border-bottom: 1px solid #ddd;
        }
        
        .records-tab {
            flex: 1;
            padding: 10px;
            background: none;
            border: none;
            cursor: pointer;
            font-size: 0.95rem;
            transition: all 0.2s;
            min-height: 45px;
        }
        
        .records-tab.active {
            border-bottom: 3px solid #3498db;
            color: #3498db;
            font-weight: bold;
        }
        
        .records-list {
            text-align: left;
            margin-bottom: 12px;
            max-height: 300px;
            overflow-y: auto;
        }
        
        .records-item {
            padding: 10px;
            border-bottom: 1px solid #eee;
            display: flex;
            justify-content: space-between;
            font-size: 0.95rem;
        }
        
        .records-item:nth-child(odd) {
            background-color: #f8f9fa;
        }
        
        .back-btn {
            padding: 12px 20px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 1rem;
            cursor: pointer;
            transition: background-color 0.2s;
            margin-top: 8px;
            min-height: 45px;
            width: 100%;
            max-width: 180px;
        }
        
        .back-btn:hover {
            background-color: #2980b9;
        }
        
        /* 新增样式：数字显示区域 */
        .cell-number {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.2rem;
            font-weight: bold;
            z-index: 1;
        }
        
        /* 完成弹窗样式 - 增大尺寸 */
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
            padding: 15px;
        }
        
        .completion-content {
            background-color: white;
            border-radius: 10px;
            padding: 30px;
            text-align: center;
            width: 100%;
            max-width: 400px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
        }
        
        .completion-content h2 {
            color: #2c3e50;
            margin-bottom: 15px;
            font-size: 1.8rem;
        }
        
        .completion-content p {
            margin-bottom: 18px;
            font-size: 1.1rem;
            color: #555;
        }
        
        .completion-time {
            font-size: 1.4rem;
            font-weight: bold;
            color: #3498db;
            margin: 15px 0;
        }
        
        .completion-buttons {
            display: flex;
            gap: 12px;
            margin-top: 25px;
        }
        
        .completion-btn {
            flex: 1;
            padding: 14px;
            border: none;
            border-radius: 5px;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.2s;
            min-height: 50px;
        }
        
        .completion-new-game {
            background-color: #3498db;
            color: white;
        }
        
        .completion-new-game:hover {
            background-color: #2980b9;
        }
        
        .completion-home {
            background-color: #95a5a6;
            color: white;
        }
        
        .completion-home:hover {
            background-color: #7f8c8d;
        }
        
        /* 移动端特定样式 */
        @media (max-width: 480px) {
            body {
                padding: 2px;
                justify-content: flex-start;
            }
            
            .start-screen, .game-screen, .records-screen {
                padding: 8px;
            }
            
            .start-screen h1, .records-screen h1 {
                font-size: 1.6rem;
            }
            
            .sudoku-board {
                max-width: 100%;
            }
            
            .cell {
                font-size: 2rem;
            }
            
            .cell-number {
                font-size: 2rem;
            }
            
            .pencil-mark {
                font-size: 0.7rem;
            }
            
            .number-btn, .action-btn {
                font-size: 1.4rem;
                min-height: 50px;
            }
            
            .control-btn {
                font-size: 0.9rem;
                min-height: 50px;
                padding: 10px;
            }
            
            .game-info, .game-stats {
                font-size: 0.85rem;
            }
            
            .completion-content {
                padding: 25px;
                max-width: 350px;
            }
            
            .completion-content h2 {
                font-size: 1.6rem;
            }
            
            .completion-time {
                font-size: 1.3rem;
            }
        }
        
        @media (max-width: 360px) {
            .cell {
                font-size: 1.8rem;
            }
            
            .cell-number {
                font-size: 1.8rem;
            }
            
            .pencil-mark {
                font-size: 0.65rem;
            }
            
            .number-btn, .action-btn {
                font-size: 1.3rem;
                min-height: 48px;
            }
            
            .control-btn {
                font-size: 0.85rem;
                padding: 8px;
            }
            
            .difficulty-btn, .start-btn, .back-btn {
                min-height: 42px;
                font-size: 0.9rem;
            }
            
            .completion-content {
                padding: 20px;
                max-width: 320px;
            }
            
            .completion-content h2 {
                font-size: 1.4rem;
            }
            
            .completion-time {
                font-size: 1.2rem;
            }
        }
        
        /* 横屏优化 */
        @media (max-height: 500px) and (orientation: landscape) {
            body {
                padding: 2px;
                justify-content: flex-start;
            }
            
            .game-screen {
                padding: 4px;
            }
            
            .sudoku-board {
                margin-bottom: 6px;
                max-height: 50vh;
            }
            
            .game-controls {
                grid-template-columns: repeat(4, 1fr);
            }
            
            .number-pad {
                grid-template-columns: repeat(10, 1fr);
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
    <!-- 开始页面 -->
    <div class="start-screen" id="start-screen">
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
        <div class="difficulty-selector">
            <button class="difficulty-btn active" data-difficulty="easy">简单</button>
            <button class="difficulty-btn" data-difficulty="medium">中等</button>
            <button class="difficulty-btn" data-difficulty="hard">困难</button>
        </div>
        <button class="start-btn" id="start-btn">开始游戏</button>
        <button class="start-btn" id="records-btn" style="background-color: #9b59b6; margin-left: 0; margin-top: 8px;">游戏记录</button>
    </div>

    <!-- 游戏页面 -->
    <div class="game-screen" id="game-screen">
        <div class="game-header">
            <button class="home-btn" id="home-btn">主页</button>
            <div class="game-info">
                <div class="timer">时间: <span id="timer">00:00</span></div>
                <div class="mistakes">错误: <span id="mistakes">0</span>/3</div>
            </div>
            <div style="width: 40px;"></div> <!-- 占位符，保持布局平衡 -->
        </div>
        
        <div class="sudoku-board" id="board">
            <!-- 数独棋盘将通过JavaScript生成 -->
        </div>
        
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

    <!-- 游戏记录页面 -->
    <div class="records-screen" id="records-screen">
        <h1>游戏记录</h1>
        <div class="records-tabs">
            <button class="records-tab active" data-difficulty="easy">简单</button>
            <button class="records-tab" data-difficulty="medium">中等</button>
            <button class="records-tab" data-difficulty="hard">困难</button>
        </div>
        <div class="records-list" id="records-list">
            <!-- 游戏记录内容将通过JavaScript生成 -->
        </div>
        <button class="back-btn" id="back-btn">返回</button>
    </div>

    <!-- 完成弹窗 -->
    <div class="completion-modal" id="completion-modal">
        <div class="completion-content">
            <h2>恭喜完成！</h2>
            <p>你成功解决了这个数独谜题</p>
            <div class="completion-time">完成时间: <span id="completion-time">00:00</span></div>
            <div class="completion-buttons">
                <button class="completion-btn completion-new-game" id="completion-new-game">新游戏</button>
                <button class="completion-btn completion-home" id="completion-home">返回主页</button>
            </div>
        </div>
    </div>

    <script>
        // 数独游戏逻辑
        document.addEventListener('DOMContentLoaded', function() {
            // 获取页面元素
            const startScreen = document.getElementById('start-screen');
            const gameScreen = document.getElementById('game-screen');
            const recordsScreen = document.getElementById('records-screen');
            const startBtn = document.getElementById('start-btn');
            const recordsBtn = document.getElementById('records-btn');
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
            let records = {
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
            const recordsTabs = document.querySelectorAll('.records-tab');
            const recordsList = document.getElementById('records-list');
            
            // 完成弹窗相关元素
            const completionModal = document.getElementById('completion-modal');
            const completionTimeElement = document.getElementById('completion-time');
            const completionNewGameBtn = document.getElementById('completion-new-game');
            const completionHomeBtn = document.getElementById('completion-home');
            
            // 加载游戏记录数据
            loadRecords();
            
            // 开始游戏按钮事件
            startBtn.addEventListener('click', function() {
                startScreen.style.display = 'none';
                gameScreen.style.display = 'flex';
                initGame();
            });
            
            // 游戏记录按钮事件
            recordsBtn.addEventListener('click', function() {
                startScreen.style.display = 'none';
                recordsScreen.style.display = 'block';
                updateRecordsDisplay('easy');
            });
            
            // 返回按钮事件
            backBtn.addEventListener('click', function() {
                recordsScreen.style.display = 'none';
                startScreen.style.display = 'block';
            });
            
            // 主页按钮事件
            homeBtn.addEventListener('click', function() {
                gameScreen.style.display = 'none';
                startScreen.style.display = 'block';
                clearInterval(timerInterval);
            });
            
            // 难度按钮点击事件（在开始页面）
            difficultyButtons.forEach(button => {
                button.addEventListener('click', function() {
                    // 更新活动按钮
                    difficultyButtons.forEach(btn => btn.classList.remove('active'));
                    this.classList.add('active');
                    
                    // 更新难度
                    currentDifficulty = this.dataset.difficulty;
                });
            });
            
            // 游戏记录标签切换事件
            recordsTabs.forEach(tab => {
                tab.addEventListener('click', function() {
                    recordsTabs.forEach(t => t.classList.remove('active'));
                    this.classList.add('active');
                    updateRecordsDisplay(this.dataset.difficulty);
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
                startScreen.style.display = 'block';
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
            
            // 生成数独题目
            function generateSudoku() {
                // 生成完整解
                solution = generateSolution();
                
                // 创建初始棋盘
                initialBoard = JSON.parse(JSON.stringify(solution));
                
                // 根据难度挖空
                const emptyCells = getEmptyCellsByDifficulty();
                const positions = [];
                
                // 创建所有可能的位置列表
                for (let i = 0; i < 9; i++) {
                    for (let j = 0; j < 9; j++) {
                        positions.push([i, j]);
                    }
                }
                
                // 随机打乱位置
                shuffleArray(positions);
                
                // 尝试挖空每个位置，但确保有唯一解
                let removed = 0;
                for (let i = 0; i < positions.length && removed < emptyCells; i++) {
                    const [row, col] = positions[i];
                    const temp = initialBoard[row][col];
                    
                    // 如果已经是空的，跳过
                    if (temp === 0) continue;
                    
                    // 尝试挖空
                    initialBoard[row][col] = 0;
                    
                    // 检查是否有唯一解
                    if (hasUniqueSolution(JSON.parse(JSON.stringify(initialBoard)))) {
                        removed++;
                    } else {
                        // 如果没有唯一解，恢复这个数字
                        initialBoard[row][col] = temp;
                    }
                }
                
                // 显示初始棋盘
                displayBoard(initialBoard);
            }
            
            // 检查数独是否有唯一解
            function hasUniqueSolution(board) {
                let solutions = 0;
                
                function solve() {
                    for (let row = 0; row < 9; row++) {
                        for (let col = 0; col < 9; col++) {
                            if (board[row][col] === 0) {
                                for (let num = 1; num <= 9; num++) {
                                    if (isValid(board, row, col, num)) {
                                        board[row][col] = num;
                                        
                                        if (solve()) {
                                            // 如果已经找到一个解，继续寻找第二个解
                                            if (solutions >= 1) {
                                                return true;
                                            }
                                        }
                                        
                                        board[row][col] = 0;
                                    }
                                }
                                return false;
                            }
                        }
                    }
                    
                    // 找到一个解
                    solutions++;
                    return true;
                }
                
                // 开始求解
                solve();
                
                // 返回是否有唯一解
                return solutions === 1;
            }
            
            // 根据难度获取需要挖空的单元格数量
            function getEmptyCellsByDifficulty() {
                switch(currentDifficulty) {
                    case 'easy': return 30; // 简单难度保留51个数字
                    case 'medium': return 40; // 中等难度保留41个数字
                    case 'hard': return 50; // 困难难度保留31个数字
                    default: return 30;
                }
            }
            
            // 生成完整的数独解决方案
            function generateSolution() {
                // 创建一个9x9的空数独
                const board = Array(9).fill().map(() => Array(9).fill(0));
                
                // 使用回溯算法填充数独
                solveSudoku(board);
                
                return board;
            }
            
            // 数独求解算法（回溯法）
            function solveSudoku(board) {
                for (let row = 0; row < 9; row++) {
                    for (let col = 0; col < 9; col++) {
                        if (board[row][col] === 0) {
                            // 随机尝试数字
                            const numbers = shuffleArray([1, 2, 3, 4, 5, 6, 7, 8, 9]);
                            
                            for (let num of numbers) {
                                if (isValid(board, row, col, num)) {
                                    board[row][col] = num;
                                    
                                    if (solveSudoku(board)) {
                                        return true;
                                    }
                                    
                                    board[row][col] = 0;
                                }
                            }
                            return false;
                        }
                    }
                }
                return true;
            }
            
            // 检查数字在指定位置是否有效
            function isValid(board, row, col, num) {
                // 检查行
                for (let x = 0; x < 9; x++) {
                    if (board[row][x] === num) return false;
                }
                
                // 检查列
                for (let x = 0; x < 9; x++) {
                    if (board[x][col] === num) return false;
                }
                
                // 检查3x3宫格
                const startRow = Math.floor(row / 3) * 3;
                const startCol = Math.floor(col / 3) * 3;
                
                for (let i = 0; i < 3; i++) {
                    for (let j = 0; j < 3; j++) {
                        if (board[startRow + i][startCol + j] === num) return false;
                    }
                }
                
                return true;
            }
            
            // 随机打乱数组
            function shuffleArray(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
                return array;
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
                
                // 保存到游戏记录
                saveToRecords();
                
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
            
            // 加载游戏记录数据
            function loadRecords() {
                const savedRecords = localStorage.getItem('sudokuRecords');
                if (savedRecords) {
                    records = JSON.parse(savedRecords);
                }
            }
            
            // 保存游戏记录数据
            function saveRecords() {
                localStorage.setItem('sudokuRecords', JSON.stringify(records));
            }
            
            // 保存到游戏记录
            function saveToRecords() {
                const currentTime = timerElement.textContent;
                const timeInSeconds = convertTimeToSeconds(currentTime);
                
                // 添加到游戏记录
                records[currentDifficulty].push({
                    time: currentTime,
                    seconds: timeInSeconds,
                    date: new Date().toLocaleDateString()
                });
                
                // 按时间排序
                records[currentDifficulty].sort((a, b) => a.seconds - b.seconds);
                
                // 只保留前10名
                if (records[currentDifficulty].length > 10) {
                    records[currentDifficulty] = records[currentDifficulty].slice(0, 10);
                }
                
                // 保存到本地存储
                saveRecords();
            }
            
            // 将时间字符串转换为秒数
            function convertTimeToSeconds(timeStr) {
                const [minutes, seconds] = timeStr.split(':').map(Number);
                return minutes * 60 + seconds;
            }
            
            // 更新游戏记录显示
            function updateRecordsDisplay(difficulty) {
                recordsList.innerHTML = '';
                
                if (records[difficulty].length === 0) {
                    recordsList.innerHTML = '<div class="records-item">暂无记录</div>';
                    return;
                }
                
                records[difficulty].forEach((record, index) => {
                    const item = document.createElement('div');
                    item.className = 'records-item';
                    item.innerHTML = `
                        <div>${index + 1}. ${record.time}</div>
                        <div>${record.date}</div>
                    `;
                    recordsList.appendChild(item);
                });
            }
        });
    </script>
</body>
</html>
