<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V55</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-storage-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; --admin: #00CCFF; --yonetici: #AA00FF; }
        * { box-sizing: border-box; transition: 0.2s; outline: none; }
        body { margin: 0; background: var(--bg); color: #EEE; font-family: 'Space Grotesk', sans-serif; display: flex; height: 100vh; overflow: hidden; }
        
        /* SOL TARAF SABİT BUTONLAR */
        .sidebar { width: 280px; background: #000; border-right: 3px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 15px; margin-bottom: 20px; }
        .nav-label { color: #555; font-size: 10px; font-weight: bold; text-transform: uppercase; margin: 15px 0 5px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 6px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; font-size: 13px; display: flex; align-items: center; gap: 10px; }
        .nav-btn:hover { background: var(--p); color: white; }
        .active-btn { background: var(--p) !important; color: white !important; }

        /* ANA EKRAN */
        .main { flex: 1; padding: 30px; overflow-y: auto; background: radial-gradient(circle at top right, #1a0000, #0A0A0A); position: relative; }
        .panel { display: none; }
        .aktif { display: block !important; animation: fadeIn 0.4s; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* AFİŞ: EKRANI TAM KAPLAYAN */
        .hero-banner { width: 100%; height: 500px; object-fit: cover; border-radius: 20px; border: 4px solid var(--p); margin-bottom: 20px; }
        
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 15px; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #333; text-align: center; position: relative; }
        .card img { width: 100%; height: 150px; object-fit: cover; border-radius: 10px; margin-bottom: 10px; }

        /* RÜTBE ROZETLERİ & İKONLAR */
        .badge { padding: 4px 10px; border-radius: 5px; font-size: 11px; font-weight: bold; text-transform: uppercase; margin-bottom: 10px; display: inline-flex; align-items: center; gap: 5px; }
        .badge-kurucu { background: var(--gold); color: #000; }
        .badge-admin { background: var(--admin); color: #000; }
        .badge-yonetici { background: var(--yonetici); color: #fff; }
        .badge-uye { background: #444; color: #fff; }

        /* SOSYAL ÖZELLİKLER */
        .star-rating { color: var(--gold); font-size: 24px; cursor: pointer; margin-bottom: 10px; }
        .msg-list { height: 200px; overflow-y: auto; background: #000; border: 1px solid #222; padding: 10px; border-radius: 10px; margin-bottom: 10px; font-size: 13px; }

        /* YETKİ KISITLAMALARI */
        .del-btn { background: #600; color: white; border: none; padding: 6px 12px; border-radius: 5px; cursor: pointer; display: none; margin-top: 10px; }
        .kurucu-modu .del-btn { display: inline-block !important; }

        input, select, textarea { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; }
        canvas { background: #111; border: 3px solid var(--p); display: block; margin: 0 auto; border-radius: 20px; max-width: 100%; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h2 style="color:var(--p); margin:0;">AKKUŞLAR</h2>
        <span style="color:var(--gold); font-size:12px;">👑 Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</span>
    </div>
    
    <div class="nav-label">TEMEL PANEL</div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Sayfa</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Ekip Kadrosu</button>
    <button class="nav-btn" onclick="sayfa(15, this)">📜 Ekip Kuralları</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🎫 Bilet Gişesi</button>
    <button class="nav-btn" onclick="sayfa(10, this)">🎮 Akkuşlar Oyun</button>
    <button class="nav-btn" onclick="sayfa(8, this)">🎨 Kaplama & Mod</button>
    <button class="nav-btn" onclick="sayfa(12, this)">✉️ Ekibe Katıl</button>
    
    <div class="nav-label">YÖNETİM</div>
    <button class="nav-btn" style="color:var(--gold);" onclick="giris('kurucu')">👑 KURUCU GİRİŞİ</button>
    <button class="nav-btn" style="color:var(--admin);" onclick="giris('admin')">🛡️ ADMİN GİRİŞİ</button>
    <button class="nav-btn" style="color:var(--yonetici);" onclick="giris('yonetici')">⚙️ YÖNETİCİ GİRİŞİ</button>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <img id="main-afis" class="hero-banner" style="display:none;">
        <h1 style="font-size: 45px; color: var(--p); margin:0;">AKKUŞLAR SEYAHAT</h1>
        
        <div class="grid" style="margin-top:20px;">
            <div class="card">
                <h3>⭐ Terminal Puanı</h3>
                <div class="star-rating" onclick="puanVer()">⭐⭐⭐⭐⭐</div>
                <p>Kaptanlar Puanlıyor!</p>
            </div>
            <div class="card" style="grid-column: span 2;">
                <h3>💬 Gelişmiş Üye Sohbeti</h3>
                <div id="msg-list" class="msg-list"></div>
                <div style="display:flex; gap:10px;">
                    <input type="text" id="chat-input" placeholder="Telsize mesaj bırak...">
                    <button onclick="mesajGonder()" style="background:var(--p); border:none; padding:0 20px; border-radius:8px; cursor:pointer; color:#fff;">GÖNDER</button>
                </div>
            </div>
        </div>
    </div>

    <div id="p2" class="panel"><h1>👥 Ekip Kadrosu</h1><div id="list-ekip" class="grid"></div></div>

    <div id="p15" class="panel">
        <h1>📜 Gelişmiş Ekip Kuralları</h1>
        <div style="background:#111; padding:30px; border-radius:15px; border-left:5px solid var(--p); line-height:2.2;">
            <b>1. Mutlak Otorite:</b> 👑 Kurucu'nun kararları kesindir.<br>
            <b>2. Hiyerarşi:</b> 🛡️ Admin ve ⚙️ Yöneticilerin talimatları uygulanır.<br>
            <b>3. Kimlik:</b> Akkuşlar kaplaması taşımayan araçlar konvoya alınmaz.<br>
            <b>4. Saygı:</b> Sohbet ve telsiz ortamında saygı esastır.<br>
            <b>5. İhraç:</b> Kurallara uymayanlar 👑 Kurucu tarafından tek tıkla silinir.
        </div>
    </div>

    <div id="p10" class="panel">
        <h1>🎮 Akkuşlar Minyatür Sürüş</h1>
        <p>Ok tuşlarıyla Akkuşlar aracını yolda tut!</p>
        <canvas id="gameCanvas" width="600" height="400"></canvas>
    </div>

    <div id="p13" class="panel">
        <h1>⚙️ YÖNETİM MERKEZİ</h1>
        <div class="grid">
            <div id="pan-kurucu" class="card" style="display:none; border:2px solid var(--gold);">
                <span class="badge badge-kurucu">👑 KURUCU PANELİ</span>
                <h3>Rol & Admin Ver</h3>
                <input type="text" id="r-ad" placeholder="Kaptan Adı...">
                <select id="r-rol"><option value="admin">🛡️ Admin</option><option value="yonetici">⚙️ Yönetici</option></select>
                <button class="action-btn" onclick="rolVer()">YETKİYİ TANIMLA</button>
                <hr style="margin:20px 0;">
                <h3>Ana Sayfa Afiş (Dosyadan)</h3>
                <input type="file" id="afis-file" accept="image/*">
                <button class="action-btn" onclick="yukle('afis')" style="background:var(--gold); color:#000;">YÜKLE</button>
            </div>

            <div id="pan-editor" class="card" style="display:none; border:2px solid var(--admin);">
                <span class="badge badge-admin">⚙️ YÖNETİCİ & ADMİN</span>
                <h3>Mod & Resim Paylaş</h3>
                <input type="text" id="i-bas" placeholder="Başlık...">
                <input type="file" id="i-file" accept="image/*">
                <button class="action-btn" onclick="yukle('icerik')">PAYLAŞ</button>
            </div>
        </div>
    </div>

    <div id="p3" class="panel"><h1>🎫 Bilet Gişesi</h1><div class="grid"><div class="card"><input type="text" id="b-ad" placeholder="Yolcu Adı..."><button class="action-btn" onclick="biletKes()">BİLET KES</button></div><div id="list-bilet"></div></div></div>
    <div id="p8" class="panel"><h1>🎨 Atölye & Modlar</h1><div id="list-icerik" class="grid"></div></div>
    <div id="p12" class="panel"><h1>📩 Ekibe Katıl</h1><div class="card" style="max-width:500px; margin:auto;"><input type="text" id="k-ad" placeholder="Oyun Adınız..."><button class="action-btn" onclick="basvuruAt()">GÖNDER</button></div><div id="onay-alani" style="display:none; margin-top:20px;"><h3>🛡️ Onay Bekleyenler (Admin Yetkili)</h3><div id="list-basvuru" class="grid"></div></div></div>
</div>

<script>
    const firebaseConfig = {
      apiKey: "AIzaSyCowK8jtpCENPWxz0EpANeRghLXlYMVkcg",
      authDomain: "akkuslar-seyahat-f3577.firebaseapp.com",
      projectId: "akkuslar-seyahat-f3577",
      storageBucket: "akkuslar-seyahat-f3577.appspot.com",
      messagingSenderId: "207828517432",
      appId: "1:207828517432:web:dedb92a79fe1d17aec9ccd"
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();
    const storage = firebase.storage();
    let rol = "uye";

    function giris(tip) {
        let s = prompt("Giriş Şifresi:");
        if(tip === 'kurucu' && s === "1907akkus") {
            rol = "kurucu";
            document.getElementById('app-body').classList.add('kurucu-modu');
            document.getElementById('pan-kurucu').style.display = "block";
            document.getElementById('pan-editor').style.display = "block";
            document.getElementById('onay-alani').style.display = "block";
            sayfa(13);
        } else if(tip === 'admin' && s === "admin123") {
            rol = "admin";
            document.getElementById('pan-editor').style.display = "block";
            document.getElementById('onay-alani').style.display = "block";
            sayfa(13);
        } else if(tip === 'yonetici' && s === "yonetici123") {
            rol = "yonetici";
            document.getElementById('pan-editor').style.display = "block";
            sayfa(13);
        }
    }

    function yukle(tip) {
        const file = tip === 'afis' ? document.getElementById('afis-file').files[0] : document.getElementById('i-file').files[0];
        if(!file) return alert("Dosya Seç!");
        const ref = storage.ref(`depo/${Date.now()}_${file.name}`);
        ref.put(file).then(s => s.ref.getDownloadURL()).then(url => {
            if(tip === 'afis') db.collection("ayarlar").doc("afis").set({url});
            else db.collection("icerik").add({bas: document.getElementById('i-bas').value, url});
            alert("Dosya Başarıyla Aktarıldı!");
        });
    }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active-btn'));
        document.getElementById('p' + n).classList.add('aktif');
        if(btn) btn.classList.add('active-btn');
        if(n === 10) initGame();
    }

    // FIREBASE OTOMASYONLARI
    db.collection("ekip").onSnapshot(s => {
        document.getElementById('list-ekip').innerHTML = s.docs.map(d => {
            let i = d.data().rol === "admin" ? "🛡️" : (d.data().rol === "yonetici" ? "⚙️" : "👤");
            return `<div class="card"><span class="badge badge-${d.data().rol}">${i} ${d.data().rol}</span><br><b>${d.data().ad}</b><br><button class="del-btn" onclick="sil('ekip','${d.id}')">KADRODAN SİL</button></div>`;
        }).join('');
    });
    db.collection("sohbet").orderBy("tarih","desc").limit(30).onSnapshot(s => {
        document.getElementById('msg-list').innerHTML = s.docs.map(d => `<div><b style="color:var(--gold)">Kaptan:</b> ${d.data().msg}</div>`).reverse().join('');
    });
    db.collection("icerik").onSnapshot(s => {
        document.getElementById('list-icerik').innerHTML = s.docs.map(d => `<div class="card"><img src="${d.data().url}"><b>${d.data().bas}</b><br><button class="del-btn" onclick="sil('icerik','${d.id}')">SİL</button></div>`).join('');
    });
    db.collection("basvuru").onSnapshot(s => {
        document.getElementById('list-basvuru').innerHTML = s.docs.map(d => `<div class="card"><b>${d.data().ad}</b><br><button onclick="onayla('${d.id}','${d.data().ad}')" style="background:green; color:#fff; border:none; padding:5px; border-radius:5px;">ONAYLA</button><button class="del-btn" onclick="sil('basvuru','${d.id}')" style="display:inline-block !important;">REDDET</button></div>`).join('');
    });
    db.collection("ayarlar").doc("afis").onSnapshot(d => { if(d.exists) { document.getElementById('main-afis').src = d.data().url; document.getElementById('main-afis').style.display = "block"; }});

    function mesajGonder() { let m = document.getElementById('chat-input').value; if(m) { db.collection("sohbet").add({msg: m, tarih: Date.now()}); document.getElementById('chat-input').value=""; } }
    function rolVer() { if(rol === "kurucu") db.collection("ekip").add({ad: document.getElementById('r-ad').value, rol: document.getElementById('r-rol').value}); }
    function basvuruAt() { db.collection("basvuru").add({ad: document.getElementById('k-ad').value, tarih: Date.now()}); alert("Başvurunuz İletildi!"); }
    function onayla(id, ad) { db.collection("ekip").add({ad: ad, rol: 'uye'}); db.collection("basvuru").doc(id).delete(); }
    function biletKes() { db.collection("biletler").add({ad: document.getElementById('b-ad').value}); }
    db.collection("biletler").onSnapshot(s => { document.getElementById('list-bilet').innerHTML = s.docs.map(d => `<div>🎫 ${d.data().ad} <button class="del-btn" onclick="sil('biletler','${d.id}')">SİL</button></div>`).join(''); });
    function sil(c, id) { if(rol === "kurucu" && confirm("Kalıcı olarak silinsin mi?")) db.collection(c).doc(id).delete(); }
    function puanVer() { alert("Teşekkürler kaptan! 5 Yıldız verildi."); }

    // OYUN MOTORU (MİNYATÜR ARAÇ)
    function initGame() {
        const c = document.getElementById('gameCanvas'); const ctx = c.getContext('2d');
        let x = 275, road = 0;
        function loop() {
            ctx.clearRect(0,0,600,400); 
            // YOL
            ctx.fillStyle="#333"; ctx.fillRect(150,0,300,400);
            ctx.fillStyle="#FFF"; for(let i=0; i<10; i++) ctx.fillRect(295, ((i*60)+road)%400, 10, 30);
            road+=5; 
            // MİNYATÜR ARAÇ (AKKUŞLAR)
            ctx.fillStyle="#CC0000"; ctx.fillRect(x, 300, 50, 75); // Gövde
            ctx.fillStyle="#000"; ctx.fillRect(x+5, 310, 40, 20); // Cam
            ctx.fillStyle="#FFF"; ctx.font="bold 9px Arial"; ctx.fillText("AKKUŞLAR", x+2, 350);
            requestAnimationFrame(loop);
        }
        document.onkeydown = (e) => { if(e.key==="ArrowLeft" && x>155) x-=15; if(e.key==="ArrowRight" && x<395) x+=15; };
        loop();
    }
</script>
</body>
</html>
