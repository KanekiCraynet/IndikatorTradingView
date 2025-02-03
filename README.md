# ZEE SIGNAL PREMIUM Indicator Documentation

## Table of Contents
1. [Overview](#overview)
2. [Key Features](#key-features)
3. [Indicator Components](#indicator-components)
   - [Golden Fibonacci Levels](#1-golden-fibonacci-levels)
   - [Liquidity Zones](#2-liquidity-zones)
   - [Order Blocks](#3-order-blocks)
   - [Market Structure Detection](#4-market-structure-detection)
   - [Pivot Points](#5-pivot-points)
4. [Configuration Options](#configuration-options)
5. [Alerts](#alerts)
6. [Visual Customization](#visual-customization)
7. [Usage Notes](#usage-notes)

---

## Overview <a name="overview"></a>
The **ZEE SIGNAL PREMIUM** is a multi-functional TradingView indicator designed to identify key market dynamics including:
- Support/Resistance Levels
- Liquidity Zones
- Market Structure Shifts
- Order Blocks
- Fibonacci Retracements
- Pivot Points

It combines institutional trading concepts with retail-friendly visualization for various trading styles (scalping, swing trading).

---

## Key Features <a name="key-features"></a>
1. **Golden Fibonacci System**: 
   - Daily Fibonacci extensions/retracements
   - 11 key Fibonacci levels (0.382-2.224)
   
2. **Liquidity Analysis**:
   - Daily/Weekly/Monthly liquidity zones
   - Historical liquidity tracking

3. **Order Block Detection**:
   - Bullish/Bearish order blocks
   - Timeframe-confluent blocks

4. **Market Structure**:
   - Break of Structure (BOS)
   - Change of Character (CHoCH)
   - Swing point detection

5. **Smart Pivot System**:
   - 6 calculation methods
   - Multi-timeframe support
   - Risk-level zones (High/Medium/Low)

---

## Indicator Components <a name="indicator-components"></a>

### 1. Golden Fibonacci Levels <a name="1-golden-fibonacci-levels"></a>
**Calculation**:
```pine
d_diff = dh - dl // Daily high-low range
d_382 = d_diff * 0.382
...
d_2240 = d_diff * 2.224
```
**Levels Plotting**:
- **Upper Cluster**: 
  - `dh + d_618` (Red)
  - `dh + d_1000` (Black)
- **Lower Cluster**:
  - `dl - d_618` (Red) 
  - `dl - d_1000` (Black)

**Key Function**:
- Identifies potential reversal zones
- Acts as dynamic support/resistance

---

### 2. Liquidity Zones <a name="2-liquidity-zones"></a>
**Timeframe Layers**:
| Timeframe | Color       | Width |
|-----------|-------------|-------|
| Daily     | Blue        | 1     |
| Weekly    | Yellow      | 1     |
| Monthly   | Purple      | 1     |

**Logic**:
```pine
// Daily liquidity
[prevDayHigh, prevDayLow] = request.security(..., "D", [high[1], low[1]])
// Weekly liquidity  
[prevWeekHigh, prevWeekLow] = request.security(..., "W", ...)
// Monthly liquidity
[prevMonthHigh, prevMonthLow] = request.security(..., "M", ...)
```

**Usage**:
- Identifies institutional order zones
- Highlights areas likely to trigger market reactions

---

### 3. Order Blocks <a name="3-order-blocks"></a>
**Detection Logic**:
1. **Bullish OB**:
   - Large down candle followed by up candle
   - Close > Open with high volume
2. **Bearish OB**:
   - Large up candle followed by down candle 
   - Close < Open with high volume

**Visualization**:
- **Bullish**: Light green zones
- **Bearish**: Light red zones

**Parameters**:
```pine
ob_showlast = 5 // Show last 5 blocks
ob_filter = 'Atr' // Filter by ATR threshold
```

---

### 4. Market Structure Detection <a name="4-market-structure-detection"></a>
**Key Concepts**:
- **BOS (Break of Structure)**: 
  - Violation of previous swing high/low
- **CHoCH (Change of Character)**:
  - Structural shift from bearish→bullish or vice versa

**Alert Conditions**:
```pine
alertcondition(bull_bos_alert, 'Bullish BOS', 'Break of Structure')
alertcondition(bear_choch_alert, 'Bearish CHoCH', 'Structural reversal')
```

**Structural Labels**:
- Strong High/Low
- Weak High/Low
- Demand/Supply Zones

---

### 5. Pivot Points <a name="5-pivot-points"></a>
**Calculation Methods**:
1. **Classic** 
2. **Fibonacci**
3. **Woodie**
4. **Camarilla**
5. **DM**
6. **Scalper**

**Risk Zones**:
| Level   | Risk        | Color       |
|---------|-------------|-------------|
| R1/S1  | High Risk   | Red/Blue    |
| R2/S2  | Medium Risk | Orange/Teal |
| R3/S3  | Low Risk    | Purple/Green|

**Configuration**:
```pine
kind = input.string("Scalper Mode", options=[LONGTERM, ARRAFIBS...])
pivot_time_frame = input.string("Daily", options=[AUTO, DAILY])
```

---

## Configuration Options <a name="configuration-options"></a>
**Main Toggles**:
```pine
dispdfibs = input(true, 'Show Fibo') // Toggle Fibonacci
show_liquidity = input(true, "Show Liquidity") 
show_ob = input(true, "Show Order Blocks")
```

**Visual Settings**:
```pine
// Fibo Colors
d_382_color = color.rgb(126, 41, 41) 
d_1000_color = color.black

// Label Positioning
position_labels = input("Right", options=["Left", "Right"])
```

**Advanced**:
```pine
max_lines_count = 500 // Maximum drawn lines
look_back = 2 // Historical periods to display
```

---

## Alerts <a name="alerts"></a>
**Available Triggers**:
1. Bullish/Bearish BOS
2. CHoCH Detection
3. Order Block Breakouts
4. Liquidity Zone Touches
5. Pivot Level Breaches

**Alert Setup Example**:
```pine
alertcondition(bull_bos_alert, 'Bullish BOS', 'Break of Structure')
alertcondition(pivot_break, 'Pivot Break', 'R1 Level Broken')
```

---

## Visual Customization <a name="visual-customization"></a>
**Color Schemes**:
```pine
// Bullish Elements
bull_css = #0ada7563 // Green
// Bearish Elements  
bear_css = #c40d0de8 // Red
```

**Line Styles**:
- Solid (Key levels)
- Dashed (Projections)
- Dotted (Historical)

---

## Usage Notes <a name="usage-notes"></a>
1. **Timeframe Recommendations**:
   - Scalping: 5M-15M with "Scalper Mode"
   - Swing Trading: 1H-4H with "Fibs Mode"
   - Position Trading: Daily+ with "Longterm Mode"

2. **Confluence Trading**:
   - Look for Fib levels coinciding with Pivot Points
   - Trade BOS/CHoCH signals near liquidity zones

3. **Risk Management**:
   - Use High Risk zones for tight stops
   - Medium/Low Risk zones for position scaling

4. **Performance**:
   - Works best with assets showing clear structure
   - Less effective in choppy/low-liquidity markets

---

**Note**: This documentation covers v5.0 of the ZEE SIGNAL PREMIUM indicator. Always test strategies in simulated environments before live trading.
