<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | RESMİ TERMİNAL</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;700&family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; }
        * { box-sizing: border-box; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: 'DM Sans', sans-serif; display: flex; height: 100vh; overflow: hidden; }
        .sidebar { width: 280px; background: #000; border-right: 3px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .logo-area { text-align: center; margin-bottom: 25px; border-bottom: 1px solid #222; padding-bottom: 15px; }
        .logo-text { color: var(--p); font-size: 22px; font-weight: 900; font-family: 'Space Grotesk'; text-transform: uppercase; margin: 0; }
        .kurucu-text { color: var(--gold); font-size: 13px; font-weight: bold; margin-top: 5px; display: block; }
        .nav-label { color: #444; font-size: 10px; font-weight: bold; text-transform: uppercase; margin: 15px 0 8px; letter-spacing: 1px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 6px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; transition: 0.3s; font-family: 'Space Grotesk'; font-size: 13px; }
        .nav-btn:hover { background: #1a1a1a; color: #fff; border-left: 4px solid var(--p); }
        .active-btn { background: var(--p) !important; color: white !important; }
        .main { flex: 1; padding: 35px; overflow-y: auto; background: radial-gradient(circle at top right, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif { display: block !important; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 20px; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #222; text-align: center; transition: 0.3s; }
        .card img { width: 100%; height: 160px; object-fit: cover; border-radius: 10px; margin-bottom: 15px; }
        input, select, textarea { width: 100%; padding: 12px; background: #050505; border: 1px solid #333; color: white; border-radius: 8px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 14px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; text-transform: uppercase; }
        .del-btn { background: #600; color: white; border: none; padding: 6px 12px; border-radius: 6px; cursor: pointer; font-size: 11px; margin-top: 10px; display: none; }
        .kurucu-modu .del-btn { display: inline-block !important; }
        .badge { display: inline-block; padding: 4px 10px; border-radius: 5px; font-size: 10px; font-weight: bold; text-transform: uppercase; margin-bottom: 10px; }
        .badge-kurucu { background: var(--gold); color: #000; }
        .badge-admin { background: var(--p); color: #fff; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h1 class="logo-text">AKKUŞLAR SEYAHAT</h1>
        <span class="kurucu-text">Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</span>
    </div>
    <div class="nav-label">TERMİNAL</div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Terminal</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Ekip Kadrosu</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🎫 Bilet İşlemleri</button>
    <button class="nav-btn" onclick="sayfa(4, this)">🛣️ Konvoy Saatleri</button>
    <div class="nav-label">ARŞİV & MEDYA</div>
    <button class="nav-btn" onclick="sayfa(5, this)">🎞️ Galeri (Resimler)</button>
    <button class="nav-btn" onclick="sayfa(6, this)">🎥 Video Merkezi</button>
    <button class="nav-btn" onclick="sayfa(7, this)">🛠️ Modlar & Kaplamalar</button>
    <button class="nav-btn" onclick="sayfa(8, this)">🎵 Müzik Listesi</button>
    <button class="nav-btn" onclick="sayfa(10, this)">🖼️ Arka Planlar</button>
    <div class="nav-label">SİSTEM</div>
    <button class="nav-btn" onclick="sayfa(11, this)">📞 İletişim & TikTok</button>
    <button class="nav-btn" onclick="sayfa(12, this)">📩 Ekibe Katıl</button>
    <button class="nav-btn" style="color:var(--p); border: 1px dashed var(--p);" onclick="yonetimGiris()">⚙️ YÖNETİM PANELİ</button>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <h1>AKKUŞLAR SEYAHAT</h1>
        <p>Hoş geldin Kurucu Kelebek. Terminal kontrolün altında!</p>
        <div class="grid"><div class="card"><h3>9</h3><p>Kaptan</p></div><div class="card"><h3>24/7</h3><p>Aktif</p></div></div>
    </div>
    <div id="p2" class="panel"><h1>👥 Ekip Kadrosu</h1><div id="list-ekip" class="grid"></div></div>
    <div id="p3" class="panel"><h1>🎫 Bilet Sistemi</h1><div class="card"><input type="text" id="b-ad" placeholder="Yolcu Adı..."><button class="action-btn" onclick="biletKes()">BİLETİ KES</button></div><div id="list-bilet" class="grid"></div></div>
    <div id="p4" class="panel"><h1>🛣️ Konvoy</h1><div class="card"><h3>Bartın - Denizli</h3><p>Cumartesi 21:00</p></div></div>
    <div id="p5" class="panel"><h1>🎞️ Galeri</h1><div id="list-galeri" class="grid"></div></div>
    <div id="p6" class="panel"><h1>🎥 Videolar</h1><div id="list-video" class="grid"></div></div>
    <div id="p7" class="panel"><h1>🛠️ Modlar</h1><div id="list-mod" class="grid"></div></div>
    <div id="p8" class="panel"><h1>🎵 Müzikler</h1><div id="list-muzik" class="grid"></div></div>
    <div id="p10" class="panel"><h1>🖼️ Arka Planlar</h1><div id="list-bg" class="grid"></div></div>
    <div id="p11" class="panel"><h1>📞 İletişim</h1><div class="card"><a href="https://www.tiktok.com/@kelebekmiisaliii" style="color:red; text-decoration:none;">TikTok Kelebek</a><br><a href="https://www.tiktok.com/@akkusailesi20" style="color:red; text-decoration:none;">TikTok Akkuş Ailesi</a></div></div>
    <div id="p12" class="panel"><h1>📩 Ekibe Katıl</h1><div class="card"><p>Başvuru için TikTok'tan ulaşın.</p></div></div>

    <div id="p9" class="panel">
        <h1>⚙️ YÖNETİM MERKEZİ</h1>
        <div class="grid">
            <div class="card" id="kurucu-ozel" style="display:none; border:2px solid gold;">
                <span class="badge badge-kurucu">Kurucu Yetkisi</span>
                <h3>Ekip Yönetimi</h3>
                <input type="text" id="ekip-ad" placeholder="İsim...">
                <select id="ekip-rol"><option value="uye">Üye</option><option value="admin">Admin</option></select>
                <button class="action-btn" onclick="ekipEkle()">KAYDET</button>
            </div>
            <div class="card">
                <h3>İçerik Yükle</h3>
                <select id="i-tip"><option value="galeri">Resim</option><option value="video">Video</option><option value="mod">Mod</option><option value="muzik">Müzik</option><option value="bg">Arka Plan</option></select>
                <input type="text" id="i-baslik" placeholder="Başlık...">
                <input type="text" id="i-url" placeholder="URL Link...">
                <button class="action-btn" onclick="icerikEkle()">YAYINLA</button>
            </div>
        </div>
        <button class="action-btn" style="background:#222; margin-top:20px;" onclick="location.reload()">GÜVENLİ ÇIKIŞ</button>
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
    let aktifYetki = "uye";

    function yonetimGiris() {
        let s = prompt("Şifre:");
        if(s === "1907akkus") {
            aktifYetki = "kurucu";
            document.getElementById('app-body').classList.add('kurucu-modu');
            document.getElementById('kurucu-ozel').style.display = "block";
            sayfa(9);
        } else if(s === "akkus123") {
            aktifYetki = "admin";
            sayfa(9);
        } else { alert("Hatalı!"); }
    }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active-btn'));
        document.getElementById('p' + n).classList.add('aktif');
        if(btn) btn.classList.add('active-btn');
    }

    db.collection("ekip").onSnapshot(s => {
        document.getElementById('list-ekip').innerHTML = s.docs.map(d => `<div class="card"><span class="badge badge-admin">${d.data().rol}</span><br><b>${d.data().ad}</b><br><button class="del-btn" onclick="veriSil('ekip','${d.id}')">SİL</button></div>`).join('');
    });

    db.collection("icerik").onSnapshot(s => {
        const data = s.docs.map(d => ({id: d.id, ...d.data()}));
        const render = (tip, id) => {
            const el = document.getElementById(id);
            if(el) el.innerHTML = data.filter(i => i.tip === tip).map(i => `<div class="card">${i.url ? `<img src="${i.url}">` : ''}<br><b>${i.i_baslik}</b><br><button class="del-btn" onclick="veriSil('icerik','${i.id}')">SİL</button></div>`).join('');
        };
        render('galeri', 'list-galeri'); render('video', 'list-video'); render('mod', 'list-mod'); render('muzik', 'list-muzik'); render('bg', 'list-bg');
    });

    db.collection("biletler").onSnapshot(s => {
        document.getElementById('list-bilet').innerHTML = s.docs.map(d => `<div class="card">🎫 ${d.data().ad} <button class="del-btn" onclick="veriSil('biletler','${d.id}')">SİL</button></div>`).join('');
    });

    function ekipEkle() { db.collection("ekip").add({ ad: document.getElementById('ekip-ad').value, rol: document.getElementById('ekip-rol').value }); }
    function icerikEkle() { db.collection("icerik").add({ tip: document.getElementById('i-tip').value, i_baslik: document.getElementById('i-baslik').value, url: document.getElementById('i-url').value }); }
    function biletKes() { if(document.getElementById('b-ad').value) db.collection("biletler").add({ ad: document.getElementById('b-ad').value }); }
    function veriSil(col, id) {
        if(aktifYetki !== "kurucu") return alert("YETKİ YOK!");
        if(confirm("Silinsin mi?")) db.collection(col).doc(id).delete();
    }
</script>
</body>
</html>
