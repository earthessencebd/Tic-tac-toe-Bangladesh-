<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>টিক-ট্যাক-টো গেম</title>
    <!-- CSS ফাইলকে লিঙ্ক করা হয়েছে -->
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="game-container">
        <h1>টিক-ট্যাক-টো</h1>

        <!-- মোড সিলেকশন স্ক্রিন -->
        <div id="modeSelectionScreen" class="mode-selection">
            <h2>কার সাথে খেলতে চান?</h2>
            <button id="playWithAI" class="mode-button">AI এর সাথে</button>
            <button id="playWithFriend" class="mode-button">বন্ধুর সাথে</button>
        </div>

        <!-- খেলোয়াড় প্রতীক এবং AI ডিফিকাল্টি প্যানেল -->
        <div id="playerChoicePanel" class="player-choice-panel">
            <h2>আপনি কি দিয়ে খেলা শুরু করতে চাচ্ছেন?</h2>
            <div class="player-choice-buttons">
                <button id="chooseX" class="choice-button">X</button>
                <button id="chooseO" class="choice-button">O</button>
            </div>

            <div id="aiDifficultyPanel" class="ai-difficulty-panel hidden">
                <label for="aiDifficultySlider">AI ক্ষমতা:</label>
                <input type="range" id="aiDifficultySlider" min="1" max="100" value="75">
                <div id="aiPowerDisplay">৭৫%</div>
            </div>
        </div>

        <!-- গেম প্লে স্ক্রিন -->
        <div id="gamePlayScreen" class="hidden">
            <div id="gameStatus"></div>
            <div class="game-board" id="gameBoard">
                <!-- জাভাস্ক্রিপ্ট দিয়ে এখানে সেল তৈরি হবে -->
            </div>
            <div id="winnerMessage" class="hidden">Winner!</div>
            <div class="game-buttons">
                <button id="newGameButton">নতুন খেলা</button>
                <button id="restartButton">পুনরায় শুরু করুন</button>
            </div>
        </div>
    </div>

    <!-- JavaScript ফাইলকে লিঙ্ক করা হয়েছে -->
    <script src="script.js"></script>
</body>
</html>
/* গেমের মূল স্টাইলিং */
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    background-color: #2c3e50; /* গাঢ় ব্যাকগ্রাউন্ড */
    color: #ecf0f1;
    overflow: hidden; /* অতিরিক্ত স্ক্রল বার এড়াতে */
}

.game-container {
    background-color: #34495e; /* কন্টেইনারের গাঢ় রঙ */
    padding: 30px;
    border-radius: 20px;
    box-shadow: 0 15px 30px rgba(0, 0, 0, 0.4);
    text-align: center;
    width: 90%; /* মোবাইল ডিভাইসের জন্য প্রস্থ সেট করা */
    max-width: 450px; /* সর্বোচ্চ প্রস্থ */
    margin: 20px; /* চারদিকে মার্জিন */
    box-sizing: border-box; /* প্যাডিং সহ প্রস্থ হিসাব */
    position: relative;
    overflow: hidden;
    border: 2px solid #2ecc71; /* সবুজ বর্ডার */
}

h1 {
    color: #2ecc71; /* সবুজ টেক্সট */
    margin-bottom: 25px;
    font-size: 2.5rem;
    text-shadow: 2px 2px 5px rgba(0,0,0,0.2);
    letter-spacing: 1px;
}

/* স্ট্যাটাস ডিসপ্লে */
#gameStatus {
    font-size: 1.6rem;
    margin-bottom: 30px;
    color: #f1c40f; /* উজ্জ্বল হলুদ */
    font-weight: bold;
    text-shadow: 1px 1px 3px rgba(0,0,0,0.3);
}

/* গেম বোর্ড স্টাইল */
.game-board {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 12px; /* সেলের মাঝে বেশি ফাঁকা স্থান */
    width: 100%;
    height: auto;
    aspect-ratio: 1 / 1; /* বোর্ডকে বর্গাকার রাখা */
    margin: 0 auto 30px auto;
    background-color: #4a627a; /* বোর্ডের ভিতরের রঙ */
    border-radius: 15px;
    padding: 12px;
    box-shadow: inset 0 0 15px rgba(0, 0, 0, 0.3);
    position: relative; /* বিজয় রেখার জন্য */
}

/* প্রতিটি সেলের স্টাইল */
.cell {
    width: 100%;
    height: auto;
    aspect-ratio: 1 / 1; /* সেলের বর্গাকার আকার নিশ্চিত করা */
    background-color: #ecf0f1; /* সেলের রঙ */
    border: 3px solid #7f8c8d; /* সেলের বর্ডার */
    border-radius: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 4.5rem; /* X/O এর আকার */
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.2s ease, box-shadow 0.3s ease;
    user-select: none;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.cell:hover {
    background-color: #e0e6ea;
    transform: scale(1.03);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
}

.cell.X {
    color: #e74c3c; /* X এর জন্য লাল রঙ */
}

.cell.O {
    color: #3498db; /* O এর জন্য নীল রঙ */
}

/* মোড সিলেকশন স্ক্রিন */
.mode-selection {
    display: flex;
    flex-direction: column;
    gap: 20px;
    margin-bottom: 30px;
}

.mode-button {
    background-color: #3498db;
    color: #ffffff;
    padding: 15px 30px;
    border: none;
    border-radius: 8px;
    font-size: 1.2rem;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.2s ease, box-shadow 0.3s ease;
    box-shadow: 0 5px 15px rgba(52, 152, 219, 0.3);
    text-transform: uppercase;
    letter-spacing: 1px;
}

.mode-button:hover {
    background-color: #2980b9;
    transform: translateY(-2px);
    box-shadow: 0 7px 20px rgba(52, 152, 219, 0.4);
}

.mode-button:active {
    transform: translateY(0);
    box-shadow: 0 3px 10px rgba(52, 152, 219, 0.3);
}

/* গেমপ্লে স্ক্রিনের বাটন কন্টেইনার */
.game-buttons {
    display: flex;
    justify-content: space-between;
    width: 100%;
    margin-top: 20px;
}

#newGameButton, #restartButton {
    background-color: #2ecc71;
    color: #ffffff;
    padding: 15px 20px;
    border: none;
    border-radius: 8px;
    font-size: 1.1rem;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.2s ease, box-shadow 0.3s ease;
    box-shadow: 0 5px 15px rgba(46, 204, 113, 0.3);
    text-transform: uppercase;
    letter-spacing: 1px;
    flex: 1;
    margin: 0 5px;
}

/* রিস্টার্ট বাটনের আলাদা স্টাইল */
#restartButton {
    background-color: #e74c3c;
    box-shadow: 0 5px 15px rgba(231, 76, 60, 0.3);
}

#restartButton:hover {
    background-color: #c0392b;
    box-shadow: 0 7px 20px rgba(231, 76, 60, 0.4);
}

/* হিডেন ক্লাস - এলিমেন্ট লুকানোর জন্য */
.hidden {
    display: none !important;
}
/* ট্রানজিশন সক্ষম করার জন্য অতিরিক্ত ক্লাস */
.visually-hidden {
    opacity: 0;
    pointer-events: none;
    transform: translateX(-100%);
    transition: opacity 0.5s ease-out, transform 0.5s ease-out;
}
.visible {
    opacity: 1;
    pointer-events: auto;
    transform: translateX(0);
}


/* বিজয় রেখার স্টাইল */
.winning-line {
    position: absolute;
    background-color: rgba(0, 0, 0, 0.8); /* কালো ও কিছুটা স্বচ্ছ */
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5); /* সূক্ষ্ম কালো শ্যাডো */
    z-index: 2;
    animation: drawLine 0.6s forwards ease-out; /* রেখা আঁকার অ্যানিমেশন */
    border-radius: 2px; /* আরও চিকন লাইন */
    height: 5px; /* রেখার মোটা */
    transform-origin: 0 50%; /* বাম-মাঝ থেকে শুরু */
    opacity: 0; /* শুরুতে লুকানো */
}
@keyframes drawLine {
    0% { transform: scaleX(0); opacity: 0; }
    100% { transform: scaleX(1); opacity: 1; }
}

/* Winner মেসেজের স্টাইল */
#winnerMessage {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 3.5rem;
    color: #f1c40f; /* কমলা রঙ */
    font-weight: bold;
    z-index: 4;
    text-shadow: 3px 3px 8px rgba(0,0,0,0.5);
    animation: fadeInScale 0.9s ease-out forwards;
    opacity: 0;
    letter-spacing: 2px;
}

@keyframes fadeInScale {
    0% { opacity: 0; transform: translate(-50%, -50%) scale(0.5); }
    100% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
}

/* কনফেটি স্টাইল */
.confetti {
    position: absolute;
    width: 12px; /* কিছুটা বড় */
    height: 12px;
    background-color: #f0f; /* ডিফল্ট রঙ */
    opacity: 0;
    animation: fall 3.5s forwards ease-out; /* দীর্ঘ এবং মসৃণ পতন */
    border-radius: 50%;
    z-index: 5;
    box-shadow: 0 0 5px rgba(0,0,0,0.3);
}

@keyframes fall {
    0% { transform: translateY(-100px) rotate(0deg); opacity: 1; }
    100% { transform: translateY(calc(100vh + 100px)) rotate(720deg); opacity: 0; }
}

/* খেলোয়াড় নির্বাচন প্যানেল */
.player-choice-panel {
    position: absolute;
    top: 0;
    left: 0; /* শুরুতেই পজিশন 0 */
    width: 100%;
    height: 100%;
    background-color: rgba(52, 73, 94, 0.98); /* গাঢ় স্বচ্ছ ব্যাকগ্রাউন্ড */
    display: none; /* JavaScript দিয়ে Active হলে Flex হবে */
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 20px;
    transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out; /* স্লাইড ট্রানজিশন */
    z-index: 10;
    border-radius: 15px;
    padding: 20px;
    box-sizing: border-box;
    transform: translateX(100%); /* শুরুতে ডান দিকে লুকানো */
    opacity: 0; /* শুরুতে লুকানো */
    pointer-events: none; /* যাতে ক্লিক না হয় */
}
.player-choice-panel.active {
    transform: translateX(0); /* স্লাইদ হয়ে আসবে */
    opacity: 1; /* দেখা যাবে */
    pointer-events: auto; /* ক্লিক করা যাবে */
    display: flex; /* Show when active */
}


.player-choice-panel h2 {
    color: #ecf0f1;
    font-size: 1.8rem;
    margin-bottom: 20px;
}

.player-choice-buttons {
    display: flex;
    gap: 20px;
}

.player-choice-buttons .choice-button {
    background-color: #9b59b6;
    color: #ffffff;
    padding: 15px 25px;
    border: none;
    border-radius: 8px;
    font-size: 1.5rem;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.2s ease, box-shadow 0.3s ease;
    box-shadow: 0 5px 15px rgba(155, 89, 182, 0.3);
    width: 80px;
    height: 80px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.player-choice-buttons .choice-button:hover {
    background-color: #8e44ad;
    transform: translateY(-2px);
    box-shadow: 0 7px 20px rgba(155, 89, 182, 0.4);
}

/* AI ডিফিকাল্টি প্যানেল */
.ai-difficulty-panel {
    margin-top: 30px;
    width: 80%;
}

.ai-difficulty-panel label {
    font-size: 1.1rem;
    font-weight: bold;
    color: #ecf0f1;
    display: block;
    margin-bottom: 10px;
}

.ai-difficulty-panel input[type="range"] {
    width: 100%;
    -webkit-appearance: none;
    height: 10px;
    border-radius: 5px;
    background: #7f8c8d;
    outline: none;
    opacity: 0.8;
    -webkit-transition: .2s;
    transition: opacity .2s;
    box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
}

.ai-difficulty-panel input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 25px;
    height: 25px;
    border-radius: 50%;
    background: #e67e22; /* কমলা রঙ */
    cursor: pointer;
    box-shadow: 0 2px 8px rgba(0,0,0,0.4);
}

.ai-difficulty-panel input[type="range"]::-moz-range-thumb {
    width: 25px;
    height: 25px;
    border-radius: 50%;
    background: #e67e22; /* কমলা রঙ */
    cursor: pointer;
    box-shadow: 0 2px 8px rgba(0,0,0,0.4);
}

#aiPowerDisplay {
    font-size: 1.2rem;
    font-weight: bold;
    color: #e67e22;
    margin-top: 10px;
}

/* মোবাইল ডিভাইসের জন্য অতিরিক্ত রেসপনসিভনেস */
@media (max-width: 480px) {
    .game-container {
        padding: 20px;
        margin: 10px;
    }
    h1 {
        font-size: 1.8rem;
    }
    #gameStatus {
        font-size: 1.2rem;
    }
    .cell {
        font-size: 3rem; /* ছোট স্ক্রিনে X/O এর আকার কমানো */
    }
    .mode-button, #newGameButton, #restartButton, .player-choice-buttons .choice-button {
        padding: 12px 18px;
        font-size: 0.9rem;
    }
    #winnerMessage {
        font-size: 2rem;
    }
    .ai-difficulty-panel {
        width: 90%;
    }
}
// জাভাস্ক্রিপ্ট কোড শুরু
const gameBoard = document.getElementById('gameBoard');
const gameStatus = document.getElementById('gameStatus');
const newGameButton = document.getElementById('newGameButton');
const restartButton = document.getElementById('restartButton');
const modeSelectionScreen = document.getElementById('modeSelectionScreen');
const playerChoicePanel = document.getElementById('playerChoicePanel');
const gamePlayScreen = document.getElementById('gamePlayScreen');
const playWithAIButton = document.getElementById('playWithAI');
const playWithFriendButton = document.getElementById('playWithFriend');
const chooseXButton = document.getElementById('chooseX');
const chooseOButton = document.getElementById('chooseO');
const winnerMessage = document.getElementById('winnerMessage');
const aiDifficultyPanel = document.getElementById('aiDifficultyPanel');
const aiDifficultySlider = document.getElementById('aiDifficultySlider');
const aiPowerDisplay = document.getElementById('aiPowerDisplay');

let board = ['', '', '', '', '', '', '', '', ''];
let currentPlayer = 'X';
let gameActive = true;
let gameMode = 'none';
let humanPlayerSymbol = 'X';
let AI_PLAYER_SYMBOL = 'O';
let currentAIDifficulty = 0.75; // AI এর ক্ষমতা: 0.0 (সবচেয়ে সহজ) থেকে 1.0 (সবচেয়ে কঠিন) - এখন ৭৫%

const winningConditions = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8],
    [0, 3, 6], [1, 4, 7], [2, 5, 8],
    [0, 4, 8], [2, 4, 6]
];

const statusMessages = {
    playerXTurn: 'খেলোয়াড় X এর পালা',
    playerOTurn: 'খেলোয়াড় O এর পালা',
    playerXWins: 'খেলোয়াড় X জিতেছে!',
    playerOWins: 'খেলোয়াড় O জিতেছে!',
    draw: 'খেলা ড্র হয়েছে!'
};

// --- Screen Management Functions ---
function showScreen(screenToShow) {
    const allScreens = [modeSelectionScreen, playerChoicePanel, gamePlayScreen];
    allScreens.forEach(screen => {
        if (screen === playerChoicePanel) {
            screen.classList.remove('active');
            if (screen !== screenToShow) {
                setTimeout(() => screen.classList.add('hidden'), 300);
            }
        } else if (screen !== screenToShow) {
            screen.classList.add('hidden');
            screen.classList.remove('visible');
        }
    });

    if (screenToShow === playerChoicePanel) {
        screenToShow.classList.remove('hidden');
        setTimeout(() => screenToShow.classList.add('active'), 10);
    } else {
        screenToShow.classList.remove('hidden');
        screenToShow.classList.add('visible');
    }
}

// --- Game Logic Functions ---
function createBoard() {
    gameBoard.innerHTML = '';
    for (let i = 0; i < 9; i++) {
        const cell = document.createElement('div');
        cell.classList.add('cell');
        cell.setAttribute('data-cell-index', i);
        cell.addEventListener('click', handleCellClick);
        gameBoard.appendChild(cell);
    }
    updateStatus(currentPlayer === 'X' ? statusMessages.playerXTurn : statusMessages.playerOTurn);
}

function handleCellClick(event) {
    const clickedCell = event.target;
    const clickedCellIndex = parseInt(clickedCell.getAttribute('data-cell-index'));

    if (board[clickedCellIndex] !== '' || !gameActive) {
        return;
    }

    if (gameMode === 'ai' && currentPlayer === AI_PLAYER_SYMBOL) {
        return;
    }

    makeMove(clickedCellIndex);
}

function makeMove(index) {
    board[index] = currentPlayer;
    const cellElement = document.querySelector(`[data-cell-index="${index}"]`);
    cellElement.innerHTML = currentPlayer;
    cellElement.classList.add(currentPlayer);

    checkResult();
    if (gameActive) {
        handlePlayerChange();
        if (gameMode === 'ai' && currentPlayer === AI_PLAYER_SYMBOL && gameActive) {
            setTimeout(makeAIMove, 700);
        }
    }
}

function handlePlayerChange() {
    currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
    updateStatus(currentPlayer === 'X' ? statusMessages.playerXTurn : statusMessages.playerOTurn);
}

function checkResult() {
    let roundWon = false;
    let winningCombo = [];

    for (let i = 0; i < winningConditions.length; i++) {
        const winCondition = winningConditions[i];
        let a = board[winCondition[0]];
        let b = board[winCondition[1]];
        let c = board[winCondition[2]];

        if (a === '' || b === '' || c === '') {
            continue;
        }
        if (a === b && b === c) {
            roundWon = true;
            winningCombo = winCondition;
            break;
        }
    }

    if (roundWon) {
        updateStatus(currentPlayer === 'X' ? statusMessages.playerXWins : statusMessages.playerOWins);
        gameActive = false;
        drawWinningLine(winningCombo);
        displayWinnerMessage();
        startConfetti();
        return;
    }

    let roundDraw = !board.includes('');
    if (roundDraw) {
        updateStatus(statusMessages.draw);
        gameActive = false;
        return;
    }
}

function updateStatus(message) {
    gameStatus.innerHTML = message;
}

// --- Reset / Restart Functions ---
function startNewGame() {
    console.log("নতুন খেলা শুরু হচ্ছে (সফট রিসেট)...");
    board = ['', '', '', '', '', '', '', '', ''];
    currentPlayer = 'X';
    gameActive = true;

    clearWinningVisuals();
    createBoard();
    updateStatus(currentPlayer === 'X' ? statusMessages.playerXTurn : statusMessages.playerOTurn);

    if (gameMode === 'ai' && currentPlayer === AI_PLAYER_SYMBOL && gameActive) {
        setTimeout(makeAIMove, 700);
    }
}

function fullRestartGame() {
    console.log("গেম সম্পূর্ণ পুনরায় শুরু হচ্ছে (ফুল রিসেট)...");
    board = ['', '', '', '', '', '', '', '', ''];
    currentPlayer = 'X';
    gameActive = true;
    gameMode = 'none';
    humanPlayerSymbol = 'X';
    currentAIDifficulty = 0.75; // AI ডিফিকাল্টি রিসেট ৭৫%

    clearWinningVisuals();
    showScreen(modeSelectionScreen);
    
    aiDifficultyPanel.classList.add('hidden');
    aiDifficultySlider.value = 75; // স্লাইডার ভ্যালুও ৭৫% সেট করা
    aiPowerDisplay.textContent = '৭৫%';

    updateStatus('');
    gameBoard.innerHTML = '';
}

// --- AI Logic ---
function makeAIMove() {
    const emptyCells = [];
    for (let i = 0; i < board.length; i++) {
        if (board[i] === '') {
            emptyCells.push(i);
        }
    }

    if (emptyCells.length === 0) {
        return;
    }

    let bestMove = -1;

    // AI DIFFICULTY লজিক: র্যান্ডম() < currentAIDifficulty হলে স্মার্ট মুভ, অন্যথায় র‍্যান্ডম মুভ
    if (Math.random() < currentAIDifficulty) {
        // স্মার্ট মুভ লজিক
        // 1. AI কি এখন জিততে পারে?
        for (let i = 0; i < emptyCells.length; i++) {
            const testBoard = [...board];
            testBoard[emptyCells[i]] = AI_PLAYER_SYMBOL;
            if (checkWinnerForPlayer(testBoard, AI_PLAYER_SYMBOL)) {
                bestMove = emptyCells[i];
                break;
            }
        }

        // 2. ইউজার কি পরের চালে জিততে পারে? তাহলে তাকে ব্লক করো।
        if (bestMove === -1) {
            for (let i = 0; i < emptyCells.length; i++) {
                const testBoard = [...board];
                testBoard[emptyCells[i]] = humanPlayerSymbol;
                if (checkWinnerForPlayer(testBoard, humanPlayerSymbol)) {
                    bestMove = emptyCells[i];
                    break;
                }
            }
        }

        // 3. কেন্দ্র (৪র্থ ইন্ডেক্স) কি খালি আছে?
        if (bestMove === -1 && board[4] === '') {
            bestMove = 4;
        }

        // 4. কোণার ঘরগুলো কি খালি আছে?
        const corners = [0, 2, 6, 8];
        if (bestMove === -1) {
            for (let i = 0; i < corners.length; i++) {
                if (board[corners[i]] === '') {
                    bestMove = corners[i];
                    break;
                }
            }
        }
    }

    if (bestMove === -1) {
        bestMove = emptyCells[Math.floor(Math.random() * emptyCells.length)];
    }

    makeMove(bestMove);
}

function checkWinnerForPlayer(currentBoard, player) {
    for (let i = 0; i < winningConditions.length; i++) {
        const [a, b, c] = winningConditions[i];
        if (currentBoard[a] === player && currentBoard[b] === player && currentBoard[c] === player) {
            return true;
        }
    }
    return false;
}

// --- Winning Visuals Functions ---
function drawWinningLine(winningCombo) {
    const cells = document.querySelectorAll('.cell');
    const startCell = cells[winningCombo[0]];
    const endCell = cells[winningCombo[2]];

    const boardRect = gameBoard.getBoundingClientRect();
    const startRect = startCell.getBoundingClientRect();
    const endRect = endCell.getBoundingClientRect();

    const startX = startRect.left + startRect.width / 2 - boardRect.left;
    const startY = startRect.top + startRect.height / 2 - boardRect.top;
    const endX = endRect.left + endRect.width / 2 - boardRect.left;
    const endY = endRect.top + endRect.height / 2 - boardRect.top;

    const line = document.createElement('div');
    line.classList.add('winning-line');
    gameBoard.appendChild(line);

    const length = Math.sqrt(Math.pow(endX - startX, 2) + Math.pow(endY - startY, 2));
    const angleRad = Math.atan2(endY - startY, endX - startX);
    const angleDeg = angleRad * 180 / Math.PI;

    line.style.width = `${length}px`;
    line.style.left = `${startX}px`;
    line.style.top = `${startY}px`;
    line.style.transformOrigin = `0 50%`;
    line.style.transform = `rotate(${angleDeg}deg)`;
}

function displayWinnerMessage() {
    winnerMessage.classList.remove('hidden');
    winnerMessage.style.color = (currentPlayer === 'X' ? '#e74c3c' : '#3498db');
}

function startConfetti() {
    const colors = ['#f0f', '#8f0', '#0ff', '#ff0', '#00f', '#f00', '#ee00ff', '#00ffee', '#f1c40f', '#2ecc71', '#3498db', '#9b59b6'];
    for (let i = 0; i < 100; i++) {
        const confetti = document.createElement('div');
        confetti.classList.add('confetti');
        confetti.style.left = `${Math.random() * 100}vw`;
        confetti.style.top = `-${Math.random() * 200}px`;
        confetti.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
        confetti.style.animationDelay = `${Math.random() * 1}s`;
        document.body.appendChild(confetti);

        confetti.addEventListener('animationend', () => {
            confetti.remove();
        });
    }
}

function clearWinningVisuals() {
    const existingLine = gameBoard.querySelector('.winning-line');
    if (existingLine) {
        existingLine.remove();
    }
    winnerMessage.classList.add('hidden');
    document.querySelectorAll('.confetti').forEach(c => c.remove());
}

// --- Event Listeners & Initial Setup ---
playWithAIButton.addEventListener('click', () => setGameMode('ai'));
playWithFriendButton.addEventListener('click', () => setGameMode('friend'));
chooseXButton.addEventListener('click', () => setPlayerSymbolAndStartGame('X'));
chooseOButton.addEventListener('click', () => setPlayerSymbolAndStartGame('O'));
newGameButton.addEventListener('click', startNewGame);
restartButton.addEventListener('click', fullRestartGame);

aiDifficultySlider.addEventListener('input', (event) => {
    aiPowerDisplay.textContent = `${event.target.value}%`;
});

// Initial setup on page load
window.onload = () => {
    showScreen(modeSelectionScreen);
    aiDifficultySlider.value = 75; // Set default AI difficulty to 75%
    aiPowerDisplay.textContent = '৭৫%';
};

// --- Helper functions for screen management ---
function setGameMode(mode) {
    gameMode = mode;
    showScreen(playerChoicePanel);
    if (gameMode === 'ai') {
        aiDifficultyPanel.classList.remove('hidden');
    } else {
        aiDifficultyPanel.classList.add('hidden');
    }
}

function setPlayerSymbolAndStartGame(symbol) {
    humanPlayerSymbol = symbol;
    AI_PLAYER_SYMBOL = (symbol === 'X' ? 'O' : 'X');
    currentPlayer = 'X';

    currentAIDifficulty = parseInt(aiDifficultySlider.value) / 100;
    
    showScreen(gamePlayScreen);
    createBoard();

    if (gameMode === 'ai' && currentPlayer === AI_PLAYER_SYMBOL) {
        setTimeout(makeAIMove, 700);
    }
}
