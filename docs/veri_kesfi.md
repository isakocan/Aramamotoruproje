# Veri Keşfi – MüşteriYorumları (26 Kasım 2026)

Kaynak: https://huggingface.co/datasets/turkish-nlp-suite/MusteriYorumlari (`data/raw/` altına indirildi, repoda yok)

## Boyut
| | 1⭐ | 2⭐ | 3⭐ | 4⭐ | 5⭐ | Toplam |
|---|---|---|---|---|---|---|
| train | 9.053 | 8.099 | 12.883 | 22.142 | 21.743 | 73.920 |
| valid | 1.905 | 1.711 | 2.629 | 4.524 | 4.231 | 15.000 |
| test | 1.915 | 1.662 | 2.542 | 4.541 | 4.340 | 15.000 |
| **Toplam** | 12.873 | **11.472** | 18.054 | 31.207 | 30.314 | 103.920 |

→ Üç parçayı birleştirip kendi 80/20 bölmemizi yapacağız. En küçük sınıf (2⭐) 11.472, yani temizlikten sonra da 10k/sınıf büyük ihtimalle yetiyor.

## Uzunluk
- Ortanca **62 karakter / ~10 kelime**. Yorumların çoğu kısa.
- %99'u 438 karakterin altında, en uzunu 2.436 karakter.
- 1⭐ yorumlar en uzunu (ort. 15 kelime), 4–5⭐ en kısası (ort. 12). Kızgın müşteri daha çok yazıyor.

## Bulunan sorunlar (ajanların işi)
| Sorun | Sayı | Hangi ajan |
|---|---|---|
| **Baştaki ve sondaki küçük "n" harfleri silinmiş** ("güzel ürü", "kalı", "alı") — kaynakta bir hata. 104k yorumdan sadece 2'si "n" ile bitiyor, hiçbiri "n" ile başlamıyor. | Tüm veri | 1 (raporlanır), 3 (karakter n-gramları bunu tolere eder) |
| Türkçe büyük/küçük harf sorunu: "İdare eder" → `"i̇dare eder"` (Python'un `lower()` hatası) | – | 1 |
| Türkçe karakter kullanılmamış yorumlar ("cok guzel") | %3,7 | 1 / 3 |
| Tekrar eden metinler | 1.351 satır | 2 |
| Aynı metin, farklı yıldız | 250 metin | 2 / 4 |
| Hem train hem test'te geçen metin | 100 | 2 (birleştirip tekrarları silince çözülür) |
| Çok kısa yorumlar (≤ 2 kelime) | 4.054 | 2 (eşik config'ten) |
| Metni yıldızıyla çelişenler ("Mükemmel bir alet, tavsiye ederim" → 1⭐) | basit kalıpla ~320 | 4 |
| Aşırı uzun yorumlar | %1 > 438 karakter | 4 |
| Boş metin | 0 | – |

## Metin özellikleri (Ajan 3 için ipuçları)
- Emoji içeren: %3,7. En çok 5⭐'da (0,088/yorum), sonra 1⭐'da.
- Ünlem: nadir (%0,7), ama en çok 1⭐'da. Kızgınlık işareti.
- Büyük harf oranı: en yüksek 5⭐ ve 1⭐'da. Uç duygular bağırıyor.
- 87.404 farklı kelime, bunların 51.003'ü yalnızca bir kez geçiyor (yazım hataları + Türkçe ekler) → TF-IDF'te kelime sayısı sınırlanmalı, karakter n-gramları faydalı.
- 1⭐ en sık kelimeler: iade, değil, kötü, kesinlikle, bile
- 5⭐ en sık kelimeler: güzel, kaliteli, tavsiye, hızlı, teşekkürler, kargo
