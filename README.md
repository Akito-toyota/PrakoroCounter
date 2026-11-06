<!doctype html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>プラコロ カウンター</title>
<style>
:root{--bg:#f3f6fa;--card:#fff;--ink:#172033;--line:#d9e1ec;--green:#16a34a;--red:#dc2626;--blue:#2563eb}
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
body{margin:0;background:var(--bg);background-size:cover;background-position:center;background-repeat:no-repeat;background-attachment:fixed;color:var(--ink);font-family:system-ui,-apple-system,"Hiragino Kaku Gothic ProN","Yu Gothic",sans-serif}
.app{min-height:100vh;max-width:560px;margin:auto;padding:14px;display:flex;flex-direction:column;gap:14px}
h1{text-align:center;margin:4px 0 0;font-size:30px}
.card{background:rgba(255,255,255,.90);backdrop-filter:blur(5px);border-radius:20px;padding:18px;box-shadow:0 5px 20px #1f293712}
label{display:block;font-weight:900;font-size:20px;margin-bottom:9px}
select{width:100%;font-size:24px;font-weight:800;padding:15px;border:2px solid var(--line);border-radius:14px;background:#fff;color:var(--ink)}
.hp-label{text-align:center;font-weight:900;font-size:20px;margin-top:18px}.hp{text-align:center;font-weight:950;color:var(--green);font-size:88px;line-height:1.05;margin:8px 0 16px;font-variant-numeric:tabular-nums}
.buttons{display:grid;grid-template-columns:1fr 1fr;gap:12px}.buttons button{border:0;border-radius:16px;padding:22px 8px;font-size:42px;font-weight:950;touch-action:manipulation}.down{background:#fee2e2;color:var(--red)}.up{background:#dcfce7;color:var(--green)}
.bgtools{display:grid;grid-template-columns:1fr 1fr;gap:10px}.bgbtn{border:0;border-radius:14px;padding:14px 8px;font-size:17px;font-weight:900;background:#111827;color:#fff}.removebg{background:#64748b}.reset{width:100%;border:0;border-radius:16px;background:var(--blue);color:#fff;font-size:22px;font-weight:900;padding:17px;touch-action:manipulation}
.note{text-align:center;color:#687386;font-size:14px}
@media(max-width:380px){.app{padding:10px}.hp{font-size:76px}h1{font-size:26px}.buttons button{font-size:36px;padding:19px 6px}}
</style>
</head>
<body>
<div class="app">
<h1>プラコロ カウンター</h1>
<div class="bgtools"><button id="chooseBg" class="bgbtn" type="button">背景画像を選択</button><button id="removeBg" class="bgbtn removebg" type="button">背景を削除</button></div><input id="bgInput" type="file" accept="image/*" hidden>
<div class="card">
<label for="pokemon">ポケモン</label>
<select id="pokemon">
<option value="">選択してください</option>
<option>ピカチュウ</option><option>ヒトカゲ</option><option>フシギダネ</option><option>ゼニガメ</option><option>ミュウ</option><option>イーブイ</option><option>ルカリオ</option><option>メタグロス</option><option>ゲンガー</option><option>イワーク</option><option>ベトベター</option><option>カイロス</option><option>ファイヤー</option><option>サンダー</option><option>フリーザー</option><option>ヤミラミ</option><option>ゲッコウガ</option><option>ボスゴドラ</option><option>レックウザ</option><option>カヌチャン</option><option>ラッキー</option>
</select>
<div class="hp-label">HP</div>
<div id="hp" class="hp">120</div>
<div class="buttons">
<button id="down" class="down" type="button" aria-label="HPを10減らす">▼ 10</button>
<button id="up" class="up" type="button" aria-label="HPを10増やす">▲ 10</button>
</div>
</div>
<button id="reset" class="reset" type="button">HPを120に戻す</button>
<div class="note">HPは0～120の範囲で10ずつ変更できます</div>
</div>
<script>
let hp=120;const hpEl=document.getElementById('hp');const bgInput=document.getElementById('bgInput');
function setBg(data){document.body.style.backgroundImage=data?`linear-gradient(rgba(243,246,250,.15),rgba(243,246,250,.15)),url(${data})`:'';}
try{const saved=localStorage.getItem('placoroBg');if(saved)setBg(saved)}catch(e){}
function render(){hpEl.textContent=hp;hpEl.style.color=hp===0?'#dc2626':hp<=40?'#ea580c':'#16a34a'}
document.getElementById('down').addEventListener('click',()=>{hp=Math.max(0,hp-10);render()});
document.getElementById('up').addEventListener('click',()=>{hp=Math.min(120,hp+10);render()});
document.getElementById('reset').addEventListener('click',()=>{hp=120;render()});
document.getElementById('chooseBg').addEventListener('click',()=>bgInput.click());
bgInput.addEventListener('change',()=>{const f=bgInput.files&&bgInput.files[0];if(!f)return;if(!f.type.startsWith('image/'))return;const r=new FileReader();r.onload=()=>{const data=r.result;setBg(data);try{localStorage.setItem('placoroBg',data)}catch(e){};};r.readAsDataURL(f);});
document.getElementById('removeBg').addEventListener('click',()=>{setBg('');bgInput.value='';try{localStorage.removeItem('placoroBg')}catch(e){}});
render();
</script>
</body>
</html>
