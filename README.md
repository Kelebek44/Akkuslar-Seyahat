<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V67</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-storage-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: #141414; --gold: #FFD700; --admin: #00CCFF; --yonetici: #AA00FF; }
        * { box-sizing: border-box; transition: 0.3s; font-family: 'DM Sans', sans-serif; }
        h1, h2, h3, .nav-btn { font-family: 'Space Grotesk', sans-serif; }
        body { margin: 0; background: var(--bg); color: #EEE; display: flex; height: 100vh; overflow: hidden; }
        .sidebar { width: 280px; background: #000; border-right: 2px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; }
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 15px; margin-bottom: 20px; }
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 6px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; font-size: 13px; display: flex; align-items: center; gap: 10px; }
        .nav-btn:hover { background: var(--p); color: white; transform: translateX(5px); }
        .active-btn { background: var(--p) !important; color: white !important; }
        .main { flex: 1; padding: 40px; overflow-y: auto; background: linear-gradient(135deg, #0A0A0A 0%, #1a0000 100%); }
        .panel { display: none; }
        .aktif { display: block !important; }
        .card { background: var(--card); padding: 25px; border-radius: 15px; border: 1px solid #222; margin-bottom: 20px; text-align: center; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; }
        .card img { width: 100%; border-radius: 10px; margin-bottom: 15px; border: 1px solid #333; height: 180px; object-fit: cover; }
        .badge { padding: 5px 12px; border-radius: 6px; font-size: 11px; font-weight: bold; text-transform: uppercase; display: inline-flex; align-items: center; gap: 5px; }
        .b-kurucu { background: var(--gold); color: #000; }
        .b-admin { background: var(--admin); color: #000; }
        .b-yonetici { background: var(--yonetici); color: #fff; }
        .kurucu-only, .editor-only { display: none; }
        .kurucu-mode .kurucu-only { display: block !important; }
        .kurucu-mode .editor-only, .admin-mode .editor-only, .yonetici-mode .editor-only { display: block !important; }
        input, select, textarea { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 15px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 14px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; text-transform: uppercase; }
        .del-btn { background: #800 !important; font-size: 11px; padding: 5px !important; display: none; margin-top: 10px; border-radius: 5px; color: white; border: none; cursor: pointer; }
        .kurucu-mode .del-btn { display: block !important; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h2 style="color:var(--p); margin:0;">AKKUŞLAR SEYAHAT</h2>
        <div style="color:var(--gold); font-size:11px; margin-top:5px; font-weight:bold;">👑 Kurucu: 乂✯ҠƐꝈƐβƐҠ✯乂</div>
    </div>
    <div class="nav-label">ANA ÜS</div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Sayfa</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👤 Ekip Kadrosu</button>
    <button class="nav-btn" onclick="sayfa(20, this)">🗺️ Konvoy Rotaları</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🎫 Bilet Sistemi</button>
    <div class="nav-label">ATÖLYE</div>
    <button class="nav-btn" onclick="sayfa(8, this)">🖼️ Galeri</button>
    <button class="nav-btn" onclick="sayfa(7, this)">🛠️ Modlar</button>
    <button class="nav-btn" onclick="sayfa(9, this)">🎨 Kaplamalar</button>
    <button class="nav-btn" onclick="sayfa(11, this)">🎵 Müzikler</button>
    <div style="margin-top:auto; padding-top:20px;">
        <button class="nav-btn" style="color:var(--gold);" onclick="giris('kurucu')">👑 KURUCU GİRİŞİ</button>
        <button class="nav-btn" style="color:var(--admin);" onclick="giris('admin')">🛡️ ADMİN GİRİŞİ</button>
        <button class="nav-btn" style="color:var(--yonetici);" onclick="giris('yonetici')">⚙️ YÖNETİCİ GİRİŞİ</button>
    </div>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <div class="card"><h1>AKKUŞLAR SEYAHAT TERMİNALİ</h1><p>Gece yolların tek hakimi.</p></div>
        <div class="card">
            <h3>💬 Ekip Telsizi</h3>
            <div id="msg-list" style="height:250px; overflow-y:auto; background:#000; padding:15px; border-radius:10px; margin-bottom:10px; text-align:left; border:1px solid #222;"></div>
            <div style="display:flex; gap:10px;">
                <input type="text" id="chat-nick" placeholder="İsim" style="width:30%;">
                <input type="text" id="chat-input" placeholder="Mesaj..." style="width:70%;">
                <button onclick="mesajAt()" class="action-btn" style="width:80px;">GÖNDER</button>
            </div>
        </div>
    </div>

    <div id="p2" class="panel"><h1>👥 Ekip Kadrosu</h1><div id="kadro-listesi" class="grid"></div></div>
    <div id="p8" class="panel"><h1>🖼️ Galeri</h1><div id="list-galeri" class="grid"></div></div>
    <div id="p7" class="panel"><h1>🛠️ Mod Merkezi</h1><div id="list-mod" class="grid"></div></div>
    <div id="p9" class="panel"><h1>🎨 Kaplamalar & Skin</h1><div id="list-kaplama" class="grid"></div></div>
    <div id="p11" class="panel"><h1>🎵 Müzik Listesi</h1><div id="list-muzik" class="grid"></div></div>
    <div id="p3" class="panel"><h1>🎫 Bilet Sistemi</h1><div class="grid"><div class="card"><h3>Bilet Al</h3><input type="text" id="b-ad" placeholder="Ad Soyad"><button onclick="biletAt()" class="action-btn">BİLET KES</button></div><div class="card"><h3>Aktif Biletler</h3><div id="list-bilet" style="text-align:left;"></div></div></div></div>
    <div id="p20" class="panel"><h1>🗺️ Konvoy Planı</h1><div class="card" style="border-left:10px solid var(--p);"><h2>İSTANBUL ➔ ANKARA ➔ DENİZLİ ➔ HATAY</h2><p>Saat: 21:00 | Sunucu: Simulation 1</p></div></div>

    <div id="p13" class="panel">
        <h1>⚙️ YÖNETİM PANELİ</h1>
        <div class="grid">
            <div class="card editor-only" style="border:2px solid var(--p);">
                <h3>İÇERİK YÜKLE</h3>
                <select id="i-tip"><option value="galeri">Galeri</option><option value="mod">Mod</option><option value="kaplama">Kaplama</option><option value="muzik">Müzik</option></select>
                <input type="text" id="i-bas" placeholder="Başlık Yazın...">
                <input type="file" id="i-file">
                <button class="action-btn" id="yukle-btn" onclick="yukle()">YÜKLEMEYİ BAŞLAT</button>
                <div id="u-durum" style="color:var(--gold); margin-top:10px; font-weight:bold;"></div>
            </div>
            <div class="card kurucu-only" style="border:2px solid var(--gold);">
                <h3>👑 ÜYE & ROL YÖNETİMİ</h3>
                <input type="text" id="r-ad" placeholder="Kaptan Adı">
                <select id="r-rol"><option value="admin">Admin</option><option value="yonetici">Yönetici</option><option value="uye">Üye</option></select>
                <button class="action-btn" onclick="rolVer()">SİSTEME KAYDET</button>
                <hr style="opacity:0.1; margin:20px 0;">
                <div id="list-basvuru"></div>
            </div>
        </div>
    </div>
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
    let aktifRol = "uye";

    function giris(tip) {
        let s = prompt("Şifre:");
        if(tip === 'kurucu' && s === "1907akkus") { aktifRol="kurucu"; document.getElementById('app-body').className='kurucu-mode'; sayfa(13); }
        else if(tip === 'admin' && s === "admin123") { aktifRol="admin"; document.getElementById('app-body').className='admin-mode'; sayfa(13); }
        else if(tip === 'yonetici' && s === "yonetici123") { aktifRol="yonetici"; document.getElementById('app-body').className='yonetici-mode'; sayfa(13); }
        else if(s) { alert("Şifre Yanlış!"); }
    }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active-btn'));
        document.getElementById('p' + n).classList.add('aktif');
        if(btn) btn.classList.add('active-btn');
    }

    // GÜÇLENDİRİLMİŞ YÜKLEME SİSTEMİ
    async function yukle() {
        const file = document.getElementById('i-file').files[0];
        const tip = document.getElementById('i-tip').value;
        const bas = document.getElementById('i-bas').value;
        const durum = document.getElementById('u-durum');

        if(!file || !bas) return alert("Dosya ve Başlık Eksik!");

        durum.innerText = "Yükleniyor... (Lütfen Sayfayı Kapatmayın)";
        document.getElementById('yukle-btn').disabled = true;

        try {
            const ref = storage.ref(`akkuslar_v67/${Date.now()}_${file.name}`);
            const uploadTask = await ref.put(file);
            const url = await uploadTask.ref.getDownloadURL();
            
            await db.collection("icerik").add({ tip, bas, url, tarih: Date.now() });
            
            durum.innerText = "✅ BAŞARIYLA YÜKLENDİ!";
            document.getElementById('i-file').value = "";
            document.getElementById('i-bas').value = "";
        } catch (err) {
            durum.innerText = "❌ HATA: " + err.message;
            console.error(err);
        } finally {
            document.getElementById('yukle-btn').disabled = false;
        }
    }

    // İÇERİK DİNLEYİCİ
    db.collection("icerik").orderBy("tarih","desc").onSnapshot(s => {
        let g="", m="", k="", mu="";
        s.docs.forEach(d => {
            let data = d.data();
            let sil = aktifRol === 'kurucu' ? `<button class="del-btn" onclick="sil('icerik','${d.id}')">SİL</button>` : '';
            let html = `<div class="card">${data.tip !== 'muzik' ? `<img src="${data.url}">` : '🎵'} <b>${data.bas}</b>${sil}</div>`;
            if(data.tip === "galeri") g += html; else if(data.tip === "mod") m += html; else if(data.tip === "kaplama") k += html; else mu += html;
        });
        document.getElementById('list-galeri').innerHTML = g;
        document.getElementById('list-mod').innerHTML = m;
        document.getElementById('list-kaplama').innerHTML = k;
        document.getElementById('list-muzik').innerHTML = mu;
    });

    // KADRO & İKONLAR
    db.collection("ekip").onSnapshot(s => {
        document.getElementById('kadro-listesi').innerHTML = s.docs.map(d => {
            let data = d.data();
            let ikon = data.rol === 'admin' ? '🛡️' : (data.rol === 'yonetici' ? '⚙️' : '👤');
            let cls = data.rol === 'admin' ? 'b-admin' : (data.rol === 'yonetici' ? 'b-yonetici' : '');
            let sil = aktifRol === 'kurucu' ? `<button class="del-btn" onclick="sil('ekip','${d.id}')">KADRODAN AT</button>` : '';
            return `<div class="card"><span class="badge ${cls}">${ikon} ${data.rol}</span><br><b>${data.ad}</b><br>${sil}</div>`;
        }).join('');
    });

    function mesajAt() { 
        let n = document.getElementById('chat-nick').value;
        let m = document.getElementById('chat-input').value;
        if(n && m) { db.collection("sohbet").add({isim: n, msg: m, tarih: Date.now()}); document.getElementById('chat-input').value=""; }
    }
    db.collection("sohbet").orderBy("tarih","desc").limit(15).onSnapshot(s => {
        document.getElementById('msg-list').innerHTML = s.docs.map(d => `<div><b style="color:var(--p)">${d.data().isim}:</b> ${d.data().msg}</div>`).reverse().join('');
    });
    
    function rolVer() { if(aktifRol === 'kurucu') db.collection("ekip").add({ad: document.getElementById('r-ad').value, rol: document.getElementById('r-rol').value}); }
    function biletAt() { db.collection("biletler").add({ad: document.getElementById('b-ad').value}); }
    db.collection("biletler").onSnapshot(s => {
        document.getElementById('list-bilet').innerHTML = s.docs.map(d => `<div style="margin-bottom:10px;">🎫 ${d.data().ad} <button class="del-btn" onclick="sil('biletler','${d.id}')">İPTAL</button></div>`).join('');
    });
    function sil(c, id) { if(confirm("Emin misin kaptan? Bu geri dönmez.")) db.collection(c).doc(id).delete(); }
</script>
</body>
</html>
