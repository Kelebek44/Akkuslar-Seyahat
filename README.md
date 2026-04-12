<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V42</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; }
        * { box-sizing: border-box; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: 'Space Grotesk', sans-serif; display: flex; height: 100vh; overflow: hidden; }
        .sidebar { width: 280px; background: #000; border-right: 3px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 15px; margin-bottom: 20px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 6px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; }
        .nav-btn:hover { background: var(--p); color: white; }
        .active-btn { background: var(--p) !important; color: white !important; }
        .main { flex: 1; padding: 30px; overflow-y: auto; background: radial-gradient(circle at top right, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif { display: block !important; animation: fadeIn 0.4s; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 15px; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #333; text-align: center; }
        .card img { width: 100%; height: 150px; object-fit: cover; border-radius: 10px; margin-bottom: 10px; }
        canvas { background: #111; border: 2px solid var(--p); display: block; margin: 0 auto; max-width: 100%; border-radius: 10px; }
        input, select { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; text-transform: uppercase; }
        .del-btn { background: #600; color: white; border: none; padding: 5px 10px; border-radius: 5px; cursor: pointer; display: none; margin-top: 10px; font-size: 11px; }
        .kurucu-modu .del-btn { display: inline-block !important; }
        .chat-box { height: 250px; overflow-y: auto; background: #050505; border: 1px solid #333; padding: 10px; border-radius: 10px; text-align: left; margin-bottom: 10px; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h2 style="color:var(--p); margin:0;">AKKUŞLAR</h2>
        <span style="color:var(--gold); font-size:12px;">Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</span>
    </div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Sayfa</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Ekip Üyeleri</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🎫 Bilet Sistemi</button>
    <button class="nav-btn" onclick="sayfa(10, this)">🎮 Minyatür Tır</button>
    <button class="nav-btn" onclick="sayfa(12, this)">📩 Ekibe Katıl</button>
    <div style="color:#444; font-size:10px; padding:10px 0;">ARŞİV</div>
    <button class="nav-btn" onclick="sayfa(5, this)">🎞️ Galeri & Modlar</button>
    <button class="nav-btn" style="color:var(--p); border:1px dashed var(--p); margin-top:20px;" onclick="yonetimGiris()">⚙️ YÖNETİM & SOHBET</button>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <img id="main-afis" style="width:100%; height:300px; object-fit:cover; border-radius:20px; border:3px solid var(--p); display:none; margin-bottom:20px;">
        <h1>🚛 AKKUŞLAR SEYAHAT</h1>
        <p>Terminal Tüm Fonksiyonlarıyla Aktif!</p>
        <div class="grid">
            <div class="card"><h3 id="u-count">0</h3><p>Kayıtlı Üye</p></div>
            <div class="card"><h3>İST-ANK-DEN-HAT</h3><p>Aktif Hatlar</p></div>
        </div>
    </div>

    <div id="p2" class="panel"><h1>👥 Kayıtlı Üyeler</h1><div id="list-ekip" class="grid"></div></div>

    <div id="p3" class="panel">
        <h1>🎫 Sanal Bilet Gişesi</h1>
        <div class="grid">
            <div class="card">
                <input type="text" id="b-ad" placeholder="Yolcu İsmi...">
                <select id="b-rota"><option>Bartın-Denizli</option><option>İstanbul-Hatay</option><option>Ankara-Denizli</option></select>
                <button class="action-btn" onclick="biletKes()">BİLETİ ONAYLA</button>
            </div>
            <div class="card"><h3>Yolcu Listesi</h3><div id="list-bilet" style="max-height:200px; overflow-y:auto; background:#000; padding:10px;"></div></div>
        </div>
    </div>

    <div id="p5" class="panel"><h1>🎞️ Galeri & Atölye</h1><div id="list-icerik" class="grid"></div></div>

    <div id="p10" class="panel">
        <h1>🎮 Minyatür Tır Sürüşü</h1>
        <p>Ok tuşlarıyla Akkuşlar Tırını yolda tut!</p>
        <canvas id="gameCanvas" width="600" height="400"></canvas>
    </div>

    <div id="p12" class="panel">
        <h1>📩 Ekibe Katılım Formu</h1>
        <div class="card" style="max-width:500px; margin:auto;">
            <input type="text" id="kat-ad" placeholder="Nick/Adınız...">
            <input type="text" id="kat-tk" placeholder="TikTok Adınız...">
            <button class="action-btn" onclick="basvuruAt()">BAŞVURUYU GÖNDER</button>
        </div>
    </div>

    <div id="p13" class="panel">
        <h1>⚙️ YÖNETİM MERKEZİ</h1>
        <div class="grid">
            <div id="kurucu-panel" class="card" style="display:none; border:2px solid var(--gold);">
                <span style="color:var(--gold); font-weight:bold;">👑 Kurucu Paneli</span><hr>
                <h3>Rol Ver & Üye Ekle</h3>
                <input type="text" id="r-ad" placeholder="İsim...">
                <select id="r-rol"><option value="uye">Üye</option><option value="admin">Yönetici</option></select>
                <button class="action-btn" onclick="rolVer()">KAYDET</button>
                <hr>
                <input type="text" id="af-link" placeholder="Afiş URL...">
                <button class="action-btn" onclick="afisSet()" style="background:var(--gold); color:#000;">AFİŞİ BAS</button>
            </div>
            <div class="card">
                <h3>💬 Yönetici Sohbeti</h3>
                <div id="chat-list" class="chat-box"></div>
                <input type="text" id="chat-in" placeholder="Mesaj...">
                <button class="action-btn" onclick="msgGonder()">GÖNDER</button>
            </div>
        </div>
        <div class="card" style="margin-top:20px;">
            <h3>🛠️ İçerik Ekle</h3>
            <input type="text" id="i-bas" placeholder="Başlık...">
            <input type="text" id="i-url" placeholder="URL...">
            <button class="action-btn" onclick="icerikEkle()">YAYINLA</button>
        </div>
    </div>
</div>

<script>
    const firebaseConfig = {
      apiKey: "AIzaSyCowK8jtpCENPWxz0EpANeRghLXlYMVkcg",
      authDomain: "akkuslar-seyahat-f3577.firebaseapp.com",
      projectId: "akkuslar-seyahat-f3577",
      storageBucket: "akkuslar-seyahat-f3577.firebasestorage.app",
      messagingSenderId: "207828517432",
      appId: "1:207828517432:web:dedb92a79fe1d17aec9ccd"
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();
    let rol = "uye";

    function yonetimGiris() {
        let s = prompt("Şifre:");
        if(s === "1907akkus") {
            rol = "kurucu";
            document.getElementById('app-body').classList.add('kurucu-modu');
            document.getElementById('kurucu-panel').style.display = "block";
            sayfa(13);
        } else if(s === "akkus123") { rol = "admin"; sayfa(13); }
    }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active-btn'));
        document.getElementById('p' + n).classList.add('aktif');
        if(btn) btn.classList.add('active-btn');
        if(n === 10) initGame();
    }

    // FIREBASE İŞLEMLERİ
    db.collection("ekip").onSnapshot(s => {
        document.getElementById('u-count').innerText = s.size;
        document.getElementById('list-ekip').innerHTML = s.docs.map(d => `<div class="card"><b>${d.data().ad}</b><br><small>${d.data().rol}</small><br><button class="del-btn" onclick="sil('ekip','${d.id}')">SİL</button></div>`).join('');
    });

    db.collection("biletler").orderBy("tarih","desc").onSnapshot(s => {
        document.getElementById('list-bilet').innerHTML = s.docs.map(d => `<div>🎫 ${d.data().ad} (${d.data().rota}) <button class="del-btn" onclick="sil('biletler','${d.id}')">SİL</button></div>`).join('');
    });

    db.collection("icerik").onSnapshot(s => {
        document.getElementById('list-icerik').innerHTML = s.docs.map(d => `<div class="card"><img src="${d.data().url}"><b>${d.data().bas}</b><br><button class="del-btn" onclick="sil('icerik','${d.id}')">SİL</button></div>`).join('');
    });

    db.collection("sohbet").orderBy("tarih","desc").limit(30).onSnapshot(s => {
        document.getElementById('chat-list').innerHTML = s.docs.map(d => `<div><b>${d.data().kim}:</b> ${d.data().msg}</div>`).reverse().join('');
    });

    db.collection("ayarlar").doc("afis").onSnapshot(d => { if(d.exists) { document.getElementById('main-afis').src = d.data().url; document.getElementById('main-afis').style.display = "block"; }});

    function rolVer() { if(rol === "kurucu") db.collection("ekip").add({ad: document.getElementById('r-ad').value, rol: document.getElementById('r-rol').value}); }
    function biletKes() { db.collection("biletler").add({ad: document.getElementById('b-ad').value, rota: document.getElementById('b-rota').value, tarih: Date.now()}); }
    function msgGonder() { db.collection("sohbet").add({kim: rol, msg: document.getElementById('chat-in').value, tarih: Date.now()}); document.getElementById('chat-in').value=""; }
    function icerikEkle() { db.collection("icerik").add({bas: document.getElementById('i-bas').value, url: document.getElementById('i-url').value}); }
    function afisSet() { if(rol === "kurucu") db.collection("ayarlar").doc("afis").set({url: document.getElementById('af-url').value}); }
    function basvuruAt() { alert("Başvurunuz Alındı!"); }
    function sil(c, id) { if(rol === "kurucu" && confirm("Silinsin mi?")) db.collection(c).doc(id).delete(); }

    function initGame() {
        const c = document.getElementById('gameCanvas'); const ctx = c.getContext('2d');
        let x = 275, road = 0;
        function draw() {
            ctx.clearRect(0,0,600,400); ctx.fillStyle="#333"; ctx.fillRect(150,0,300,400);
            ctx.fillStyle="#FFF"; for(let i=0; i<10; i++) ctx.fillRect(295, ((i*60)+road)%400, 10, 30);
            road+=5; ctx.fillStyle="red"; ctx.fillRect(x, 320, 50, 70); 
            requestAnimationFrame(draw);
        }
        document.onkeydown = (e) => { if(e.key === "ArrowLeft" && x > 150) x -= 15; if(e.key === "ArrowRight" && x < 400) x += 15; };
        draw();
    }
</script>
</body>
</html>
