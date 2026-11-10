<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
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

html{
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
  height:100%;
  margin:0;
  padding:0;

  overflow:hidden;
  overscroll-behavior:none;

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

  border:1px solid #263447;
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

.reset:focus-visible,
.pokemon-select:focus-visible,
.dial-container:focus-visible{
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

  padding:10px;

  background:#0b1017;

  border:1px solid #29364a;
  border-radius:18px;

  display:flex;
  flex-direction:column;

  align-items:center;

  overflow:hidden;
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

/* 通常時：黒背景＋白文字 */

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

/* 開いた選択肢：白背景＋黒文字 */

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
}

/* =========================
   モニターヘッダー
========================= */

.digital-header{
  width:100%;

  flex:0 0 auto;

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
  /*
    横幅だけでなく画面高さにも制限。
    縦に短いスマホでは自動縮小。
  */

  width:min(
    88vw,
    56dvh,
    410px
  );

  max-width:100%;

  aspect-ratio:1 / 1;

  flex:0 1 auto;

  margin:0 auto;

  position:relative;

  overflow:visible;

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
   円
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

.dial-track{
  fill:none;

  stroke:#1d2734;
  stroke-width:24;
}

/* =========================
   HPゲージ
========================= */

.dial-progress{
  fill:none;

  stroke:var(--green);
  stroke-width:24;

  transform:rotate(-90deg);
  transform-origin:180px 180px;

  transition:stroke .12s ease;
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
  fill:var(--green);

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:82px;
  font-weight:950;

  text-anchor:middle;

  font-variant-numeric:tabular-nums;

  transition:fill .12s ease;
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

  transition:stroke .12s ease;

  pointer-events:none;
}

.handle-center{
  fill:var(--green);

  transition:fill .12s ease;

  pointer-events:none;
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
  flex:0 0 auto;

  margin-top:-6px;

  padding:4px 8px;

  color:#728095;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:10px;
  font-weight:900;

  letter-spacing:.8px;

  text-align:center;

  white-space:nowrap;
}

/* =========================
   MIN STEP MAX
========================= */

.limit-indicator{
  width:100%;

  flex:0 0 auto;

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

  letter-spacing:1px;
}

.limit-indicator strong{
  color:#b7c4d4;
}

/* =========================
   幅が狭いスマホ
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

  .dial-hp-value{
    font-size:74px;
  }

  .dial-guide{
    font-size:9px;
  }

}

/* =========================
   高さが低いスマホ
========================= */

@media(max-height:750px){

  .app{
    gap:5px;

    padding-top:
      max(5px,env(safe-area-inset-top));

    padding-bottom:
      max(5px,env(safe-area-inset-bottom));
  }

  header{
    padding:7px 9px;
  }

  .system-label{
    display:none;
  }

  h1{
    font-size:19px;
  }

  .reset{
    padding:7px 10px;
    font-size:12px;
  }

  .card{
    padding:6px;
  }

  .select-title{
    margin-bottom:2px;
  }

  .pokemon-select{
    height:46px;

    font-size:18px;
  }

  .select-frame::after{
    bottom:14px;
  }

  .digital-unit{
    margin-top:5px;

    padding:4px;
  }

  .digital-header{
    padding-bottom:0;

    font-size:8px;
  }

  .dial-container{
    width:min(
      88vw,
      54dvh,
      330px
    );
  }

  .dial-guide{
    margin-top:-10px;

    padding:2px 4px;

    font-size:8px;
  }

  .limit-indicator{
    padding:3px 7px 1px;

    font-size:8px;
  }

}

/* =========================
   かなり高さが低いスマホ
========================= */

@media(max-height:620px){

  .app{
    padding-top:4px;
    padding-bottom:4px;
  }

  header{
    padding:5px 8px;
  }

  h1{
    font-size:18px;
  }

  .pokemon-select{
    height:42px;
  }

  .digital-unit{
    margin-top:3px;
  }

  .dial-container{
    width:min(
      86vw,
      50dvh,
      285px
    );
  }

  .dial-guide{
    display:none;
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
        タップで移動 ／ RINGをドラッグしてHPを変更
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

let pointerStartX = 0;

let pointerStartY = 0;

let pointerMoved = false;

/* 10HP = 30度 */

const DEGREES_PER_STEP = 30;

/* タップとドラッグの判定 */

const TAP_MOVE_THRESHOLD = 10;

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

/* =========================
   ポケモン一覧
========================= */

pokemonData.forEach(name => {

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
    angle *
    Math.PI /
    180;

  return {

    x:
      centerX +
      radius *
      Math.cos(radians),

    y:
      centerY +
      radius *
      Math.sin(radians)

  };

}

/* =========================
   HP → 角度
========================= */

function hpToAngle(value){

  return -90 +
    (value / MAX_HP)
    * 360;

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
   目盛り作成
========================= */

function createDialScale(){

  scaleLines.innerHTML =
    "";

  scaleLabels.innerHTML =
    "";

  for(
    let value = 0;
    value < MAX_HP;
    value += HP_STEP
  ){

    const angle =
      -90 +
      (value / MAX_HP)
      * 360;

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
          ? "0/120"
          : value;

      scaleLabels.appendChild(
        text
      );

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
    CIRCUMFERENCE *
    ratio;

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

  hpValueEl.textContent =
    String(hp).padStart(
      3,
      "0"
    );

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

  dialProgress.style.strokeDasharray =
    CIRCUMFERENCE;

  dialProgress.style.strokeDashoffset =
    dashOffset;

  dialProgress.style.stroke =
    hpColor;

  hpValueEl.style.fill =
    hpColor;

  dialHandle.style.stroke =
    hpColor;

  handleCenter.style.fill =
    hpColor;

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
   ポインター座標
========================= */

function getPointerPosition(
  event
){

  const rect =
    hpDial.getBoundingClientRect();

  const scaleX =
    360 /
    rect.width;

  const scaleY =
    360 /
    rect.height;

  return {

    x:
      (
        event.clientX -
        rect.left
      ) * scaleX,

    y:
      (
        event.clientY -
        rect.top
      ) * scaleY

  };

}

/* =========================
   ポインター角度
========================= */

function getPointerAngle(
  event
){

  const position =
    getPointerPosition(
      event
    );

  return Math.atan2(
    position.y -
      CENTER_Y,

    position.x -
      CENTER_X
  ) * 180 /
    Math.PI;

}

/* =========================
   タップジャンプ
========================= */

function jumpToTappedHp(
  event
){

  const position =
    getPointerPosition(
      event
    );

  const dx =
    position.x -
    CENTER_X;

  const dy =
    position.y -
    CENTER_Y;

  const distance =
    Math.sqrt(
      dx * dx +
      dy * dy
    );

  /*
    リング付近だけ有効
  */

  if(
    distance <
    DIAL_RADIUS - 45
  ){

    return;

  }

  if(
    distance >
    DIAL_RADIUS + 45
  ){

    return;

  }

  let angle =
    Math.atan2(
      dy,
      dx
    ) * 180 /
    Math.PI;

  /*
    上を0度として
    時計回り0～360へ変換
  */

  let clockwiseAngle =
    angle + 90;

  if(
    clockwiseAngle < 0
  ){

    clockwiseAngle +=
      360;

  }

  if(
    clockwiseAngle >= 360
  ){

    clockwiseAngle -=
      360;

  }

  let tappedStep =
    Math.round(
      clockwiseAngle /
      DEGREES_PER_STEP
    );

  if(
    tappedStep >= 12
  ){

    tappedStep = 0;

  }

  let tappedHp =
    tappedStep *
    HP_STEP;

  /*
    0と120の重なる上端だけ
    現在値に近い方を選択
  */

  if(
    tappedStep === 0
  ){

    tappedHp =
      hp >= 60
        ? 120
        : 0;

  }

  hp =
    Math.max(
      MIN_HP,
      Math.min(
        MAX_HP,
        tappedHp
      )
    );

  previousPointerAngle =
    null;

  accumulatedAngle =
    0;

  renderHP();

}

/* =========================
   ドラッグ
========================= */

function updateHpFromDrag(
  event
){

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

  /*
    円の境界をまたいでも
    360度ジャンプさせない
  */

  if(
    difference > 180
  ){

    difference -=
      360;

  }

  if(
    difference < -180
  ){

    difference +=
      360;

  }

  accumulatedAngle +=
    difference;

  /*
    時計回り +10
  */

  while(
    accumulatedAngle >=
    DEGREES_PER_STEP
  ){

    if(
      hp < MAX_HP
    ){

      hp =
        Math.min(
          MAX_HP,
          hp + HP_STEP
        );

      renderHP();

    }

    accumulatedAngle -=
      DEGREES_PER_STEP;

  }

  /*
    反時計回り -10
  */

  while(
    accumulatedAngle <=
    -DEGREES_PER_STEP
  ){

    if(
      hp > MIN_HP
    ){

      hp =
        Math.max(
          MIN_HP,
          hp - HP_STEP
        );

      renderHP();

    }

    accumulatedAngle +=
      DEGREES_PER_STEP;

  }

  /*
    120→0へ飛ばない
  */

  if(
    hp === MAX_HP &&
    accumulatedAngle > 0
  ){

    accumulatedAngle =
      0;

  }

  /*
    0→120へ飛ばない
  */

  if(
    hp === MIN_HP &&
    accumulatedAngle < 0
  ){

    accumulatedAngle =
      0;

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

      /*
        一部ブラウザ用
      */

    }

  }
);

/* =========================
   操作中
========================= */

dialContainer.addEventListener(
  "pointermove",
  event => {

    if(
      !isDragging
    ){

      return;

    }

    event.preventDefault();

    const moveX =
      event.clientX -
      pointerStartX;

    const moveY =
      event.clientY -
      pointerStartY;

    const moveDistance =
      Math.sqrt(
        moveX * moveX +
        moveY * moveY
      );

    if(
      moveDistance >
      TAP_MOVE_THRESHOLD
    ){

      pointerMoved =
        true;

    }

    if(
      pointerMoved
    ){

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

  if(
    isDragging &&
    allowTap &&
    !pointerMoved
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

    /*
      一部ブラウザ用
    */

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
   キーボード
========================= */

dialContainer.addEventListener(
  "keydown",
  event => {

    if(
      event.key ===
        "ArrowLeft" ||
      event.key ===
        "ArrowDown"
    ){

      event.preventDefault();

      hp =
        Math.max(
          MIN_HP,
          hp - HP_STEP
        );

      renderHP();

    }

    if(
      event.key ===
        "ArrowRight" ||
      event.key ===
        "ArrowUp"
    ){

      event.preventDefault();

      hp =
        Math.min(
          MAX_HP,
          hp + HP_STEP
        );

      renderHP();

    }

  }
);

/* =========================
   ポケモン変更
========================= */

pokemonSelect.addEventListener(
  "change",
  () => {

    hp =
      MAX_HP;

    previousPointerAngle =
      null;

    accumulatedAngle =
      0;

    renderHP();

  }
);

/* =========================
   RESET
========================= */

resetBtn.addEventListener(
  "click",
  () => {

    hp =
      MAX_HP;

    previousPointerAngle =
      null;

    accumulatedAngle =
      0;

    renderHP();

  }
);

/* =========================
   スクロール抑止

   ダイヤル操作中の
   touchmoveをブラウザ側へ
   渡さない
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
   初期表示
========================= */

createDialScale();

renderHP();

</script>

</body>
</html>
