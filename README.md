<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V49</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; }
        * { box-sizing: border-box; transition: 0.2s; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: 'Space Grotesk', sans-serif; display: flex; height: 100vh; overflow: hidden; }
        .sidebar { width: 280px; background: #000; border-right: 3px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 6px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; font-size: 13px; }
        .nav-btn:hover { background: var(--p); color: white; }
        .main { flex: 1; padding: 30px; overflow-y: auto; background: radial-gradient(circle at top right, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif { display: block !important; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 15px; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #333; text-align: center; }
        .card img { width: 100%; height: 150px; object-fit: cover; border-radius: 10px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; text-transform: uppercase; text-decoration: none; display: block; }
        .back-btn { background: #222; color: #fff; border: none; padding: 8px 15px; border-radius: 5px; cursor: pointer; margin-bottom: 20px; font-size: 12px; }
        .chat-container { background: #000; border: 1px solid #333; border-radius: 10px; padding: 15px; margin-top: 20px; }
        .chat-messages { height: 150px; overflow-y: auto; border-bottom: 1px solid #222; margin-bottom: 10px; padding-bottom: 10px; text-align: left; }
        .star-rating { color: var(--gold); font-size: 20px; margin: 10px 0; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div style="text-align:center; border-bottom:2px solid var(--p); padding-bottom:15px; margin-bottom:15px;">
        <h2 style="color:var(--p); margin:0;">AKKUŞLAR</h2>
        <span style="color:var(--gold); font-size:11px;">Hükümdar: 乂✯ҠƐꝈƐβƐҠ✯乂</span>
    </div>
    <button class="nav-btn" onclick="sayfa(1)">🏠 Ana Sayfa</button>
    <button class="nav-btn" onclick="sayfa(2)">👥 Üyeler</button>
    <button class="nav-btn" onclick="sayfa(8)">🛠️ Atölye & Modlar</button>
    <button class="nav-btn" onclick="sayfa(5)">🎞️ Galeri</button>
    <button class="nav-btn" onclick="sayfa(12)">📩 Ekibe Katıl</button>
    <button class="nav-btn" style="color:var(--p); border:1px dashed var(--p); margin-top:20px;" onclick="yonetimGiris()">⚙️ YÖNETİM</button>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <img id="main-afis" style="width:100%; height:300px; border-radius:20px; object-fit:cover; display:none; border:2px solid var(--p); margin-bottom:20px;">
        <h1>🚛 AKKUŞLAR TERMİNALİ</h1>
        
        <div class="grid">
            <div class="card">
                <h3>⭐ Terminal Puanı</h3>
                <div class="star-rating" id="stars-view">⭐⭐⭐⭐⭐</div>
                <button class="back-btn" onclick="puanVer()">PUAN VER</button>
            </div>
            <div class="card">
                <h3>👥 Üye Sohbeti</h3>
                <div id="u-chat" class="chat-messages"></div>
                <input type="text" id="u-msg" placeholder="Mesajınız..." style="width:70%; padding:5px; background:#000; color:#fff; border:1px solid #333;">
                <button onclick="uMesajGonder()" style="background:var(--p); border:none; color:#fff; padding:5px; cursor:pointer;">GÖNDER</button>
            </div>
        </div>
    </div>

    <div id="p2" class="panel"><button class="back-btn" onclick="sayfa(1)">⬅ GERİ DÖN</button><h1>👥 Ekip Kadrosu</h1><div id="list-ekip" class="grid"></div></div>

    <div id="p8" class="panel">
        <button class="back-btn" onclick="sayfa(1)">⬅ GERİ DÖN</button>
        <h1>🛠️ Modlar & Kaplamalar</h1>
        <div id="list-icerik" class="grid"></div>
    </div>

    <div id="p5" class="panel"><button class="back-btn" onclick="sayfa(1)">⬅ GERİ DÖN</button><h1>🎞️ Galeri</h1><div id="list-galeri" class="grid"></div></div>

    <div id="p13" class="panel">
        <h1>⚙️ YÖNETİM</h1>
        <div class="grid">
            <div id="k-panel" class="card" style="display:none; border:2px solid var(--gold);">
                <h3>👑 Kurucu Paneli</h3>
                <input type="text" id="af-url" placeholder="Afiş Linki...">
                <button class="action-btn" onclick="afisSet()">AFİŞİ BAS</button>
                <hr>
                <input type="text" id="r-ad" placeholder="Üye Adı...">
                <button class="action-btn" onclick="uyeEkle()">KAYDET</button>
            </div>
            <div class="card">
                <h3>🛠️ İçerik Yükle</h3>
                <select id="i-tip"><option value="mod">Mod/Kaplama</option><option value="galeri">Resim</option></select>
                <input type="text" id="i-bas" placeholder="Başlık...">
                <input type="text" id="i-url" placeholder="Görsel URL...">
                <input type="text" id="i-indir" placeholder="İndirme Linki (Opsiyonel)...">
                <button class="action-btn" onclick="icerikEkle()">YAYINLA</button>
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
    let rol = "uye";

    function yonetimGiris() {
        let s = prompt("Şifre:");
        if(s === "1907akkus") { rol="kurucu"; document.getElementById('k-panel').style.display="block"; sayfa(13); }
        else if(s === "admin123") { rol="admin"; sayfa(13); }
    }

    function sayfa(n) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.getElementById('p' + n).classList.add('aktif');
    }

    // ÜYE SOHBETİ
    db.collection("u-sohbet").orderBy("tarih","desc").limit(20).onSnapshot(s => {
        document.getElementById('u-chat').innerHTML = s.docs.map(d => `<div style="
