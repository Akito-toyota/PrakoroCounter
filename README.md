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

  background:var(--page-bg);
  color:var(--text);

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

/* =========================
   全体
========================= */

.app{
  width:100%;
  max-width:600px;

  height:100vh;
  height:100dvh;

  margin:0 auto;

  padding:
    max(8px,env(safe-area-inset-top))
    8px
    max(8px,env(safe-area-inset-bottom));

  display:flex;
  flex-direction:column;

  gap:8px;

  overflow:hidden;
}

/* =========================
   ヘッダー
========================= */

header{
  width:100%;
  flex:0 0 auto;

  padding:10px 12px;

  display:flex;
  align-items:center;
  justify-content:space-between;

  gap:10px;

  background:#090d13;

  border:1px solid var(--line);
  border-radius:14px;
}

.title-area{
  min-width:0;
}

.system-label{
  margin-bottom:3px;

  color:#64748b;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:9px;
  font-weight:900;

  letter-spacing:2px;
}

h1{
  margin:0;

  color:#ffffff;

  font-size:clamp(20px,5.8vw,30px);
  font-weight:950;

  line-height:1.1;

  white-space:nowrap;
}

/* =========================
   RESET
========================= */

.reset{
  flex-shrink:0;

  padding:10px 13px;

  background:#171f2b;
  color:#ffffff;

  border:1px solid #3a4656;
  border-radius:9px;

  font-size:13px;
  font-weight:900;

  cursor:pointer;
  touch-action:manipulation;
}

.reset:active{
  transform:scale(.96);

  background:#212c3a;
}

/* =========================
   メイン
========================= */

.card{
  width:100%;

  flex:1 1 auto;
  min-height:0;

  padding:10px;

  background:var(--panel);

  border:1px solid #29364a;
  border-radius:18px;

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
  margin-bottom:4px;

  color:#7d8998;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:9px;
  font-weight:900;

  letter-spacing:1.5px;
}

.pokemon-select{
  width:100%;
  height:54px;

  padding:7px 42px 7px 12px;

  appearance:none;
  -webkit-appearance:none;

  background:#0c121b;
  color:#ffffff;

  border:1px solid #354255;
  border-radius:10px;

  font-size:clamp(19px,5vw,26px);
  font-weight:950;

  text-align:center;
  text-align-last:center;

  outline:none;

  cursor:pointer;
}

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

  right:15px;
  bottom:17px;

  color:#ffffff;

  font-size:12px;

  pointer-events:none;
}

/* =========================
   HPモニター
========================= */

.digital-unit{
  width:100%;

  flex:1 1 auto;
  min-height:0;

  margin-top:8px;
  padding:7px;

  background:#070a0f;

  border:1px solid #29364a;
  border-radius:16px;

  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;

  overflow:hidden;

  position:relative;
}

.digital-header{
  width:100%;

  padding:1px 5px 3px;

  display:flex;
  align-items:center;
  justify-content:space-between;

  color:#69778a;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:9px;
  font-weight:900;

  letter-spacing:1.3px;
}

.status-area{
  display:flex;

  align-items:center;

  gap:5px;
}

.status-light{
  width:7px;
  height:7px;

  background:var(--green);

  border-radius:50%;
}

/* =========================
   ダイヤル
========================= */

.dial-container{
  width:min(88vw,56dvh,410px);

  max-width:100%;

  aspect-ratio:1/1;

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
   ゲージ
========================= */

.dial-track{
  fill:none;

  stroke:#1d2734;

  stroke-width:24;
}

/*
  ラグ対策：
  stroke-dashoffsetのtransitionを削除。
  JSのHP変更と同時に即座に更新。
*/

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

  right:14px;
  bottom:42px;

  z-index:10;

  width:74px;
  height:74px;

  padding:0;

  display:none;
  align-items:center;
  justify-content:center;

  background:#16a34a;
  color:#ffffff;

  border:4px solid #86efac;

  border-radius:50%;

  box-shadow:
    0 5px 0 #075d2b,
    0 8px 14px rgba(0,0,0,.4);

  font-size:18px;
  font-weight:950;

  cursor:pointer;

  touch-action:manipulation;
}

.evolution-button.show{
  display:flex;
}

.evolution-button:active{
  transform:
    translateY(4px)
    scale(.96);
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
    1.5s ease both;
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
    1.35s ease-out both;
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

  color:#ffffff;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:clamp(25px,7vw,42px);
  font-weight:950;

  letter-spacing:3px;

  opacity:0;

  text-align:center;

  text-shadow:
    0 2px 0 #087a39,
    0 0 12px #22c55e;
}

.evolution-overlay.active
.evolution-text{
  animation:
    evolutionText
    1.5s ease both;
}

.digital-unit.evolving{
  animation:
    monitorEvolution
    1.5s ease;
}

.digital-unit.evolving
.dial-progress{
  animation:
    gaugeEvolution
    1.5s ease;
}

.digital-unit.evolving
.dial-hp-value{
  animation:
    numberEvolution
    1.5s ease;
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
   下部
========================= */

.dial-guide{
  margin-top:-6px;

  padding:4px 8px;

  color:#728095;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:10px;
  font-weight:900;

  text-align:center;
}

.limit-indicator{
  width:100%;

  padding:6px 8px 2px;

  display:flex;
  align-items:center;
  justify-content:space-between;

  border-top:1px solid #1f2b3b;

  color:#59697e;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:9px;
  font-weight:900;
}

.limit-indicator strong{
  color:#b7c4d4;
}

/* =========================
   スマホ調整
========================= */

@media(max-width:380px){

  .app{
    padding-left:6px;
    padding-right:6px;
  }

  header{
    padding:8px 9px;
  }

  h1{
    font-size:19px;
  }

  .reset{
    padding:8px 10px;

    font-size:12px;
  }

  .card{
    padding:7px;
  }

  .pokemon-select{
    height:50px;

    font-size:18px;
  }

  .digital-unit{
    margin-top:6px;

    padding:5px;
  }

  .dial-container{
    width:min(
      90vw,
      55dvh,
      350px
    );
  }

  .evolution-button{
    right:8px;
    bottom:34px;

    width:64px;
    height:64px;

    font-size:16px;
  }

}

@media(max-height:750px){

  .app{
    gap:5px;
  }

  .system-label{
    display:none;
  }

  header{
    padding:7px 9px;
  }

  h1{
    font-size:19px;
  }

  .card{
    padding:6px;
  }

  .pokemon-select{
    height:46px;
  }

  .digital-unit{
    margin-top:5px;

    padding:4px;
  }

  .dial-container{
    width:min(
      88vw,
      54dvh,
      330px
    );
  }

  .evolution-button{
    right:8px;
    bottom:28px;

    width:60px;
    height:60px;

    font-size:15px;
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
    >

      <svg
        id="hpDial"
        class="hp-dial"
        viewBox="0 0 360 360"
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
          class="dial-unit-label"
          x="180"
          y="243"
        >
          LIFE POINT
        </text>

        <text
          id="hpPercent"
          class="dial-percent"
          x="180"
          y="268"
        >
          100%
        </text>

        <circle
          id="dialHandle"
          class="dial-handle"
          cx="180"
          cy="43"
          r="16"
        ></circle>

        <circle
          id="handleCenter"
          class="handle-center"
          cx="180"
          cy="43"
          r="6"
        ></circle>

        <line
          id="handleLine"
          class="handle-line"
          x1="180"
          y1="39"
          x2="180"
          y2="47"
        ></line>

      </svg>

    </div>

    <button
      id="evolutionButton"
      class="evolution-button"
      type="button"
    >
      進化
    </button>

    <div class="dial-guide">
      タップで移動 ／ RINGをドラッグ
    </div>

    <div class="limit-indicator">

      <span>
        MIN <strong>000</strong>
      </span>

      <span>
        STEP <strong>010</strong>
      </span>

      <span>
        MAX <strong id="maxHpLabel">120</strong>
      </span>

    </div>

    <div
      id="evolutionOverlay"
      class="evolution-overlay"
    >

      <div class="evolution-flash"></div>

      <div class="evolution-flash second"></div>

      <div class="evolution-flash third"></div>

      <div
        id="evolutionText"
        class="evolution-text"
      >
        EVOLUTION
      </div>

    </div>

  </section>

</main>

</div>

<script>

/* =========================
   進化データ
========================= */

const evolutionData = {

  "ヒトカゲ":[
    {name:"ヒトカゲ",maxHp:120},
    {name:"リザード",maxHp:150},
    {name:"リザードン",maxHp:180}
  ],

  "フシギダネ":[
    {name:"フシギダネ",maxHp:120},
    {name:"フシギソウ",maxHp:150},
    {name:"フシギバナ",maxHp:180}
  ],

  "ゼニガメ":[
    {name:"ゼニガメ",maxHp:120},
    {name:"カメール",maxHp:150},
    {name:"カメックス",maxHp:180}
  ]

};


/* =========================
   ポケモン一覧
========================= */

const pokemonData = [

  "ピカチュウ",
  "ヒトカゲ",
  "フシギダネ",
  "ゼニガメ",
  "ミュウ",
  "イーブイ",
  "ルカリオ",
  "メタグロス",
  "ゲンガー",
  "イワーク",
  "ベトベター",
  "カイロス",
  "ファイヤー",
  "サンダー",
  "フリーザー",
  "ヤミラミ",
  "ゲッコウガ",
  "ボスゴドラ",
  "レックウザ",
  "カヌチャン",
  "ラッキー"

];


/* =========================
   基本状態
========================= */

const MIN_HP = 0;
const HP_STEP = 10;
const BASE_MAX_HP = 120;

let hp =
  BASE_MAX_HP;

let maxHp =
  BASE_MAX_HP;

let selectedBasePokemon =
  "";

let evolutionStage =
  0;

let isEvolving =
  false;


/* =========================
   操作状態
========================= */

let isDragging =
  false;

let previousPointerAngle =
  null;

let accumulatedAngle =
  0;

let pointerStartX =
  0;

let pointerStartY =
  0;

let pointerMoved =
  false;


/*
  これより動いたら
  タップではなくドラッグ
*/

const TAP_MOVE_THRESHOLD =
  8;


/* =========================
   円
========================= */

const CENTER_X = 180;
const CENTER_Y = 180;

const DIAL_RADIUS =
  137;

const HANDLE_RADIUS =
  DIAL_RADIUS;

const SCALE_INNER_RADIUS =
  153;

const SCALE_OUTER_RADIUS =
  161;

const LABEL_RADIUS =
  169;

const CIRCUMFERENCE =
  2 *
  Math.PI *
  DIAL_RADIUS;


/* =========================
   DOM
========================= */

const pokemonSelect =
  document.getElementById(
    "pokemonSelect"
  );

const hpValueEl =
  document.getElementById(
    "hpValue"
  );

const hpPercentEl =
  document.getElementById(
    "hpPercent"
  );

const dialContainer =
  document.getElementById(
    "dialContainer"
  );

const hpDial =
  document.getElementById(
    "hpDial"
  );

const dialProgress =
  document.getElementById(
    "dialProgress"
  );

const dialHandle =
  document.getElementById(
    "dialHandle"
  );

const handleCenter =
  document.getElementById(
    "handleCenter"
  );

const handleLine =
  document.getElementById(
    "handleLine"
  );

const scaleLines =
  document.getElementById(
    "scaleLines"
  );

const scaleLabels =
  document.getElementById(
    "scaleLabels"
  );

const resetBtn =
  document.getElementById(
    "resetBtn"
  );

const maxHpLabel =
  document.getElementById(
    "maxHpLabel"
  );

const evolutionButton =
  document.getElementById(
    "evolutionButton"
  );

const evolutionOverlay =
  document.getElementById(
    "evolutionOverlay"
  );

const evolutionText =
  document.getElementById(
    "evolutionText"
  );

const digitalUnit =
  document.getElementById(
    "digitalUnit"
  );

const monitorName =
  document.getElementById(
    "monitorName"
  );


/* =========================
   ポケモン一覧生成
========================= */

pokemonData.forEach(
  name => {

    const option =
      document.createElement(
        "option"
      );

    option.value =
      name;

    option.textContent =
      name;

    pokemonSelect.appendChild(
      option
    );

  }
);


/* =========================
   進化情報
========================= */

function getEvolutionLine(){

  return evolutionData[
    selectedBasePokemon
  ] || null;

}


function getCurrentPokemonName(){

  const line =
    getEvolutionLine();

  if(!line){

    return selectedBasePokemon;

  }

  return line[
    evolutionStage
  ].name;

}


function hasNextEvolution(){

  const line =
    getEvolutionLine();

  return Boolean(
    line &&
    evolutionStage <
      line.length - 1
  );

}


/* =========================
   選択欄表示
========================= */

function restoreEvolutionOptionNames(){

  Array.from(
    pokemonSelect.options
  ).forEach(
    option => {

      if(
        evolutionData[
          option.value
        ]
      ){

        option.textContent =
          option.value;

      }

    }
  );

}


function updateSelectedOptionText(){

  const option =
    pokemonSelect.options[
      pokemonSelect.selectedIndex
    ];

  if(
    option &&
    pokemonSelect.value
  ){

    option.textContent =
      getCurrentPokemonName();

  }

}


/* =========================
   進化ボタン
========================= */

function renderEvolutionButton(){

  const show =
    hasNextEvolution() &&
    !isEvolving;

  evolutionButton.classList.toggle(
    "show",
    show
  );

  evolutionButton.disabled =
    !show;

}


/* =========================
   座標
========================= */

function polarToCartesian(
  centerX,
  centerY,
  radius,
  angle
){

  const radians =
    angle *
    Math.PI /
    180;

  return {

    x:
      centerX +
      radius *
      Math.cos(
        radians
      ),

    y:
      centerY +
      radius *
      Math.sin(
        radians
      )

  };

}


/* =========================
   10HPあたり角度
========================= */

function getDegreesPerStep(){

  return 360 /
    (
      maxHp /
      HP_STEP
    );

}


/* =========================
   HP → 角度
========================= */

function hpToAngle(
  value
){

  return -90 +
    (
      value /
      maxHp
    ) *
    360;

}


/* =========================
   HP色
========================= */

function getHpColor(){

  if(hp <= 40){

    return "#ff334f";

  }

  if(hp <= 80){

    return "#ffd400";

  }

  return "#00d47b";

}


/* =========================
   目盛り
========================= */

function createDialScale(){

  scaleLines.innerHTML =
    "";

  scaleLabels.innerHTML =
    "";

  for(
    let value = 0;
    value < maxHp;
    value += HP_STEP
  ){

    const angle =
      -90 +
      (
        value /
        maxHp
      ) *
      360;

    const isMajor =
      value % 30 === 0;

    const inner =
      isMajor
        ? SCALE_INNER_RADIUS - 6
        : SCALE_INNER_RADIUS;

    const start =
      polarToCartesian(
        CENTER_X,
        CENTER_Y,
        inner,
        angle
      );

    const end =
      polarToCartesian(
        CENTER_X,
        CENTER_Y,
        SCALE_OUTER_RADIUS,
        angle
      );

    const line =
      document.createElementNS(
        "http://www.w3.org/2000/svg",
        "line"
      );

    line.setAttribute(
      "x1",
      start.x
    );

    line.setAttribute(
      "y1",
      start.y
    );

    line.setAttribute(
      "x2",
      end.x
    );

    line.setAttribute(
      "y2",
      end.y
    );

    line.setAttribute(
      "class",
      isMajor
        ? "scale-line major"
        : "scale-line"
    );

    scaleLines.appendChild(
      line
    );

    if(isMajor){

      const pos =
        polarToCartesian(
          CENTER_X,
          CENTER_Y,
          LABEL_RADIUS,
          angle
        );

      const text =
        document.createElementNS(
          "http://www.w3.org/2000/svg",
          "text"
        );

      text.setAttribute(
        "x",
        pos.x
      );

      text.setAttribute(
        "y",
        pos.y + 4
      );

      text.setAttribute(
        "class",
        "scale-label"
      );

      text.textContent =
        value === 0
          ? "0/" + maxHp
          : value;

      scaleLabels.appendChild(
        text
      );

    }

  }

}


/* =========================
   HP描画
========================= */

function renderHP(){

  hp =
    Math.max(
      MIN_HP,
      Math.min(
        maxHp,
        hp
      )
    );

  const ratio =
    hp /
    maxHp;

  const color =
    getHpColor();

  const dashOffset =
    CIRCUMFERENCE *
    (
      1 -
      ratio
    );

  const angle =
    hpToAngle(
      hp
    );

  const pos =
    polarToCartesian(
      CENTER_X,
      CENTER_Y,
      HANDLE_RADIUS,
      angle
    );


  hpValueEl.textContent =
    String(
      hp
    ).padStart(
      3,
      "0"
    );


  hpPercentEl.textContent =
    Math.round(
      ratio *
      100
    ) +
    "%";


  maxHpLabel.textContent =
    maxHp;


  dialProgress.style.strokeDasharray =
    CIRCUMFERENCE;


  /*
    transitionなし。
    即座にゲージ更新。
  */

  dialProgress.style.strokeDashoffset =
    dashOffset;


  dialProgress.style.stroke =
    color;


  hpValueEl.style.fill =
    color;


  dialHandle.style.stroke =
    color;


  handleCenter.style.fill =
    color;


  dialHandle.setAttribute(
    "cx",
    pos.x
  );


  dialHandle.setAttribute(
    "cy",
    pos.y
  );


  handleCenter.setAttribute(
    "cx",
    pos.x
  );


  handleCenter.setAttribute(
    "cy",
    pos.y
  );


  const inner =
    polarToCartesian(
      CENTER_X,
      CENTER_Y,
      HANDLE_RADIUS - 4,
      angle
    );


  const outer =
    polarToCartesian(
      CENTER_X,
      CENTER_Y,
      HANDLE_RADIUS + 4,
      angle
    );


  handleLine.setAttribute(
    "x1",
    inner.x
  );


  handleLine.setAttribute(
    "y1",
    inner.y
  );


  handleLine.setAttribute(
    "x2",
    outer.x
  );


  handleLine.setAttribute(
    "y2",
    outer.y
  );

}


/* =========================
   全体描画
========================= */

function renderAll(){

  createDialScale();

  renderHP();

  updateSelectedOptionText();

  renderEvolutionButton();

  monitorName.textContent =
    selectedBasePokemon
      ? getCurrentPokemonName()
      : "LIFE MONITOR";

}


/* =========================
   pointer座標
========================= */

function getPointerPosition(
  event
){

  const rect =
    hpDial.getBoundingClientRect();

  return {

    x:
      (
        event.clientX -
        rect.left
      ) *
      (
        360 /
        rect.width
      ),

    y:
      (
        event.clientY -
        rect.top
      ) *
      (
        360 /
        rect.height
      )

  };

}


function getPointerAngle(
  event
){

  const p =
    getPointerPosition(
      event
    );

  return Math.atan2(
    p.y -
      CENTER_Y,

    p.x -
      CENTER_X
  ) *
  180 /
  Math.PI;

}


/* =========================
   タップ → HPジャンプ
========================= */

function jumpToTappedHp(
  event
){

  if(isEvolving){

    return;

  }

  const p =
    getPointerPosition(
      event
    );


  const dx =
    p.x -
    CENTER_X;


  const dy =
    p.y -
    CENTER_Y;


  const distance =
    Math.sqrt(
      dx * dx +
      dy * dy
    );


  /*
    リング付近のみ
    タップジャンプを許可
  */

  if(
    distance <
      DIAL_RADIUS - 45 ||
    distance >
      DIAL_RADIUS + 45
  ){

    return;

  }


  let angle =
    Math.atan2(
      dy,
      dx
    ) *
    180 /
    Math.PI;


  /*
    上を0度にする。
    時計回りで0～360。
  */

  let clockwiseAngle =
    angle +
    90;


  if(
    clockwiseAngle <
    0
  ){

    clockwiseAngle +=
      360;

  }


  if(
    clockwiseAngle >=
    360
  ){

    clockwiseAngle -=
      360;

  }


  const degreesPerStep =
    getDegreesPerStep();


  let step =
    Math.round(
      clockwiseAngle /
      degreesPerStep
    );


  const totalSteps =
    maxHp /
    HP_STEP;


  if(
    step >=
    totalSteps
  ){

    step = 0;

  }


  let nextHp =
    step *
    HP_STEP;


  /*
    0とMAXは同じ場所。

    現在値が半分以上ならMAX、
    半分未満なら0。
  */

  if(step === 0){

    nextHp =
      hp >=
      maxHp / 2
        ? maxHp
        : 0;

  }


  hp =
    nextHp;


  accumulatedAngle =
    0;


  previousPointerAngle =
    null;


  renderHP();

}


/* =========================
   ドラッグ
========================= */

function updateHpFromDrag(
  event
){

  if(isEvolving){

    return;

  }


  const currentAngle =
    getPointerAngle(
      event
    );


  if(
    previousPointerAngle ===
    null
  ){

    previousPointerAngle =
      currentAngle;

    return;

  }


  let difference =
    currentAngle -
    previousPointerAngle;


  if(
    difference >
    180
  ){

    difference -=
      360;

  }


  if(
    difference <
    -180
  ){

    difference +=
      360;

  }


  accumulatedAngle +=
    difference;


  const stepAngle =
    getDegreesPerStep();


  /*
    時計回り +10
  */

  while(
    accumulatedAngle >=
    stepAngle
  ){

    if(
      hp <
      maxHp
    ){

      hp =
        Math.min(
          maxHp,
          hp +
          HP_STEP
        );


      /*
        即描画
      */

      renderHP();

    }


    accumulatedAngle -=
      stepAngle;

  }


  /*
    反時計回り -10
  */

  while(
    accumulatedAngle <=
    -stepAngle
  ){

    if(
      hp >
      MIN_HP
    ){

      hp =
        Math.max(
          MIN_HP,
          hp -
          HP_STEP
        );


      renderHP();

    }


    accumulatedAngle +=
      stepAngle;

  }


  if(
    hp ===
      maxHp &&
    accumulatedAngle >
      0
  ){

    accumulatedAngle =
      0;

  }


  if(
    hp ===
      MIN_HP &&
    accumulatedAngle <
      0
  ){

    accumulatedAngle =
      0;

  }


  /*
    必ず現在角度を毎回更新。
    これで指への追従遅延を減らす。
  */

  previousPointerAngle =
    currentAngle;

}


/* =========================
   pointerdown
========================= */

dialContainer.addEventListener(
  "pointerdown",
  event => {

    if(isEvolving){

      return;

    }


    event.preventDefault();


    isDragging =
      true;


    pointerMoved =
      false;


    pointerStartX =
      event.clientX;


    pointerStartY =
      event.clientY;


    previousPointerAngle =
      getPointerAngle(
        event
      );


    accumulatedAngle =
      0;


    dialContainer.classList.add(
      "dragging"
    );


    try{

      dialContainer.setPointerCapture(
        event.pointerId
      );

    }catch(error){
    }

  }
);


/* =========================
   pointermove
========================= */

dialContainer.addEventListener(
  "pointermove",
  event => {

    if(
      !isDragging ||
      isEvolving
    ){

      return;

    }


    event.preventDefault();


    const dx =
      event.clientX -
      pointerStartX;


    const dy =
      event.clientY -
      pointerStartY;


    const distance =
      Math.sqrt(
        dx * dx +
        dy * dy
      );


    if(
      distance >=
      TAP_MOVE_THRESHOLD
    ){

      pointerMoved =
        true;

    }


    if(pointerMoved){

      updateHpFromDrag(
        event
      );

    }

  },
  {
    passive:false
  }
);


/* =========================
   操作終了
========================= */

function finishPointer(
  event,
  allowTap
){

  /*
    動いていなければ
    タップ操作として処理
  */

  if(
    isDragging &&
    allowTap &&
    !pointerMoved &&
    !isEvolving
  ){

    jumpToTappedHp(
      event
    );

  }


  isDragging =
    false;


  pointerMoved =
    false;


  previousPointerAngle =
    null;


  accumulatedAngle =
    0;


  dialContainer.classList.remove(
    "dragging"
  );


  try{

    if(
      dialContainer.hasPointerCapture(
        event.pointerId
      )
    ){

      dialContainer.releasePointerCapture(
        event.pointerId
      );

    }

  }catch(error){
  }

}


dialContainer.addEventListener(
  "pointerup",
  event => {

    finishPointer(
      event,
      true
    );

  }
);


dialContainer.addEventListener(
  "pointercancel",
  event => {

    finishPointer(
      event,
      false
    );

  }
);


/* =========================
   スクロール防止
========================= */

dialContainer.addEventListener(
  "touchmove",
  event => {

    event.preventDefault();

  },
  {
    passive:false
  }
);


/* =========================
   ポケモン変更
========================= */

pokemonSelect.addEventListener(
  "change",
  () => {

    if(isEvolving){

      return;

    }


    Array.from(
      pokemonSelect.options
    ).forEach(
      option => {

        if(
          evolutionData[
            option.value
          ]
        ){

          option.textContent =
            option.value;

        }

      }
    );


    selectedBasePokemon =
      pokemonSelect.value;


    evolutionStage =
      0;


    maxHp =
      BASE_MAX_HP;


    hp =
      BASE_MAX_HP;


    renderAll();

  }
);


/* =========================
   進化
========================= */

evolutionButton.addEventListener(
  "click",
  () => {

    if(
      isEvolving ||
      !hasNextEvolution()
    ){

      return;

    }


    const line =
      getEvolutionLine();


    const nextStage =
      evolutionStage +
      1;


    const next =
      line[
        nextStage
      ];


    /*
      ダメージ量を保持
    */

    const damage =
      maxHp -
      hp;


    isEvolving =
      true;


    evolutionButton.disabled =
      true;


    evolutionButton.classList.remove(
      "show"
    );


    evolutionText.textContent =
      "EVOLUTION";


    evolutionOverlay.classList.add(
      "active"
    );


    digitalUnit.classList.add(
      "evolving"
    );


    setTimeout(
      () => {

        evolutionStage =
          nextStage;


        maxHp =
          next.maxHp;


        hp =
          Math.max(
            MIN_HP,
            maxHp -
            damage
          );


        updateSelectedOptionText();


        monitorName.textContent =
          next.name;


        createDialScale();


        renderHP();


        evolutionText.textContent =
          next.name;

      },
      650
    );


    setTimeout(
      () => {

        evolutionOverlay.classList.remove(
          "active"
        );


        digitalUnit.classList.remove(
          "evolving"
        );


        isEvolving =
          false;


        renderEvolutionButton();

      },
      1550
    );

  }
);


/* =========================
   RESET
========================= */

resetBtn.addEventListener(
  "click",
  () => {

    if(isEvolving){

      return;

    }


    const selected =
      pokemonSelect.value;


    const option =
      pokemonSelect.options[
        pokemonSelect.selectedIndex
      ];


    if(
      option &&
      selected
    ){

      option.textContent =
        selected;

    }


    selectedBasePokemon =
      selected;


    evolutionStage =
      0;


    maxHp =
      BASE_MAX_HP;


    hp =
      BASE_MAX_HP;


    previousPointerAngle =
      null;


    accumulatedAngle =
      0;


    renderAll();

  }
);


/* =========================
   初期表示
========================= */

renderAll();

</script>

</body>
</html>
