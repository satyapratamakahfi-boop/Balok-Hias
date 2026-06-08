<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>Satya's Block Blast · Puzzle Challenge</title>
  <style>
    * {
      user-select: none;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      background: linear-gradient(135deg, #0b2b2f 0%, #0a1c22 100%);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      font-family: 'Segoe UI', 'Poppins', 'Roboto', system-ui, sans-serif;
      margin: 0;
      padding: 20px;
    }

    .game-wrapper {
      background: rgba(6, 20, 24, 0.7);
      backdrop-filter: blur(3px);
      border-radius: 70px;
      padding: 20px 24px 28px 24px;
      box-shadow: 0 25px 40px rgba(0,0,0,0.5), inset 0 1px 0 rgba(255,255,255,0.1);
    }

    .game-panel {
      background: #172d36;
      border-radius: 52px;
      padding: 22px;
      box-shadow: inset 0 1px 4px rgba(0,0,0,0.3), 0 12px 22px rgba(0,0,0,0.4);
    }

    /* header skor + restart */
    .header-stats {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 18px;
      padding: 0 10px;
    }

    .score-card {
      background: #0a171f;
      border-radius: 60px;
      padding: 6px 24px;
      box-shadow: inset 0 1px 3px #00000055, 0 5px 10px rgba(0,0,0,0.3);
      display: flex;
      align-items: baseline;
      gap: 12px;
    }

    .score-label {
      color: #b9d9f0;
      font-weight: 600;
      font-size: 1rem;
      letter-spacing: 1px;
    }

    .score-value {
      font-size: 2.4rem;
      font-weight: 800;
      color: #f9cf6e;
      text-shadow: 0 2px 3px black;
      line-height: 1;
    }

    .restart-btn {
      background: #2b5f74;
      border: none;
      font-size: 1.2rem;
      font-weight: bold;
      padding: 8px 28px;
      border-radius: 50px;
      color: #fff3e0;
      cursor: pointer;
      transition: 0.1s linear;
      box-shadow: 0 6px 0 #0f2a2f;
      font-family: inherit;
    }

    .restart-btn:active {
      transform: translateY(3px);
      box-shadow: 0 3px 0 #0f2a2f;
    }

    /* canvas papan utama */
    .board-area {
      display: flex;
      justify-content: center;
      margin-bottom: 18px;
    }

    canvas#boardCanvas {
      background-color: #10242e;
      border-radius: 32px;
      display: block;
      box-shadow: 0 12px 20px rgba(0,0,0,0.5), inset 0 0 0 2px #3c7a8a;
      cursor: pointer;
      width: 100%;
      height: auto;
      max-width: 400px;
    }

    /* shapes area */
    .shapes-section {
      margin-top: 16px;
    }

    .shapes-label {
      text-align: center;
      color: #cbe5f0;
      font-weight: 500;
      letter-spacing: 1px;
      font-size: 0.85rem;
      margin-bottom: 12px;
      background: #0e212a;
      display: inline-block;
      width: auto;
      padding: 5px 18px;
      border-radius: 50px;
      margin-left: auto;
      margin-right: auto;
    }

    .shapes-container {
      display: flex;
      justify-content: center;
      gap: 18px;
      flex-wrap: wrap;
    }

    .shape-card {
      background: #1c3b47;
      border-radius: 32px;
      padding: 10px 8px;
      transition: 0.08s linear;
      cursor: pointer;
      box-shadow: 0 8px 14px rgba(0,0,0,0.4);
      border: 3px solid transparent;
    }

    .shape-card.selected {
      border-color: #ffbf69;
      background: #2f5f70;
      transform: scale(1.02);
      box-shadow: 0 0 0 2px #ffdd99;
    }

    .mini-canvas {
      display: block;
      margin: 0 auto;
      background: #0b1f27;
      border-radius: 24px;
      box-shadow: inset 0 0 0 1px #4f8c9e;
    }

    .status-area {
      text-align: center;
      margin-top: 20px;
      display: flex;
      justify-content: center;
    }

    .game-status {
      background: #07161f;
      padding: 6px 22px;
      border-radius: 60px;
      color: #ffcf9a;
      font-weight: 500;
      font-size: 0.8rem;
      letter-spacing: 0.5px;
    }

    .footer-credit {
      text-align: center;
      margin-top: 18px;
      font-size: 0.75rem;
      color: #97b7c7;
      font-weight: 500;
    }

    @media (max-width: 550px) {
      .game-wrapper { padding: 12px; }
      .mini-canvas { width: 70px; height: 70px; }
      .score-value { font-size: 1.8rem; }
      .restart-btn { padding: 6px 18px; font-size: 1rem; }
    }
  </style>
</head>
<body>
<div class="game-wrapper">
  <div class="game-panel">
    <div class="header-stats">
      <div class="score-card">
        <span class="score-label">💥 SKOR</span>
        <span class="score-value" id="scoreDisplay">0</span>
      </div>
      <button class="restart-btn" id="restartButton">🔄 RESTART</button>
    </div>

    <div class="board-area">
      <canvas id="boardCanvas" width="400" height="400"></canvas>
    </div>

    <div class="shapes-section">
      <div class="shapes-label">◈  PILIH BENTUK BLOK  ◈</div>
      <div class="shapes-container" id="shapesContainer"></div>
    </div>

    <div class="status-area">
      <div class="game-status" id="gameStatusText">✨ Klik blok, lalu letakkan di papan ✨</div>
    </div>
    <div class="footer-credit">
      ⚡ SATYA'S BLOCK BLAST | Hancurkan baris & kolom penuh!
    </div>
  </div>
</div>

<script>
  (function(){
    // ------------------- KONFIGURASI GRID -------------------
    const GRID_SIZE = 8;         // papan 8x8
    const CELL_SIZE = 50;        // 400/8
    let grid = Array(GRID_SIZE).fill().map(() => Array(GRID_SIZE).fill(0));
    let currentScore = 0;
    let gameActive = true;
    let selectedShapeIndex = -1;   // -1 artinya tidak ada blok terpilih

    // ------------------- MASTER BENTUK (pattern) -------------------
    const MASTER_SHAPES = [
      { name: "● 1x1", pattern: [[1]] },
      { name: "▬ 1x2", pattern: [[1,1]] },
      { name: "▮ 2x1", pattern: [[1],[1]] },
      { name: "◼ 2x2", pattern: [[1,1],[1,1]] },
      { name: "└ L", pattern: [[1,0],[1,1]] },
      { name: "┘ L", pattern: [[0,1],[1,1]] },
      { name: "━━ 3x1", pattern: [[1,1,1]] },
      { name: "┃ 1x3", pattern: [[1],[1],[1]] }
    ];

    // Tiga slot blok aktif yang tersedia
    let currentShapes = [];     // setiap elemen = { pattern, name, width, height }

    // DOM elements
    const boardCanvas = document.getElementById('boardCanvas');
    const ctx = boardCanvas.getContext('2d');
    const scoreSpan = document.getElementById('scoreDisplay');
    const gameStatusDiv = document.getElementById('gameStatusText');
    const shapesContainer = document.getElementById('shapesContainer');
    const restartBtn = document.getElementById('restartButton');

    // Helper: mengambil bentuk acak dari master
    function getRandomShape() {
      const idx = Math.floor(Math.random() * MASTER_SHAPES.length);
      const original = MASTER_SHAPES[idx];
      // clone pattern agar tidak terpengaruh referensi
      const patternCopy = original.pattern.map(row => [...row]);
      return {
        name: original.name,
        pattern: patternCopy,
        height: patternCopy.length,
        width: patternCopy[0].length
      };
    }

    // inisialisasi 3 blok random
    function initRandomShapes() {
      const shapes = [];
      for (let i = 0; i < 3; i++) {
        shapes.push(getRandomShape());
      }
      return shapes;
    }

    // Update UI skor
    function updateScoreUI() {
      scoreSpan.innerText = currentScore;
    }

    // ----- GAMBAR GRID UTAMA (papan 8x8) -----
    function drawGrid() {
      ctx.clearRect(0, 0, 400, 400);
      // Garis grid
      ctx.strokeStyle = "#2b6d7a";
      ctx.lineWidth = 1.5;
      for (let i = 0; i <= GRID_SIZE; i++) {
        ctx.beginPath();
        ctx.moveTo(i * CELL_SIZE, 0);
        ctx.lineTo(i * CELL_SIZE, 400);
        ctx.stroke();
        ctx.moveTo(0, i * CELL_SIZE);
        ctx.lineTo(400, i * CELL_SIZE);
        ctx.stroke();
      }
      
      // Isi sel
      for (let row = 0; row < GRID_SIZE; row++) {
        for (let col = 0; col < GRID_SIZE; col++) {
          const x = col * CELL_SIZE;
          const y = row * CELL_SIZE;
          if (grid[row][col] === 1) {
            // Blok terisi: gradien menarik
            const grad = ctx.createLinearGradient(x, y, x+CELL_SIZE, y+CELL_SIZE);
            grad.addColorStop(0, '#6bc5e0');
            grad.addColorStop(1, '#3696b3');
            ctx.fillStyle = grad;
            ctx.fillRect(x, y, CELL_SIZE-0.5, CELL_SIZE-0.5);
            ctx.fillStyle = "#e7f4ff";
            ctx.globalAlpha = 0.35;
            ctx.fillRect(x+3, y+3, CELL_SIZE-7, CELL_SIZE-7);
            ctx.globalAlpha = 1;
          } else {
            // efek sel kosong
            ctx.fillStyle = "#0f2935";
            ctx.fillRect(x, y, CELL_SIZE-0.5, CELL_SIZE-0.5);
            ctx.fillStyle = "#1d4a5a";
            ctx.fillRect(x+1, y+1, CELL_SIZE-2.5, CELL_SIZE-2.5);
          }
        }
      }
    }

    // Gambar shape di mini canvas (untuk pilihan blok)
    function drawShapeOnCanvas(canvas, shape, size = 70) {
      const shCtx = canvas.getContext('2d');
      const w = canvas.width, h = canvas.height;
      shCtx.clearRect(0, 0, w, h);
      shCtx.fillStyle = "#122f3a";
      shCtx.fillRect(0, 0, w, h);
      
      const pattern = shape.pattern;
      const rows = pattern.length;
      const cols = pattern[0].length;
      
      // Ukuran sel dalam mini canvas
      const cellW = (w - 16) / cols;
      const cellH = (h - 16) / rows;
      const offsetX = (w - (cellW * cols)) / 2;
      const offsetY = (h - (cellH * rows)) / 2;
      
      for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
          if (pattern[r][c] === 1) {
            const x = offsetX + c * cellW;
            const y = offsetY + r * cellH;
            shCtx.fillStyle = "#f9b953";
            shCtx.shadowBlur = 4;
            shCtx.shadowColor = "rgba(0,0,0,0.5)";
            shCtx.fillRect(x, y, cellW-1, cellH-1);
            shCtx.fillStyle = "#ffe29a";
            shCtx.fillRect(x+1.5, y+1.5, cellW-4, cellH-4);
            shCtx.shadowBlur = 0;
          }
        }
      }
    }

    // Render pilihan 3 blok di container
    function renderShapeSelector() {
      shapesContainer.innerHTML = '';
      for (let i = 0; i < currentShapes.length; i++) {
        const shape = currentShapes[i];
        const cardDiv = document.createElement('div');
        cardDiv.className = 'shape-card';
        if (selectedShapeIndex === i) cardDiv.classList.add('selected');
        
        const canvas = document.createElement('canvas');
        canvas.width = 80;
        canvas.height = 80;
        canvas.classList.add('mini-canvas');
        drawShapeOnCanvas(canvas, shape, 80);
        
        cardDiv.appendChild(canvas);
        cardDiv.addEventListener('click', (e) => {
          e.stopPropagation();
          if (!gameActive) {
            gameStatusDiv.innerText = '⚠️ GAME OVER! Tekan RESTART untuk bermain lagi ⚠️';
            return;
          }
          // Pilih blok index i
          if (selectedShapeIndex === i) {
            // jika sudah terpilih, tetap terpilih (opsional: bisa unselect? biarkan saja)
          }
          selectedShapeIndex = i;
          updateShapeSelectorHighlight();
          gameStatusDiv.innerText = `✅ Blok "${shape.name}" siap. Klik di papan untuk menempatkan.`;
        });
        shapesContainer.appendChild(cardDiv);
      }
    }
    
    function updateShapeSelectorHighlight() {
      const cards = document.querySelectorAll('.shape-card');
      cards.forEach((card, idx) => {
        if (idx === selectedShapeIndex) card.classList.add('selected');
        else card.classList.remove('selected');
      });
    }

    // ----- LOGIKA PENEMPATAN & VALIDASI -----
    function canPlaceShape(shape, grid, startRow, startCol) {
      const pattern = shape.pattern;
      const rows = shape.height;
      const cols = shape.width;
      for (let dr = 0; dr < rows; dr++) {
        for (let dc = 0; dc < cols; dc++) {
          if (pattern[dr][dc] === 1) {
            const r = startRow + dr;
            const c = startCol + dc;
            if (r >= GRID_SIZE || c >= GRID_SIZE) return false;
            if (grid[r][c] !== 0) return false;
          }
        }
      }
      return true;
    }
    
    // Tempatkan shape ke grid (tanpa pembersihan baris/kolom dulu)
    function placeShapeOnGrid(shape, startRow, startCol) {
      const pattern = shape.pattern;
      const rows = shape.height;
      const cols = shape.width;
      for (let dr = 0; dr < rows; dr++) {
        for (let dc = 0; dc < cols; dc++) {
          if (pattern[dr][dc] === 1) {
            grid[startRow + dr][startCol + dc] = 1;
          }
        }
      }
    }
    
    // ----- HAPUS BARIS PENUH & KOLOM PENUH (khas Block Blast) -----
    function clearFullRowsAndColumns() {
      const rowsToClear = [];
      const colsToClear = [];
      
      // Cek baris penuh
      for (let r = 0; r < GRID_SIZE; r++) {
        let full = true;
        for (let c = 0; c < GRID_SIZE; c++) {
          if (grid[r][c] === 0) { full = false; break; }
        }
        if (full) rowsToClear.push(r);
      }
      
      // Cek kolom penuh
      for (let c = 0; c < GRID_SIZE; c++) {
        let full = true;
        for (let r = 0; r < GRID_SIZE; r++) {
          if (grid[r][c] === 0) { full = false; break; }
        }
        if (full) colsToClear.push(c);
      }
      
      if (rowsToClear.length === 0 && colsToClear.length === 0) return 0;
      
      const cellsToErase = new Set();
      for (let r of rowsToClear) {
        for (let c = 0; c < GRID_SIZE; c++) cellsToErase.add(`${r},${c}`);
      }
      for (let c of colsToClear) {
        for (let r = 0; r < GRID_SIZE; r++) cellsToErase.add(`${r},${c}`);
      }
      
      // Hapus semua sel yang terkena
      for (const coord of cellsToErase) {
        const [row, col] = coord.split(',').map(Number);
        grid[row][col] = 0;
      }
      
      return cellsToErase.size;   // jumlah sel yang hancur
    }
    
    // Cek apakah suatu shape bisa diletakkan di manapun pada grid
    function isAnyPlacementPossible(shape, grid) {
      for (let row = 0; row <= GRID_SIZE - shape.height; row++) {
        for (let col = 0; col <= GRID_SIZE - shape.width; col++) {
          if (canPlaceShape(shape, grid, row, col)) return true;
        }
      }
      return false;
    }
    
    // Cek GAME OVER: semua 3 blok tidak ada satupun yang bisa diletakkan
    function isGameOver() {
      for (let i = 0; i < currentShapes.length; i++) {
        if (isAnyPlacementPossible(currentShapes[i], grid)) return false;
      }
      return true;
    }
    
    // Mengganti shape pada slot tertentu dengan shape random baru
    function refreshShapeSlot(slotIndex) {
      currentShapes[slotIndex] = getRandomShape();
    }
    
    // Aksi utama: letakkan shape yang sedang terpilih pada koordinat (row, col)
    function attemptPlaceShape(row, col) {
      if (!gameActive) {
        gameStatusDiv.innerText = '❌ Game selesai. Tekan RESTART!';
        return false;
      }
      if (selectedShapeIndex === -1) {
        gameStatusDiv.innerText = '🔸 Pilih blok terlebih dahulu! (klik salah satu bentuk)';
        return false;
      }
      
      const shape = currentShapes[selectedShapeIndex];
      if (!canPlaceShape(shape, grid, row, col)) {
        gameStatusDiv.innerText = `🚫 Tidak bisa tempatkan di sini! Coba posisi lain.`;
        return false;
      }
      
      // --- Tempatkan blok ---
      placeShapeOnGrid(shape, row, col);
      
      // --- Hapus baris & kolom penuh, dapatkan poin ---
      const clearedCells = clearFullRowsAndColumns();
      let pointGain = 0;
      if (clearedCells > 0) {
        pointGain = clearedCells * 10;
        currentScore += pointGain;
        updateScoreUI();
        gameStatusDiv.innerText = `💥 +${pointGain} poin! (${clearedCells} sel hancur) 💥`;
      } else {
        gameStatusDiv.innerText = `📦 Blok "${shape.name}" ditempatkan.`;
      }
      
      // --- Ganti shape yang telah dipakai dengan shape random baru ---
      refreshShapeSlot(selectedShapeIndex);
      
      // --- Render ulang tampilan ---
      drawGrid();
      renderShapeSelector();   // refresh daftar blok
      
      // --- Cek Game Over setelah perubahan ---
      if (isGameOver()) {
        gameActive = false;
        gameStatusDiv.innerText = '💀 GAME OVER 💀  Tekan RESTART untuk memulai lagi.';
        drawGrid();  // tampilkan papan akhir
        renderShapeSelector();
        return true;
      }
      
      // Reset pilihan blok (karena sudah digunakan)
      selectedShapeIndex = -1;
      updateShapeSelectorHighlight();
      if (gameActive) {
        gameStatusDiv.innerText = '✨ Berhasil! Pilih blok lain & letakkan ✨';
      }
      return true;
    }
    
    // ------------------- RESTART GAME -------------------
    function restartGame() {
      // reset papan
      grid = Array(GRID_SIZE).fill().map(() => Array(GRID_SIZE).fill(0));
      currentScore = 0;
      gameActive = true;
      selectedShapeIndex = -1;
      updateScoreUI();
      
      // generate 3 blok acak baru
      currentShapes = initRandomShapes();
      
      // render ulang semua komponen
      drawGrid();
      renderShapeSelector();
      gameStatusDiv.innerText = '🔄 Permainan baru! Klik blok, lalu letakkan di papan.';
    }
    
    // ----- HANDLE KLIK PADA CANVAS (konversi ke koordinat grid) -----
    function handleCanvasClick(e) {
      if (!gameActive) {
        gameStatusDiv.innerText = '⛔ Game Over, tekan RESTART untuk bermain lagi.';
        return;
      }
      const rect = boardCanvas.getBoundingClientRect();
      const scaleX = boardCanvas.width / rect.width;
      const scaleY = boardCanvas.height / rect.height;
      
      let mouseX = (e.clientX - rect.left) * scaleX;
      let mouseY = (e.clientY - rect.top) * scaleY;
      
      const col = Math.floor(mouseX / CELL_SIZE);
      const row = Math.floor(mouseY / CELL_SIZE);
      
      if (row >= 0 && row < GRID_SIZE && col >= 0 && col < GRID_SIZE) {
        attemptPlaceShape(row, col);
      } else {
        gameStatusDiv.innerText = 'Klik di area papan (grid 8x8)';
      }
    }
    
    // ------------------- INITIALISASI GAME -------------------
    function init() {
      restartGame();  // siapkan state awal
      boardCanvas.addEventListener('click', handleCanvasClick);
      restartBtn.addEventListener('click', () => {
        restartGame();
      });
    }
    
    window.addEventListener('load', init);
  })();
</script>
</body>
</html>
