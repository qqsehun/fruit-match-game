<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no" />
    <title>🍉 水果消消乐 </title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background: #1a1a2e;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            font-family: 'Segoe UI', 'PingFang SC', Roboto, sans-serif;
            overflow: hidden;
            touch-action: none;
        }

        #game-wrapper {
            background: linear-gradient(145deg, #2d1b3d, #1a1a2e);
            border-radius: 48px;
            padding: 24px 28px 32px 28px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.8), inset 0 0 40px rgba(255, 200, 100, 0.06);
            border: 1px solid rgba(255, 255, 255, 0.06);
            position: relative;
        }

        #game-container {
            position: relative;
            background: rgba(30, 20, 40, 0.7);
            border-radius: 32px;
            padding: 16px 20px 20px 20px;
            backdrop-filter: blur(4px);
            border: 1px solid rgba(255, 215, 150, 0.15);
        }

        #header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 8px 14px 8px;
            color: #ffeedd;
            font-weight: 600;
            text-shadow: 0 2px 8px rgba(0, 0, 0, 0.5);
            flex-wrap: wrap;
            flex-shrink: 0;
            gap: 8px 12px;
        }

        .game-title {
            font-size: 16px;
            letter-spacing: 1px;
            background: linear-gradient(135deg, #f9d976, #f39f86);
            white-space: nowrap;
            flex-shrink: 0;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            font-weight: 800;
            display: flex;
            align-items: center;
            gap: 6px;
            
        }
        .game-title span {
            font-size: 20px;
            -webkit-text-fill-color: initial;
        }

        .goal-area {
            display: flex;
            align-items: center;
            gap: 4px;
            background: rgba(0, 0, 0, 0.3);
            padding: 6px 18px 6px 14px;
            border-radius: 30px;
            border: 1px solid rgba(255, 200, 100, 0.2);
            backdrop-filter: blur(4px);
            flex-shrink: 0;
        }
        .goal-icon {
            font-size: 18px;
            line-height: 1;
        }
        .goal-label {
            font-size: 11px;
            color: #ddccbb;
            letter-spacing: 0.5px;
        }
        .goal-count {
            font-size: 20px;
            font-weight: 800;
            color: #ffb347;
            min-width: 24px;
            text-align: center;
            transition: all 0.2s;
        }
        .goal-count.done {
            color: #7bed9f;
            animation: popGoal 0.4s ease;
        }
        @keyframes popGoal {
            0% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.6);
                color: #ffd93d;
            }
            100% {
                transform: scale(1);
            }
        }

        .collected-area {
            display: flex;
            align-items: center;
            gap: 4px;
            font-size: 12px;
            color: #ccbbaa;
            background: rgba(0, 0, 0, 0.2);
            padding: 2px 8px 2px 6px;
            border-radius: 30px;
            flex-shrink: 0;
            white-space: nowrap;
        }
        .collected-area span {
            color: #ffb347;
            font-weight: 700;
            font-size: 16px;
        }

        #gameCanvas {
            display: block;
            margin: 0 auto;
            border-radius: 20px;
            box-shadow: inset 0 0 30px rgba(0, 0, 0, 0.6), 0 8px 32px rgba(0, 0, 0, 0.5);
            background: transparent;
            width: 100%;
            height: auto;
            max-width: 570px; 
            aspect-ratio: 570 / 510;
            touch-action: none;
            cursor: pointer;
        }

        #footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding-top: 14px;
            color: #887a6a;
            font-size: 13px;
            letter-spacing: 0.3px;
            flex-wrap: nowrap;
            gap: 6px 12px;
        }
        #footer .hint {
            display: flex;
            align-items: center;
            gap: 6px;
            background: rgba(255, 255, 255, 0.04);
            padding: 4px 16px 4px 12px;
            border-radius: 30px;
            border: 1px solid rgba(255, 255, 255, 0.05);
        }
        #footer .hint i {
            font-style: normal;
            font-size: 16px;
        }
        #resetBtn {
            background: rgba(255, 200, 100, 0.12);
            border: 1px solid rgba(255, 200, 100, 0.2);
            color: #f0d5b0;
            padding: 6px 20px;
            border-radius: 30px;
            font-size: 14px;
            cursor: pointer;
            transition: all 0.25s;
            font-weight: 500;
            backdrop-filter: blur(4px);
        }
        #resetBtn:hover {
            background: rgba(255, 200, 100, 0.25);
            border-color: rgba(255, 200, 100, 0.4);
            transform: scale(1.04);
            color: #fff5e8;
        }
        #resetBtn:active {
            transform: scale(0.94);
        }

        .overlay {
            position: absolute;
            inset: 0;
            border-radius: 32px;
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 50;
            animation: fadeIn 0.5s ease;
        }
        .overlay.show {
            display: flex;
        }
        @keyframes fadeIn {
            0% {
                opacity: 0;
                transform: scale(0.9);
            }
            100% {
                opacity: 1;
                transform: scale(1);
            }
        }

        #home-overlay {
            background: rgba(0, 0, 0, 0.6);
            backdrop-filter: blur(10px);
        }
        .home-card {
            background: linear-gradient(145deg, #2d1b3d, #1a1a2e);
            padding: 40px 48px 44px;
            border-radius: 48px;
            text-align: center;
            border: 1px solid rgba(255, 215, 150, 0.2);
            box-shadow: 0 30px 80px rgba(0, 0, 0, 0.7);
            max-width: 400px;
            width: 90%;
        }
        .home-card .big-icon {
            font-size: 80px;
            display: block;
            margin-bottom: 4px;
        }
        .home-card h1 {
            color: #ffd93d;
            font-size: 36px;
            margin-bottom: 4px;
            letter-spacing: 2px;
        }
        .home-card p {
            color: #ddccbb;
            font-size: 16px;
            margin-bottom: 28px;
            line-height: 1.6;
        }
        .home-card .btn-start {
            background: linear-gradient(135deg, #f9d976, #f39f86);
            border: none;
            color: #1a1a2e;
            font-size: 22px;
            font-weight: 700;
            padding: 16px 50px;
            border-radius: 60px;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 6px 24px rgba(243, 159, 134, 0.3);
        }
        .home-card .btn-start:hover {
            transform: translateY(-3px) scale(1.03);
            box-shadow: 0 10px 40px rgba(243, 159, 134, 0.45);
        }
        .home-card .btn-start:active {
            transform: scale(0.95);
        }

        #win-overlay {
            background: rgba(0, 0, 0, 0.55);
            backdrop-filter: blur(8px);
        }
        .win-card {
            background: linear-gradient(145deg, #2d1b3d, #201530);
            padding: 40px 48px 44px;
            border-radius: 48px;
            text-align: center;
            border: 1px solid rgba(255, 215, 150, 0.2);
            box-shadow: 0 30px 80px rgba(0, 0, 0, 0.7);
            max-width: 380px;
            width: 90%;
        }
        .win-card .big-icon {
            font-size: 72px;
            display: block;
            margin-bottom: 8px;
            animation: bounceWin 1s ease infinite;
        }
        @keyframes bounceWin {
            0%,
            100% {
                transform: translateY(0);
            }
            50% {
                transform: translateY(-14px);
            }
        }
        .win-card h2 {
            color: #ffd93d;
            font-size: 32px;
            margin-bottom: 6px;
            letter-spacing: 2px;
        }
        .win-card p {
            color: #ddccbb;
            font-size: 16px;
            margin-bottom: 24px;
            line-height: 1.6;
        }
        .win-card .btn-group {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            justify-content: center;
        }
        .win-card .btn-play,
        .win-card .btn-home {
            border: none;
            font-size: 18px;
            font-weight: 700;
            padding: 12px 32px;
            border-radius: 60px;
            cursor: pointer;
            transition: all 0.3s;
            flex: 1 1 auto;
            min-width: 120px;
        }
        .win-card .btn-play {
            background: linear-gradient(135deg, #f9d976, #f39f86);
            color: #1a1a2e;
            box-shadow: 0 6px 24px rgba(243, 159, 134, 0.3);
        }
        .win-card .btn-play:hover {
            transform: translateY(-3px) scale(1.03);
            box-shadow: 0 10px 40px rgba(243, 159, 134, 0.45);
        }
        .win-card .btn-home {
            background: rgba(255, 255, 255, 0.08);
            color: #ddccbb;
            border: 1px solid rgba(255, 255, 255, 0.15);
        }
        .win-card .btn-home:hover {
            background: rgba(255, 255, 255, 0.18);
            transform: translateY(-3px);
        }
        .win-card .btn-play:active,
        .win-card .btn-home:active {
            transform: scale(0.95);
        }

        @media (max-width: 640px) {
            #game-wrapper {
                padding: 12px 10px 16px 10px;
                border-radius: 28px;
            }
            #game-container {
                padding: 10px 12px 14px 12px;
                border-radius: 20px;
            }
            #header {
                padding-bottom: 10px;
                font-size: 14px;
            }
            .game-title {
                font-size: 17px;
            }
            .game-title span {
                font-size: 22px;
            }
            .goal-area {
                padding: 4px 12px 4px 10px;
            }
            .goal-icon {
                font-size: 20px;
            }
            .goal-label {
                font-size: 11px;
            }
            .goal-count {
                font-size: 20px;
                min-width: 24px;
            }
            .collected-area {
                font-size: 12px;
                padding: 2px 10px 2px 8px;
            }
            .collected-area span {
                font-size: 16px;
            }
            #footer {
                font-size: 11px;
                padding-top: 10px;
            }
            #resetBtn {
                font-size: 12px;
                padding: 4px 14px;
            }
            .home-card,
            .win-card {
                padding: 28px 20px 32px;
            }
            .home-card .big-icon {
                font-size: 60px;
            }
            .home-card h1 {
                font-size: 28px;
            }
            .win-card .big-icon {
                font-size: 52px;
            }
            .win-card h2 {
                font-size: 26px;
            }
            .win-card .btn-play,
            .win-card .btn-home {
                font-size: 16px;
                padding: 10px 24px;
                min-width: 100px;
            }
            .home-card .btn-start {
                font-size: 18px;
                padding: 14px 36px;
            }
        }

        @media (max-width: 420px) {
            #game-wrapper {
                padding: 6px 4px 10px 4px;
                border-radius: 20px;
            }
            #game-container {
                padding: 6px 6px 10px 6px;
                border-radius: 16px;
            }
            .game-title {
                font-size: 14px;
            }
            .game-title span {
                font-size: 18px;
            }
            .goal-area {
                padding: 2px 8px 2px 6px;
                gap: 4px;
            }
            .goal-icon {
                font-size: 16px;
            }
            .goal-count {
                font-size: 16px;
                min-width: 18px;
            }
            #footer .hint {
                padding: 2px 10px 2px 8px;
                font-size: 10px;
            }
            .win-card .btn-group {
                flex-direction: column;
                gap: 8px;
            }
            .win-card .btn-play,
            .win-card .btn-home {
                width: 100%;
            }
        }
    </style>
</head>
<body>

    <div id="game-wrapper">
        <div id="game-container">
            <!-- 顶部 UI -->
            <div id="header">
                <div class="game-title">
                    <span>🍉</span> 水果消消乐
                </div>
                <div class="goal-area">
                    <span class="goal-icon">🍊</span>
                    <span class="goal-label">目标</span>
                    <span class="goal-count" id="goalCount">12</span>
                </div>
                <div class="collected-area">
                    🧺 已收 <span id="collectedCount">0</span>
                </div>
            </div>

            <!-- 画布 -->
            <canvas id="gameCanvas" width="570" height="510"></canvas>

            <!-- 底部 -->
            <div id="footer">
                <div class="hint">
                    <!-- <i>👆</i> 点击相邻水果交换  -->
                </div>
                <button id="resetBtn">⟳(๑˃̵ᴗ˂̵) 新一局</button>
            </div>

            <!-- 首页遮罩 -->
            <div id="home-overlay" class="overlay show">
                <div class="home-card">
                    <span class="big-icon">🍉</span>
                    <h1>水果消消乐</h1>
                    <p>收集指定水果</p>
                    <button class="btn-start" id="startBtn">🎮 开始游戏</button>
                </div>
            </div>

            <!-- 通关弹窗 -->
            <div id="win-overlay" class="overlay">
                <div class="win-card">
                    <span class="big-icon">🎉</span>
                    <h2>通关啦！</h2>
                    <p>你成功收集了所有目标橙子 🍊<br />太棒了！</p>
                    <div class="btn-group">
                        <button class="btn-play" id="playAgainBtn">⟳(๑˃̵ᴗ˂̵) 再来一局</button>
                        <button class="btn-home" id="homeBtn">⸝⸝𖠿⸝⸝  返回首页</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        (function() {
            'use strict';

            // ========== 常量 ==========
            const FRUIT_TYPES = {
                GREEN_APPLE: 0,
                BLUEBERRY: 1,
                STRAWBERRY: 2,
                BANANA: 3,
                ORANGE: 4,
                WATERMELON: 5,
                BOMB: 6,
                ROCKET_H: 7,
                ROCKET_V: 8,
            };

            // 可调节参数
            const INITIAL_TARGET = 12;
            const MIN_MATCH_FOR_SPECIAL = 4;
            const SPECIAL_TYPE_WEIGHTS = {
                [FRUIT_TYPES.BOMB]: 6,
                [FRUIT_TYPES.ROCKET_H]: 3,
                [FRUIT_TYPES.ROCKET_V]: 3,
            };

            const ROWS = 8;
            const COLS = 9;
            const CELL_SIZE = 60;
            const PADDING = 10;
            const BOARD_LEFT = PADDING;
            const BOARD_TOP = PADDING;
            const BOARD_WIDTH = COLS * CELL_SIZE;
            const BOARD_HEIGHT = ROWS * CELL_SIZE;
            const CANVAS_W = BOARD_WIDTH + PADDING * 2;
            const CANVAS_H = BOARD_HEIGHT + PADDING * 2;

            const FRUIT_EMOJI = {
                0: '🍏',
                1: '🫐',
                2: '🍓',
                3: '🍌',
                4: '🍊',
                5: '🍉',
                6: '💣',
                7: '🚀',
                8: '🚀',
            };

            const FRUIT_COLORS = {
                0: '#7BC67E',
                1: '#5B7FD4',
                2: '#E8575A',
                3: '#F7D44A',
                4: '#F5A342',
                5: '#3C9E4A',
                6: '#E06040',
                7: '#D0A050',
                8: '#50A0D0',
            };

            // ========== DOM ==========
            const canvas = document.getElementById('gameCanvas');
            const ctx = canvas.getContext('2d');
            const goalCountEl = document.getElementById('goalCount');
            const collectedCountEl = document.getElementById('collectedCount');
            const winOverlay = document.getElementById('win-overlay');
            const homeOverlay = document.getElementById('home-overlay');
            const resetBtn = document.getElementById('resetBtn');
            const playAgainBtn = document.getElementById('playAgainBtn');
            const startBtn = document.getElementById('startBtn');
            const homeBtn = document.getElementById('homeBtn');

            // ========== 音效 ==========
            class AudioManager {
                constructor() {
                    this.ctx = null;
                    this.initialized = false;
                    this.bgmInterval = null;
                    this.isBgmPlaying = false;
                }
                init() {
                    if (this.initialized) return;
                    try {
                        this.ctx = new(window.AudioContext || window.webkitAudioContext)();
                        this.initialized = true;
                    } catch (e) { console.warn('Web Audio not supported'); }
                }
                playTone(freq, duration, type = 'sine', volume = 0.3) {
                    if (!this.initialized) return;
                    try {
                        const osc = this.ctx.createOscillator();
                        const gain = this.ctx.createGain();
                        osc.type = type;
                        osc.frequency.value = freq;
                        gain.gain.setValueAtTime(volume, this.ctx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + duration);
                        osc.connect(gain);
                        gain.connect(this.ctx.destination);
                        osc.start();
                        osc.stop(this.ctx.currentTime + duration);
                    } catch (e) { /* ignore */ }
                }
                playEliminate() {
                    this.playTone(600, 0.12, 'square', 0.15);
                    setTimeout(() => this.playTone(800, 0.1, 'square', 0.12), 80);
                }
                playSpecial() {
                    this.playTone(400, 0.2, 'sawtooth', 0.2);
                    setTimeout(() => this.playTone(600, 0.15, 'sawtooth', 0.15), 100);
                    setTimeout(() => this.playTone(900, 0.2, 'sawtooth', 0.2), 200);
                }
                playSwap() { this.playTone(500, 0.08, 'sine', 0.1); }
                playBack() { this.playTone(300, 0.1, 'triangle', 0.08); }
                playWin() {
                    const notes = [523, 659, 784, 1047];
                    notes.forEach((f, i) => setTimeout(() => this.playTone(f, 0.3, 'sine', 0.2), i * 150));
                }
                startBGM() {
                    if (this.isBgmPlaying || !this.initialized) return;
                    this.isBgmPlaying = true;
                    const masterGain = this.ctx.createGain();
                    masterGain.gain.value = 0.08;
                    masterGain.connect(this.ctx.destination);
                    let noteIndex = 0;
                    const notes = [261.63, 329.63, 392.00, 523.25, 392.00, 329.63];
                    const durations = [0.4, 0.4, 0.4, 0.6, 0.4, 0.6];
                    const playNote = () => {
                        if (!this.isBgmPlaying) return;
                        const freq = notes[noteIndex % notes.length];
                        const dur = durations[noteIndex % durations.length];
                        try {
                            const osc = this.ctx.createOscillator();
                            const gain = this.ctx.createGain();
                            osc.type = 'triangle';
                            osc.frequency.value = freq;
                            gain.gain.setValueAtTime(0.08, this.ctx.currentTime);
                            gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + dur);
                            osc.connect(gain);
                            gain.connect(masterGain);
                            osc.start();
                            osc.stop(this.ctx.currentTime + dur);
                        } catch (e) { /* ignore */ }
                        noteIndex++;
                        this.bgmInterval = setTimeout(playNote, dur * 1.2 * 1000);
                    };
                    playNote();
                }
                stopBGM() {
                    this.isBgmPlaying = false;
                    if (this.bgmInterval) { clearTimeout(this.bgmInterval);
                        this.bgmInterval = null; }
                }
                resume() {
                    if (this.ctx && this.ctx.state === 'suspended') this.ctx.resume();
                }
            }

            const audio = new AudioManager();

            // ========== 状态 ==========
            let board = [];
            let targetRemain = INITIAL_TARGET;
            let collected = 0;
            let isAnimating = false;
            let isWin = false;
            let isGameActive = false;
            let selectedRow = -1;
            let selectedCol = -1;
            let snowflakes = [];
            let particles = [];

            // ========== 工具 ==========
            function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }

            function randFloat(min, max) { return Math.random() * (max - min) + min; }

            function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

            function isAdjacent(r1, c1, r2, c2) {
                const dr = Math.abs(r1 - r2);
                const dc = Math.abs(c1 - c2);
                return (dr === 1 && dc === 0) || (dr === 0 && dc === 1);
            }

            function inBounds(r, c) { return r >= 0 && r < ROWS && c >= 0 && c < COLS; }

            function isSpecialType(type) {
                return type === FRUIT_TYPES.BOMB || type === FRUIT_TYPES.ROCKET_H || type === FRUIT_TYPES.ROCKET_V;
            }

            // ========== 棋盘操作 ==========
            function createEmptyBoard() {
                return Array.from({ length: ROWS }, () => Array(COLS).fill(0));
            }

            function getRandomFruit() {
                // 6种普通水果：青苹果、蓝莓、草莓、香蕉、橙子、西瓜
                return rand(0, 5);
            }

            function findMatches(board) {
                const matches = new Set();
                const groups = [];
                for (let r = 0; r < ROWS; r++) {
                    for (let c = 0; c < COLS - 2; c++) {
                        const val = board[r][c];
                        if (val === undefined || isSpecialType(val)) continue;
                        let len = 1;
                        while (c + len < COLS && board[r][c + len] === val) len++;
                        if (len >= 3) {
                            const group = [];
                            for (let i = 0; i < len; i++) {
                                const key = r + ',' + (c + i);
                                matches.add(key);
                                group.push({ r, c: c + i });
                            }
                            groups.push({ type: val, positions: group, length: len });
                        }
                        c += len - 1;
                    }
                }
                for (let c = 0; c < COLS; c++) {
                    for (let r = 0; r < ROWS - 2; r++) {
                        const val = board[r][c];
                        if (val === undefined || isSpecialType(val)) continue;
                        let len = 1;
                        while (r + len < ROWS && board[r + len][c] === val) len++;
                        if (len >= 3) {
                            const group = [];
                            for (let i = 0; i < len; i++) {
                                const key = (r + i) + ',' + c;
                                matches.add(key);
                                group.push({ r: r + i, c });
                            }
                            groups.push({ type: val, positions: group, length: len });
                        }
                        r += len - 1;
                    }
                }
                return { matchSet: matches, groups };
            }

            // ========== 粒子系统 ==========
            function createParticles(row, col, color, count = 16) {
                const cx = BOARD_LEFT + col * CELL_SIZE + CELL_SIZE / 2;
                const cy = BOARD_TOP + row * CELL_SIZE + CELL_SIZE / 2;
                for (let i = 0; i < count; i++) {
                    const angle = Math.random() * Math.PI * 2;
                    const speed = 1 + Math.random() * 4;
                    const size = 3 + Math.random() * 6;
                    particles.push({
                        x: cx,
                        y: cy,
                        vx: Math.cos(angle) * speed,
                        vy: Math.sin(angle) * speed - 1,
                        size: size,
                        life: 1.0,
                        decay: 0.008 + Math.random() * 0.015,
                        color: color,
                    });
                }
            }

            function updateParticles() {
                for (let i = particles.length - 1; i >= 0; i--) {
                    const p = particles[i];
                    p.x += p.vx;
                    p.y += p.vy;
                    p.vy += 0.08;
                    p.life -= p.decay;
                    if (p.life <= 0) {
                        particles.splice(i, 1);
                    }
                }
            }

            function drawParticles() {
                for (const p of particles) {
                    ctx.globalAlpha = p.life;
                    ctx.shadowBlur = 0;
                    ctx.fillStyle = p.color;
                    ctx.beginPath();
                    ctx.arc(p.x, p.y, p.size * p.life, 0, Math.PI * 2);
                    ctx.fill();
                }
                ctx.globalAlpha = 1;
            }

            // ========== 消除逻辑（无炸弹） ==========
            function performElimination(board, matchSet, groups) {
                const eliminated = { count: 0, orangeCount: 0 };
                const positionsToRemove = [];
                for (const key of matchSet) {
                    const [r, c] = key.split(',').map(Number);
                    positionsToRemove.push({ r, c });
                }

                // 统计橙子，并生成粒子
                for (const pos of positionsToRemove) {
                    const val = board[pos.r][pos.c];
                    if (val === FRUIT_TYPES.ORANGE) eliminated.orangeCount++;
                    // 生成粉碎粒子
                    const color = FRUIT_COLORS[val] || '#ffcc00';
                    createParticles(pos.r, pos.c, color, 12 + Math.floor(Math.random() * 10));
                }

                // 生成特殊道具（仅当存在长度>=MIN_MATCH_FOR_SPECIAL的组，且只生成一个）
                const specialGroups = groups.filter(g => g.length >= MIN_MATCH_FOR_SPECIAL);
                let specialPos = null;
                if (specialGroups.length > 0) {
                    const group = specialGroups[rand(0, specialGroups.length - 1)];
                    const idx = rand(0, group.positions.length - 1);
                    const pos = group.positions[idx];
                    // 按权重选择类型
                    const typeKeys = Object.keys(SPECIAL_TYPE_WEIGHTS).map(Number);
                    const weights = typeKeys.map(k => SPECIAL_TYPE_WEIGHTS[k]);
                    const total = weights.reduce((a, b) => a + b, 0);
                    let rnd = rand(0, total - 1);
                    let selectedType = typeKeys[0];
                    for (let i = 0; i < typeKeys.length; i++) {
                        rnd -= weights[i];
                        if (rnd < 0) { selectedType = typeKeys[i]; break; }
                    }
                    specialPos = { r: pos.r, c: pos.c, type: selectedType };
                    // 从待移除列表中移除该位置（保留为道具）
                    const idxRemove = positionsToRemove.findIndex(p => p.r === pos.r && p.c === pos.c);
                    if (idxRemove !== -1) positionsToRemove.splice(idxRemove, 1);
                }

                // 执行移除
                for (const pos of positionsToRemove) {
                    board[pos.r][pos.c] = undefined;
                }
                eliminated.count = positionsToRemove.length;
                if (specialPos) {
                    board[specialPos.r][specialPos.c] = specialPos.type;
                }
                return eliminated;
            }

            function applyGravity(board) {
                for (let c = 0; c < COLS; c++) {
                    let writeRow = ROWS - 1;
                    for (let r = ROWS - 1; r >= 0; r--) {
                        if (board[r][c] !== undefined) {
                            board[writeRow][c] = board[r][c];
                            if (writeRow !== r) board[r][c] = undefined;
                            writeRow--;
                        }
                    }
                    for (let r = writeRow; r >= 0; r--) {
                        board[r][c] = undefined;
                    }
                }
            }

            function fillTop(board) {
                for (let c = 0; c < COLS; c++) {
                    for (let r = 0; r < ROWS; r++) {
                        if (board[r][c] === undefined) {
                            board[r][c] = getRandomFruit();
                        }
                    }
                }
            }

            function initBoard() {
                const newBoard = createEmptyBoard();
                for (let r = 0; r < ROWS; r++) {
                    for (let c = 0; c < COLS; c++) {
                        newBoard[r][c] = getRandomFruit();
                    }
                }
                // 消除初始三连（不生成道具）
                let hasMatch = true;
                while (hasMatch) {
                    const { matchSet } = findMatches(newBoard);
                    if (matchSet.size === 0) break;
                    for (const key of matchSet) {
                        const [r, c] = key.split(',').map(Number);
                        newBoard[r][c] = undefined;
                    }
                    applyGravity(newBoard);
                    fillTop(newBoard);
                }
                return newBoard;
            }

            // ========== 绘制 ==========
            function drawFruit(ctx, r, c, type, scale = 1) {
                const x = BOARD_LEFT + c * CELL_SIZE + CELL_SIZE / 2;
                const y = BOARD_TOP + r * CELL_SIZE + CELL_SIZE / 2;
                const size = (CELL_SIZE * 0.7 * scale);
                ctx.save();
                ctx.translate(x, y);
                const emoji = FRUIT_EMOJI[type] || '❓';
                ctx.shadowColor = 'rgba(0,0,0,0.6)';
                ctx.shadowBlur = 12;
                ctx.shadowOffsetY = 2;
                ctx.font =
                `${size}px "Segoe UI Emoji", "Apple Color Emoji", "Noto Color Emoji", "Helvetica", sans-serif`;
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillStyle = '#ffffff';
                ctx.shadowBlur = 0;
                ctx.strokeStyle = 'rgba(0,0,0,0.3)';
                ctx.lineWidth = 2;
                ctx.strokeText(emoji, 0, 0);
                ctx.shadowColor = 'rgba(0,0,0,0.5)';
                ctx.shadowBlur = 8;
                ctx.fillText(emoji, 0, 0);
                ctx.restore();
            }

            function drawBoard() {
                ctx.clearRect(0, 0, CANVAS_W, CANVAS_H);
                drawSnowflakes();
                ctx.shadowBlur = 0;
                ctx.fillStyle = 'rgba(20, 14, 30, 0.55)';
                ctx.beginPath();
                ctx.roundRect(BOARD_LEFT - 4, BOARD_TOP - 4, BOARD_WIDTH + 8, BOARD_HEIGHT + 8, 16);
                ctx.fill();
                ctx.strokeStyle = 'rgba(255, 220, 180, 0.08)';
                ctx.lineWidth = 1;
                for (let r = 0; r <= ROWS; r++) {
                    ctx.beginPath();
                    ctx.moveTo(BOARD_LEFT, BOARD_TOP + r * CELL_SIZE);
                    ctx.lineTo(BOARD_LEFT + BOARD_WIDTH, BOARD_TOP + r * CELL_SIZE);
                    ctx.stroke();
                }
                for (let c = 0; c <= COLS; c++) {
                    ctx.beginPath();
                    ctx.moveTo(BOARD_LEFT + c * CELL_SIZE, BOARD_TOP);
                    ctx.lineTo(BOARD_LEFT + c * CELL_SIZE, BOARD_TOP + BOARD_HEIGHT);
                    ctx.stroke();
                }
                for (let r = 0; r < ROWS; r++) {
                    for (let c = 0; c < COLS; c++) {
                        const val = board[r][c];
                        if (val === undefined) continue;
                        if (selectedRow === r && selectedCol === c) {
                            ctx.shadowBlur = 0;
                            ctx.fillStyle = 'rgba(255, 215, 100, 0.35)';
                            ctx.beginPath();
                            ctx.roundRect(BOARD_LEFT + c * CELL_SIZE + 2, BOARD_TOP + r * CELL_SIZE + 2, CELL_SIZE - 4,
                                CELL_SIZE - 4, 8);
                            ctx.fill();
                        }
                        drawFruit(ctx, r, c, val, 1);
                    }
                }
                drawParticles();
            }

            // ========== 雪花 ==========
            function initSnowflakes(count = 70) {
                snowflakes = [];
                for (let i = 0; i < count; i++) {
                    snowflakes.push({
                        x: randFloat(0, CANVAS_W),
                        y: randFloat(0, CANVAS_H),
                        r: randFloat(1.5, 4.5),
                        speed: randFloat(0.3, 1.2),
                        wind: randFloat(-0.2, 0.2),
                        opacity: randFloat(0.2, 0.6),
                    });
                }
            }

            function drawSnowflakes() {
                for (const s of snowflakes) {
                    ctx.shadowBlur = 0;
                    ctx.beginPath();
                    ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
                    ctx.fillStyle = `rgba(255, 255, 255, ${s.opacity})`;
                    ctx.fill();
                }
            }

            function updateSnowflakes() {
                for (const s of snowflakes) {
                    s.y += s.speed;
                    s.x += s.wind + Math.sin(s.y * 0.01) * 0.15;
                    if (s.y > CANVAS_H + 10) { s.y = -10;
                        s.x = randFloat(0, CANVAS_W); }
                    if (s.x > CANVAS_W + 10) s.x = -10;
                    if (s.x < -10) s.x = CANVAS_W + 10;
                }
            }

            if (!CanvasRenderingContext2D.prototype.roundRect) {
                CanvasRenderingContext2D.prototype.roundRect = function(x, y, w, h, radii) {
                    const r = typeof radii === 'number' ? radii : (radii || 0);
                    this.moveTo(x + r, y);
                    this.lineTo(x + w - r, y);
                    this.quadraticCurveTo(x + w, y, x + w, y + r);
                    this.lineTo(x + w, y + h - r);
                    this.quadraticCurveTo(x + w, y + h, x + w - r, y + h);
                    this.lineTo(x + r, y + h);
                    this.quadraticCurveTo(x, y + h, x, y + h - r);
                    this.lineTo(x, y + r);
                    this.quadraticCurveTo(x, y, x + r, y);
                    return this;
                };
            }

            // ========== 道具触发 ==========
            function triggerBomb(board, row, col) {
                const affected = [];
                const dirs = [
                    [-1, 0],
                    [1, 0],
                    [0, -1],
                    [0, 1]
                ];
                for (const d of dirs) {
                    const nr = row + d[0],
                        nc = col + d[1];
                    if (inBounds(nr, nc) && board[nr][nc] !== undefined) affected.push({ r: nr, c: nc });
                }
                let orangeCount = 0;
                for (const pos of affected) {
                    const val = board[pos.r][pos.c];
                    if (val === FRUIT_TYPES.ORANGE) orangeCount++;
                    const color = FRUIT_COLORS[val] || '#ffcc00';
                    createParticles(pos.r, pos.c, color, 10);
                    board[pos.r][pos.c] = undefined;
                }
                // 自身也粉碎
                createParticles(row, col, '#FFD700', 20);
                board[row][col] = undefined;
                return { affectedCount: affected.length, orangeCount };
            }

            function triggerRocket(board, row, col, isHorizontal) {
                const affected = [];
                let orangeCount = 0;
                if (isHorizontal) {
                    for (let c = 0; c < COLS; c++) {
                        if (c === col) continue;
                        if (board[row][c] !== undefined) {
                            affected.push({ r: row, c });
                            if (board[row][c] === FRUIT_TYPES.ORANGE) orangeCount++;
                        }
                    }
                } else {
                    for (let r = 0; r < ROWS; r++) {
                        if (r === row) continue;
                        if (board[r][col] !== undefined) {
                            affected.push({ r, c: col });
                            if (board[r][col] === FRUIT_TYPES.ORANGE) orangeCount++;
                        }
                    }
                }
                for (const pos of affected) {
                    const val = board[pos.r][pos.c];
                    const color = FRUIT_COLORS[val] || '#ffcc00';
                    createParticles(pos.r, pos.c, color, 8);
                    board[pos.r][pos.c] = undefined;
                }
                createParticles(row, col, '#FFD700', 16);
                board[row][col] = undefined;
                return { affectedCount: affected.length, orangeCount };
            }

            // ========== 主消除循环 ==========
            async function processMatches(triggerFromSwap = false) {
                if (isAnimating) return;
                isAnimating = true;
                while (true) {
                    const { matchSet, groups } = findMatches(board);
                    if (matchSet.size === 0) break;
                    const result = performElimination(board, matchSet, groups);
                    if (result.orangeCount > 0) {
                        const actualReduce = Math.min(result.orangeCount, targetRemain);
                        targetRemain -= actualReduce;
                        collected += actualReduce;
                        updateUI();
                        if (targetRemain <= 0) {
                            targetRemain = 0;
                            updateUI();
                            audio.playWin();
                            await sleep(300);
                            showWin();
                            isAnimating = false;
                            return;
                        }
                    }
                    audio.playEliminate();
                    drawBoard();
                    await sleep(180);
                    applyGravity(board);
                    fillTop(board);
                    drawBoard();
                    await sleep(180);
                    
                    if (Math.random() < 0.3) { // 30% 概率
                        // 随机找一个空位或已消除位置生成炸弹？
                        // 或者直接在棋盘随机位置生成炸弹
                        const emptyPositions = [];
                        for (let r = 0; r < ROWS; r++) {
                            for (let c = 0; c < COLS; c++) {
                                if (board[r][c] === undefined) emptyPositions.push({r, c});
                            }
                        }
                        if (emptyPositions.length > 0) {
                            const pos = emptyPositions[rand(0, emptyPositions.length - 1)];
                            board[pos.r][pos.c] = FRUIT_TYPES.BOMB;
                        }
                    }
                }
                selectedRow = -1;
                selectedCol = -1;
                drawBoard();
                isAnimating = false;
            }

            // ========== 交换逻辑 ==========
            async function trySwap(r1, c1, r2, c2) {
                if (isAnimating || isWin || !isGameActive) return false;
                if (!inBounds(r1, c1) || !inBounds(r2, c2)) return false;
                if (!isAdjacent(r1, c1, r2, c2)) return false;
                if (board[r1][c1] === undefined || board[r2][c2] === undefined) return false;
                if (isSpecialType(board[r1][c1]) || isSpecialType(board[r2][c2])) {
                    audio.playBack();
                    return false;
                }
                const temp = board[r1][c1];
                board[r1][c1] = board[r2][c2];
                board[r2][c2] = temp;
                drawBoard();
                audio.playSwap();
                const { matchSet } = findMatches(board);
                if (matchSet.size > 0) {
                    selectedRow = -1;
                    selectedCol = -1;
                    await processMatches(true);
                    return true;
                } else {
                    const tempBack = board[r1][c1];
                    board[r1][c1] = board[r2][c2];
                    board[r2][c2] = tempBack;
                    drawBoard();
                    audio.playBack();
                    ctx.shadowBlur = 0;
                    ctx.fillStyle = 'rgba(255, 80, 80, 0.15)';
                    ctx.beginPath();
                    ctx.roundRect(BOARD_LEFT + c1 * CELL_SIZE + 2, BOARD_TOP + r1 * CELL_SIZE + 2, CELL_SIZE - 4,
                        CELL_SIZE - 4, 8);
                    ctx.fill();
                    ctx.beginPath();
                    ctx.roundRect(BOARD_LEFT + c2 * CELL_SIZE + 2, BOARD_TOP + r2 * CELL_SIZE + 2, CELL_SIZE - 4,
                        CELL_SIZE - 4, 8);
                    ctx.fill();
                    await sleep(120);
                    drawBoard();
                    return false;
                }
            }

            // ========== 点击道具 ==========
            async function handleSpecialClick(row, col) {
                if (isAnimating || isWin || !isGameActive) return;
                const type = board[row][col];
                if (!isSpecialType(type)) return;
                audio.resume();
                audio.playSpecial();
                let orangeCount = 0;
                if (type === FRUIT_TYPES.BOMB) {
                    const boom = triggerBomb(board, row, col);
                    orangeCount = boom.orangeCount;
                } else if (type === FRUIT_TYPES.ROCKET_H) {
                    const boom = triggerRocket(board, row, col, true);
                    orangeCount = boom.orangeCount;
                } else if (type === FRUIT_TYPES.ROCKET_V) {
                    const boom = triggerRocket(board, row, col, false);
                    orangeCount = boom.orangeCount;
                }
                if (orangeCount > 0) {
                    const actualReduce = Math.min(orangeCount, targetRemain);
                    targetRemain -= actualReduce;
                    collected += actualReduce;
                    updateUI();
                    if (targetRemain <= 0) {
                        targetRemain = 0;
                        updateUI();
                        audio.playWin();
                        await sleep(300);
                        showWin();
                        return;
                    }
                }
                drawBoard();
                await sleep(150);
                applyGravity(board);
                fillTop(board);
                drawBoard();
                await sleep(150);
                await processMatches(false);
            }

            // ========== UI ==========
            function updateUI() {
                goalCountEl.textContent = targetRemain;
                collectedCountEl.textContent = collected;
                if (targetRemain <= 0) goalCountEl.classList.add('done');
                else goalCountEl.classList.remove('done');
            }

            function showWin() { isWin = true;
                winOverlay.classList.add('show');
                audio.stopBGM(); }

            function hideWin() { winOverlay.classList.remove('show');
                isWin = false; }

            function showHome() { homeOverlay.classList.add('show');
                isGameActive = false;
                resetGameState();
                drawBoard();
                audio.stopBGM(); }

            function hideHome() {
                homeOverlay.classList.remove('show');
                isGameActive = true;
                board = initBoard();
                targetRemain = INITIAL_TARGET;
                collected = 0;
                isWin = false;
                isAnimating = false;
                selectedRow = -1;
                selectedCol = -1;
                particles = [];
                updateUI();
                drawBoard();
                audio.init();
                audio.resume();
                audio.startBGM();
            }

            function resetGameState() {
                particles = [];
                board = initBoard();
                targetRemain = INITIAL_TARGET;
                collected = 0;
                isWin = false;
                isAnimating = false;
                selectedRow = -1;
                selectedCol = -1;
                updateUI();
                drawBoard();
            }

            function resetAndStart() {
                hideWin();
                particles = [];
                board = initBoard();
                targetRemain = INITIAL_TARGET;
                collected = 0;
                isWin = false;
                isAnimating = false;
                selectedRow = -1;
                selectedCol = -1;
                updateUI();
                drawBoard();
                isGameActive = true;
                audio.init();
                audio.resume();
                audio.startBGM();
            }

            // ========== 交互 ==========
            function getGridPos(clientX, clientY) {
                const rect = canvas.getBoundingClientRect();
                const scaleX = canvas.width / rect.width;
                const scaleY = canvas.height / rect.height;
                const x = (clientX - rect.left) * scaleX;
                const y = (clientY - rect.top) * scaleY;
                if (x < BOARD_LEFT || x > BOARD_LEFT + BOARD_WIDTH || y < BOARD_TOP || y > BOARD_TOP + BOARD_HEIGHT)
                    return { r: -1, c: -1 };
                const c = Math.floor((x - BOARD_LEFT) / CELL_SIZE);
                const r = Math.floor((y - BOARD_TOP) / CELL_SIZE);
                if (r >= 0 && r < ROWS && c >= 0 && c < COLS) return { r, c };
                return { r: -1, c: -1 };
            }

            function handleClick(clientX, clientY) {
                if (isAnimating || isWin || !isGameActive) return;
                const { r, c } = getGridPos(clientX, clientY);
                if (r === -1 || c === -1) { selectedRow = -1;
                    selectedCol = -1;
                    drawBoard(); return; }
                if (board[r][c] === undefined) { selectedRow = -1;
                    selectedCol = -1;
                    drawBoard(); return; }
                if (isSpecialType(board[r][c])) {
                    handleSpecialClick(r, c);
                    selectedRow = -1;
                    selectedCol = -1;
                    return;
                }
                if (selectedRow === -1 && selectedCol === -1) {
                    selectedRow = r;
                    selectedCol = c;
                    drawBoard();
                    return;
                }
                if (selectedRow === r && selectedCol === c) {
                    selectedRow = -1;
                    selectedCol = -1;
                    drawBoard();
                    return;
                }
                if (isAdjacent(selectedRow, selectedCol, r, c)) {
                    const sr = selectedRow,
                        sc = selectedCol;
                    selectedRow = -1;
                    selectedCol = -1;
                    trySwap(sr, sc, r, c);
                } else {
                    selectedRow = r;
                    selectedCol = c;
                    drawBoard();
                }
            }

            canvas.addEventListener('mousedown', (e) => { e.preventDefault();
                handleClick(e.clientX, e.clientY); });
            canvas.addEventListener('touchstart', (e) => {
                e.preventDefault();
                const touch = e.touches[0];
                if (touch) handleClick(touch.clientX, touch.clientY);
            }, { passive: false });

            let dragStart = null;
            canvas.addEventListener('mousedown', (e) => {
                const { r, c } = getGridPos(e.clientX, e.clientY);
                if (r >= 0 && c >= 0 && board[r][c] !== undefined && isGameActive && !isSpecialType(board[r][c]))
                    dragStart = { r, c };
                else dragStart = null;
            });
            canvas.addEventListener('mousemove', (e) => {
                if (!dragStart || isAnimating || isWin || !isGameActive) return;
                const { r, c } = getGridPos(e.clientX, e.clientY);
                if (r >= 0 && c >= 0 && board[r][c] !== undefined && !isSpecialType(board[r][c]) && isAdjacent(
                        dragStart.r, dragStart.c, r, c)) {
                    const sr = dragStart.r,
                        sc = dragStart.c;
                    dragStart = null;
                    trySwap(sr, sc, r, c);
                }
            });
            canvas.addEventListener('mouseup', () => { dragStart = null; });
            canvas.addEventListener('mouseleave', () => { dragStart = null; });

            let touchStart = null;
            canvas.addEventListener('touchstart', (e) => {
                const touch = e.touches[0];
                if (!touch) return;
                const { r, c } = getGridPos(touch.clientX, touch.clientY);
                if (r >= 0 && c >= 0 && board[r][c] !== undefined && isGameActive && !isSpecialType(board[r][c]))
                    touchStart = { r, c };
                else touchStart = null;
            }, { passive: true });
            canvas.addEventListener('touchmove', (e) => {
                if (!touchStart || isAnimating || isWin || !isGameActive) return;
                const touch = e.touches[0];
                if (!touch) return;
                const { r, c } = getGridPos(touch.clientX, touch.clientY);
                if (r >= 0 && c >= 0 && board[r][c] !== undefined && !isSpecialType(board[r][c]) && isAdjacent(
                        touchStart.r, touchStart.c, r, c)) {
                    const sr = touchStart.r,
                        sc = touchStart.c;
                    touchStart = null;
                    trySwap(sr, sc, r, c);
                }
            }, { passive: true });
            canvas.addEventListener('touchend', () => { touchStart = null; }, { passive: true });

            // ========== 按钮 ==========
            resetBtn.addEventListener('click', () => { if (isGameActive) resetAndStart(); });
            playAgainBtn.addEventListener('click', resetAndStart);
            startBtn.addEventListener('click', hideHome);
            homeBtn.addEventListener('click', () => { hideWin();
                showHome();
                resetGameState(); });

            // ========== 动画循环 ==========
            function gameLoop() {
                updateSnowflakes();
                updateParticles();
                drawBoard();
                requestAnimationFrame(gameLoop);
            }

            // ========== 启动 ==========
            function init() {
                initSnowflakes(80);
                board = initBoard();
                targetRemain = INITIAL_TARGET;
                collected = 0;
                isGameActive = false;
                homeOverlay.classList.add('show');
                particles = [];
                updateUI();
                drawBoard();
                gameLoop();
                audio.init();
            }

            init();
            window.addEventListener('resize', drawBoard);
            console.log('🍉 水果消消乐');

        })();
    </script>
</body>
</html>
