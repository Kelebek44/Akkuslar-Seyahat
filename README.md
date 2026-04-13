<div class="card editor-only">
    <h3>İÇERİK EKLE (URL İLE)</h3>
    <select id="i-tip">
        <option value="galeri">Galeri / Kaplama</option>
        <option value="mod">Mod Merkezi</option>
    </select>
    <input type="text" id="i-bas" placeholder="Dosya Başlığı">
    <input type="text" id="i-url" placeholder="Resim Linkini Buraya Yapıştır (https://...)">
    <button class="action-btn" onclick="urlIleYukle()">SİSTEME EKLE</button>
</div>

<script>
async function urlIleYukle() {
    const tip = document.getElementById('i-tip').value;
    const bas = document.getElementById('i-bas').value;
    const url = document.getElementById('i-url').value;

    if(!url || !bas) return alert("Kaptan, link veya başlık eksik!");

    try {
        await db.collection("icerik").add({
            tip: tip,
            bas: bas,
            url: url,
            tarih: Date.now()
        });
        alert("✅ Resim anında eklendi!");
        document.getElementById('i-url').value = "";
    } catch (e) { alert("Hata: " + e.message); }
}
</script>
