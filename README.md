# Tic-Tac-Toe with AI Teacher

A complete implementation of Tic-Tac-Toe with an AI Teacher agent that uses optimal strategy, all in an interactive Jupyter notebook.

## Project Overview

This project implements:
- **Game Logic**: Core tic-tac-toe game mechanics
- **AI Teacher Agent**: Optimal strategy-based AI player
- **Human Player**: Interactive gameplay
- **Analysis Tools**: Teacher vs Teacher with visualization
- **Multiple Difficulty Levels**: Easy (50%), Medium (75%), Hard (90%), Perfect (100%)

## Features

- Interactive gameplay (Human vs Teacher)
- AI vs AI analysis with statistics and graphs
- Multiple difficulty levels for Teacher AI
- Visual analysis with matplotlib
- All-in-one Jupyter notebook format

## Files

- `tictactoe_complete.ipynb` - Complete implementation in interactive notebook format

## Installation

### Requirements
```bash
pip install matplotlib
```

Python 3.6+ required

## How to Run

```bash
jupyter notebook tictactoe_complete.ipynb
```

Run cells in order to:
1. Load game logic and AI agents
2. Play Human vs Teacher (interactive)
3. Run Teacher vs Teacher analysis (with visualization)

## Teacher AI Strategy

The Teacher AI uses the following move hierarchy:

1. **Win** - Take winning move if available
2. **Block Win** - Block opponent's winning move
3. **Fork** - Create double threat
4. **Block Fork** - Block opponent's fork
5. **Center** - Take center square
6. **Corner** - Take corner square
7. **Side** - Take side square
8. **Random** - Random valid move

Difficulty levels control how often the Teacher follows optimal strategy:
- Easy: 50% optimal moves
- Medium: 75% optimal moves
- Hard: 90% optimal moves
- Perfect: 100% optimal moves

## Key Finding: Teacher Cannot Play Optimally as Second Player

### Observation

When running Teacher vs Teacher with both at 100% optimal difficulty:
- **Teacher 1 (X, first player)**: 100% win rate
- **Teacher 2 (O, second player)**: 0% win rate
- **Draws**: 0%

### Expected vs Actual Behavior

**Expected**: In optimal tic-tac-toe play, when both players play perfectly, every game should end in a draw.

**Actual**: The first player (X) wins 100% of games, indicating the Teacher AI has a bug when playing as the second player (O).

### Why This Happens

The Teacher AI's strategy is designed for offensive (first player) play rather than defensive (second player) play:

1. **First-Player Bias**: The move hierarchy prioritizes offensive tactics (forks, center control) that work well for X but not for O

2. **Incomplete Defensive Patterns**: The `blockFork()` method doesn't cover all defensive patterns needed when playing second against an optimal opponent

3. **Board Transformation Issues**: The `_transform_board()` method swaps X↔O for O's perspective, but some strategic patterns don't translate correctly

4. **Missing Counter-Strategies**: When X plays optimally and takes center or corner first, O needs specific defensive responses that aren't fully implemented

### Testing Results

| Configuration | Teacher 1 (X) | Teacher 2 (O) | T1 Wins | T2 Wins | Draws |
|---------------|---------------|---------------|---------|---------|-------|
| Both Perfect  | 100%          | 100%          | 100%    | 0%      | 0%    |
| T1 Perfect, T2 Hard | 100%  | 90%           | ~94%    | ~0%     | ~6%   |
| Both Hard     | 90%           | 90%           | ~78%    | ~7%     | ~15%  |

**Conclusion**: The Teacher AI only plays optimally as X (first position), not as O (second position).

## Board Positions

Positions are entered as `row,col` where both are 0-2:

```
0,0 | 0,1 | 0,2
----+-----+----
1,0 | 1,1 | 1,2
----+-----+----
2,0 | 2,1 | 2,2
```

## Visualization

The Teacher vs Teacher analysis generates graphs showing:
- Blue line: Teacher 1 (X) cumulative wins
- Red line: Teacher 2 (O) cumulative wins
- Green line: Draws
- Statistics box with final percentages

