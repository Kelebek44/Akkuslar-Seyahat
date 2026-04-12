<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V40</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; }
        * { box-sizing: border-box; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: 'Space Grotesk', sans-serif; display: flex; height: 100vh; overflow: hidden; }
        .sidebar { width: 280px; background: #000; border-right: 3px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 10px; margin-bottom: 15px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 5px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; }
        .nav-btn:hover { background: var(--p); color: white; }
        .active-btn { background: var(--p) !important; color: white !important; }
        .main { flex: 1; padding: 30px; overflow-y: auto; background: radial-gradient(circle at top right, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif { display: block !important; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(230px, 1fr)); gap: 15px; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #333; text-align: center; }
        canvas { background: #111; border: 2px solid var(--p); display: block; margin: 0 auto; max-width: 100%; border-radius: 10px; }
        input, select { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; text-transform: uppercase; }
        .del-btn { background: #600; color: white; border: none; padding: 5px 10px; border-radius: 5px; cursor: pointer; display: none; margin-top: 10px; font-size: 11px; }
        .kurucu-modu .del-btn { display: inline-block !important; }
        .bilet-list { max-height: 200px; overflow-y: auto; background: #000; padding: 10px; border-radius: 10px; border: 1px solid #222; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h2 style="color:var(--p); margin:0;">AKKUŞLAR</h2>
        <span style="color:var(--gold); font-size:12px;">Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</span>
    </div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Sayfa</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Üyeler</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🎫 Bilet Sistemi</button>
    <button class="nav-btn" onclick="sayfa(10, this)">🎮 Minyatür Tır</button>
    <button class="nav-btn" onclick="sayfa(12, this)">📩 Ekibe Katıl</button>
    <div style="color:#444; font-size:10px; padding:10px 0;">ATÖLYE</div>
    <button class="nav-btn" onclick="sayfa(5, this)">🎞️ Galeri</button>
    <button class="nav-btn" onclick="sayfa(7, this)">🛠️ Modlar</button>
    <button class="nav-btn" onclick="sayfa(8, this)">🎨 Kaplamalar</button>
    <button class="nav-btn" style="color:var(--p); border:1px dashed var(--p); margin-top:20px;" onclick="yonetimGiris()">⚙️ YÖNETİM & ROL</button>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <img id="main-afis" style="width:100%; height:300px; object-fit:cover; border-radius:20px; border:3px solid var(--p); display:none; margin-bottom:20px;">
        <h1>🚛 AKKUŞLAR SEYAHAT</h1>
        <p>Terminal v40 Sürümü Yayında!</p>
    </div>

    <div id="p2" class="panel"><h1>👥 Kayıtlı Üyeler</h1><div id="list-ekip" class="grid"></div></div>

    <div id="p3" class="panel">
        <h1>🎫 Sanal Bilet Gişesi</h1>
        <div class="grid">
            <div class="card">
                <h3>Bilet Kes</h3>
                <input type="text" id="b-ad" placeholder="Yolcu Adı Soyadı...">
                <select id="b-rota">
                    <option value="Bartın - Denizli">Bartın - Denizli</option>
                    <option value="İstanbul - Hatay">İstanbul - Hatay</option>
                    <option value="Ankara - Denizli">Ankara - Denizli</option>
                </select>
                <button class="action-btn" onclick="biletKes()">BİLETİ ONAYLA</button>
            </div>
            <div class="card">
                <h3>Yolcu Listesi</h3>
                <div id="list-bilet" class="bilet-list"></div>
            </div>
        </div>
    </div>

    <div id="p10" class="panel">
        <h1>🎮 Minyatür Tır Yolu</h1>
        <p>Ok tuşlarıyla Akkuşlar tırını şeritte tut!</p>
        <canvas id="gameCanvas" width="600" height="400"></canvas>
    </div>

    <div id="p12" class="panel">
        <h1>📩 Ekibe Katılım Başvurusu</h1>
        <div class="card" style="max-width:500px; margin:auto;">
            <input type="text" id="kat-ad" placeholder="Oyun Adınız (Nick)...">
            <input type="text" id="kat-tiktok" placeholder="TikTok Kullanıcı Adınız...">
            <button class="action-btn" onclick="basvuruGonder()">BAŞVURUYU TAMAMLA</button>
            <p style="font-size:12px; margin-top:15px; color:#888;">Başvurunuz Kurucu panelinde görünecektir.</p>
        </div>
    </div>

    <div id="p13" class="panel">
        <h1>⚙️ YÖNETİM & ROL VERME</h1>
        <div class="grid">
            <div id="kurucu-panel" class="card" style="display:none; border:2px solid var(--gold);">
                <span style="color:var(--gold); font-weight:bold;">👑 Kurucu Özel</span>
                <hr>
                <h3>Admin & Rol Ata</h3>
                <input type="text" id="r-ad" placeholder="Kaptan Adı...">
                <select id="r-rol"><option value="uye">Üye</option><option value="admin">Yönetici</option></select>
                <button class="action-btn" onclick="rolVer()">ROLU ONAYLA</button>
                <hr>
                <h3>Afiş Güncelle</h3>
                <input type="text" id="af-url" placeholder="Resim Linki...">
                <button class="action-btn" onclick="afisDegis()" style="background:var(--gold); color:#000;">AFİŞİ BAS</button>
            </div>
            <div class="card">
                <h3>📢 Duyuru & İçerik</h3>
                <select id="i-tip">
                    <option value="galeri">Galeri Resmi</option>
                    <option value="mod">ETS2 Modu</option>
                    <option value="kaplama">Tır Kaplaması</option>
                </select>
                <input type="text" id="i-bas" placeholder="Başlık...">
                <input type="text" id="i-link" placeholder="URL...">
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
    const db
