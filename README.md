<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AKKUŞLAR SEYAHAT | TERMİNAL V90</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@700&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet">
    <style>
        :root { --p: #CC0000; --bg: #0A0A0A; --card: rgba(15,15,15,0.95); --gold: #FFD700; --admin: #00CCFF; --yon: #AA00FF; }
        * { box-sizing: border-box; transition: 0.3s; font-family: 'DM Sans', sans-serif; }
        body { margin: 0; background: var(--bg); color: #EEE; display: flex; height: 100vh; overflow: hidden; background-size: cover !important; background-position: center !important; }
        
        /* SOL SİDEBAR */
        .sidebar { width: 290px; background: rgba(0,0,0,0.98); border-right: 2px solid var(--p); display: flex; flex-direction: column; padding: 20px; overflow-y: auto; flex-shrink: 0; z-index: 10; }
        
        /* LOGO ALANI (ÖZEL TASARIM) */
        .logo-area { text-align: center; border-bottom: 2px solid var(--p); padding-bottom: 15px; margin-bottom: 20px; display: flex; flex-direction: column; align-items: center; gap: 8px; }
        #fixed-logo { width: 70px; height: 70px; border-radius: 50%; border: 2px solid var(--p); object-fit: cover; box-shadow: 0 0 10px rgba(204,0,0,0.3); }
        
        /* NAV BUTONLARI */
        .nav-btn { background: #111; color: #AAA; padding: 12px; margin-bottom: 5px; cursor: pointer; border: none; text-align: left; width: 100%; border-radius: 8px; font-weight: bold; font-size: 11px; display: flex; align-items: center; gap: 10px; }
        .nav-btn:hover, .active-btn { background: var(--p); color: white; transform: translateX(5px); }
        
        /* ANA İÇERİK */
        .main { flex: 1; padding: 40px; overflow-y: auto; background: rgba(0,0,0,0.5); z-index: 5; }
        .panel { display: none; }
        .aktif { display: block !important; }
        .card { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid #333; margin-bottom: 20px; backdrop-filter: blur(10px); text-align: center; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 15px; }
        
        /* BADGE VE YETKİLER */
        .badge { padding: 4px 10px; border-radius: 5px; font-size: 10px; font-weight: bold; margin-bottom: 10px; display: inline-block; text-transform: uppercase; }
        .b-kurucu { background: var(--gold); color: #000; }
        .b-admin { background: var(--admin); color: #000; }
        .b-yonetici { background: var(--yon); color: #fff; }
        .kurucu-only, .editor-only { display: none; }
        .kurucu-mode .kurucu-only, .kurucu-mode .editor-only { display: block !important; }
        .admin-mode .editor-only, .yonetici-mode .editor-only { display: block !important; }
        
        /* SİLME VE İNDİRME */
        .del-btn { background: #800; color: white; border: none; padding: 8px; border-radius: 5px; cursor: pointer; display: none; margin-top: 10px; width: 100%; font-weight: bold; font-size: 11px; }
        .kurucu-mode .del-btn { display: block !important; }
        .download-btn { background: #28a745; color: white; border: none; padding: 10px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; display: block; text-decoration: none; margin-top: 10px; text-align: center; font-size: 12px; text-transform: uppercase; }
        .download-btn:hover { background: #218838; }
        
        /* GERİ VE AKSİYON BUTONLARI */
        .back-btn { background: #222; color: #fff; padding: 10px 15px; border-radius: 8px; border: 1px solid #444; cursor: pointer; margin-bottom: 20px; font-weight: bold; font-size: 11px; display: inline-flex; align-items: center; gap: 8px; text-transform: uppercase; }
        .back-btn:hover { background: #333; }
        input, select { width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 10px; }
        .action-btn { background: var(--p); color: white; border: none; padding: 15px; border-radius: 8px; cursor: pointer; width: 100%; font-weight: bold; text-transform: uppercase; }
        
        /* RESİM ÇERÇEVESİ */
        .img-item { width: 100%; height: 200px; border-radius: 10px; object-fit: cover; border: 1px solid #444; margin-bottom: 10px; }
    </style>
</head>
<body id="app-body">

<div class="sidebar">
    <div class="logo-area">
        <h2 style="color:var(--p); margin:0; font-family:'Space Grotesk'; font-size:20px;">AKKUŞLAR SEYAHAT</h2>
        <img id="fixed-logo" src="https://i.wpfc.ml/bh/lbsj5t.jpg" alt="Logo">
        <div style="color:var(--gold); font-size:12px; font-weight:bold;">👑 KURUCU: 乂✯ҠƐꝈƐβƐҠ✯乂</div>
    </div>
    <button class="nav-btn active-btn" onclick="sayfa(1, this)">🏠 Ana Ekran</button>
    <button class="nav-btn" onclick="sayfa(2, this)">👥 Ekip Üyeleri</button>
    <button class="nav-btn" onclick="sayfa(3, this)">🛡️ Yönetim Kadrosu</button>
    <button class="nav-btn" onclick="sayfa(8, this)">🖼️ Galeri / Kaplama</button>
    <button class="nav-btn" onclick="sayfa(7, this)">🛠️ Modlar / Müzikler</button>
    <button class="nav-btn" onclick="sayfa(12, this)">🛡️ Yönetim Telsizi</button>
    <button class="nav-btn" onclick="sayfa(5, this)">✍️ Ekibe Katıl</button>
    <button class="nav-btn" onclick="sayfa(15, this)">📞 İletişim</button>
    <div style="margin-top:auto; padding-top:20px; border-top:1px solid #222;">
        <button class="nav-btn" style="color:var(--gold);" onclick="giris('kurucu')">👑 KURUCU GİRİŞ</button>
        <button class="nav-btn" style="color:var(--admin);" onclick="giris('admin')">🛡️ ADMİN GİRİŞ</button>
        <button class="nav-btn" style="color:var(--yon);" onclick="giris('yonetici')">⚙️ YÖNETİCİ GİRİŞ</button>
    </div>
</div>

<div class="main">
    <div id="p1" class="panel aktif">
        <div class="card" style="background:rgba(0,0,0,0.6);">
            <h1 style="font-size:2.5rem; margin:0; font-family:'Space Grotesk'; color:white;">AKKUŞLAR TERMİNAL</h1>
            <p style="color:var(--gold); font-weight:bold; letter-spacing:1px; margin-top:5px;">Yolların Tek Hakimi</p>
        </div>
        <div class="grid">
            <div class="card"><h3>Üye Sayısı</h3><h1 id="count-uye" style="color:var(--p); margin:0;">0</h1></div>
            <div class="card"><h3>Yönetim</h3><h1 id="count-yon" style="color:var(--gold); margin:0;">0</h1></div>
        </div>
        <div class="card">
            <h3>📢 Ekip Telsizi</h3>
            <div id="msg-list-uye" style="height:250px; overflow-y:auto; background:rgba(0,0,0,0.8); padding:15px; border-radius:10px; text-align:left;"></div>
            <div style="display:flex; gap:10px; margin-top:15px;">
                <input type="text" id="nick-uye" placeholder="İsim" style="width:20%;">
                <input type="text" id="msg-uye" placeholder="Mesaj..." style="width:80%;">
                <button onclick="mesajGonder('sohbet_uye', 'nick-uye', 'msg-uye')" class="action-btn" style="width:100px;">GÖNDER</button>
            </div>
        </div>
    </div>

    <div id="p2" class="panel"><button class="back-btn" onclick="sayfa(1)">🔙 Ana Sayfa'ya Dön</button><h1>👥 Ekip Üyeleri</h1><div id="list-uye-adlari" class="grid"></div></div>
    <div id="p3" class="panel"><button class="back-btn" onclick="sayfa(1)">🔙 Ana Sayfa'ya Dön</button><h1>🛡️ Yönetim Kadrosu</h1><div id="list-yon-adlari" class="grid"></div></div>
    <div id="p8" class="panel"><button class="back-btn" onclick="sayfa(1)">🔙 Ana Sayfa'ya Dön</button><h1>🖼️ Galeri & Kaplamalar</h1><div id="list-galeri" class="grid"></div></div>
    <div id="p7" class="panel"><button class="back-btn" onclick="sayfa(1)">🔙 Ana Sayfa'ya Dön</button><h1>🛠️ Modlar & Müzikler</h1><div id="list-mod" class="grid"></div></div>

    <div id="p13" class="panel">
        <button class="back-btn" onclick="sayfa(1)">🔙 Ana Sayfa'ya Dön</button>
        <h1>⚙️ YÖNETİM MERKEZİ</h1>
        <div class="grid">
            <div class="card editor-only">
                <h3>İÇERİK EKLE (URL)</h3>
                <select id="i-tip"><option value="galeri">Galeri / Kaplama</option><option value="mod">Mod / Müzik</option></select>
                <input type="text" id="i-bas" placeholder="Başlık Yazın">
                <input type="text" id="i-url" placeholder="URL Linki Yapıştır">
                <button class="action-btn" onclick="urlIleYukle()">EKLE</button>
            </div>
            <div class="card kurucu-only" style="border:2px solid var(--gold);">
                <h3>🖼️ TERMİNAL ARKA PLANI</h3>
                <input type="text" id="bg-url" placeholder="Arka Plan URL">
                <button class="action-btn" onclick="setBg()">AYARLA</button>
            </div>
            <div class="card editor-only"><h3>📥 BAŞVURULAR</h3><div id="list-basvuru"></div></div>
            <div class="card kurucu-only"><h3>👑 ROL VER</h3><input type="text" id="r-ad" placeholder="İsim"><select id="r-rol"><option value="admin">Admin</option><option value="yonetici">Yönetici</option><option value="uye">Üye</option></select><button class="action-btn" onclick="rolVer()">KAYDET</button></div>
        </div>
    </div>

    <div id="p15" class="panel"><button class="back-btn" onclick="sayfa(1)">🔙 Ana Sayfa'ya Dön</button><h1>📞 İletişim</h1><div class="card"><p><a href="https://www.tiktok.com/@kelebekmiisaliii" target="_blank" style="color:var(--p); text-decoration:none;">TikTok 1</a></p><p><a href="https://www.tiktok.com/@akkusailesi20" target="_blank" style="color:var(--p); text-decoration:none;">TikTok 2</a></p></div></div>
    <div id="p5" class="panel"><button class="back-btn" onclick="sayfa(1)">🔙 Ana Sayfa'ya Dön</button><h1>✍️ Ekibe Katıl</h1><div class="card"><input type="text" id="k-ad" placeholder="İsminiz..."><button onclick="basvur()" class="action-btn">GÖNDER</button></div></div>
    <div id="p12" class="panel"><button class="back-btn" onclick="sayfa(1)">🔙 Ana Sayfa'ya Dön</button><h1>🛡️ Yönetim Telsizi</h1><div class="card"><div id="msg-list-yon" style="height:300px; overflow-y:auto; background:#000; padding:15px; border-radius:10px; border:2px solid var(--gold); text-align:left;"></div><div style="display:flex; gap:10px; margin-top:15px;"><input type="text" id="nick-yon" placeholder="İsim" style="width:20%;"><input type="text" id="msg-yon" placeholder="Mesaj..." style="width:80%;"><button onclick="mesajGonder('sohbet_yon', 'nick-yon', 'msg-yon')" class="action-btn" style="background:var(--gold); color:black; width:100px;">GÖNDER</button></div></div></div>
</div>

<script>
    // BAĞLANTI AYARLARI
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
    let aktifRol = "uye";

    // GİRİŞ SİSTEMİ
    function giris(tip) {
        let s = prompt("Şifre:");
        if(tip==='kurucu' && s==='1907akkus') { aktifRol="kurucu"; document.getElementById('app-body').className='kurucu-mode'; sayfa(13); }
        else if(tip==='admin' && s==='admin123') { aktifRol="admin"; document.getElementById('app-body').className='admin-mode'; sayfa(13); }
        else if(tip==='yonetici' && s==='yonetici123') { aktifRol="yonetici"; document.getElementById('app-body').className='yonetici-mode'; sayfa(13); }
        else { alert("Hatalı Şifre!"); }
    }

    function sayfa(n, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('aktif'));
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active-btn'));
        document.getElementById('p'+n).classList.add('aktif');
        if(btn) btn.classList.add('active-btn');
    }

    // ARKA PLAN AYARI
    function setBg() { 
        const url = document.getElementById('bg-url').value;
        if(url) db.collection("ayarlar").doc("tema").set({bg: url}).then(() => alert("Güncellendi!"));
    }

    db.collection("ayarlar").doc("tema").onSnapshot(d => { 
        if(d.exists()) {
            const url = d.data().bg;
            document.body.style.backgroundImage = `url('${url}')`;
        } 
    });

    // İÇERİK EKLEME
    async function urlIleYukle() {
        const t = document.getElementById('i-tip').value;
        const b = document.getElementById('i-bas').value;
        const u = document.getElementById('i-url').value;
        if(!u || !b) return alert("Eksik bilgi!");
        await db.collection("icerik").add({tip:t, bas:b, url:u, tarih:Date.now()});
        alert("Eklendi!");
        document.getElementById('i-url').value = "";
        document.getElementById('i-bas').value = "";
    }

    // KADRO LİSTELEME
    db.collection("ekip").onSnapshot(s => {
        let u=0, y=0, uH="", yH="";
        s.docs.forEach(d => {
            const dt = d.data();
            const h = `<div class="card"><span class="badge b-${dt.rol}">${dt.rol.toUpperCase()}</span><br><b>${dt.ad}</b><button class="del-btn" onclick="sil('ekip','${d.id}')">KADRODAN AT</button></div>`;
            if(dt.rol==='uye') { u++; uH+=h; } else { y++; yH+=h; }
        });
        document.getElementById('count-uye').innerText = u;
        document.getElementById('count-yon').innerText = y;
        document.getElementById('list-uye-adlari').innerHTML = uH;
        document.getElementById('list-yon-adlari').innerHTML = yH;
    });

    // İÇERİK LİSTELEME (İNDİR BUTONLU)
    db.collection("icerik").orderBy("tarih","desc").onSnapshot(s => {
        let g="", m="";
        s.docs.forEach(d => {
            const dt = d.data();
            const downBtn = `<a href="${dt.url}" target="_blank" class="download-btn">📥 MODU İNDİR</a>`;
            const cardHtml = `<div class="card">
                                <img src="${dt.url}" class="img-item" onerror="this.src='https://via.placeholder.com/300x200?text=Indirilebilir+Dosya'">
                                <br><b>${dt.bas}</b><br>
                                ${dt.tip==='mod' ? downBtn : ''}
                                <button class="del-btn" onclick="sil('icerik','${d.id}')">SİL</button>
                             </div>`;
            if(dt.tip==='galeri') g += cardHtml; else m += cardHtml;
        });
        document.getElementById('list-galeri').innerHTML = g;
        document.getElementById('list-mod').innerHTML = m;
    });

    // TELSİZ
    function mesajGonder(c, n, m) {
        const ni = document.getElementById(n).value;
        const me = document.getElementById(m).value;
        if(ni && me) db.collection(c).add({isim:ni, msg:me, tarih:Date.now()});
        document.getElementById(m).value = "";
    }
    db.collection("sohbet_uye").orderBy("tarih","desc").limit(15).onSnapshot(s => { document.getElementById('msg-list-uye').innerHTML = s.docs.map(d => `<div><b>${d.data().isim}:</b> ${d.data().msg}</div>`).reverse().join(''); });
    db.collection("sohbet_yon").orderBy("tarih","desc").limit(15).onSnapshot(s => { document.getElementById('msg-list-yon').innerHTML = s.docs.map(d => `<div style="color:var(--gold)"><b>[YÖN] ${d.data().isim}:</b> ${d.data().msg}</div>`).reverse().join(''); });

    // YÖNETİM
    function rolVer() { db.collection("ekip").add({ad: document.getElementById('r-ad').value, rol: document.getElementById('r-rol').value}); }
    function basvur() { db.collection("basvurular").add({ad: document.getElementById('k-ad').value}); alert("Gitti!"); }
    db.collection("basvurular").onSnapshot(s => { if(aktifRol!=='uye') document.getElementById('list-basvuru').innerHTML = s.docs.map(d => `<div class="card">👤 ${d.data().ad} <button class="action-btn" style="width:auto; padding:5px;" onclick="sil('basvurular','${d.id}')">ONAYLA</button></div>`).join(''); });
    function sil(c, id) { if(confirm("Silinsin mi?")) db.collection(c).doc(id).delete(); }
</script>
</body>
</html>
