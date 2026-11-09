<!doctype html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta
  name="viewport"
  content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no"
>
<title>プラコロカウンター</title>

<style>
:root{
  --page-bg:#05070a;
  --panel:#0b1017;
  --line:#263447;
  --text:#eaf2ff;
  --muted:#718096;

  --green:#00d47b;
  --yellow:#ffd400;
  --red:#ff334f;
}

*{
  box-sizing:border-box;
  -webkit-tap-highlight-color:transparent;
}

html,
body{
  width:100%;
  min-height:100%;
  margin:0;
}

body{
  background:#05070a;
  color:var(--text);
  overscroll-behavior:none;

  font-family:
    system-ui,
    -apple-system,
    "Hiragino Kaku Gothic ProN",
    "Yu Gothic",
    sans-serif;
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

  min-height:100vh;
  min-height:100dvh;

  margin:0 auto;

  padding:
    max(12px,env(safe-area-inset-top))
    12px
    max(12px,env(safe-area-inset-bottom));

  display:flex;
  flex-direction:column;
  gap:12px;
}

/* =========================
   ヘッダー
========================= */

header{
  width:100%;

  padding:12px 14px;

  display:flex;
  align-items:center;
  justify-content:space-between;
  gap:10px;

  background:#090d13;

  border:1px solid #263447;
  border-radius:16px;
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

  font-size:10px;
  font-weight:900;
  letter-spacing:2px;
}

h1{
  margin:0;

  color:#ffffff;

  font-size:clamp(21px,6vw,31px);
  font-weight:950;

  line-height:1.1;
  white-space:nowrap;
}

/* =========================
   RESET
========================= */

.reset{
  flex-shrink:0;

  padding:11px 14px;

  background:#171f2b;
  color:#ffffff;

  border:1px solid #3a4656;
  border-radius:10px;

  font-size:14px;
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
.dial-container:focus-visible{
  outline:3px solid #00d47b;
  outline-offset:3px;
}

/* =========================
   メイン
========================= */

.card{
  width:100%;
  flex:1;

  padding:14px;

  background:#0b1017;

  border:1px solid #29364a;
  border-radius:20px;

  display:flex;
  flex-direction:column;
  align-items:center;
}

/* =========================
   ポケモン選択
========================= */

.select-frame{
  width:100%;
  position:relative;
}

.select-title{
  margin-bottom:5px;

  color:#7d8998;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:10px;
  font-weight:900;
  letter-spacing:1.5px;
}

/* 通常時：黒背景＋白文字 */

.pokemon-select{
  width:100%;
  height:60px;

  padding:8px 42px 8px 12px;

  appearance:none;
  -webkit-appearance:none;

  background:#0c121b;
  color:#ffffff;

  border:1px solid #354255;
  border-radius:10px;

  font-size:clamp(20px,5vw,27px);
  font-weight:950;

  text-align:center;
  text-align-last:center;

  outline:none;
  cursor:pointer;
}

/* 開いた選択肢：白背景＋黒文字 */

.pokemon-select option{
  background:#ffffff;
  color:#000000;
  font-weight:800;
}

.pokemon-select:focus{
  border-color:#00d47b;
}

/* ▼ */

.select-frame::after{
  content:"▼";

  position:absolute;

  right:15px;
  bottom:19px;

  color:#ffffff;

  font-size:13px;

  pointer-events:none;
}

/* =========================
   HPモニター
========================= */

.digital-unit{
  width:100%;
  flex:1;

  margin-top:14px;
  padding:10px;

  background:#070a0f;

  border:1px solid #29364a;
  border-radius:18px;

  display:flex;
  flex-direction:column;
  justify-content:center;
}

/* =========================
   モニターヘッダー
========================= */

.digital-header{
  padding:2px 5px 7px;

  display:flex;
  align-items:center;
  justify-content:space-between;

  color:#69778a;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:10px;
  font-weight:900;
  letter-spacing:1.5px;
}

.status-area{
  display:flex;
  align-items:center;
  gap:6px;
}

.status-light{
  width:8px;
  height:8px;

  background:#00d47b;

  border-radius:50%;
}

/* =========================
   円形ダイヤル
========================= */

.dial-container{
  width:min(88vw,420px);
  aspect-ratio:1/1;

  margin:0 auto;

  position:relative;

  user-select:none;
  touch-action:none;

  cursor:grab;
}

.dial-container.dragging{
  cursor:grabbing;
}

.hp-dial{
  width:100%;
  height:100%;

  display:block;
  touch-action:none;
}

/* =========================
   円外装
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
  ゲージは単色表示。
  発光やぼかしなし。
  色はJavaScriptでHPに合わせて変更。
*/

.dial-progress{
  fill:none;

  stroke:#00d47b;
  stroke-width:24;

  transform:rotate(-90deg);
  transform-origin:180px 180px;

  transition:
    stroke .15s ease;
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
   HP文字
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
  fill:#00d47b;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:82px;
  font-weight:950;

  text-anchor:middle;

  font-variant-numeric:tabular-nums;

  transition:
    fill .15s ease;
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
}

/* =========================
   操作ノブ
========================= */

.dial-handle{
  fill:#05080c;

  stroke:#00d47b;
  stroke-width:6;

  transition:
    stroke .15s ease;
}

.handle-center{
  fill:#00d47b;

  pointer-events:none;

  transition:
    fill .15s ease;
}

.handle-line{
  stroke:#05080c;

  stroke-width:2;
  stroke-linecap:round;

  pointer-events:none;
}

/* =========================
   操作説明
========================= */

.dial-guide{
  margin-top:-6px;

  padding:7px 10px;

  color:#728095;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:11px;
  font-weight:900;

  letter-spacing:1px;

  text-align:center;
}

/* =========================
   MIN STEP MAX
========================= */

.limit-indicator{
  width:100%;

  padding:9px 10px 4px;

  display:flex;
  align-items:center;
  justify-content:space-between;

  border-top:1px solid #1f2b3b;

  color:#59697e;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:10px;
  font-weight:900;
  letter-spacing:1px;
}

.limit-indicator strong{
  color:#b7c4d4;
}

/* =========================
   スマホ
========================= */

@media(max-width:380px){

  .app{
    padding-left:8px;
    padding-right:8px;
  }

  header{
    padding:10px;
  }

  h1{
    font-size:20px;
  }

  .reset{
    padding:9px 11px;
    font-size:13px;
  }

  .card{
    padding:10px 8px;
  }

  .pokemon-select{
    height:56px;
    font-size:19px;
  }

  .digital-unit{
    margin-top:8px;
    padding:7px;
  }

  .dial-container{
    width:min(92vw,360px);
  }

  .dial-hp-value{
    font-size:74px;
  }
}

/* =========================
   高さが低いスマホ
========================= */

@media(max-height:700px){

  .app{
    gap:7px;

    padding-top:7px;
    padding-bottom:7px;
  }

  .system-label{
    display:none;
  }

  header{
    padding:8px 10px;
  }

  h1{
    font-size:20px;
  }

  .card{
    padding:7px;
  }

  .pokemon-select{
    height:52px;
  }

  .digital-unit{
    margin-top:6px;
    padding:6px;
  }

  .dial-container{
    width:min(68vh,350px);
  }

  .dial-guide{
    margin-top:-10px;
    padding:4px;

    font-size:9px;
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

    <section class="digital-unit">

      <div class="digital-header">

        <span>
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

      <div class="dial-guide">
        RINGをドラッグしてHPを変更
      </div>

      <div class="limit-indicator">

        <span>
          MIN <strong>000</strong>
        </span>

        <span>
          STEP <strong>010</strong>
        </span>

        <span>
          MAX <strong>120</strong>
        </span>

      </div>

    </section>

  </main>

</div>

<script>

/* =========================
   ポケモン
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
   HP設定
========================= */

const MAX_HP = 120;
const MIN_HP = 0;
const HP_STEP = 10;

let hp = MAX_HP;

let isDragging = false;
let previousPointerAngle = null;
let accumulatedAngle = 0;

/*
  120HP ÷ 10 = 12段階
  360度 ÷ 12 = 30度
*/

const DEGREES_PER_STEP = 30;

/* =========================
   円設定
========================= */

const CENTER_X = 180;
const CENTER_Y = 180;

const DIAL_RADIUS = 137;
const HANDLE_RADIUS = 137;

const SCALE_INNER_RADIUS = 153;
const SCALE_OUTER_RADIUS = 161;
const LABEL_RADIUS = 169;

const CIRCUMFERENCE =
  2 * Math.PI * DIAL_RADIUS;

/* =========================
   HTML要素
========================= */

const pokemonSelect =
  document.getElementById("pokemonSelect");

const hpValueEl =
  document.getElementById("hpValue");

const hpPercentEl =
  document.getElementById("hpPercent");

const dialContainer =
  document.getElementById("dialContainer");

const hpDial =
  document.getElementById("hpDial");

const dialProgress =
  document.getElementById("dialProgress");

const dialHandle =
  document.getElementById("dialHandle");

const handleCenter =
  document.getElementById("handleCenter");

const handleLine =
  document.getElementById("handleLine");

const scaleLines =
  document.getElementById("scaleLines");

const scaleLabels =
  document.getElementById("scaleLabels");

const resetBtn =
  document.getElementById("resetBtn");

/* =========================
   ポケモン一覧作成
========================= */

pokemonData.forEach(name => {

  const option =
    document.createElement("option");

  option.value = name;
  option.textContent = name;

  pokemonSelect.appendChild(option);

});

/* =========================
   円座標
========================= */

function polarToCartesian(
  centerX,
  centerY,
  radius,
  angle
){

  const radians =
    angle * Math.PI / 180;

  return {

    x:
      centerX +
      radius * Math.cos(radians),

    y:
      centerY +
      radius * Math.sin(radians)

  };

}

/* =========================
   HP → 角度
========================= */

function hpToAngle(value){

  return -90 +
    (value / MAX_HP) * 360;

}

/* =========================
   HPによる色変更
========================= */

function getHpColor(){

  /*
    0～40
    赤
  */

  if(hp <= 40){

    return "#ff334f";

  }

  /*
    50～80
    黄色
  */

  if(hp <= 80){

    return "#ffd400";

  }

  /*
    90～120
    緑
  */

  return "#00d47b";

}

/* =========================
   目盛り
========================= */

function createDialScale(){

  scaleLines.innerHTML = "";
  scaleLabels.innerHTML = "";

  for(
    let value = 0;
    value < MAX_HP;
    value += HP_STEP
  ){

    const angle =
      -90 +
      (value / MAX_HP) * 360;

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

    scaleLines.appendChild(line);

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
          ? "0/120"
          : value;

      scaleLabels.appendChild(text);

    }

  }

}

/* =========================
   HP表示
========================= */

function renderHP(){

  const ratio =
    hp / MAX_HP;

  const hpColor =
    getHpColor();

  const progressLength =
    CIRCUMFERENCE * ratio;

  const dashOffset =
    CIRCUMFERENCE -
    progressLength;

  const handleAngle =
    hpToAngle(hp);

  const handlePos =
    polarToCartesian(
      CENTER_X,
      CENTER_Y,
      HANDLE_RADIUS,
      handleAngle
    );

  /*
    デジタル表示
    0 → 000
    40 → 040
    120 → 120
  */

  hpValueEl.textContent =
    String(hp).padStart(3,"0");

  hpPercentEl.textContent =
    Math.round(
      ratio * 100
    ) + "%";

  dialContainer.setAttribute(
    "aria-valuenow",
    hp
  );

  dialContainer.setAttribute(
    "aria-valuetext",
    "HP " + hp
  );

  /*
    ゲージ量
  */

  dialProgress.style.strokeDasharray =
    CIRCUMFERENCE;

  dialProgress.style.strokeDashoffset =
    dashOffset;

  /*
    HPによる色
  */

  dialProgress.style.stroke =
    hpColor;

  hpValueEl.style.fill =
    hpColor;

  dialHandle.style.stroke =
    hpColor;

  handleCenter.style.fill =
    hpColor;

  /*
    ノブ位置
  */

  dialHandle.setAttribute(
    "cx",
    handlePos.x
  );

  dialHandle.setAttribute(
    "cy",
    handlePos.y
  );

  handleCenter.setAttribute(
    "cx",
    handlePos.x
  );

  handleCenter.setAttribute(
    "cy",
    handlePos.y
  );

  /*
    ノブ内線
  */

  const lineInner =
    polarToCartesian(
      CENTER_X,
      CENTER_Y,
      HANDLE_RADIUS - 4,
      handleAngle
    );

  const lineOuter =
    polarToCartesian(
      CENTER_X,
      CENTER_Y,
      HANDLE_RADIUS + 4,
      handleAngle
    );

  handleLine.setAttribute(
    "x1",
    lineInner.x
  );

  handleLine.setAttribute(
    "y1",
    lineInner.y
  );

  handleLine.setAttribute(
    "x2",
    lineOuter.x
  );

  handleLine.setAttribute(
    "y2",
    lineOuter.y
  );

}

/* =========================
   ポインター角度取得
========================= */

function getPointerAngle(event){

  const rect =
    hpDial.getBoundingClientRect();

  const scaleX =
    360 / rect.width;

  const scaleY =
    360 / rect.height;

  const x =
    (event.clientX - rect.left)
    * scaleX;

  const y =
    (event.clientY - rect.top)
    * scaleY;

  return Math.atan2(
    y - CENTER_Y,
    x - CENTER_X
  ) * 180 / Math.PI;

}

/* =========================
   ダイヤル操作
========================= */

function updateHpFromDrag(event){

  const currentAngle =
    getPointerAngle(event);

  if(previousPointerAngle === null){

    previousPointerAngle =
      currentAngle;

    return;

  }

  let difference =
    currentAngle -
    previousPointerAngle;

  /*
    円の0度境界をまたいだ際に
    360度飛ばないよう補正
  */

  if(difference > 180){

    difference -= 360;

  }

  if(difference < -180){

    difference += 360;

  }

  accumulatedAngle +=
    difference;

  /*
    時計回り
    HP +10
  */

  while(
    accumulatedAngle >=
    DEGREES_PER_STEP
  ){

    if(hp < MAX_HP){

      hp = Math.min(
        MAX_HP,
        hp + HP_STEP
      );

      renderHP();

    }

    accumulatedAngle -=
      DEGREES_PER_STEP;

  }

  /*
    反時計回り
    HP -10
  */

  while(
    accumulatedAngle <=
    -DEGREES_PER_STEP
  ){

    if(hp > MIN_HP){

      hp = Math.max(
        MIN_HP,
        hp - HP_STEP
      );

      renderHP();

    }

    accumulatedAngle +=
      DEGREES_PER_STEP;

  }

  /*
    120 → 0へ飛ばない
  */

  if(
    hp === MAX_HP &&
    accumulatedAngle > 0
  ){

    accumulatedAngle = 0;

  }

  /*
    0 → 120へ飛ばない
  */

  if(
    hp === MIN_HP &&
    accumulatedAngle < 0
  ){

    accumulatedAngle = 0;

  }

  previousPointerAngle =
    currentAngle;

}

/* =========================
   操作開始
========================= */

dialContainer.addEventListener(
  "pointerdown",
  event => {

    event.preventDefault();

    isDragging = true;

    previousPointerAngle =
      getPointerAngle(event);

    accumulatedAngle = 0;

    dialContainer.classList.add(
      "dragging"
    );

    dialContainer.setPointerCapture(
      event.pointerId
    );

  }
);

/* =========================
   操作中
========================= */

dialContainer.addEventListener(
  "pointermove",
  event => {

    if(!isDragging){
      return;
    }

    event.preventDefault();

    updateHpFromDrag(event);

  }
);

/* =========================
   操作終了
========================= */

function stopDragging(event){

  isDragging = false;

  previousPointerAngle = null;
  accumulatedAngle = 0;

  dialContainer.classList.remove(
    "dragging"
  );

  if(
    event &&
    dialContainer.hasPointerCapture(
      event.pointerId
    )
  ){

    dialContainer.releasePointerCapture(
      event.pointerId
    );

  }

}

dialContainer.addEventListener(
  "pointerup",
  stopDragging
);

dialContainer.addEventListener(
  "pointercancel",
  stopDragging
);

/* =========================
   キーボード操作
========================= */

dialContainer.addEventListener(
  "keydown",
  event => {

    if(
      event.key === "ArrowLeft" ||
      event.key === "ArrowDown"
    ){

      event.preventDefault();

      hp = Math.max(
        MIN_HP,
        hp - HP_STEP
      );

      renderHP();

    }

    if(
      event.key === "ArrowRight" ||
      event.key === "ArrowUp"
    ){

      event.preventDefault();

      hp = Math.min(
        MAX_HP,
        hp + HP_STEP
      );

      renderHP();

    }

  }
);

/* =========================
   ポケモン変更

   HPは120に戻す
========================= */

pokemonSelect.addEventListener(
  "change",
  () => {

    hp = MAX_HP;

    previousPointerAngle = null;
    accumulatedAngle = 0;

    renderHP();

  }
);

/* =========================
   RESET

   ポケモンは維持
   HPだけ120
========================= */

resetBtn.addEventListener(
  "click",
  () => {

    hp = MAX_HP;

    previousPointerAngle = null;
    accumulatedAngle = 0;

    renderHP();

  }
);

/* =========================
   初期表示
========================= */

createDialScale();
renderHP();

</script>

</body>
</html>
