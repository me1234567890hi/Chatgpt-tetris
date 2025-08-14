import React, { useEffect, useMemo, useRef, useState } from "react";

// --- Config ---
const COLS = 10;
const ROWS = 20;
const PREVIEW_SIZE = 4;
const LINES_PER_LEVEL = 10;
const START_SPEED_MS = 800; // drop every X ms at level 1
const MIN_SPEED_MS = 80;
const SPEED_STEP_MS = 60; // speed-up per level

const SHAPES = {
  I: [
    [[0,0,0,0],[1,1,1,1],[0,0,0,0],[0,0,0,0]],
    [[0,0,1,0],[0,0,1,0],[0,0,1,0],[0,0,1,0]],
    [[0,0,0,0],[0,0,0,0],[1,1,1,1],[0,0,0,0]],
    [[0,1,0,0],[0,1,0,0],[0,1,0,0],[0,1,0,0]],
  ],
  O: [
    [[0,1,1,0],[0,1,1,0],[0,0,0,0],[0,0,0,0]],
    [[0,1,1,0],[0,1,1,0],[0,0,0,0],[0,0,0,0]],
    [[0,1,1,0],[0,1,1,0],[0,0,0,0],[0,0,0,0]],
    [[0,1,1,0],[0,1,1,0],[0,0,0,0],[0,0,0,0]],
  ],
  T: [
    [[0,1,0,0],[1,1,1,0],[0,0,0,0],[0,0,0,0]],
    [[0,1,0,0],[0,1,1,0],[0,1,0,0],[0,0,0,0]],
    [[0,0,0,0],[1,1,1,0],[0,1,0,0],[0,0,0,0]],
    [[0,1,0,0],[1,1,0,0],[0,1,0,0],[0,0,0,0]],
  ],
  S: [
    [[0,1,1,0],[1,1,0,0],[0,0,0,0],[0,0,0,0]],
    [[0,1,0,0],[0,1,1,0],[0,0,1,0],[0,0,0,0]],
    [[0,0,0,0],[0,1,1,0],[1,1,0,0],[0,0,0,0]],
    [[1,0,0,0],[1,1,0,0],[0,1,0,0],[0,0,0,0]],
  ],
  Z: [
    [[1,1,0,0],[0,1,1,0],[0,0,0,0],[0,0,0,0]],
    [[0,0,1,0],[0,1,1,0],[0,1,0,0],[0,0,0,0]],
    [[0,0,0,0],[1,1,0,0],[0,1,1,0],[0,0,0,0]],
    [[0,1,0,0],[1,1,0,0],[1,0,0,0],[0,0,0,0]],
  ],
  J: [
    [[1,0,0,0],[1,1,1,0],[0,0,0,0],[0,0,0,0]],
    [[0,1,1,0],[0,1,0,0],[0,1,0,0],[0,0,0,0]],
    [[0,0,0,0],[1,1,1,0],[0,0,1,0],[0,0,0,0]],
    [[0,1,0,0],[0,1,0,0],[1,1,0,0],[0,0,0,0]],
  ],
  L: [
    [[0,0,1,0],[1,1,1,0],[0,0,0,0],[0,0,0,0]],
    [[0,1,0,0],[0,1,0,0],[0,1,1,0],[0,0,0,0]],
    [[0,0,0,0],[1,1,1,0],[1,0,0,0],[0,0,0,0]],
    [[1,1,0,0],[0,1,0,0],[0,1,0,0],[0,0,0,0]],
  ],
};

const COLORS = {
  I: "#2dd4bf",
  O: "#f59e0b",
  T: "#a78bfa",
  S: "#22c55e",
  Z: "#ef4444",
  J: "#3b82f6",
  L: "#f97316",
  X: "#111827", // board background cell
};

function emptyBoard() {
  return Array.from({ length: ROWS }, () => Array(COLS).fill(null));
}

function randomPiece() {
  const keys = Object.keys(SHAPES);
  const type = keys[Math.floor(Math.random() * keys.length)];
  return { type, rot: 0, x: 3, y: -1 };
}

function rotateMatrix(m) {
  // 4x4 matrix rotation CW
  const res = Array.from({ length: 4 }, () => Array(4).fill(0));
  for (let y = 0; y < 4; y++) {
    for (let x = 0; x < 4; x++) {
      res[x][3 - y] = m[y][x];
    }
  }
  return res;
}

function getShape(piece) {
  return SHAPES[piece.type][piece.rot % 4];
}

function collides(board, piece) {
  const shape = getShape(piece);
  for (let y = 0; y < 4; y++) {
    for (let x = 0; x < 4; x++) {
      if (!shape[y][x]) continue;
      const nx = piece.x + x;
      const ny = piece.y + y;
      if (nx < 0 || nx >= COLS || ny >= ROWS) return true;
      if (ny >= 0 && board[ny][nx]) return true;
    }
  }
  return false;
}

function merge(board, piece) {
  const copy = board.map((row) => row.slice());
  const shape = getShape(piece);
  for (let y = 0; y < 4; y++) {
    for (let x = 0; x < 4; x++) {
      if (shape[y][x]) {
        const nx = piece.x + x;
        const ny = piece.y + y;
        if (ny >= 0 && ny < ROWS && nx >= 0 && nx < COLS) {
          copy[ny][nx] = piece.type;
        }
      }
    }
  }
  return copy;
}

function clearLines(board) {
  let lines = 0;
  const next = [];
  for (let y = 0; y < ROWS; y++) {
    if (board[y].every((c) => c)) {
      lines++;
    } else {
      next.push(board[y]);
    }
  }
  while (next.length < ROWS) next.unshift(Array(COLS).fill(null));
  return { next, lines };
}

function scoreForLines(n) {
  // Standard-ish Tetris scoring per line clear (single 100, double 300, triple 500, tetris 800)
  const table = { 0: 0, 1: 100, 2: 300, 3: 500, 4: 800 };
  return table[n] || 0;
}

function speedForLevel(level) {
  return Math.max(MIN_SPEED_MS, START_SPEED_MS - (level - 1) * SPEED_STEP_MS);
}

export default function Tetris() {
  const [board, setBoard] = useState(emptyBoard);
  const [current, setCurrent] = useState(randomPiece);
  const [nextPiece, setNextPiece] = useState(randomPiece);
  const [running, setRunning] = useState(false);
  const [gameOver, setGameOver] = useState(false);
  const [score, setScore] = useState(0);
  const [lines, setLines] = useState(0);
  const level = Math.floor(lines / LINES_PER_LEVEL) + 1;

  const dropTimer = useRef(null);
  const holdDown = useRef(false);

  const placeNewPiece = (p) => {
    if (collides(board, p)) {
      setRunning(false);
      setGameOver(true);
      return false;
    }
    setCurrent(p);
    return true;
  };

  const spawn = () => {
    const fresh = { ...nextPiece, x: 3, y: -1, rot: 0 };
    setNextPiece(randomPiece());
    placeNewPiece(fresh);
  };

  const tryMove = (dx, dy, droplike = false) => {
    const np = { ...current, x: current.x + dx, y: current.y + dy };
    if (!collides(board, np)) {
      setCurrent(np);
      return true;
    } else if (droplike && dy > 0) {
      // lock piece
      const merged = merge(board, current);
      const { next, lines: cleared } = clearLines(merged);
      setBoard(next);
      if (cleared) {
        setLines((l) => l + cleared);
        setScore((s) => s + scoreForLines(cleared) * level);
      }
      spawn();
      return false;
    }
    return false;
  };

  const hardDrop = () => {
    let np = { ...current };
    while (!collides(board, { ...np, y: np.y + 1 })) {
      np.y++;
    }
    // lock
    const merged = merge(board, np);
    const { next, lines: cleared } = clearLines(merged);
    setBoard(next);
    setScore((s) => s + 2 * (np.y - current.y)); // small bonus
    if (cleared) {
      setLines((l) => l + cleared);
      setScore((s) => s + scoreForLines(cleared) * level);
    }
    spawn();
  };

  const rotate = () => {
    const nextRot = (current.rot + 1) % 4;
    const np = { ...current, rot: nextRot };
    // simple wall kicks: try offsets
    const kicks = [0, -1, 1, -2, 2];
    for (const k of kicks) {
      const test = { ...np, x: np.x + k };
      if (!collides(board, test)) {
        setCurrent(test);
        return;
      }
    }
  };

  const reset = () => {
    setBoard(emptyBoard());
    setCurrent(randomPiece());
    setNextPiece(randomPiece());
    setScore(0);
    setLines(0);
    setGameOver(false);
    setRunning(true);
  };

  // Input handlers
  useEffect(() => {
    const onKey = (e) => {
      if (!running && !gameOver && (e.key === "Enter" || e.key === " ")) {
        setRunning(true);
        return;
      }
      if (gameOver) return;
      switch (e.key) {
        case "ArrowLeft":
          if (running) tryMove(-1, 0);
          break;
        case "ArrowRight":
          if (running) tryMove(1, 0);
          break;
        case "ArrowDown":
          if (running) tryMove(0, 1, true);
          break;
        case "ArrowUp":
          if (running) rotate();
          break;
        case " ": // Space
          if (running) hardDrop();
          break;
        case "p":
        case "P":
          if (!gameOver) setRunning((r) => !r);
          break;
        case "r":
        case "R":
          reset();
          break;
        default:
          break;
      }
    };
    window.addEventListener("keydown", onKey);
    return () => window.removeEventListener("keydown", onKey);
  }, [running, gameOver, current, board, level]);

  // Drop timer
  useEffect(() => {
    if (!running || gameOver) return;
    const interval = speedForLevel(level);
    dropTimer.current && clearInterval(dropTimer.current);
    dropTimer.current = setInterval(() => {
      tryMove(0, 1, true);
    }, interval);
    return () => {
      dropTimer.current && clearInterval(dropTimer.current);
    };
  }, [running, gameOver, level, current, board]);

  // Render helpers
  const displayBoard = useMemo(() => {
    // overlay current piece on the board for rendering
    const b = board.map((row) => row.slice());
    const shape = getShape(current);
    for (let y = 0; y < 4; y++) {
      for (let x = 0; x < 4; x++) {
        if (!shape[y][x]) continue;
        const nx = current.x + x;
        const ny = current.y + y;
        if (ny >= 0 && ny < ROWS && nx >= 0 && nx < COLS) {
          b[ny][nx] = current.type;
        }
      }
    }
    return b;
  }, [board, current]);

  const nextMatrix = useMemo(() => SHAPES[nextPiece.type][0], [nextPiece]);

  return (
    <div className="min-h-screen w-full flex items-center justify-center bg-slate-900 p-6 text-slate-100">
      <div className="grid lg:grid-cols-[minmax(0,1fr)_300px] gap-6 w-full max-w-5xl">
        {/* Game Panel */}
        <div className="bg-slate-800 rounded-2xl p-4 shadow-xl">
          <div className="flex items-center justify-between mb-4">
            <h1 className="text-2xl font-bold tracking-tight">Tetris</h1>
            <div className="text-sm opacity-80">Level {level}</div>
          </div>
          <div className="aspect-[1/2] w-full mx-auto max-w-[480px]">
            <div
              className="grid w-full h-full"
              style={{ gridTemplateColumns: `repeat(${COLS}, 1fr)` }}
            >
              {displayBoard.map((row, y) =>
                row.map((cell, x) => (
                  <div
                    key={`${x}-${y}`}
                    className="border border-slate-800/60"
                    style={{
                      background:
                        cell ? COLORS[cell] : "#0f172a",
                      boxShadow: cell ? "inset 0 0 6px rgba(0,0,0,0.4)" : undefined,
                    }}
                  />
                ))
              )}
            </div>
          </div>

          <div className="mt-4 flex items-center gap-2 flex-wrap">
            {!running && !gameOver && (
              <button onClick={() => setRunning(true)} className="px-4 py-2 rounded-xl bg-emerald-500 hover:opacity-90 transition shadow">Start</button>
            )}
            {running && (
              <button onClick={() => setRunning(false)} className="px-4 py-2 rounded-xl bg-amber-500 hover:opacity-90 transition shadow">Pause</button>
            )}
            {!running && !gameOver && (
              <button onClick={reset} className="px-4 py-2 rounded-xl bg-sky-500 hover:opacity-90 transition shadow">Reset</button>
            )}
            {gameOver && (
              <button onClick={reset} className="px-4 py-2 rounded-xl bg-rose-500 hover:opacity-90 transition shadow">Play Again</button>
            )}
            <div className="ml-auto text-sm opacity-80">Use arrows, ↑ rotate, Space drop, P pause, R reset</div>
          </div>
        </div>

        {/* Sidebar */}
        <div className="bg-slate-800 rounded-2xl p-4 shadow-xl">
          <div className="space-y-4">
            <div>
              <div className="text-sm uppercase tracking-wide opacity-80">Score</div>
              <div className="text-3xl font-bold">{score}</div>
            </div>
            <div>
              <div className="text-sm uppercase tracking-wide opacity-80">Lines</div>
              <div className="text-2xl font-semibold">{lines}</div>
            </div>
            <div>
              <div className="text-sm uppercase tracking-wide opacity-80">Next</div>
              <div className="grid grid-cols-4 gap-[2px] bg-slate-900 p-[2px] rounded-lg w-40 h-40">
                {nextMatrix.flat().map((v, i) => (
                  <div
                    key={i}
                    className="rounded-sm"
                    style={{
                      background: v ? COLORS[nextPiece.type] : "#0b1220",
                      border: "1px solid rgba(15,23,42,0.6)",
                    }}
                  />
                ))}
              </div>
            </div>
            {gameOver && (
              <div className="p-3 rounded-xl bg-rose-500/20 border border-rose-500/40">
                <div className="font-semibold">Game Over</div>
                <div className="text-sm opacity-90">Press Play Again or hit R to restart.</div>
              </div>
            )}

            {/* Mobile controls */}
            <div className="mt-2 grid grid-cols-3 gap-2 sm:hidden">
              <button onClick={() => tryMove(-1, 0)} className="p-3 rounded-xl bg-slate-700">◀</button>
              <button onClick={() => rotate()} className="p-3 rounded-xl bg-slate-700">⟳</button>
              <button onClick={() => tryMove(1, 0)} className="p-3 rounded-xl bg-slate-700">▶</button>
              <button onClick={() => tryMove(0, 1, true)} className="col-span-2 p-3 rounded-xl bg-slate-700">Soft Drop</button>
              <button onClick={() => hardDrop()} className="p-3 rounded-xl bg-slate-700">Hard Drop</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
