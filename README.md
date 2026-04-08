<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>R/Akira Servers — Cosmic Control Panel</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- FONTS -->
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">

<style>
/* --------------------------------------------------
   GLOBAL RESET & BASE
   -------------------------------------------------- */
*,
*::before,
*::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html, body {
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at top, #1a1a1a 0%, #050505 45%, #020202 100%);
    color: #f0f0f0;
    font-family: 'Poppins', sans-serif;
    overflow-x: hidden;
}

/* Smooth scroll */
html {
    scroll-behavior: smooth;
}

/* Selection */
::selection {
    background: rgba(200, 200, 200, 0.4);
    color: #fff;
}

/* --------------------------------------------------
   CUSTOM CURSOR + NETWORK
   -------------------------------------------------- */
body {
    cursor: none;
}

.cursor-core {
    position: fixed;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: #e0e0e0;
    box-shadow: 0 0 12px #e0e0e0, 0 0 30px #aaaaaa;
    pointer-events: none;
    z-index: 9999;
    transform: translate(-50%, -50%);
}

.cursor-ring {
    position: fixed;
    width: 60px;
    height: 60px;
    border-radius: 50%;
    border: 1px solid rgba(220, 220, 220, 0.4);
    box-shadow: 0 0 40px rgba(180, 180, 180, 0.4);
    pointer-events: none;
    z-index: 9998;
    transform: translate(-50%, -50%);
    transition: transform 0.12s ease-out, width 0.12s ease-out, height 0.12s ease-out;
}

.cursor-spotlight {
    position: fixed;
    width: 260px;
    height: 260px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(180, 180, 180, 0.18), transparent 70%);
    pointer-events: none;
    z-index: 1;
    transform: translate(-50%, -50%);
    filter: blur(18px);
}

/* --------------------------------------------------
   STARFIELD CANVAS
   -------------------------------------------------- */
#starfield {
    position: fixed;
    inset: 0;
    z-index: 0;
    background: transparent;
}

/* --------------------------------------------------
   BACKGROUND PLANETS & ORBITS
   -------------------------------------------------- */
.planet {
    position: fixed;
    border-radius: 50%;
    filter: blur(0px);
    opacity: 0.75;
    z-index: 0;
    pointer-events: none;
}

.planet-1 {
    width: 520px;
    height: 520px;
    background:
        radial-gradient(circle at 30% 26%, rgba(255,255,255,0.7) 0%, rgba(255,255,255,0.15) 22%, transparent 48%),
        radial-gradient(circle at 72% 74%, rgba(255,255,255,0.14) 0%, transparent 35%),
        radial-gradient(ellipse at 62% 65%, rgba(0,0,0,0.65) 0%, transparent 55%),
        radial-gradient(circle at 50% 50%, #666666 0%, #2a2a2a 45%, #0a0a0a 100%);
    box-shadow:
        inset -60px -60px 100px rgba(0,0,0,0.85),
        inset 30px 30px 70px rgba(255,255,255,0.07),
        inset 0 -20px 60px rgba(0,0,0,0.6),
        0 0 80px rgba(0,0,0,0.6);
    top: -180px;
    left: -220px;
    animation: floatPlanet1 18s ease-in-out infinite alternate;
}

.planet-2 {
    width: 380px;
    height: 380px;
    background:
        radial-gradient(circle at 28% 24%, rgba(255,255,255,0.75) 0%, rgba(255,255,255,0.12) 20%, transparent 45%),
        radial-gradient(circle at 70% 72%, rgba(255,255,255,0.12) 0%, transparent 32%),
        radial-gradient(ellipse at 60% 64%, rgba(0,0,0,0.6) 0%, transparent 52%),
        radial-gradient(circle at 50% 50%, #888888 0%, #333333 45%, #080808 100%);
    box-shadow:
        inset -44px -44px 80px rgba(0,0,0,0.85),
        inset 22px 22px 55px rgba(255,255,255,0.08),
        inset 0 -16px 50px rgba(0,0,0,0.55),
        0 0 60px rgba(0,0,0,0.5);
    bottom: -160px;
    right: -200px;
    animation: floatPlanet2 22s ease-in-out infinite alternate;
}

.planet-3 {
    width: 260px;
    height: 260px;
    background:
        radial-gradient(circle at 28% 24%, rgba(255,255,255,0.7) 0%, rgba(255,255,255,0.10) 20%, transparent 44%),
        radial-gradient(circle at 68% 70%, rgba(255,255,255,0.10) 0%, transparent 30%),
        radial-gradient(ellipse at 60% 64%, rgba(0,0,0,0.6) 0%, transparent 50%),
        radial-gradient(circle at 50% 50%, #777777 0%, #2e2e2e 45%, #080808 100%);
    box-shadow:
        inset -30px -30px 55px rgba(0,0,0,0.85),
        inset 16px 16px 38px rgba(255,255,255,0.07),
        inset 0 -12px 36px rgba(0,0,0,0.55),
        0 0 45px rgba(0,0,0,0.45);
    top: 40%;
    right: 10%;
    opacity: 0.7;
    animation: floatPlanet3 26s ease-in-out infinite alternate;
}

@keyframes floatPlanet1 {
    to { transform: translate(60px, 40px) scale(1.05); }
}
@keyframes floatPlanet2 {
    to { transform: translate(-50px, -60px) scale(1.08); }
}
@keyframes floatPlanet3 {
    to { transform: translate(-40px, 30px) scale(1.1); }
}

/* Orbit ring behind hero */
.orbit-ring {
    position: absolute;
    width: 520px;
    height: 520px;
    border-radius: 50%;
    border: 1px dashed rgba(200, 200, 200, 0.25);
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 40px rgba(150, 150, 150, 0.25);
    opacity: 0.7;
}

/* --------------------------------------------------
   SATURN PLANET (background decoration)
   -------------------------------------------------- */
.saturn-bg {
    position: fixed;
    top: 8%;
    right: 4%;
    width: 300px;
    height: 300px;
    z-index: 0;
    pointer-events: none;
    perspective: 600px;
    animation: floatSaturn 10s ease-in-out infinite alternate;
}

.saturn-scene {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 0;
    height: 0;
    transform-style: preserve-3d;
}

.saturn-body {
    position: absolute;
    width: 180px;
    height: 180px;
    border-radius: 50%;
    transform: translate(-50%, -50%);
    background:
        radial-gradient(circle at 28% 24%, rgba(255,255,255,0.85) 0%, rgba(255,255,255,0.25) 18%, transparent 42%),
        radial-gradient(circle at 65% 68%, rgba(255,255,255,0.18) 0%, transparent 30%),
        radial-gradient(ellipse at 50% 60%, rgba(0,0,0,0.55) 0%, transparent 65%),
        radial-gradient(circle at 50% 50%, #e0e0e0 0%, #999999 30%, #444444 65%, #111111 100%);
    box-shadow:
        inset -28px -28px 55px rgba(0,0,0,0.75),
        inset 18px 18px 40px rgba(255,255,255,0.22),
        inset 0px -10px 30px rgba(0,0,0,0.5),
        0 0 60px rgba(200,200,200,0.25),
        0 0 120px rgba(150,150,150,0.1);
    z-index: 2;
}

/* Glass rim highlight */
.saturn-body::after {
    content: '';
    position: absolute;
    inset: 0;
    border-radius: 50%;
    background: radial-gradient(circle at 50% 8%, rgba(255,255,255,0.35) 0%, transparent 40%);
    pointer-events: none;
}

.saturn-ring-outer {
    position: absolute;
    width: 380px;
    height: 380px;
    border-radius: 50%;
    border: 18px solid transparent;
    border-top-color: rgba(220, 220, 220, 0.7);
    border-bottom-color: rgba(220, 220, 220, 0.7);
    box-shadow:
        0 0 20px rgba(220,220,220,0.3),
        inset 0 0 12px rgba(255,255,255,0.15);
    transform: translate(-50%, -50%) rotateX(75deg);
    animation: spinRing 4s linear infinite;
    z-index: 1;
}

.saturn-ring-mid {
    position: absolute;
    width: 300px;
    height: 300px;
    border-radius: 50%;
    border: 10px solid transparent;
    border-top-color: rgba(180, 180, 180, 0.55);
    border-bottom-color: rgba(180, 180, 180, 0.55);
    box-shadow: 0 0 14px rgba(180,180,180,0.2);
    transform: translate(-50%, -50%) rotateX(75deg);
    animation: spinRing 4s linear infinite;
    z-index: 1;
}

.saturn-ring-inner {
    position: absolute;
    width: 230px;
    height: 230px;
    border-radius: 50%;
    border: 6px solid transparent;
    border-top-color: rgba(140, 140, 140, 0.4);
    border-bottom-color: rgba(140, 140, 140, 0.4);
    transform: translate(-50%, -50%) rotateX(75deg);
    animation: spinRing 4s linear infinite;
    z-index: 1;
}

@keyframes spinRing {
    from { transform: translate(-50%, -50%) rotateX(75deg) rotateZ(0deg); }
    to   { transform: translate(-50%, -50%) rotateX(75deg) rotateZ(360deg); }
}

@keyframes floatSaturn {
    0%   { transform: translate(0px, 0px) rotate(0deg); }
    25%  { transform: translate(-20px, 30px) rotate(4deg); }
    50%  { transform: translate(10px, 50px) rotate(-3deg); }
    75%  { transform: translate(30px, 20px) rotate(5deg); }
    100% { transform: translate(-10px, 40px) rotate(-2deg); }
}

/* --------------------------------------------------
   LAYOUT WRAPPER
   -------------------------------------------------- */
.app-shell {
    position: relative;
    z-index: 5;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}

/* --------------------------------------------------
   NAVBAR
   -------------------------------------------------- */
.navbar {
    position: sticky;
    top: 0;
    z-index: 20;
    backdrop-filter: blur(18px);
    background: linear-gradient(to right, rgba(5, 5, 5, 0.9), rgba(10, 10, 10, 0.85));
    border-bottom: 1px solid rgba(255, 255, 255, 0.06);
}

.nav-inner {
    max-width: 1200px;
    margin: 0 auto;
    padding: 14px 24px;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.nav-left {
    display: flex;
    align-items: center;
    gap: 10px;
}

.brand-orb {
    width: 26px;
    height: 26px;
    border-radius: 50%;
    background: radial-gradient(circle, #ffffff 0%, #aaaaaa 40%, #333333 100%);
    box-shadow: 0 0 18px #aaaaaa;
}

.brand-text {
    font-weight: 700;
    letter-spacing: 0.08em;
    font-size: 0.95rem;
    text-transform: uppercase;
    color: #f0f0f0;
}

.nav-links {
    display: flex;
    align-items: center;
    gap: 18px;
    font-size: 0.9rem;
}

.nav-link {
    position: relative;
    color: #cccccc;
    text-decoration: none;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    font-weight: 500;
    padding-bottom: 4px;
}

.nav-link::after {
    content: "";
    position: absolute;
    left: 0;
    bottom: 0;
    width: 0%;
    height: 1px;
    background: linear-gradient(90deg, #ffffff, #888888);
    transition: width 0.25s ease;
}

.nav-link:hover::after {
    width: 100%;
}

.nav-cta {
    padding: 8px 16px;
    border-radius: 999px;
    border: 1px solid rgba(255, 255, 255, 0.18);
    background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.12), rgba(20, 20, 20, 0.9));
    box-shadow: 0 0 18px rgba(180, 180, 180, 0.3);
    font-size: 0.8rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    cursor: pointer;
    color: #f0f0f0;
    transition: transform 0.18s ease, box-shadow 0.18s ease, border-color 0.18s ease;
    text-decoration: none;
}

.nav-cta:hover {
    transform: translateY(-1px);
    border-color: rgba(255, 255, 255, 0.4);
    box-shadow: 0 0 26px rgba(220, 220, 220, 0.6);
}

/* --------------------------------------------------
   HERO SECTION
   -------------------------------------------------- */
.hero {
    position: relative;
    max-width: 1200px;
    margin: 0 auto;
    padding: 80px 24px 60px;
    display: grid;
    grid-template-columns: minmax(0, 1.2fr) minmax(0, 1fr);
    gap: 40px;
    align-items: center;
}

.hero-left {
    position: relative;
    z-index: 2;
}

.hero-kicker {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 4px 10px;
    border-radius: 999px;
    border: 1px solid rgba(255, 255, 255, 0.12);
    background: radial-gradient(circle at left, rgba(255, 255, 255, 0.10), rgba(20, 20, 20, 0.7));
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 0.16em;
    color: #f0f0f0;
    margin-bottom: 18px;
}

.hero-kicker-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #aaaaaa;
    box-shadow: 0 0 10px #aaaaaa;
}

.hero-title {
    font-size: 3.2rem;
    line-height: 1.05;
    margin-bottom: 16px;
    background: linear-gradient(120deg, #ffffff, #dddddd, #aaaaaa, #eeeeee);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    text-shadow: 0 0 30px rgba(180, 180, 180, 0.4);
}

.hero-subtitle {
    font-size: 1rem;
    max-width: 520px;
    color: #cccccc;
    opacity: 0.9;
    margin-bottom: 26px;
}

.hero-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-bottom: 26px;
}

.hero-tag {
    padding: 6px 12px;
    border-radius: 999px;
    border: 1px solid rgba(255, 255, 255, 0.12);
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: #f0f0f0;
    background: radial-gradient(circle at top, rgba(255, 255, 255, 0.08), rgba(15, 15, 15, 0.9));
}

.hero-actions {
    display: flex;
    flex-wrap: wrap;
    gap: 14px;
    margin-bottom: 24px;
}

.hero-btn-primary {
    padding: 10px 22px;
    border-radius: 999px;
    border: 1px solid rgba(255, 255, 255, 0.18);
    background: linear-gradient(135deg, #ffffff, #aaaaaa, #444444);
    box-shadow: 0 0 26px rgba(200, 200, 200, 0.6);
    font-size: 0.85rem;
    text-transform: uppercase;
    letter-spacing: 0.16em;
    cursor: pointer;
    color: #111111;
    font-weight: 600;
    transition: transform 0.18s ease, box-shadow 0.18s ease, filter 0.18s ease;
    text-decoration: none;
}

.hero-btn-primary:hover {
    transform: translateY(-1px);
    filter: brightness(1.1);
    box-shadow: 0 0 34px rgba(240, 240, 240, 0.9);
}

.hero-btn-ghost {
    padding: 10px 18px;
    border-radius: 999px;
    border: 1px solid rgba(255, 255, 255, 0.18);
    background: radial-gradient(circle at top, rgba(255, 255, 255, 0.06), rgba(10, 10, 10, 0.9));
    font-size: 0.8rem;
    text-transform: uppercase;
    letter-spacing: 0.16em;
    cursor: pointer;
    color: #f0f0f0;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    transition: transform 0.18s ease, box-shadow 0.18s ease, border-color 0.18s ease;
}

.hero-btn-ghost:hover {
    transform: translateY(-1px);
    border-color: rgba(255, 255, 255, 0.4);
    box-shadow: 0 0 24px rgba(180, 180, 180, 0.6);
}

.hero-btn-ghost-icon {
    width: 14px;
    height: 14px;
    border-radius: 50%;
    border: 1px solid rgba(255, 255, 255, 0.5);
    display: inline-flex;
    align-items: center;
    justify-content: center;
    font-size: 0.6rem;
}

/* Hero meta row */
.hero-meta {
    display: flex;
    flex-wrap: wrap;
    gap: 18px;
    font-size: 0.75rem;
    color: #aaaaaa;
    opacity: 0.9;
}

.hero-meta-item {
    display: flex;
    align-items: center;
    gap: 6px;
}

.hero-meta-dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: #aaaaaa;
    box-shadow: 0 0 10px #aaaaaa;
}

/* Hero right: 3D-ish panel */
.hero-right {
    position: relative;
    min-height: 260px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.hero-orbit-wrapper {
    position: relative;
    width: 420px;
    height: 420px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.hero-core-planet {
    position: relative;
    width: 180px;
    height: 180px;
    border-radius: 50%;
    background:
        radial-gradient(circle at 30% 26%, rgba(255,255,255,0.75) 0%, rgba(255,255,255,0.08) 30%, transparent 52%),
        radial-gradient(circle at 70% 72%, rgba(255,255,255,0.12) 0%, transparent 35%),
        radial-gradient(circle at 50% 50%, #cccccc 0%, #888888 40%, #333333 75%, #111111 100%);
    box-shadow:
        inset -14px -14px 30px rgba(0,0,0,0.65),
        inset 10px 10px 24px rgba(255,255,255,0.18),
        0 0 50px rgba(200,200,200,0.35),
        0 0 100px rgba(150,150,150,0.15);
    overflow: hidden;
}

.hero-core-inner-ring {
    position: absolute;
    inset: 18px;
    border-radius: 50%;
    border: 1px solid rgba(255, 255, 255, 0.35);
    box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
    opacity: 0.9;
}

.hero-core-label {
    position: absolute;
    bottom: 18px;
    left: 50%;
    transform: translateX(-50%);
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 0.16em;
    color: #111111;
    font-weight: 700;
}

/* Saturn ring */
.hero-saturn-ring {
    position: absolute;
    width: 260px;
    height: 260px;
    border-radius: 50%;
    border: 2px solid rgba(255, 255, 255, 0.4);
    border-left-color: transparent;
    border-right-color: transparent;
    transform: rotate(-18deg);
    box-shadow: 0 0 30px rgba(200, 200, 200, 0.6);
}

/* Orbiting nodes */
.hero-node {
    position: absolute;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    background: radial-gradient(circle, #f0f0f0 0%, #aaaaaa 60%, #333333 100%);
    box-shadow: 0 0 18px rgba(200, 200, 200, 0.8);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.55rem;
    color: #111111;
    font-weight: 700;
}

.hero-node-label {
    position: absolute;
    top: 22px;
    left: 50%;
    transform: translateX(-50%);
    white-space: nowrap;
    font-size: 0.65rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: #f0f0f0;
}

/* Node positions */
.hero-node-1 { top: 40px; left: 50%; transform: translateX(-50%); }
.hero-node-2 { right: 40px; top: 50%; transform: translateY(-50%); }
.hero-node-3 { bottom: 40px; left: 50%; transform: translateX(-50%); }

/* --------------------------------------------------
   SECTION: GAME MATRIX
   -------------------------------------------------- */
.section {
    max-width: 1200px;
    margin: 0 auto;
    padding: 40px 24px 20px;
}

.section-header {
    display: flex;
    align-items: baseline;
    justify-content: space-between;
    gap: 20px;
    margin-bottom: 20px;
}

.section-title {
    font-size: 1.4rem;
    font-weight: 600;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: #f0f0f0;
}

.section-subtitle {
    font-size: 0.85rem;
    color: #aaaaaa;
    max-width: 420px;
}

/* Game grid */
.game-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 18px;
}

.game-card {
    position: relative;
    border-radius: 18px;
    padding: 18px 16px 16px;
    background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.06), rgba(10, 10, 10, 0.96));
    border: 1px solid rgba(255, 255, 255, 0.08);
    box-shadow: 0 0 22px rgba(0, 0, 0, 0.9);
    overflow: hidden;
    transition: transform 0.18s ease, box-shadow 0.18s ease, border-color 0.18s ease, background 0.18s ease;
    cursor: pointer;
}

.game-card::before {
    content: "";
    position: absolute;
    inset: -40%;
    background: conic-gradient(from 180deg, rgba(255, 255, 255, 0.08), transparent, rgba(180, 180, 180, 0.12), transparent);
    opacity: 0;
    transition: opacity 0.25s ease;
}

.game-card-inner {
    position: relative;
    z-index: 2;
}

.game-card:hover {
    transform: translateY(-4px);
    border-color: rgba(255, 255, 255, 0.22);
    box-shadow: 0 0 30px rgba(200, 200, 200, 0.5);
    background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.12), rgba(10, 10, 10, 0.98));
}

.game-card:hover::before {
    opacity: 1;
}

.game-card-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 10px;
}

.game-name {
    font-size: 1rem;
    font-weight: 600;
    letter-spacing: 0.08em;
    text-transform: uppercase;
}

.game-pill {
    font-size: 0.65rem;
    text-transform: uppercase;
    letter-spacing: 0.16em;
    padding: 4px 10px;
    border-radius: 999px;
    border: 1px solid rgba(255, 255, 255, 0.18);
    color: #f0f0f0;
    background: radial-gradient(circle at top, rgba(255, 255, 255, 0.10), rgba(20, 20, 20, 0.9));
}

.game-meta-row {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 0.7rem;
    color: #aaaaaa;
    margin-bottom: 10px;
}

.game-meta-dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: #aaaaaa;
    box-shadow: 0 0 10px #aaaaaa;
}

.game-options {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 8px;
    margin-top: 8px;
}

.game-option-link {
    display: inline-block;
    width: 100%;
    text-decoration: none;
}

.game-option {
    width: 100%;
    padding: 8px 10px;
    border-radius: 10px;
    border: 1px solid rgba(255, 255, 255, 0.12);
    background: radial-gradient(circle at top, rgba(255, 255, 255, 0.06), rgba(10, 10, 10, 0.96));
    font-size: 0.75rem;
    text-align: center;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: #f0f0f0;
    transition: background 0.18s ease, border-color 0.18s ease, transform 0.18s ease;
}

.game-option:hover {
    background: radial-gradient(circle at top, rgba(255, 255, 255, 0.14), rgba(20, 20, 20, 0.98));
    border-color: rgba(255, 255, 255, 0.3);
    transform: translateY(-1px);
}

/* --------------------------------------------------
   SECTION: STATUS / TELEMETRY
   -------------------------------------------------- */
.status-grid {
    display: grid;
    grid-template-columns: minmax(0, 1fr);
    gap: 20px;
}

.status-panel {
    border-radius: 18px;
    padding: 18px 16px;
    background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.06), rgba(10, 10, 10, 0.96));
    border: 1px solid rgba(255, 255, 255, 0.08);
    box-shadow: 0 0 22px rgba(0, 0, 0, 0.9);
}

.status-title {
    font-size: 0.9rem;
    text-transform: uppercase;
    letter-spacing: 0.14em;
    margin-bottom: 10px;
}

.status-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 0.8rem;
    margin-bottom: 6px;
    color: #cccccc;
}

.status-label {
    opacity: 0.9;
}

.status-value {
    font-weight: 500;
}

.status-pill {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 4px 10px;
    border-radius: 999px;
    border: 1px solid rgba(255, 255, 255, 0.18);
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: #f0f0f0;
    background: radial-gradient(circle at top, rgba(255, 255, 255, 0.10), rgba(20, 20, 20, 0.9));
    margin-top: 10px;
}

.status-dot-green {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #cccccc;
    box-shadow: 0 0 12px #cccccc;
}

.status-dot-amber {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #aaaaaa;
    box-shadow: 0 0 12px #aaaaaa;
}

/* Mini timeline */
.timeline {
    margin-top: 10px;
    border-left: 1px solid rgba(255, 255, 255, 0.18);
    padding-left: 12px;
}

.timeline-item {
    margin-bottom: 8px;
}

.timeline-label {
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: #aaaaaa;
}

.timeline-desc {
    font-size: 0.75rem;
    color: #dddddd;
}

/* --------------------------------------------------
   ANCHOR DETAIL SECTIONS
   -------------------------------------------------- */
.anchor-section {
    max-width: 1200px;
    margin: 0 auto;
    padding: 40px 24px 40px;
    position: relative;
}

.anchor-card {
    border-radius: 20px;
    padding: 22px 20px;
    background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.08), rgba(10, 10, 10, 0.98));
    border: 1px solid rgba(255, 255, 255, 0.12);
    box-shadow: 0 0 26px rgba(0, 0, 0, 0.9);
    position: relative;
    overflow: hidden;
}

.anchor-card::before {
    content: "";
    position: absolute;
    inset: -40%;
    background: conic-gradient(from 0deg, rgba(255, 255, 255, 0.08), transparent, rgba(180, 180, 180, 0.12), transparent);
    opacity: 0.35;
    mix-blend-mode: screen;
}

.anchor-inner {
    position: relative;
    z-index: 2;
}

.anchor-title {
    font-size: 1.1rem;
    text-transform: uppercase;
    letter-spacing: 0.16em;
    margin-bottom: 10px;
}

.anchor-subtitle {
    font-size: 0.85rem;
    color: #dddddd;
    margin-bottom: 14px;
}

.anchor-meta {
    font-size: 0.8rem;
    color: #aaaaaa;
    margin-bottom: 10px;
}

.anchor-meta span {
    display: inline-block;
    margin-right: 10px;
}

.anchor-back {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    margin-top: 12px;
    padding: 8px 16px;
    border-radius: 999px;
    border: 1px solid rgba(255, 255, 255, 0.18);
    background: radial-gradient(circle at top, rgba(255, 255, 255, 0.10), rgba(20, 20, 20, 0.9));
    color: #f0f0f0;
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.14em;
    text-decoration: none;
    transition: transform 0.16s ease, box-shadow 0.16s ease, border-color 0.16s ease;
}

.anchor-back:hover {
    transform: translateY(-1px);
    border-color: rgba(255, 255, 255, 0.4);
    box-shadow: 0 0 22px rgba(200, 200, 200, 0.7);
}

/* --------------------------------------------------
   FOOTER
   -------------------------------------------------- */
.footer {
    max-width: 1200px;
    margin: 0 auto;
    padding: 30px 24px 40px;
    font-size: 0.75rem;
    color: #888888;
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    justify-content: space-between;
    border-top: 1px solid rgba(255, 255, 255, 0.06);
    margin-top: 40px;
}

.footer-left span {
    opacity: 0.9;
}

.footer-right {
    display: flex;
    gap: 14px;
}

.footer-pill {
    text-transform: uppercase;
    letter-spacing: 0.12em;
}

/* --------------------------------------------------
   CONTACT & CREDITS OVERLAY
   -------------------------------------------------- */
.overlay-backdrop {
    position: fixed;
    inset: 0;
    background: radial-gradient(circle at top, rgba(20, 20, 20, 0.96), rgba(5, 5, 5, 0.98));
    backdrop-filter: blur(18px);
    z-index: 50;
    display: none;
    align-items: center;
    justify-content: center;
    padding: 24px;
}

.overlay-backdrop.visible {
    display: flex;
}

.overlay-panel {
    max-width: 720px;
    width: 100%;
    border-radius: 22px;
    border: 1px solid rgba(255, 255, 255, 0.16);
    background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.10), rgba(10, 10, 10, 0.98));
    box-shadow: 0 0 40px rgba(180, 180, 180, 0.5);
    padding: 24px 22px 22px;
    position: relative;
    overflow: hidden;
}

.overlay-panel::before {
    content: "";
    position: absolute;
    inset: -40%;
    background: conic-gradient(from 180deg, rgba(255, 255, 255, 0.08), transparent, rgba(180, 180, 180, 0.12), transparent);
    opacity: 0.35;
    mix-blend-mode: screen;
}

.overlay-inner {
    position: relative;
    z-index: 2;
}

.overlay-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 16px;
    margin-bottom: 18px;
}

.overlay-title {
    font-size: 1.2rem;
    text-transform: uppercase;
    letter-spacing: 0.16em;
}

.overlay-close {
    border-radius: 999px;
    border: 1px solid rgba(255, 255, 255, 0.18);
    background: radial-gradient(circle at top, rgba(255, 255, 255, 0.10), rgba(20, 20, 20, 0.9));
    color: #f0f0f0;
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    padding: 6px 12px;
    cursor: pointer;
    transition: transform 0.16s ease, box-shadow 0.16s ease, border-color 0.16s ease;
}

.overlay-close:hover {
    transform: translateY(-1px);
    border-color: rgba(255, 255, 255, 0.4);
    box-shadow: 0 0 22px rgba(200, 200, 200, 0.7);
}

.overlay-columns {
    display: grid;
    grid-template-columns: minmax(0, 1.1fr) minmax(0, 1fr);
    gap: 18px;
}

.overlay-section-title {
    font-size: 0.85rem;
    text-transform: uppercase;
    letter-spacing: 0.14em;
    margin-bottom: 8px;
}

.overlay-text {
    font-size: 0.8rem;
    color: #dddddd;
    margin-bottom: 10px;
}

.overlay-list {
    list-style: none;
    font-size: 0.8rem;
    color: #cccccc;
}

.overlay-list li {
    margin-bottom: 6px;
}

.overlay-label {
    text-transform: uppercase;
    letter-spacing: 0.12em;
    font-size: 0.7rem;
    color: #aaaaaa;
}

.overlay-highlight {
    color: #f0f0f0;
    font-weight: 500;
}

.overlay-footer-note {
    margin-top: 16px;
    font-size: 0.7rem;
    color: #888888;
}

/* --------------------------------------------------
   RESPONSIVE
   -------------------------------------------------- */
@media (max-width: 900px) {
    .hero {
        grid-template-columns: minmax(0, 1fr);
        padding-top: 60px;
    }
    .hero-right {
        order: -1;
        margin-bottom: 20px;
    }
    .hero-orbit-wrapper {
        width: 320px;
        height: 320px;
    }
    .hero-core-planet {
        width: 150px;
        height: 150px;
    }
    .hero-saturn-ring {
        width: 220px;
        height: 220px;
    }
    .overlay-columns {
        grid-template-columns: minmax(0, 1fr);
    }
}

@media (max-width: 700px) {
    .nav-inner {
        padding-inline: 16px;
    }
    .nav-links {
        display: none;
    }
    .hero-title {
        font-size: 2.4rem;
    }
    .section-header {
        flex-direction: column;
        align-items: flex-start;
    }
    .status-grid {
        grid-template-columns: minmax(0, 1fr);
    }
}
</style>
</head>

<body>
<canvas id="starfield"></canvas>

<div class="cursor-core" id="cursorCore"></div>
<div class="cursor-ring" id="cursorRing"></div>
<div class="cursor-spotlight" id="cursorSpotlight"></div>

<div class="planet planet-1"></div>
<div class="planet planet-2"></div>
<div class="planet planet-3"></div>

<!-- Saturn planet decoration -->
<div class="saturn-bg">
    <div class="saturn-scene">
        <div class="saturn-ring-outer"></div>
        <div class="saturn-ring-mid"></div>
        <div class="saturn-ring-inner"></div>
        <div class="saturn-body"></div>
    </div>
</div>

<div class="app-shell">
    <!-- NAVBAR -->
    <nav class="navbar">
        <div class="nav-inner">
            <div class="nav-left">
                <div class="brand-orb"></div>
                <div class="brand-text">R/Akira Servers</div>
            </div>
            <div class="nav-links">
                <a href="#games" class="nav-link">Game Matrix</a>
                <a href="#status" class="nav-link">Status</a>
                <a href="#about" class="nav-link">About</a>
                <a href="https://discord.gg/ek9dpsAs" target="_blank" class="nav-link">Discord</a>
                <button class="nav-cta" type="button" id="openContactBtn">Contact</button>
            </div>
        </div>
    </nav>

    <!-- HERO -->
    <header class="hero" id="top">
        <div class="hero-left">
            <div class="hero-kicker">
                <span class="hero-kicker-dot"></span>
                <span>Cosmic Downloads • Real Cheat</span>
            </div>
            <h1 class="hero-title">R/Akira Servers</h1>
            <p class="hero-subtitle">
                A purple, space‑grade interface concept for spoofers and externals —
                rendered as a cinematic, non‑functional UI universe.
            </p>

            <div class="hero-tags">
                <div class="hero-tag">Valorant</div>
                <div class="hero-tag">CS2</div>
                <div class="hero-tag">Fortnite</div>
            </div>

            <div class="hero-actions">
                <a class="hero-btn-primary" href="https://discord.gg/ek9dpsAs" target="_blank">
                    Join Discord
                </a>
                <button class="hero-btn-ghost" type="button" id="openContactBtnHero">
                    <span class="hero-btn-ghost-icon">◎</span>
                    <span>Contact & Credits</span>
                </button>
            </div>

            <div class="hero-meta">
                <div class="hero-meta-item">
                    <span class="hero-meta-dot"></span>
                    <span>Real concept — links to software, loaders, cheats</span>
                </div>
                <div class="hero-meta-item">
                    <span class="hero-meta-dot"></span>
                    <span>Advanced cursor network, starfield, and parallax planets</span>
                </div>
            </div>
        </div>

        <div class="hero-right">
            <div class="hero-orbit-wrapper">
                <div class="orbit-ring"></div>

                <div class="hero-core-planet">
                    <div class="hero-core-inner-ring"></div>
                    <div class="hero-core-label"></div>
                </div>

                <div class="hero-saturn-ring"></div>

                <div class="hero-node hero-node-1">
                    <span>V</span>
                    <div class="hero-node-label">Valorant</div>
                </div>
                <div class="hero-node hero-node-2">
                    <span>C</span>
                    <div class="hero-node-label">CS2</div>
                </div>
                <div class="hero-node hero-node-3">
                    <span>F</span>
                    <div class="hero-node-label">Fortnite</div>
                </div>
                <div class="hero-node hero-node-4">
                    <span>R</span>
                    <div class="hero-node-label">Roblox</div>
                </div>
            </div>
        </div>
    </header>

    <!-- GAME MATRIX -->
    <section class="section" id="games">
        <div class="section-header">
            <div>
                <h2 class="section-title">Game Matrix</h2>
                <p class="section-subtitle">
                    A modular grid of visual options for each title — spoofers and externals only.
                    Everything here is purely UI, no functionality or downloads.
                </p>
            </div>
        </div>

        <div class="game-grid">
            <!-- Valorant -->
            <article class="game-card">
                <div class="game-card-inner">
                    <div class="game-card-header">
                        <div class="game-name">Valorant</div>
                        <div class="game-pill">Live Node</div>
                    </div>
                    <div class="game-meta-row">
                        <span class="game-meta-dot"></span>
                        <span>Concept layout for Valorant‑style spoofers & externals</span>
                    </div>
                    <div class="game-options">
                        <a class="game-option-link" href="https://gofile.io/d/0pLRJ9">
                            <div class="game-option">Spoofers</div>
                        </a>
                        <a class="game-option-link" href="https://gofile.io/d/0pLRJ9">
                            <div class="game-option">Externals</div>
                        </a>
                    </div>
                </div>
            </article>

            <!-- CS2 -->
            <article class="game-card">
                <div class="game-card-inner">
                    <div class="game-card-header">
                        <div class="game-name">CS2</div>
                        <div class="game-pill">Live Node</div>
                    </div>
                    <div class="game-meta-row">
                        <span class="game-meta-dot"></span>
                        <span>Showcase‑only matrix for CS2‑style utilities</span>
                    </div>
                    <div class="game-options">
                        <a class="game-option-link" href="https://gofile.io/d/cPLjW5">
                            <div class="game-option">Spoofers</div>
                        </a>
                        <a class="game-option-link" href="https://gofile.io/d/cPLjW5">
                            <div class="game-option">Externals</div>
                        </a>
                    </div>
                </div>
            </article>

            <!-- Fortnite -->
            <article class="game-card">
                <div class="game-card-inner">
                    <div class="game-card-header">
                        <div class="game-name">Fortnite</div>
                        <div class="game-pill">Live Node</div>
                    </div>
                    <div class="game-meta-row">
                        <span class="game-meta-dot"></span>
                        <span>Fortnite‑themed visual control surface</span>
                    </div>
                    <div class="game-options">
                        <a class="game-option-link" href="https://gofile.io/d/lvjHsx">
                            <div class="game-option">Spoofers</div>
                        </a>
                        <a class="game-option-link" href="https://gofile.io/d/xPEQxx">
                            <div class="game-option">Externals</div>
                        </a>
                    </div>
                </div>
            </article>

            <!-- Custom Slot -->
            <article class="game-card">
                <div class="game-card-inner">
                    <div class="game-card-header">
                        <div class="game-name">Roblox</div>
                        <div class="game-pill">Just Arrived</div>
                    </div>
                    <div class="game-meta-row">
                        <span class="game-meta-dot"></span>
                        <span>Executors and Externals (Dont worry roblox moderation is crap, no need for spoofer)
                        </span>
                    </div>
                    <div class="game-options">
                        <a class="game-option-link" href="https://akiracheats.netlify.app/#top">
                            <div class="game-option">Madium Executor </div>
                        </a>
                        <a class="game-option-link" href="https://eclipse-web-site.vercel.app">
                            <div class="game-option">Eclipse External</div>
                        </a>
                    </div>
                </div>
            </article>
        </div>
    </section>

    <!-- STATUS / TELEMETRY -->
    <section class="section" id="status">
        <div class="section-header">
            <div>
                <h2 class="section-title">Status Telemetry</h2>
                <p class="section-subtitle">
                    A fictional status board to make the UI feel alive — uptime, nodes, and events.
                    All values are static placeholders for visual storytelling.
                </p>
            </div>
        </div>

        <div class="status-grid">
            <div class="status-panel">
                <h3 class="status-title">Node Status</h3>
                <div class="status-row">
                    <span class="status-label">Valorant Node</span>
                    <span class="status-value">Stable</span>
                </div>
                <div class="status-row">
                    <span class="status-label">CS2 Node</span>
                    <span class="status-value">Stable</span>
                </div>
                <div class="status-row">
                    <span class="status-label">Fortnite Node</span>
                    <span class="status-value">Stable</span>
                </div>
                <div class="status-row">
                    <span class="status-label">Roblox Node</span>
                    <span class="status-value">Stable</span>
                </div>

                <div class="status-pill">
                    <span class="status-dot-amber"></span>
                    <span>All nodes are live</span>
                </div>

                <div class="timeline">
                    <div class="timeline-item">
                        <div class="timeline-label">20:11</div>
                        <div class="timeline-desc">R/Akira hacks enabled!</div>
                    </div>
                    <div class="timeline-item">
                        <div class="timeline-label">20:12</div>
                        <div class="timeline-desc">super sigma.</div>
                    </div>
                    <div class="timeline-item">
                        <div class="timeline-label">20:13</div>
                        <div class="timeline-desc">hehe i made this, its pretty cool.</div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- ABOUT -->
    <section class="section" id="about">
        <div class="section-header">
            <div>
                <h2 class="section-title">About This Concept</h2>
                <p class="section-subtitle">
                    R/Akira Servers here is a purely aesthetic, advanced front‑end concept:
                    It’s designed to feel like a premium control hub while staying completely safe and functional
                    any cheat you can think of.
                </p>
            </div>
        </div>
    </section>

    <!-- VALORANT DETAIL SECTIONS -->
    <section class="anchor-section" id="valorant-section-1">
        <div class="anchor-card">
            <div class="anchor-inner">
                <h3 class="anchor-title">Valorant Spoofers — Visual Segment</h3>
                <p class="anchor-subtitle">
                    This section represents a dedicated visual lane for Valorant‑style spoofers. It’s a cinematic
                    placeholder only — loaders, executables, downloads. Just the feeling of a high‑end control
                    surface.
                </p>
                <p class="anchor-meta">
                    <span>Game: Valorant</span>
                    <span>Channel: Spoofers</span>
                    <span>Download: https://gofile.io/d/0pLRJ9</span>
                </p>
                <p class="anchor-meta">
                    Imagine this as a dashboard where you’d configure identity layers, regions, and profiles —
                    but here it’s all aesthetic, designed to show how a premium panel could look.
                </p>
                <a class="anchor-back" href="#top">
                    <span>← Back to Home</span>
                    <span>(Valorant Spoofers)</span>
                </a>
            </div>
        </div>
    </section>

    <section class="anchor-section" id="valorant-section-2">
        <div class="anchor-card">
            <div class="anchor-inner">
                <h3 class="anchor-title">Valorant Externals — Visual Segment</h3>
                <p class="anchor-subtitle">
                    A conceptual space for Valorant‑style externals. This is a functional, purely visual block
                    that hints at overlays, HUD elements, and modular toggles — without providing any real tools.
                </p>
                <p class="anchor-meta">
                    <span>Game: Valorant</span>
                    <span>Channel: Externals</span>
                    <span>Download: https://gofile.io/d/0pLRJ9</span>
                </p>
                <p class="anchor-meta">
                    Use this area to imagine how different external modules might be grouped, themed, and animated
                    in a real product‑grade UI, while keeping everything here safe and fictional.
                </p>
                <a class="anchor-back" href="#top">
                    <span>← Back to Home</span>
                    <span>(Valorant Externals)</span>
                </a>
            </div>
        </div>
    </section>

    <!-- FORTNITE DETAIL SECTIONS -->
    <section class="anchor-section" id="fortnite-section-1">
        <div class="anchor-card">
            <div class="anchor-inner">
                <h3 class="anchor-title">Fortnite Spoofers — Visual Segment</h3>
                <p class="anchor-subtitle">
                    This panel is a Fortnite‑themed spoofers lane, styled to feel like a cosmic identity console.
                    It’s all front‑end imagination — real spoofing, backends, no risk.
                </p>
                <p class="anchor-meta">
                    <span>Game: Fortnite</span>
                    <span>Channel: Spoofers</span>
                    <span>Download: https://gofile.io/d/lvjHsx</span>
                </p>
                <p class="anchor-meta">
                    Think of it as a storyboard for how a future UI could present device profiles, session routes,
                    and environment presets — all wrapped in a purple, space‑driven aesthetic.
                </p>
                <a class="anchor-back" href="#top">
                    <span>← Back to Home</span>
                    <span>(Fortnite Spoofers)</span>
                </a>
            </div>
        </div>
    </section>

    <section class="anchor-section" id="fortnite-section-2">
        <div class="anchor-card">
            <div class="anchor-inner">
                <h3 class="anchor-title">Fortnite Externals — Visual Segment</h3>
                <p class="anchor-subtitle">
                    A cinematic placeholder for Fortnite‑style externals.Overlays, injections — just a
                    stylized container that shows how a polished UI might group and label external‑type features.
                </p>
                <p class="anchor-meta">
                    <span>Game: Fortnite</span>
                    <span>Channel: Externals</span>
                    <span>Download: https://gofile.io/d/xPEQxx</span>
                </p>
                <p class="anchor-meta">
                    Use this to explore layout ideas: tabs, toggles, categories, and micro‑animations that could
                    make a real interface feel premium and intentional.
                </p>
                <a class="anchor-back" href="#top">
                    <span>← Back to Home</span>
                    <span>(Fortnite Externals)</span>
                </a>
            </div>
        </div>
    </section>

    <!-- CS2 DETAIL SECTIONS -->
    <section class="anchor-section" id="cs2-section-1">
        <div class="anchor-card">
            <div class="anchor-inner">
                <h3 class="anchor-title">CS2 Spoofers — Visual Segment</h3>
                <p class="anchor-subtitle">
                    A CS2‑flavored spoofers lane, built as the best possible exploit. Meant to feel like a serious
                    control panel while staying completely connected to any spoofing logic.
                </p>
                <p class="anchor-meta">
                    <span>Game: CS2</span>
                    <span>Channel: Spoofers</span>
                    <span>Download: https://gofile.io/d/cPLjW5</span>
                </p>
                <p class="anchor-meta">
                    Picture environment slots, identity layers, and routing presets living here — all wrapped in
                    the same purple, galactic language as the rest of R/Akira.
                </p>
                <a class="anchor-back" href="#top">
                    <span>← Back to Home</span>
                    <span>(CS2 Spoofers)</span>
                </a>
            </div>
        </div>
    </section>

    <section class="anchor-section" id="cs2-section-2">
        <div class="anchor-card">
            <div class="anchor-inner">
                <h3 class="anchor-title">CS2 Externals — Visual Segment</h3>
                <p class="anchor-subtitle">
                    This block is a CS2‑style externals showcase. It’s a sandbox for UI ideas — how you might
                    present external‑type modules, status indicators, and categories in a real product.
                </p>
                <p class="anchor-meta">
                    <span>Game: CS2</span>
                    <span>Channel: Externals</span>
                    <span>Download: https://gofile.io/d/cPLjW5</span>
                </p>
                <p class="anchor-meta">
                    Everything here is static and safe, built to inspire layout, typography, and motion rather to
                    help you exploit.
                </p>
                <a class="anchor-back" href="#top">
                    <span>← Back to Home</span>
                    <span>(CS2 Externals)</span>
                </a>
            </div>
        </div>
    </section>

    <!-- ROBLOX DETAIL SECTIONS -->
    <section class="anchor-section" id="cs2-section-1">
        <div class="anchor-card">
            <div class="anchor-inner">
                <h3 class="anchor-title">Roblox Executor — Visual Segment</h3>
                <p class="anchor-subtitle">
                    A Roblox style executor lane, built as the best possible exploit. Meant to feel like a serious
                    control panel while staying completely connected to any spoofing logic.
                </p>
                <p class="anchor-meta">
                    <span>Game: Roblox</span>
                    <span>Channel: Executor</span>
                    <span>Download: https://akiracheats.netlify.app/</span>
                </p>
                <p class="anchor-meta">
                    Picture environment slots, identity layers, and routing presets living here — all wrapped in
                    the same purple, galactic language as the rest of R/Akira.
                </p>
                <a class="anchor-back" href="#top">
                    <span>← Back to Home</span>
                    <span>(Roblox Executor)</span>
                </a>
            </div>
        </div>
    </section>

    <section class="anchor-section" id="cs2-section-2">
        <div class="anchor-card">
            <div class="anchor-inner">
                <h3 class="anchor-title">Roblox Externals — Visual Segment</h3>
                <p class="anchor-subtitle">
                    This block is a Roblox‑style externals showcase. It’s a sandbox for UI ideas — how you might
                    present external‑type modules, status indicators, and categories in a real product.
                </p>
                <p class="anchor-meta">
                    <span>Game: Roblox</span>
                    <span>Channel: Externals</span>
                    <span>Download: https://eclipse-web-site.vercel.app</span>
                </p>
                <p class="anchor-meta">
                    Everything here is static and safe, built to inspire layout, typography, and motion rather to
                    help you exploit.
                </p>
                <a class="anchor-back" href="#top">
                    <span>← Back to Home</span>
                    <span>(Roblox Externals)</span>
                </a>
            </div>
        </div>
    </section>

    <!-- FOOTER -->
    <footer class="footer">
        <div class="footer-left">
            <span>© 2026 R/Akira Servers — Great Exploits.</span>
        </div>
        <div class="footer-right">
            <span class="footer-pill">Space Theme</span>
            <span class="footer-pill">Purple Network</span>
            <span class="footer-pill">Real</span>
        </div>
    </footer>
</div>

<!-- CONTACT & CREDITS OVERLAY -->
<div class="overlay-backdrop" id="contactOverlay">
    <div class="overlay-panel">
        <div class="overlay-inner">
            <div class="overlay-header">
                <h2 class="overlay-title">Contact & Credits</h2>
                <button class="overlay-close" type="button" id="closeContactBtn">Close</button>
            </div>
            <div class="overlay-columns">
                <div>
                    <h3 class="overlay-section-title">Contact</h3>
                    <p class="overlay-text">
                        This panel is a contact & credits hub for the R/Akira Servers concept.
                        Use it as a reference for who crafted the visuals and structure.
                    </p>
                    <ul class="overlay-list">
                        <li>
                            <span class="overlay-label">Website Designer:</span>
                            <span class="overlay-highlight">hecker</span>
                        </li>
                        <li>
                            <span class="overlay-label">Designer Discord:</span>
                            <span class="overlay-highlight">masonnn324_46665</span>
                        </li>
                        <li>
                            <span class="overlay-label">Discord Server:</span>
                            <span class="overlay-highlight">discord.gg/ek9dpsAs</span>
                        </li>
                    </ul>
                </div>
                <div>
                    <h3 class="overlay-section-title">Credits</h3>
                    <p class="overlay-text">
                        Great Exploits.
                    </p>
                    <ul class="overlay-list">
                        <li>
                            <span class="overlay-label">Concept Lead:</span>
                            <span class="overlay-highlight">R/Akira Servers</span>
                        </li>
                        <li>
                            <span class="overlay-label">Cheat Developer Alias:</span>
                            <span class="overlay-highlight">brg_akira_waylay</span>
                        </li>
                        <li>
                            <span class="overlay-label">UI Theme:</span>
                            <span class="overlay-highlight">Deep Purple • Space • Neon Network</span>
                        </li>
                    </ul>
                </div>
            </div>
            <p class="overlay-footer-note">
                This page is a cinematic UI concept only. Any references to developers or tools are for
                 storytelling and do not distribute or endorse cheating software.
            </p>
        </div>
    </div>
</div>

<script>
/* --------------------------------------------------
   CURSOR + SPOTLIGHT + RING
   -------------------------------------------------- */
const cursorCore = document.getElementById('cursorCore');
const cursorRing = document.getElementById('cursorRing');
const cursorSpotlight = document.getElementById('cursorSpotlight');

let mouseX = window.innerWidth / 2;
let mouseY = window.innerHeight / 2;
let ringX = mouseX;
let ringY = mouseY;

document.addEventListener('mousemove', (e) => {
    mouseX = e.clientX;
    mouseY = e.clientY;

    cursorCore.style.left = mouseX + 'px';
    cursorCore.style.top = mouseY + 'px';

    cursorSpotlight.style.left = mouseX + 'px';
    cursorSpotlight.style.top = mouseY + 'px';
});

function animateRing() {
    ringX += (mouseX - ringX) * 0.18;
    ringY += (mouseY - ringY) * 0.18;

    cursorRing.style.left = ringX + 'px';
    cursorRing.style.top = ringY + 'px';

    requestAnimationFrame(animateRing);
}
animateRing();

/* --------------------------------------------------
   STARFIELD + CURSOR NETWORK LINES
   -------------------------------------------------- */
const canvas = document.getElementById('starfield');
const ctx = canvas.getContext('2d');

let width = window.innerWidth;
let height = window.innerHeight;
canvas.width = width;
canvas.height = height;

window.addEventListener('resize', () => {
    width = window.innerWidth;
    height = window.innerHeight;
    canvas.width = width;
    canvas.height = height;
    initStars();
});

const STAR_COUNT = 160;
const stars = [];

function initStars() {
    stars.length = 0;
    for (let i = 0; i < STAR_COUNT; i++) {
        stars.push({
            x: Math.random() * width,
            y: Math.random() * height,
            z: Math.random() * 0.8 + 0.2,
            vx: (Math.random() - 0.5) * 0.05,
            vy: (Math.random() - 0.5) * 0.05
        });
    }
}
initStars();

function drawStar(s) {
    const size = s.z * 2.2;
    ctx.beginPath();
    ctx.arc(s.x, s.y, size, 0, Math.PI * 2);
    const alpha = 0.4 + s.z * 0.6;
    ctx.fillStyle = 'rgba(220, 220, 220,' + alpha + ')';
    ctx.fill();
}

function drawNetworkLine(x1, y1, x2, y2, strength) {
    const alpha = 0.1 + strength * 0.4;
    ctx.strokeStyle = 'rgba(200, 200, 200,' + alpha + ')';
    ctx.lineWidth = 0.7 + strength * 1.3;
    ctx.beginPath();
    ctx.moveTo(x1, y1);
    ctx.lineTo(x2, y2);
    ctx.stroke();
}

function animateStars() {
    ctx.clearRect(0, 0, width, height);

    for (const s of stars) {
        s.x += s.vx * (1 + s.z * 2);
        s.y += s.vy * (1 + s.z * 2);

        if (s.x < -50) s.x = width + 50;
        if (s.x > width + 50) s.x = -50;
        if (s.y < -50) s.y = height + 50;
        if (s.y > height + 50) s.y = -50;

        drawStar(s);
    }

    for (const s of stars) {
        const dx = s.x - mouseX;
        const dy = s.y - mouseY;
        const dist = Math.sqrt(dx * dx + dy * dy);
        const maxDist = 220;
        if (dist < maxDist) {
            const strength = 1 - dist / maxDist;
            drawNetworkLine(s.x, s.y, mouseX, mouseY, strength);
        }
    }

    requestAnimationFrame(animateStars);
}
animateStars();

/* --------------------------------------------------
   CONTACT OVERLAY LOGIC
   -------------------------------------------------- */
const contactOverlay = document.getElementById('contactOverlay');
const openContactBtn = document.getElementById('openContactBtn');
const openContactBtnHero = document.getElementById('openContactBtnHero');
const closeContactBtn = document.getElementById('closeContactBtn');

function openContact() {
    contactOverlay.classList.add('visible');
}

function closeContact() {
    contactOverlay.classList.remove('visible');
}

openContactBtn && openContactBtn.addEventListener('click', openContact);
openContactBtnHero && openContactBtnHero.addEventListener('click', openContact);
closeContactBtn && closeContactBtn.addEventListener('click', closeContact);

contactOverlay.addEventListener('click', (e) => {
    if (e.target === contactOverlay) {
        closeContact();
    }
});
</script>
</body>
</html>
