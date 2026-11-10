<html lang="ja">
<head>
<meta charset="utf-8">
<meta
  name="viewport"
  content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover"
>
<title>プラコロカウンター</title>

<style>
:root{
  --page-bg:#05070a;
  --panel:#0b1017;
  --line:#263447;
  --text:#eaf2ff;

  --green:#00d47b;
  --yellow:#ffd400;
  --red:#ff334f;
  --evolution:#22c55e;

  /*
    JSで実際のブラウザ表示高さを設定。
    JSが動く前は100dvhを使用。
  */
  --app-height:100dvh;
}

*{
  box-sizing:border-box;
  -webkit-tap-highlight-color:transparent;
}

html,
body{
  width:100%;
  height:100%;

  margin:0;
  padding:0;

  overflow:hidden;
  overscroll-behavior:none;
}

body{
  position:fixed;
  inset:0;

  width:100%;

  height:var(--app-height);

  background:var(--page-bg);
  color:var(--text);

  overflow:hidden;
  overscroll-behavior:none;

  font-family:
    system-ui,
    -apple-system,
    "Hiragino Kaku Gothic ProN",
    "Yu Gothic",
    sans-serif;

  touch-action:manipulation;
}

button,
select{
  font-family:inherit;
}

button{
  user-select:none;
  -webkit-user-select:none;
}

/* =========================
   全体

   100vh固定ではなく
   実表示高さに合わせる
========================= */

.app{
  width:100%;
  max-width:600px;

  height:var(--app-height);

  margin:0 auto;

  padding:
    max(5px,env(safe-area-inset-top))
    6px
    max(5px,env(safe-area-inset-bottom));

  display:flex;
  flex-direction:column;

  gap:5px;

  overflow:hidden;
}

/* =========================
   ヘッダー
========================= */

header{
  width:100%;
  flex:0 0 auto;

  padding:8px 10px;

  display:flex;
  align-items:center;
  justify-content:space-between;

  gap:8px;

  background:#090d13;

  border:1px solid var(--line);
  border-radius:12px;
}

.title-area{
  min-width:0;
}

.system-label{
  margin-bottom:2px;

  color:#64748b;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:8px;
  font-weight:900;

  letter-spacing:1.7px;
}

h1{
  margin:0;

  color:#ffffff;

  font-size:clamp(19px,5.5vw,29px);
  font-weight:950;

  line-height:1.05;

  white-space:nowrap;
}

/* =========================
   RESET
========================= */

.reset{
  flex-shrink:0;

  padding:8px 11px;

  background:#171f2b;
  color:#ffffff;

  border:1px solid #3a4656;
  border-radius:8px;

  font-size:12px;
  font-weight:900;

  cursor:pointer;

  touch-action:manipulation;
}

.reset:active{
  transform:scale(.96);

  background:#212c3a;
}

.reset:focus-visible,
.pokemon-select:focus-visible,
.dial-container:focus-visible,
.evolution-button:focus-visible{
  outline:3px solid var(--green);
  outline-offset:2px;
}

/* =========================
   メイン
========================= */

.card{
  width:100%;

  flex:1 1 auto;
  min-height:0;

  padding:7px;

  background:var(--panel);

  border:1px solid #29364a;
  border-radius:16px;

  display:flex;
  flex-direction:column;
  align-items:center;

  overflow:hidden;

  position:relative;
}

/* =========================
   ポケモン選択
========================= */

.select-frame{
  width:100%;

  flex:0 0 auto;

  position:relative;
}

.select-title{
  margin-bottom:2px;

  color:#7d8998;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:8px;
  font-weight:900;

  letter-spacing:1.3px;
}

.pokemon-select{
  width:100%;
  height:48px;

  padding:6px 40px 6px 10px;

  appearance:none;
  -webkit-appearance:none;

  background:#0c121b;
  color:#ffffff;

  border:1px solid #354255;
  border-radius:9px;

  font-size:clamp(18px,5vw,25px);
  font-weight:950;

  text-align:center;
  text-align-last:center;

  outline:none;

  cursor:pointer;

  touch-action:manipulation;
}

/* 開いた一覧 */

.pokemon-select option{
  background:#ffffff;
  color:#000000;

  font-weight:800;
}

.pokemon-select:focus{
  border-color:var(--green);
}

.select-frame::after{
  content:"▼";

  position:absolute;

  right:14px;
  bottom:15px;

  color:#ffffff;

  font-size:11px;

  pointer-events:none;
}

/* =========================
   HPモニター
========================= */

.digital-unit{
  width:100%;

  flex:1 1 auto;
  min-height:0;

  margin-top:5px;
  padding:4px;

  background:#070a0f;

  border:1px solid #29364a;
  border-radius:14px;

  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;

  overflow:hidden;

  position:relative;
}

/* =========================
   モニターヘッダー
========================= */

.digital-header{
  width:100%;
  flex:0 0 auto;

  padding:0 5px 1px;

  display:flex;
  align-items:center;
  justify-content:space-between;

  color:#69778a;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:8px;
  font-weight:900;

  letter-spacing:1.1px;
}

.status-area{
  display:flex;
  align-items:center;

  gap:4px;
}

.status-light{
  width:6px;
  height:6px;

  background:var(--green);

  border-radius:50%;
}

/* =========================
   HPダイヤル

   実表示高さの約53%以内に
   自動的に収める
========================= */

.dial-container{
  width:min(
    88vw,
    calc(var(--app-height) * .53),
    400px
  );

  max-width:100%;
  max-height:100%;

  aspect-ratio:1 / 1;

  flex:0 1 auto;

  margin:0 auto;

  position:relative;

  user-select:none;
  -webkit-user-select:none;

  touch-action:none;
  overscroll-behavior:none;

  cursor:grab;
}

.dial-container.dragging{
  cursor:grabbing;
}

.hp-dial{
  width:100%;
  height:100%;

  display:block;

  overflow:visible;

  user-select:none;
  -webkit-user-select:none;

  touch-action:none;
  overscroll-behavior:none;
}

/* =========================
   外装
========================= */

.outer-case{
  fill:#05080c;

  stroke:#263447;
  stroke-width:5;
}

.outer-case-detail{
  fill:none;

  stroke:#111924;
  stroke-width:10;
}

/* =========================
   HPゲージ
========================= */

.dial-track{
  fill:none;

  stroke:#1d2734;
  stroke-width:24;
}

.dial-progress{
  fill:none;

  stroke:var(--green);
  stroke-width:24;

  transform:rotate(-90deg);
  transform-origin:180px 180px;
}

/* =========================
   中央
========================= */

.dial-center{
  fill:#05080c;

  stroke:#28374a;
  stroke-width:4;
}

.dial-center-inner{
  fill:#090e14;

  stroke:#162333;
  stroke-width:2;
}

/* =========================
   HP表示
========================= */

.dial-hp-label{
  fill:#69778a;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:17px;
  font-weight:900;

  letter-spacing:6px;

  text-anchor:middle;
}

.dial-hp-value{
  fill:var(--green);

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:82px;
  font-weight:950;

  text-anchor:middle;

  font-variant-numeric:tabular-nums;
}

.dial-unit-label{
  fill:#526175;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:11px;
  font-weight:900;

  letter-spacing:3px;

  text-anchor:middle;
}

.dial-percent{
  fill:#8290a3;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:13px;
  font-weight:900;

  text-anchor:middle;
}

/* =========================
   目盛り
========================= */

.scale-line{
  stroke:#46566a;

  stroke-width:2;
  stroke-linecap:round;

  pointer-events:none;
}

.scale-line.major{
  stroke:#8d9bad;

  stroke-width:4;
}

.scale-label{
  fill:#aebccc;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:12px;
  font-weight:900;

  text-anchor:middle;

  pointer-events:none;
}

/* =========================
   操作ノブ
========================= */

.dial-handle{
  fill:#05080c;

  stroke:var(--green);
  stroke-width:6;

  pointer-events:none;
}

.handle-center{
  fill:var(--green);

  pointer-events:none;
}

.handle-line{
  stroke:#05080c;

  stroke-width:2;
  stroke-linecap:round;

  pointer-events:none;
}

/* =========================
   進化ボタン
========================= */

.evolution-button{
  position:absolute;

  right:9px;
  bottom:29px;

  z-index:10;

  width:60px;
  height:60px;

  padding:0;

  display:none;
  align-items:center;
  justify-content:center;

  background:#16a34a;
  color:#ffffff;

  border:3px solid #86efac;
  border-radius:50%;

  box-shadow:
    0 4px 0 #075d2b,
    0 6px 10px rgba(0,0,0,.4);

  font-size:15px;
  font-weight:950;

  cursor:pointer;

  touch-action:manipulation;
}

.evolution-button.show{
  display:flex;
}

.evolution-button:active{
  transform:
    translateY(3px)
    scale(.96);

  box-shadow:
    0 1px 0 #075d2b;
}

.evolution-button:disabled{
  display:none;
}

/* =========================
   進化演出
========================= */

.evolution-overlay{
  position:absolute;
  inset:0;

  z-index:20;

  display:none;
  align-items:center;
  justify-content:center;

  overflow:hidden;

  background:rgba(1,6,8,.68);

  pointer-events:none;
}

.evolution-overlay.active{
  display:flex;

  animation:
    evolutionBackground
    1.5s
    ease
    both;
}

.evolution-flash{
  position:absolute;

  width:40px;
  height:40px;

  border:6px solid #ffffff;
  border-radius:50%;

  opacity:0;
}

.evolution-overlay.active
.evolution-flash{
  animation:
    evolutionRing
    1.35s
    ease-out
    both;
}

.evolution-flash.second{
  animation-delay:.18s !important;
}

.evolution-flash.third{
  animation-delay:.36s !important;
}

.evolution-text{
  position:relative;
  z-index:4;

  padding:12px 22px;

  color:#ffffff;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:clamp(25px,7vw,42px);
  font-weight:950;

  letter-spacing:3px;

  text-align:center;

  opacity:0;

  text-shadow:
    0 2px 0 #087a39,
    0 0 12px #22c55e;
}

.evolution-overlay.active
.evolution-text{
  animation:
    evolutionText
    1.5s
    ease
    both;
}

.digital-unit.evolving{
  animation:
    monitorEvolution
    1.5s
    ease;
}

.digital-unit.evolving
.dial-progress{
  animation:
    gaugeEvolution
    1.5s
    ease;
}

.digital-unit.evolving
.dial-hp-value{
  animation:
    numberEvolution
    1.5s
    ease;
}

@keyframes evolutionBackground{

  0%{
    background:rgba(1,6,8,0);
  }

  25%{
    background:rgba(1,15,8,.78);
  }

  65%{
    background:rgba(4,35,17,.82);
  }

  100%{
    background:rgba(1,6,8,0);
  }

}

@keyframes evolutionRing{

  0%{
    width:30px;
    height:30px;

    opacity:0;
  }

  15%{
    opacity:1;
  }

  70%{
    opacity:.8;

    border-color:#22c55e;
  }

  100%{
    width:650px;
    height:650px;

    opacity:0;

    border-width:2px;
  }

}

@keyframes evolutionText{

  0%{
    opacity:0;

    transform:scale(.55);

    filter:blur(8px);
  }

  30%{
    opacity:1;

    transform:scale(1.15);

    filter:blur(0);
  }

  65%{
    opacity:1;

    transform:scale(1);
  }

  100%{
    opacity:0;

    transform:scale(1.06);
  }

}

@keyframes monitorEvolution{

  0%{
    border-color:#29364a;
  }

  25%{
    border-color:#ffffff;
  }

  50%{
    border-color:#22c55e;

    box-shadow:
      inset 0 0 30px rgba(34,197,94,.35),
      0 0 24px rgba(34,197,94,.7);
  }

  100%{
    border-color:#29364a;
  }

}

@keyframes gaugeEvolution{

  0%{
    opacity:1;
  }

  20%{
    opacity:.15;

    stroke:#ffffff;
  }

  40%{
    opacity:1;

    stroke:#22c55e;
  }

  60%{
    opacity:.25;
  }

  100%{
    opacity:1;
  }

}

@keyframes numberEvolution{

  0%{
    opacity:1;

    transform:scale(1);

    transform-origin:center;
  }

  25%{
    opacity:0;

    transform:scale(.55);
  }

  55%{
    opacity:1;

    transform:scale(1.2);
  }

  100%{
    opacity:1;

    transform:scale(1);
  }

}

/* =========================
   操作説明
========================= */

.dial-guide{
  flex:0 0 auto;

  margin-top:-7px;

  padding:2px 6px;

  color:#728095;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:9px;
  font-weight:900;

  text-align:center;

  white-space:nowrap;
}

/* =========================
   下部
========================= */

.limit-indicator{
  width:100%;
  flex:0 0 auto;

  padding:4px 7px 1px;

  display:flex;
  align-items:center;
  justify-content:space-between;

  border-top:1px solid #1f2b3b;

  color:#59697e;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:8px;
  font-weight:900;
}

.limit-indicator strong{
  color:#b7c4d4;
}

/* =========================
   高さが小さいスマホ

   ブラウザ上部UI表示時にも
   収まりやすくする
========================= */

@media(max-height:700px){

  .app{
    padding-top:
      max(3px,env(safe-area-inset-top));

    padding-bottom:
      max(3px,env(safe-area-inset-bottom));

    gap:3px;
  }

  header{
    padding:5px 8px;
  }

  .system-label{
    display:none;
  }

  h1{
    font-size:18px;
  }

  .reset{
    padding:6px 9px;

    font-size:11px;
  }

  .card{
    padding:5px;
  }

  .select-title{
    display:none;
  }

  .pokemon-select{
    height:42px;

    font-size:17px;
  }

  .select-frame::after{
    bottom:13px;
  }

  .digital-unit{
    margin-top:3px;

    padding:3px;
  }

  .digital-header{
    font-size:7px;
  }

  .dial-container{
    width:min(
      87vw,
      calc(var(--app-height) * .51),
      325px
    );
  }

  .dial-guide{
    margin-top:-9px;

    padding:1px 4px;

    font-size:8px;
  }

  .limit-indicator{
    padding:2px 6px 1px;

    font-size:7px;
  }

  .evolution-button{
    right:7px;
    bottom:20px;

    width:54px;
    height:54px;

    font-size:13px;
  }

}

@media(max-height:600px){

  header{
    padding:4px 7px;
  }

  h1{
    font-size:17px;
  }

  .pokemon-select{
    height:38px;

    font-size:16px;
  }

  .select-frame::after{
    bottom:12px;
  }

  .dial-container{
    width:min(
      85vw,
      calc(var(--app-height) * .48),
      280px
    );
  }

  .dial-guide{
    display:none;
  }

  .limit-indicator{
    padding-top:1px;
  }

  .evolution-button{
    bottom:15px;

    width:48px;
    height:48px;

    font-size:12px;
  }

}

@media(prefers-reduced-motion:reduce){

  *,
  *::before,
  *::after{
    animation-duration:.01ms !important;
    animation-iteration-count:1 !important;
  }

}
</style>
</head>

<body>

<div class="app">

<header>

  <div class="title-area">

    <div class="system-label">
      DIGITAL LIFE COUNTER
    </div>

    <h1>
      プラコロカウンター
    </h1>

  </div>

  <button
    id="resetBtn"
    class="reset"
    type="button"
  >
    RESET
  </button>

</header>

<main class="card">

  <div class="select-frame">

    <div class="select-title">
      MONSTER SELECT
    </div>

    <select
      id="pokemonSelect"
      class="pokemon-select"
      aria-label="ポケモンを選択"
    >

      <option value="">
        ポケモンを選択
      </option>

    </select>

  </div>

  <section
    id="digitalUnit"
    class="digital-unit"
  >

    <div class="digital-header">

      <span id="monitorName">
        LIFE MONITOR
      </span>

      <span class="status-area">

        <span class="status-light"></span>

        ACTIVE

      </span>

    </div>

    <div
      id="dialContainer"
      class="dial-container"
      role="slider"
      aria-label="HP"
      aria-valuemin="0"
      aria-valuemax="120"
      aria-valuenow="120"
      aria-valuetext="HP 120"
      tabindex="0"
    >

      <svg
        id="hpDial"
        class="hp-dial"
        viewBox="0 0 360 360"
        aria-hidden="true"
      >

        <circle
          class="outer-case"
          cx="180"
          cy="180"
          r="167"
        ></circle>

        <circle
          class="outer-case-detail"
          cx="180"
          cy="180"
          r="153"
        ></circle>

        <circle
          class="dial-track"
          cx="180"
          cy="180"
          r="137"
        ></circle>

        <circle
          id="dialProgress"
          class="dial-progress"
          cx="180"
          cy="180"
          r="137"
        ></circle>

        <g id="scaleLines"></g>

        <g id="scaleLabels"></g>

        <circle
          class="dial-center"
          cx="180"
          cy="180"
          r="96"
        ></circle>

        <circle
          class="dial-center-inner"
          cx="180"
          cy="180"
          r="87"
        ></circle>

        <text
          class="dial-hp-label"
          x="180"
          y="140"
        >
          HP
        </text>

        <text
          id="hpValue"
          class="dial-hp-value"
          x="180"
          y="218"
        >
          120
        </text>

        <text
          class
