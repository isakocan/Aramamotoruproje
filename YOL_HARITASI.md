# BLM3120 Dönem Projesi – Yol Haritası

Kaynak: `BLM3120-TermProject-2026-2027.pdf`
Başlangıç: 23 Kasım 2026

## Sabit tarihler
| Tarih | Ne |
|---|---|
| **30 Kasım (Pzt)** | 1 sayfalık öneri – sınıfta basılı teslim |
| **13 Aralık (Paz) 23:59** | Kod + rapor (2–5 sayfa) zip → mail |
| **14 / 21 Aralık (Pzt)** | Sınıfta demo + anında değişiklik |

## Fazlar

### Faz 0 – Kararlar (23–25 Kasım)
- [x] Aile: **Supervised Learning** — 3 algoritma: **Decision Tree** (baseline) / **k-NN** (alternatif) / **Random Forest** (gelişmiş). Naive Bayes: vakit kalırsa ek referans.
- [x] Veri seti: **MüşteriYorumları** (Hepsiburada + Trendyol, 103.920 yorum, CC-BY-SA) — https://huggingface.co/datasets/turkish-nlp-suite/MusteriYorumlari
- [x] Görev: **5 sınıf** (1–5 yıldız tahmini); sınıf biçimi config parametresi
- [x] Örnekleme: **dengeli, her yıldızdan 10k = 50k** (sınıf başı sayı config'ten). Dengesiz veri deneyi yok.
- [x] Bölme: train/test 80/20 (40k / 10k), stratified. Parametre seçimi + anlamlılık testi için eğitim verisi içinde **stratified 5-fold CV** (fold sayısı config'ten).
- [x] LLM: **NVIDIA Build** – `nvidia/nemotron-3.5-lightning-30b-a3b`, `enable_thinking: False` (gerekirse ana ajan için `nemotron-3-ultra-550b-a55b`). Key `.env` içinde (`.env.example` yok).
- [x] Agentic kütüphane: **smolagents** — `ToolCallingAgent` (CodeAgent değil; hesap tool'larda kalsın), ana ajan görev ajanlarını managed agent olarak çağırır, kullanıcı onayı `ask_user` tool'u ile
- [x] GUI: **Streamlit** (Claude'un seçimi) — özel tema + CSS ile cilalı görünüm; ajanlar arka planda çalışır, ekran canlı yenilenir. Tasarım aşamasında mockup gösterilecek.
- [x] Grup: **2 kişi** (ad-soyad + öğrenci no öneriye yazılacak; demoda ikisi de her yeri bilmeli)

### Faz 1 – Öneri + veri keşfi (26–29 Kasım)
- [x] Veri setini indir, kayıt sayısı / format / dağılımları çıkar → `docs/veri_kesfi.md`
- [x] 1 sayfalık öneriyi yaz → `oneri/proposal.pdf`
- [ ] Çıktı al → **30 Kasım teslim**

### Faz 2 – Tool'lar, LLM olmadan (30 Kasım – 3 Aralık)
- [ ] Proje iskeleti, `config` dosyası (tüm parametreler tek yerde)
- [ ] 6 görevin Python fonksiyonları (transform, filter, features, outlier, run, evaluate)
- [ ] Shared workspace: her adım dosyaya yazar, bir sonraki dosyadan okur
- [ ] Testler + uçtan uca düz Python pipeline çalışıyor

### Faz 3 – Ajan katmanı (3–6 Aralık)
- [ ] 6 görev ajanı (her biri sadece kendi tool'ları)
- [ ] Ana ajan: plan → onay → sırayla ajan başlat → raporu kontrol et → tekrar/devam
- [ ] Kalıcı değişiklik öncesi kullanıcı onayı (outlier silme)
- [ ] Her LLM çağrısının loglanması (ajan, mesaj, cevap, süre)
- [ ] Takip istekleri: parametre değiştir, son N ajanı tekrar çalıştır

### Faz 4 – GUI (6–9 Aralık)
- [ ] Ekran 1: Dataset Manager
- [ ] Ekran 2: Workflow Runner & Agent Monitor (sohbet, canlı durum, mesajlar)
- [ ] Ekran 3: Results & Analysis (tablolar, grafikler, örnek çıktılar, export)

### Faz 5 – Performans deneyleri (9–11 Aralık)
- [ ] %10 / %50 / %100 veri boyutunda süreler (ajan + algoritma)
- [ ] Ana parametrenin etkisi
- [ ] Outlier adımı açık / kapalı karşılaştırması
- [ ] Bellek kullanımı, ölçeklenebilirlik
- [ ] LLM çağrı sayısı / süresi (ajan bazında)
- [ ] İstatistiksel anlamlılık testi
- [ ] Grafikler (bar, runtime eğrisi, vb.)

### Faz 6 – Rapor + teslim (11–13 Aralık)
- [ ] Rapor (PDF'teki 6. bölüm maddelerinin hepsi)
- [ ] Ekran görüntüleri, tasarım diyagramı, prompt örnekleri
- [ ] `TP_OgrenciNo.zip` → blm3581ir@gmail.com — **13 Aralık 23:59**

### Faz 7 – Demo hazırlığı (14 Aralık öncesi)
- [ ] Demo senaryosu (4–5 dk) provası
- [ ] Olası anlık değişiklikleri dene: parametre değiştirme, rapora alan ekleme, bir ajanı kapatma

## Tasarım ilkeleri
- **Önce tool'lar, sonra ajanlar:** sayılar her zaman tool'dan gelir, LLM sadece karar verir.
- **Veri LLM'e gitmez:** sadece yol, sayı, küçük örnek, özet istatistik.
- **Her şey config'te:** demo'daki anlık değişiklikler tek satırlık iş olsun.
- **Ajanlar açılıp kapatılabilir** (config bayrağı) — demo'da istenebilir.
