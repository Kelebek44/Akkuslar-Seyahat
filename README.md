<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | V28 FINAL</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;700&family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #0A0A0A; --card: #141414; --p: #CC0000; --kurucu: #FFD700; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: 'DM Sans', sans-serif; display: flex; height: 100vh; overflow: hidden; }
        
        /* 📜 SOL MENÜ - TAM LİSTE GARANTİLİ */
        .sidebar { 
            width: 260px; background: #000; border-right: 3px solid var(--p); 
            display: flex; flex-direction: column; padding: 15px; 
            height: 100vh; overflow-y: auto; overflow-x: hidden;
            scrollbar-width: thin; scrollbar-color: var(--p) #000;
        }
        .sidebar::-webkit-scrollbar { width: 4px; }
        .sidebar::-webkit-scrollbar-thumb { background: var(--p); }

        .logo { color: var(--p); text-align: center; font-size: 20px; font-weight: 900; font-family: 'Space Grotesk'; text-transform: uppercase; margin-bottom: 5px; }
        .kurucu-imza { color: var(--kurucu); text-align: center; font-size: 11px; font-weight: bold; margin-bottom: 15px; letter-spacing: 1px; }
        
        .nav-label { color: #555; font-size: 9px; margin: 12px 0 5px; font-weight: bold; text-transform: uppercase; border-bottom: 1px solid #222; }
        .nav-btn { 
            background: #111; border: none; color: #BBB; padding: 10px; margin-bottom: 5px; 
            cursor: pointer; text-align: left; font-size: 12px; border-radius: 6px; 
            width: 100%; transition: 0.2s; font-family: 'Space Grotesk';
            display: block; /* Görünürlüğü garanti eder */
        }
        .nav-btn:hover { background: #222; color: white; border-left: 4px solid var(--p); }
        .active-btn { background: var(--p) !important; color: white !important; }

        .main { flex: 1; padding: 30px; overflow-y: auto; background: radial-gradient(circle at center, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif { display: block !important; }

        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); gap: 15px; }
        .card { background: var(--card); padding: 15px; border-radius: 12px; border: 1px solid #333; text-align: center; }
        .card img { width: 100%; height: 130px; object-fit: cover; border-radius: 8px; margin-bottom: 10px; }
        
        .action-btn { background: var(--p); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; font-weight: bold; width: 100%; margin-top: 10px; }
        .del-btn { background: #900; color: white; border: none; padding: 5px 10px; border-radius: 4px; cursor: pointer; display: none; margin-top: 8px; font-size: 10px; }
        .kurucu-modu .del-btn { display: inline-block !important; }
        
        #status-box { background: #111; padding: 10px; border-radius: 8px; margin-top: 15px; text-align: center; border: 1px solid #222; font-size: 11px; }
    </style>
</head>
<body id="main-body">

<div class="sidebar">
    <div class="logo">AKKUŞLAR SEYAHAT</div>
    <div class="kurucu-imza">Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</div>
    
    <div class="nav-label">ANA LİSTE</div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Sayfa</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Ekip Üyeleri</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🎫 Bilet Sistemi</button>
    <button class="nav-btn" onclick="sayfa(4, this)">🛣️ Konvoy Planı</button>

    <div class="nav-label">ARŞİV & ATÖLYE</div>
    <button class="nav-btn" onclick="sayfa(5, this)">🎞️ Galeri & Resimler</button>
    <button class="nav-btn" onclick="sayfa(6, this)">🛠️ Modlar & Kaplamalar</button>
    <button class="nav-btn" onclick="sayfa(7, this)">🎵 Müzik Listesi</button>
    <button class="nav-btn" onclick="sayfa(8, this)">🎥 Video Galerisi</button>
    <button class="nav-btn" onclick="sayfa(10, this)">🖼️ Arka Planlar</button>

    <div class="nav-label">YÖNETİM & SOSYAL</div>
    <button class="nav-btn" onclick="sayfa(11, this)">📞 Sosyal Medya</button>
    <button class="nav-btn" onclick="sayfa(12, this)">📩 Ekibe Katıl</button>
    <button class="nav-btn" style="color:var(--p); border: 1px dashed var(--p); font-weight: bold;" onclick="yonetimKontrol()">⚙️ YÖNETİM PANELİ</button>
    
    <div id="status-box">
        <div style="color: #555;">YETKİ DURUMU</div>
        <div id="status-text" style="font-weight: bold; color: #888;">Standart Üye</div>
    </div>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <h1>AKKUŞLAR SEYAHAT</h1>
        <h3 style="color:var(--kurucu)">Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</h3>
        <p>ETS2 dünyasının en büyük filosuna hoş geldiniz kaptanlar!</p>
    </div>

    <div id="p2" class="panel"><h1>👥 Ekip Kadrosu</h1><div id="list-ekip" class="grid"></div></div>
    <div id="p3" class="panel"><h1>🎫 Bilet Sistemi</h1><div class="card"><input type="text" id="b-ad" placeholder="Ad Soyad..."><button class="action-btn" onclick="biletKes()">BİLETİ KES</button></div><div id="list-bilet" class="grid"></div></div>
    <div id="p4" class="panel"><h1>🛣️ Konvoy Planı</h1><div class="card"><h3>Bartın - Denizli</h3><p>Cumartesi 21:00</p></div></div>
    <div id="p5" class="panel"><h1>🎞️ Galeri</h1><div id="list-galeri" class="grid"></div></div>
    <div id="p6" class="panel"><h1>🛠️ Modlar</h1><div id="list-mod" class="grid"></div></div>
    <div id="p7" class="panel"><h1>🎵 Müzikler</h1><div id="list-muzik" class="grid"></div></div>
    <div id="p8" class="panel"><h1>🎥 Videolar</h1><div id="list-video" class="grid"></div></div>
    <div id="p10" class="panel"><h1>🖼️ Arka Planlar</h1><div id="list-bg" class="grid"></div></div>
    <div id="p11" class="panel">
        <h1>📞 Sosyal Medya</h1>
        <div class="card">
            <a href="https://www.tiktok.com/@kelebekmiisaliii" target="_blank" style="color:white;text-decoration:none;display:block;margin:10px;">TikTok @kelebekmiisaliii</a>
            <a href="https://www.tiktok.com/@akkusailesi20" target="_blank" style="color:white;text-decoration:none;display:block;margin:10px;">TikTok @akkusailesi20</a>
        </div>
    </div>
    <div id="p12" class="panel"><h1>📩 Ekibe Katılım</h1><div class="card"><p>Başvuru için TikTok üzerinden iletişime geçiniz.</p></div></div>

    <div id="p9" class="panel">
        <h1 id="panel-title">⚙️ YÖNETİM PANELİ</h1>
        <div class="grid">
            <div class="card" id="rol-box" style="display:none; border: 2px solid var(--kurucu);">
                <h3>👑 Rol Yönetimi</h3>
                <input type="text" id="adm-ad" placeholder="Kaptan Nick...">
                <select id="adm-rol"><option value="uye">Üye</option><option value="yonetici">Yönetici</option><option value="admin">Admin</option></select>
                <button class="action-btn" onclick="ekipEkle()">ROL VER</button>
            </div>
            <div class="card">
                <h3>📦 İçerik Ekle</h3>
                <select id="i-tip"><option value="galeri">Resim</option><option value="mod">Mod</option><option value="muzik">Müzik</option><option value="video">Video</option><option value="bg">Arka Plan</option></select>
                <input type="text" id="i-ad" placeholder="Başlık...">
                <input type="text" id="i-url" placeholder="URL Linki...">
                <button class="action-btn" onclick="icerikEkle()">YAYINLA</button>
            </div>
        </div>
        <button class="action-btn" style="background:#333;" onclick="cikisYap()">ÇIKIŞ YAP</button>
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
    
    let yetki = "uye"; 
    const adminSifre = "akkus123"; 
    const kurucuSifre = "1907akkus"; 

    function yonetimKontrol() {
        if(yetki !== "uye") { sayfa(9, document.querySelector('button[onclick="yonetimKontrol()"]')); }
        else {
            let s = prompt("Şifre:");
            if(s === kurucuSifre) {
                yetki = "kurucu";
                document.getElementById('main-body').classList.add('kurucu-modu');
                document.getElementById('status-text').innerText = "KURUCU KELEBEK";
                document.getElementById('status-text').style.color = "gold";
                document.getElementById('rol-box').style.display = "block";
                sayfa(9, document.querySelector('button[onclick="yonetimKontrol()"]'));
            } else if(s === adminSifre) {
                yetki = "admin";
                document.getElementById('status-text').innerText = "Yönetici";
                document.getElementById('status-text').style.color = "red";
                sayfa(9, document.querySelector('button[onclick="yonetimKontrol()"]'));
            } else { alert("Yanlış Şifre!"); }
        }
    }

    function cikisYap() { location.reload(); }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active-btn'));
        document.getElementById('p' + n).classList.add('aktif');
        btn.classList.add('active-btn');
    }

    db.collection("ekip").onSnapshot(s => {
        document.getElementById('list-ekip').innerHTML = s.docs.map(d => `<div class="card"><b>${d.data().ad}</b><br><span style="color:var(--${d.data().rol})">${d.data().rol}</span><br><button class="del-btn" onclick="sil('ekip','${d.id}')">SİL</button></div>`).join('');
    });

    db.collection("icerik").onSnapshot(s => {
        const data = s.docs.map(d => ({id: d.id, ...d.data()}));
