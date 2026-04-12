<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>AKKUŞLAR SEYAHAT | KURUCU KELEBEK V27</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;700&family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #0A0A0A; --card: #141414; --p: #CC0000; --kurucu: #FFD700; --admin: #CC0000; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: 'DM Sans', sans-serif; display: flex; height: 100vh; overflow: hidden; }
        
        /* 📜 SOL MENÜ - KAYDIRILABİLİR YAPILDI */
        .sidebar { width: 300px; background: #000; border-right: 3px solid var(--p); display: flex; flex-direction: column; padding: 20px; flex-shrink: 0; height: 100vh; overflow-y: auto; scrollbar-width: thin; scrollbar-color: var(--p) #000; }
        .sidebar::-webkit-scrollbar { width: 5px; }
        .sidebar::-webkit-scrollbar-thumb { background: var(--p); border-radius: 10px; }

        .logo { color: var(--p); text-align: center; font-size: 24px; font-weight: 900; font-family: 'Space Grotesk'; text-transform: uppercase; margin-bottom: 5px; }
        .kurucu-imza { color: var(--kurucu); text-align: center; font-size: 13px; font-weight: bold; margin-bottom: 20px; letter-spacing: 1px; }
        
        .nav-label { color: #555; font-size: 10px; margin: 15px 0 5px; font-weight: bold; text-transform: uppercase; border-bottom: 1px solid #222; padding-bottom: 3px; }
        .nav-btn { background: #111; border: 1px solid #222; color: #BBB; padding: 12px; margin-bottom: 6px; cursor: pointer; text-align: left; font-size: 13px; border-radius: 8px; width: 100%; border: none; transition: 0.3s; font-family: 'Space Grotesk'; display: flex; align-items: center; gap: 10px; }
        .nav-btn:hover { background: #1a1a1a; color: white; border-left: 5px solid var(--p); }
        .active-btn { background: var(--p) !important; color: white !important; box-shadow: 0 0 10px rgba(204, 0, 0, 0.3); }

        .main { flex: 1; padding: 40px; overflow-y: auto; background-image: radial-gradient(circle at center, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif { display: block !important; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 20px; margin-top: 20px; }
        .card { background: var(--card); padding: 18px; border-radius: 15px; border: 1px solid #333; text-align: center; }
        .card img { width: 100%; height: 150px; object-fit: cover; border-radius: 10px; margin-bottom: 12px; }
        input, select { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: white; border-radius: 8px; margin-top: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 15px; border-radius: 8px; cursor: pointer; font-weight: bold; width: 100%; margin-top: 15px; }
        .del-btn { background: #900; color: white; border: none; padding: 6px 12px; border-radius: 5px; cursor: pointer; margin-top: 10px; font-size: 11px; display: none; }
        .kurucu-modu .del-btn { display: inline-block !important; }
        #status-box { background: #111; padding: 15px; border-radius: 10px; margin-top: 20px; text-align: center; border: 1px solid #222; }
    </style>
</head>
<body id="main-body">

<div class="sidebar">
    <div class="logo">AKKUŞLAR SEYAHAT</div>
    <div class="kurucu-imza">Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</div>
    
    <div class="nav-label">ANA MENÜ</div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Terminal</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Ekip Kadrosu</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🎫 Bilet Sistemi</button>
    <button class="nav-btn" onclick="sayfa(4, this)">🛣️ Konvoy Takvimi</button>

    <div class="nav-label">ATÖLYE & MEDYA</div>
    <button class="nav-btn" onclick="sayfa(5, this)">🎞️ Galeri & Resimler</button>
    <button class="nav-btn" onclick="sayfa(6, this)">🛠️ Modlar & Kaplamalar</button>
    <button class="nav-btn" onclick="sayfa(7, this)">🎵 Müzik Listesi</button>

    <div class="nav-label">İLETİŞİM</div>
    <button class="nav-btn" onclick="sayfa(8, this)">📞 Sosyal Medya</button>
    
    <div class="nav-label">YÖNETİM</div>
    <button class="nav-btn" style="color:var(--p); border: 1px dashed var(--p);" onclick="yonetimKontrol()">⚙️ YÖNETİM PANELİ</button>
    
    <div id="status-box">
        <div style="font-size: 9px; color: #555;">YETKİ DURUMU</div>
        <div id="status-text" style="font-weight: bold; color: #888; font-size: 12px;">Üye</div>
    </div>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <h1>AKKUŞLAR SEYAHAT</h1>
        <h3 style="color:var(--kurucu)">Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</h3>
        <div class="grid">
            <div class="card"><h3>Kaptanlar</h3><p id="c-ekip" style="font-size:24px;">0</p></div>
            <div class="card"><h3>İçerikler</h3><p id="c-mod" style="font-size:24px;">0</p></div>
        </div>
    </div>

    <div id="p2" class="panel"><h1>👥 Ekip Kadrosu</h1><div id="list-ekip" class="grid"></div></div>
    <div id="p3" class="panel"><h1>🎫 Bilet Sistemi</h1><div class="card"><input type="text" id="b-ad" placeholder="Yolcu Adı..."><button class="action-btn" onclick="biletKes()">BİLETİ İŞLE</button></div><div id="list-bilet" class="grid"></div></div>
    <div id="p4" class="panel"><h1>🛣️ Konvoy Takvimi</h1><div class="card"><h3>Bartın - Denizli Seferi</h3><p>Cumartesi 21:00</p></div></div>
    <div id="p5" class="panel"><h1>🎞️ Galeri</h1><div id="list-galeri" class="grid"></div></div>
    <div id="p6" class="panel"><h1>🛠️ Modlar & Kaplamalar</h1><div id="list-mod" class="grid"></div></div>
    <div id="p7" class="panel"><h1>🎵 Müzik Listesi</h1><div id="list-muzik" class="grid"></div></div>
    <div id="p8" class="panel">
        <h1>📞 Sosyal Medya</h1>
        <div class="card" style="text-align: left;">
            <a href="https://www.tiktok.com/@kelebekmiisaliii" target="_blank" style="color:white; display:block; margin:10px 0;">🎵 TikTok: @kelebekmiisaliii</a>
            <a href="https://www.tiktok.com/@akkusailesi20" target="_blank" style="color:white; display:block; margin:10px 0;">🎵 TikTok: @akkusailesi20</a>
        </div>
    </div>

    <div id="p9" class="panel">
        <h1 id="panel-title">⚙️ YÖNETİM</h1>
        <div class="grid">
            <div class="card" id="rol-box" style="display:none; border: 2px solid var(--kurucu);">
                <h3>👑 Rol Yönetimi (Kurucu)</h3>
                <input type="text" id="adm-ad" placeholder="Kaptan Nick...">
                <select id="adm-rol"><option value="uye">Üye</option><option value="yonetici">Yönetici</option><option value="admin">Admin</option></select>
                <button class="action-btn" onclick="ekipEkle()">ROLÜ VER</button>
            </div>
            <div class="card">
                <h3>📦 İçerik Ekle (Admin+)</h3>
                <select id="i-tip"><option value="galeri">Resim</option><option value="mod">Mod</option><option value="muzik">Müzik</option></select>
                <input type="text" id="i-ad" placeholder="Başlık...">
                <input type="text" id="i-url" placeholder="Link...">
                <button class="action-btn" onclick="icerikEkle()">YAYINLA</button>
            </div>
        </div>
        <button class="action-btn" style="background:#333;" onclick="cikisYap()">OTURUMU KAPAT</button>
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
                document.getElementById('rol-box').style.display = "none";
                sayfa(9, document.querySelector('button[onclick="yonetimKontrol()"]'));
            }
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
        document.getElementById('c-ekip').innerText = s.size;
        document.getElementById('list-ekip').innerHTML = s.docs.map(d => `<div class="card"><b>${d.data().ad}</b><br><span class="badge" style="background:var(--${d.data().rol}); color:white;">${d.data().rol}</span><br><button class="del-btn" onclick="sil('ekip','${d.id}')">SİL</button></div>`).join('');
    });

    db.collection("icerik").onSnapshot(s => {
        const data = s.docs.map(d => ({id: d.id, ...d.data()}));
        document.getElementById('c-mod').innerText = data.length;
        const r = (t, id) => { document.getElementById(id).innerHTML = data.filter(i => i.tip === t).map(i => `<div class="card">${i.url.includes('http') ? `<img src="${i.url}">` : ''} <b>${i.ad}</b><br><button class="del-btn" onclick="sil('icerik','${i.id}')">SİL</button></div>`).join(''); };
        r('galeri', 'list-galeri'); r('mod', 'list-mod'); r('muzik', 'list-muzik');
    });

    db.collection("biletler").onSnapshot(s => {
        document.getElementById('list-bilet').innerHTML = s.docs.map(d => `<div class="card">🎫 Bilet: ${d.data().ad} <br><button class="del-btn" onclick="sil('biletler','${d.id}')">SİL</button></div>`).join('');
    });

    function ekipEkle() { if(yetki === "kurucu") db.collection("ekip").add({ad: document.getElementById('adm-ad').value, rol: document.getElementById('adm-rol').value}); }
    function icerikEkle() { if(yetki !== "uye") db.collection("icerik").add({tip: document.getElementById('i-tip').value, ad: document.getElementById('i-ad').value, url: document.getElementById('i-url').value}); }
    function biletKes() { if(document.getElementById('b-ad').value) db.collection("biletler").add({ad: document.getElementById('b-ad').value}); }
    function sil(c, i) { if(yetki === "kurucu" && confirm("Emin misin?")) db.collection(c).doc(i).delete(); }
</script>
</body>
</html>


