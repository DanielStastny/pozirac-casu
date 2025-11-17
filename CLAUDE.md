# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Czech-language web application for a countdown timer with a pixel art animation. The timer features a "požírač" (eater) character that progressively consumes a snake as time counts down, ending with an explosion when the timer reaches zero.

## Project Requirements (from instrukce.md)

The application implements the following workflow:
1. User enters time in minutes using up/down arrows (0-1000 minutes), then clicks Start
2. Timer begins countdown in HH:mm:ss format displayed above the animation
3. Animation progresses proportionally to remaining time
4. When time expires, animation ends with an explosion
5. During countdown: Pause and Reset buttons are available
6. During pause: User can adjust time up/down by minutes using arrow buttons
7. All controls are positioned below the animation

## Key Files

- **animace_pixel.html** - Base pixel art animation (snake & eater with chewing mouth animation)
- **eater_data.json** - Pixel coordinate data for the eater character (two arrays: "filled" for body, "mouth" for mouth area)
- **instrukce.md** - Czech language requirements specification

## Animation Architecture

### Core Animation Parameters (from animace_pixel.html)
- Grid-based system: 16px grid units
- SVG viewBox: 100% x 100%
- Snake dimensions:
  - Body: 96 columns × 6 rows (1536px × 96px)
  - Head: 14 columns (224px)
  - Tongue: 4 columns (64px)
  - Total length: 1824px
- Animation timing:
  - Eating phase: 7000ms
  - Explosion phase: 1300ms
  - Total cycle: 8300ms

### Animation System
1. **Eating Phase**: Eater moves left-to-right while snake is clipped from left-to-right using SVG clipPath
2. **Mouth Animation**: Chewing motion using sinusoidal oscillation (3.5-4 Hz frequency)
3. **Explosion Phase**: Scale transformation, particle system with 120-170 colored rectangles, flash effects

### Key Animation Functions
- `buildSnakePixels()` - Generates procedural snake body pattern
- `updateChewingMouth(elapsed)` / `animateMouth(elapsed)` - Handles mouth animation
- `initBits()` - Creates explosion particle system
- `step(timestamp)` - Main requestAnimationFrame loop

## Development Notes

### Data Format
The eater pixel data uses coordinate objects with x/y properties representing a 32×32 grid where each coordinate is a filled 16px square in the final render.

### Styling Approach
Uses CSS custom properties for theming:
- Snake colors: dark (#142749), mid (#1f3b63), light (#8fb9eb), belly (#bed5f2)
- Eater colors: body (#ece4c8), outline (#2b4054)
- Background: gradient with soft blues

### To Integrate Timer
When building the final timer application:
1. Extract animation logic from `animace_pixel.html`
2. Replace hardcoded `eatDuration` (7000ms) with user-input time converted to milliseconds
3. Add UI controls (minute input with arrows, Start/Pause/Reset buttons) below the SVG
4. Display countdown in HH:mm:ss format centered above the animation
5. Synchronize animation progress (`p = elapsed / totalDuration`) with actual countdown
6. Implement pause state that freezes both timer and animation, allows time adjustment
