<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V36</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; }
        * { box-sizing: border-box; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: 'Space Grotesk', sans-serif; display: flex; height: 100vh; overflow: hidden; }
        
        .sidebar { width: 280px; background: #000; border-right: 3px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; }
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 10px; margin-bottom: 15px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 5px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; }
        .nav-btn:hover { background: var(--p); color: white; }
        .active-btn { background: var(--p) !important; color: white !important; }

        .main { flex: 1; padding: 30px; overflow-y: auto; background: radial-gradient(circle at top right, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif { display: block !important; }

        /* ANA SAYFA AFİŞİ */
        .hero-banner { width: 100%; height: 350px; border-radius: 20px; border: 3px solid var(--p); object-fit: cover; margin-bottom: 20px; display: none; }
        
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(230px, 1fr)); gap: 15px; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #333; text-align: center; }
        
        /* OYUN */
        #game-area { text-align: center; background: #000; padding: 20px; border-radius: 15px; border: 2px solid #444; }
        canvas { background: #111; border: 2px solid var(--p); max-width: 100%; }

        /* SOHBET */
        .chat-area { height: 250px; overflow-y: auto; background: #050505; border: 1px solid #333; padding: 10px; border-radius: 10px; text-align: left; margin-bottom: 10px; }
        .chat-msg { margin-bottom: 8px; font-size: 14px; }
        .chat-name { color: var(--gold); font-weight: bold; }

        input, select { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h2 style="color:var(--p); margin:0;">AKKUŞLAR</h2>
        <span style="color:var(--gold); font-size:12px; font-weight:bold;">Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</span>
    </div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Sayfa</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Üyeler</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🛣️ Seferler & Rotalar</button>
    <button class="nav-btn" onclick="sayfa(10, this)">🎮 Mini Oyun</button>
    <button class="nav-btn" onclick="sayfa(12, this)">📩 Ekibe Katıl</button>
    <button class="nav-btn" style="color:var(--p); border:1px dashed var(--p); margin-top:20px;" onclick="yonetimGiris()">⚙️ YÖNETİM & SOHBET</button>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <img id="main-afis" class="hero-banner" src="">
        <h1>🚛 AKKUŞLAR SEYAHAT TERMİNALİ</h1>
        <p>Hükümdar: 乂✯ҠƐꝈƐβƐҠ✯乂</p>
        <div class="grid">
            <div class="card"><h3 id="u-count">0</h3><p>Kayıtlı Üye</p></div>
            <div class="card"><h3>AKTİF</h3><p>Sefer Durumu</p></div>
        </div>
    </div>

    <div id="p3" class="panel">
        <h1>🛣️ Güncel Sefer Rotaları</h1>
        <div class="grid">
            <div class="card"><h3>İSTANBUL ➔ DENİZLİ</h3><p>Durum: Açık</p></div>
            <div class="card"><h3>ANKARA ➔ HATAY</h3><p>Durum: Açık</p></div>
            <div class="card"><h3>BARTIN ➔ DENİZLİ</h3><p>Durum: Açık</p></div>
            <div class="card"><h3>HATAY ➔ İSTANBUL</h3><p>Durum: Açık</p></div>
        </div>
    </div>

    <div id="p10" class="panel">
        <h1>🎮 Tır Sürme Simülatörü</h1>
        <div id="game-area">
            <p>Ok tuşlarıyla Akkuşlar Tırını kontrol et!</p>
            <canvas id="gameCanvas" width="600" height="300"></canvas>
        </div>
    </div>

    <div id="p12" class="panel">
        <h1>📩 Ekibimize Katılın</h1>
        <div class="card" style="max-width:600px; margin:auto;">
            <h3>Kaptan Başvurusu</h3>
            <p>Akkuşlar Seyahat ailesinin bir parçası olmak için TikTok üzerinden bize ulaşın.</p>
            <p style="color:var(--gold);">Şartlar: Saygı, Sadakat ve Aktiflik.</p>
            <a href="https://www.tiktok.com/@kelebekmiisaliii" target="_blank" class="action-btn" style="text-decoration:none; display:block;">TIKTOK'TAN MESAJ AT</a>
        </div>
    </div>

    <div id="p13" class="panel">
        <h1>⚙️ YÖNETİM MERKEZİ & SOHBET</h1>
        <div class="grid">
            <div class="card" id="kurucu-ozel" style="display:none; border:2px solid var(--gold);">
                <h3>👑 Kurucu: Afiş Yönetimi</h3>
                <input type="text" id="afis-link" placeholder="Resim URL yapıştır...">
                <button class="action-btn" onclick="afisSet()" style="background:var(--gold); color:#000;">ANA RESMİ GÜNCELLE</button>
                <hr style="border:1px solid #333; margin:15px 0;">
                <input type="text" id="uye-ad" placeholder="Üye Adı...">
                <button class="action-btn" onclick="uyeEkle()">ÜYEYİ KAYDET</button>
            </div>
            <div class="card">
                <h3>💬 Admin Sohbet Odası</h3>
                <div id="chat-box" class="chat-area"></div>
                <input type="text" id="chat-input" placeholder="Mesajınız...">
                <button class="action-btn" onclick="mesajGonder()">GÖNDER</button>
            </div>
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
    let aktifRol = "uye";

    function yonetimGiris() {
        let s = prompt("Yetkili Şifresi:");
        if(s === "1907akkus") {
            aktifRol = "kurucu";
            document.getElementById('app-body').classList.add('kurucu-modu');
            document.getElementById('kurucu-ozel').style.display = "block";
            sayfa(13);
        } else if(s === "akkus123") {
            aktifRol = "admin";
            sayfa(13);
        }
    }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active-btn'));
        document.getElementById('p' + n).classList.add('aktif');
        if(btn) btn.classList.add('active-btn');
        if(n === 10) initGame();
    }

    // ANA SAYFA AFİŞ SİSTEMİ
    db.collection("ayarlar").doc("afis").onSnapshot(d => {
        if(d.exists && d.data().url) {
            const img = document.getElementById('main-afis');
            img.src = d.data().url; img.style.display = "block";
        }
    });

    function afisSet() {
        if(aktifRol === "kurucu") db.collection("ayarlar").doc("afis").set({url: document.getElementById('afis-link').value});
    }

    // SOHBET SİSTEMİ
    db.collection("sohbet").orderBy("tarih", "desc").limit(30).onSnapshot(s => {
        document.getElementById('chat-box').innerHTML = s.docs.map(d => `
            <div class="chat-msg"><span class="chat-name">${d.data().kim}:</span> ${d.data().msg}</div>
        `).reverse().join('');
    });

    function mesajGonder() {
        let m = document.getElementById('chat-input').value;
        if(m) {
            db.collection("sohbet").add({ kim: aktifRol, msg: m, tarih: Date.now() });
            document.getElementById('chat-input').value = "";
        }
    }

    // MİNİ OYUN (TIR SÜRME)
    function initGame() {
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        let tx = 50, ty = 130;
        function ciz() {
            ctx.clearRect(0,0,600,300);
            ctx.fillStyle = "#CC0000"; ctx.fillRect(tx, ty, 60, 30); // Çekici
            ctx.fillStyle = "#fff"; ctx.font = "10px Arial"; ctx.fillText("AKKUŞLAR", tx+5, ty+18);
        }
        document.onkeydown = (e) => {
            if(e.key === "ArrowRight") tx += 10;
            if(e.key === "ArrowLeft") tx -= 10;
            if(e.key === "ArrowUp") ty -= 10;
            if(e.key === "ArrowDown") ty += 10;
            ciz();
        };
        ciz();
    }

    db.collection("ekip").onSnapshot(s => { document.getElementById('u-count').innerText = s.size; });
    function uyeEkle() { if(aktifRol === "kurucu") db.collection("ekip").add({ad: document.getElementById('uye-ad').value, rol: 'uye'}); }
</script>
</body>
</html>
