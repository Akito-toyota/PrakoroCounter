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
  --page-bg:#f4f7fb;
  --ink:#19212d;
  --blue:#2563eb;
  --red:#e11d48;
  --green:#16a34a;
  --orange:#f59e0b;
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
  background:var(--page-bg);
  color:var(--ink);
  font-family:
    system-ui,
    -apple-system,
    "Hiragino Kaku Gothic ProN",
    "Yu Gothic",
    sans-serif;
}

.app{
  width:100%;
  max-width:600px;

  min-height:100vh;
  min-height:100dvh;

  margin:0 auto;

  padding:
    max(12px, env(safe-area-inset-top))
    12px
    max(12px, env(safe-area-inset-bottom));

  display:flex;
  flex-direction:column;
  gap:12px;
}

/* ヘッダー */

header{
  width:100%;

  display:flex;
  align-items:center;
  justify-content:space-between;

  gap:10px;
}

h1{
  margin:0;

  font-size:clamp(22px,6vw,32px);
  font-weight:950;

  white-space:nowrap;
}

/* ボタン共通 */

button{
  border:0;
  border-radius:14px;

  font-family:inherit;
  font-weight:900;

  cursor:pointer;
  touch-action:manipulation;

  transition:
    transform .08s ease,
    filter .15s ease;
}

button:active{
  transform:scale(.96);
}

button:focus-visible,
select:focus-visible{
  outline:4px solid rgba(37,99,235,.35);
  outline-offset:2px;
}

/* リセット */

.reset{
  flex-shrink:0;

  padding:11px 15px;

  background:var(--orange);
  color:#ffffff;

  border:2px solid rgba(0,0,0,.15);

  box-shadow:0 3px 8px rgba(0,0,0,.16);

  font-size:16px;
}

/* メインカード */

.card{
  width:100%;
  flex:1;
  min-height:0;

  padding:18px 14px;

  background:#ffffff;

  border:3px solid rgba(0,0,0,.14);
  border-radius:22px;

  box-shadow:0 6px 20px rgba(35,48,74,.18);

  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;

  transition:background-color .3s ease;
}

/* ポケモン選択 */

.pokemon-select{
  width:100%;

  padding:14px 38px 14px 10px;

  background:rgba(255,255,255,.94);
  color:var(--ink);

  border:3px solid rgba(0,0,0,.25);
  border-radius:14px;

  box-shadow:0 3px 9px rgba(0,0,0,.14);

  font-family:inherit;
  font-size:clamp(22px,7vw,30px);
  font-weight:900;

  text-align:center;
  text-align-last:center;

  outline:none;
}

.pokemon-select:focus{
  border-color:var(--blue);
}

/* 弱点 */

.weakness{
  min-height:52px;

  margin-top:22px;
  padding:10px 24px;

  display:flex;
  align-items:center;
  justify-content:center;

  background:rgba(255,255,255,.94);
  color:#1f2937;

  border:3px solid rgba(0,0,0,.18);
  border-radius:999px;

  box-shadow:0 3px 9px rgba(0,0,0,.13);

  font-size:clamp(19px,5vw,25px);
  font-weight:950;

  text-align:center;
}

.weakness.empty{
  visibility:hidden;
}

/* HP */

.hp-panel{
  min-width:220px;

  margin-top:20px;
  padding:10px 22px 14px;

  background:rgba(255,255,255,.94);

  border:3px solid rgba(0,0,0,.16);
  border-radius:22px;

  box-shadow:0 3px 10px rgba(0,0,0,.13);

  text-align:center;
}

.hp-title{
  color:#596579;

  font-size:20px;
  font-weight:900;
}

.hp{
  margin:4px 0 0;

  color:var(--green);

  font-size:clamp(90px,30vw,145px);
  line-height:.95;

  font-weight:950;
  font-variant-numeric:tabular-nums;
}

.hp.danger{
  color:var(--red);
}

.hp.zero{
  color:#6b7280;
}

/* HP操作 */

.stepper{
  width:100%;

  margin-top:22px;

  display:grid;
  grid-template-columns:1fr 1fr;

  gap:12px;
}

.stepper button{
  width:100%;
  min-height:100px;

  border:3px solid rgba(0,0,0,.1);

  box-shadow:0 4px 10px rgba(0,0,0,.15);

  font-size:50px;
}

.down{
  background:#fee2e2;
  color:#b91c1c;
}

.up{
  background:#dcfce7;
  color:#15803d;
}

/* 小さいスマホ */

@media(max-width:380px){

  .app{
    padding-left:8px;
    padding-right:8px;
  }

  h1{
    font-size:21px;
  }

  .reset{
    padding:10px 12px;
    font-size:14px;
  }

  .card{
    padding:14px 10px;
  }

  .pokemon-select{
    font-size:21px;
  }

  .weakness{
    margin-top:16px;
    font-size:18px;
  }

  .hp-panel{
    min-width:200px;
    margin-top:15px;
  }

  .hp{
    font-size:100px;
  }

  .stepper{
    margin-top:15px;
  }

  .stepper button{
    min-height:88px;
    font-size:44px;
  }
}

/* 高さが低いスマホ */

@media(max-height:650px){

  .app{
    gap:8px;

    padding-top:8px;
    padding-bottom:8px;
  }

  h1{
    font-size:22px;
  }

  .card{
    padding:10px;
  }

  .pokemon-select{
    padding-top:10px;
    padding-bottom:10px;
  }

  .weakness{
    min-height:42px;

    margin-top:9px;
    padding:6px 16px;
  }

  .hp-panel{
    margin-top:9px;
    padding:6px 18px 9px;
  }

  .hp{
    font-size:85px;
  }

  .stepper{
    margin-top:9px;
  }

  .stepper button{
    min-height:70px;
    font-size:38px;
  }
}
</style>
</head>

<body>

<div class="app">

  <header>
    <h1>プラコロカウンター</h1>

    <button
      id="resetBtn"
      class="reset"
      type="button"
    >
      リセット
    </button>
  </header>

  <main
    id="card"
    class="card"
  >

    <select
      id="pokemonSelect"
      class="pokemon-select"
      aria-label="ポケモンを選択"
    >
      <option value="">ポケモンを選択</option>
    </select>

    <div
      id="weakness"
      class="weakness empty"
      aria-live="polite"
    >
      弱点：
    </div>

    <div class="hp-panel">

      <div class="hp-title">
        HP
      </div>

      <div
        id="hp"
        class="hp"
        aria-live="
