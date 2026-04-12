<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V48</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; --admin: #00CCFF; }
        * { box-sizing: border-box; transition: 0.2s; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: 'Space Grotesk', sans-serif; display: flex; height: 100vh; overflow: hidden; }
        
        /* SIDEBAR */
        .sidebar { width: 280px; background: #000; border-right: 3px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 15px; margin-bottom: 20px; }
        .nav-label { color: #444; font-size: 10px; font-weight: bold; text-transform: uppercase; margin: 15px 0 5px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 6px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; font-size: 13px; display: flex; align-items: center; gap: 10px; }
        .nav-btn:hover { background: var(--p); color: white; }
        .active-btn { background: var(--p) !important; color: white !important; }

        /* ANA EKRAN */
        .main { flex: 1; padding: 30px; overflow-y: auto; background: radial-gradient(circle at top right, #1a0000, #0A0A0A); }
        .panel { display: none; }
        .aktif { display: block !important; animation: fadeIn 0.4s; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* AFİŞ: EKRANI KAPLAYAN */
        .hero-banner { width: 100%; height: 550px; object-fit: cover; border-radius: 20px; border: 4px solid var(--p); margin-bottom: 20px; }
        
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 15px; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #333; text-align: center; position: relative; }
        .card img { width: 100%; height: 150px; object-fit: cover; border-radius: 10px; margin-bottom: 10px; }

        /* ROZETLER */
        .badge { padding: 3px 8px; border-radius: 4px; font-size: 10px; font-weight: bold; text-transform: uppercase; margin-bottom: 5px; display: inline-block; }
        .badge-kurucu { background: var(--gold); color: #000; }
        .badge-admin { background: var(--admin); color: #000; }
        .badge-yonetici { background: #AA00FF; color: #fff; }

        /* SİLME (TEK KURUCU) */
        .del-btn { background: #600; color: white; border: none; padding: 5px 10px; border-radius: 5px; cursor: pointer; display: none; margin-top: 10px; font-size: 11px; }
        .kurucu-modu .del-btn { display: inline-block !important; }

        input, select { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; text-transform: uppercase; }
        canvas { background: #111; border: 2px solid var(--p); display: block; margin: 0 auto; border-radius: 15px; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h2 style="color:var(--p); margin:0;">AKKUŞLAR</h2>
        <span style="color:var(--gold); font-size:12px;">👑 Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</span>
    </div>
    <div class="nav-label">TERMİNAL</div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Sayfa</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👤 Ekip Kadrosu</button>
    <button class="nav-btn" onclick="sayfa(15, this)">📜 Ekip Kuralları</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🎫 Bilet Gişesi</button>
    <button class="nav-btn" onclick="sayfa(10, this)">🎮 Akkuşlar Oyun</button>
    <button class="nav-btn" onclick="sayfa(8, this)">🎨 Kaplama & Mod</button>
    <button class="nav-btn" onclick="sayfa(12, this)">✉️ Ekibe Katıl</button>
    <button class="nav-btn" style="color:var(--p); border:1px dashed var(--p); margin-top:20px;" onclick="yonetimGiris()">⚙️ YÖNETİM PANELİ</button>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <img id="main-afis" class="hero-banner" style="display:none;">
        <h1 style="font-size: 45px; color: var(--p); margin: 0;">AKKUŞLAR SEYAHAT</h1>
        <p style="color:var(--gold);">Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</p>
    </div>

    <div id="p2" class="panel"><h1>👥 Ekip Kadrosu</h1><div id="list-ekip" class="grid"></div></div>

    <div id="p15" class="panel">
        <h1>📜 Ekip Kuralları</h1>
        <div style="background:#111; padding:30px; border-radius:15px; border-left:5px solid var(--p); line-height:2;">
            1. Saygı ve sadakat her şeyden önce gelir.<br>
            2. Konvoy saatleri ve rotalara uyum zorunludur.<br>
            3. İzinsiz başka ekiplere sızmak yasaktır.<br>
            4. Akkuşlar tır kaplaması dışında sürüş yapılmaz.<br>
            5. Kurucu, Admin ve Yönetici kararlarına itiraz edilemez.
        </div>
    </div>

    <div id="p3" class="panel">
        <h1>🎫 Sanal Bilet Sistemi</h1>
        <div class="grid">
            <div class="card">
                <h3>Bilet Kes</h3>
                <input type="text" id="b-ad" placeholder="Yolcu İsmi...">
                <select id="b-rota"><option>Bartın - Denizli</option><option>İstanbul - Hatay</option><option>Ankara - Denizli</option></select>
                <button class="action-btn" onclick="biletKes()">BİLETİ ONAYLA</button>
            </div>
            <div class="card" style="text-align:left;"><h3>Yolcu Listesi</h3><div id="list-bilet"></div></div>
        </div>
    </div>

    <div id="p10" class="panel">
        <h1>🎮 Akkuşlar Minyatür Sürüş</h1>
        <p>Ok tuşlarıyla Akkuşlar aracını şeritte tut!</p>
        <canvas id="gameCanvas" width="600" height="400"></canvas>
    </div>

    <div id="p8" class="panel"><h1>🎨 Kaplamalar & Atölye</h1><div id="list-icerik" class="grid"></div></div>

    <div id="p12" class="panel">
        <h1>✉️ Ekibe Katılım Başvurusu</h1>
        <div class="card" style="max-width:500px; margin:auto;">
            <input type="text" id="k-ad" placeholder="Oyun Adınız...">
            <input type="text" id="k-tk" placeholder="TikTok Adınız...">
            <button class="action-btn" onclick="basvuruAt()">BAŞVURUYU GÖNDER</button>
        </div>
        <div id="admin-onay-kutu" style="display:none; margin-top:30px;">
            <h3>🛡️ Onay Bekleyen Kaptanlar (Admin/Kurucu)</h3>
            <div id="list-basvuru" class="grid"></div>
        </div>
    </div>

    <div id="p13" class="panel">
        <h1>⚙️ YÖNETİM MERKEZİ</h1>
        <div class="grid">
            <div id="panel-kurucu" class="card" style="display:none; border:2px solid var(--gold);">
                <span class="badge badge-kurucu">👑 KURUCU PANELİ</span>
                <h3>Rütbe & Admin Ata</h3>
                <input type="text" id="r-ad" placeholder="İsim...">
                <select id="r-rol"><option value="admin">🛡️ Admin</option><option value="yonetici">⚙️ Yönetici</option></select>
                <button class="action-btn" onclick="rolVer()">YETKİYİ VER</button>
                <hr style="margin:20px 0;">
                <h3>Ana Sayfa Afiş</h3>
                <input type="text" id="afis-url" placeholder="Geniş Resim URL Linki...">
                <button class="action-btn" onclick="afisSet()" style="background:var(--gold); color:#000;">AFİŞİ KAPLA</button>
            </div>

            <div id="panel-admin" class="card" style="display:none; border:2px solid var(--admin);">
                <span class="badge badge-admin">🛡️ ADMİN PANELİ</span>
                <h3>Başvuru Onay</h3>
                <p>Onaylar "Ekibe Katıl" sayfasındadır.</p>
                <button class="action-btn" onclick="sayfa(12)">GİT</button>
            </div>

            <div id="panel-yonetici" class="card" style="display:none; border:2px solid #AA00FF;">
                <span class="badge badge-yonetici">⚙️ YÖNETİCİ PANELİ</span>
                <h3>Mod & Resim Yükle</h3>
                <input type="text" id="i-bas" placeholder="Başlık...">
                <input type="text" id="i-url" placeholder="Resim URL...">
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
    let aktifRol = "uye";

    function yonetimGiris() {
        let s = prompt("Giriş Şifresi:");
        if(s === "1907akkus") {
            aktifRol = "kurucu";
            document.getElementById('app-body').classList.add('kurucu-modu');
            document.getElementById('panel-kurucu').style.display = "block";
            document.getElementById('panel-admin').style.display = "block";
            document.getElementById('panel-yonetici').style.display = "block";
            document.getElementById('admin-onay-kutu').style.display = "block";
            sayfa(13);
        } else if(s === "admin123") {
            aktifRol = "admin";
            document.getElementById('panel-admin').style.display = "block";
            document.getElementById('panel-yonetici').style.display = "block";
            document.getElementById('admin-onay-kutu').style.display = "block";
            sayfa(13);
        } else if(s === "yonetici123") {
            aktifRol = "yonetici";
            document.getElementById('panel-yonetici').style.display = "block";
            sayfa(13);
        }
    }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active-btn'));
        const target = document.getElementById('p' + n);
        if(target) target.classList.add('aktif');
        if(btn) btn.classList.add('active-btn');
        if(n === 10) initGame();
    }

    db.collection("ekip").onSnapshot(s => {
        document.getElementById('list-ekip').innerHTML = s.docs.map(d => `<div class="card"><span class="badge badge-${d.data().rol}">${d.data().rol}</span><br><b>👤 ${d.data().ad}</b><br><button class="del-btn" onclick="sil('ekip','${d.id}')">KADRODAN SİL</button></div>`).join('');
    });

    db.collection("basvuru").onSnapshot(s => {
        document.getElementById('list-basvuru').innerHTML = s.docs.map(d => `<div class="card"><b>${d.data().ad}</b><br><small>TK: ${d.data().tk}</small><br>
            <button onclick="onayla('${d.id}','${d.data().ad}')" style="background:green; color:white; border:none; padding:5px; border-radius:5px; cursor:pointer;">ONAYLA</button>
            <button class="del-btn" onclick="sil('basvuru','${d.id}')" style="display:inline-block !important;">REDDET</button></div>`).join('');
    });

    db.collection("biletler").orderBy("tarih","desc").onSnapshot(s => {
        document.getElementById('list-bilet').innerHTML = s.docs.map(d => `<div style="padding:5px; border-bottom:1px solid #222;">🎫 ${d.data().ad} <br> <small>${d.data().rota}</small> <button class="del-btn" onclick="sil('biletler','${d.id}')">İPTAL</button></div>`).join('');
    });

    db.collection("icerik").onSnapshot(s => {
        document.getElementById('list-icerik').innerHTML = s.docs.map(d => `<div class="card"><img src="${d.data().url}"><b>${d.data().bas}</b><br><button class="del-btn" onclick="sil('icerik','${d.id}')">İÇERİĞİ SİL</button></div>`).join('');
    });

    db.collection("ayarlar").doc("afis").onSnapshot(d => { if(d.exists) { document.getElementById('main-afis').src = d.data().url; document.getElementById('main-afis').style.display = "block"; }});

    function rolVer() { if(aktifRol === "kurucu") db.collection("ekip").add({ad: document.getElementById('r-ad').value, rol: document.getElementById('r-rol').value}); }
    function afisSet() { if(aktifRol === "kurucu") db.collection("ayarlar").doc("afis").set({url: document.getElementById('afis-url').value}); }
    function icerikEkle() { if(aktifRol !== "uye") db.collection("icerik").add({bas: document.getElementById('i-bas').value, url: document.getElementById('i-url').value}); }
    function biletKes() { db.collection("biletler").add({ad: document.getElementById('b-ad').value, rota: document.getElementById('b-rota').value, tarih: Date.now()}); }
    function basvuruAt() { db.collection("basvuru").add({ad: document.getElementById('k-ad').value, tk: document.getElementById('k-tk').value}); alert("Başvuru İletildi!"); }
    function onayla(id, ad) { db.collection("ekip").add({ad: ad, rol: 'uye'}); db.collection("basvuru").doc(id).delete(); }
    function sil(c, id) { if(aktifRol === "kurucu" && confirm("Silinsin mi?")) db.collection(c).doc(id).delete(); }

    function initGame() {
        const c = document.getElementById('gameCanvas'); const ctx = c.getContext('2d');
        let x = 270, road = 0;
        function loop() {
            ctx.clearRect(0,0,600,400); ctx.fillStyle="#333"; ctx.fillRect(150,0,300,400);
            ctx.fillStyle="#FFF"; for(let i=0; i<10; i++) ctx.fillRect(295, ((i*60)+road)%400, 10, 30);
            road+=5;
            // MİNYATÜR ARAÇ
            ctx.fillStyle="#CC0000"; ctx.fillRect(x, 300, 50, 80);
            ctx.fillStyle="#000"; ctx.fillRect(x+5, 310, 40, 20);
            ctx.fillStyle="#FFF"; ctx.font="bold 9px Arial"; ctx.fillText("AKKUŞLAR", x+1, 350);
            requestAnimationFrame(loop);
        }
        document.onkeydown = (e) => { if(e.key==="ArrowLeft" && x>155) x-=15; if(e.key==="ArrowRight" && x<395) x+=15; };
        loop();
    }
</script>
</body>
</html>
