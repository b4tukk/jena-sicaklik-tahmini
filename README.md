# 24 Saat Sonraki Sıcaklığı Tahmin Etme: Jena Hava İstasyonu

Derin öğrenme (zaman serisi) projesi. Son 72 saatlik çok değişkenli hava gözleminden **24 saat sonraki sıcaklığı** tahmin eden modelleri, basit referanslarla ve klasik ML ile **zamana göre bölünmüş** test verisinde (2016) karşılaştırır.

- `jena_sicaklik_tahmini.ipynb`: çalıştırılmış not defteri (veri yoksa özgün kaynaktan indirir)
- `Rapor.docx`: 3 sayfalık proje raporu
- `figures/`, `metrics.json`: grafikler ve sayısal sonuçlar

## Yöntem
İki referans (süreklilik, iklimsel ortalama) · Ridge · HistGradientBoosting · LSTM · GRU · 1B-CNN. Derin modeller 3 tohumla eğitilir; farklar gün bloklu bootstrap ile sınanır; hata mevsime ve saate göre incelenir.

## Sonuçlar (test 2016, MAE °C)
| Model | MAE | R² |
|---|---|---|
| Süreklilik (yarın = bugün) | 2,44 | 0,847 |
| İklimsel ortalama | 3,29 | 0,742 |
| Ridge | 2,26 | 0,876 |
| HistGradientBoosting | 2,19 | 0,884 |
| LSTM (3 tohum ort.) | 2,15 | 0,888 |
| **GRU (3 tohum ort.)** | **2,11** | **0,893** |
| 1B-CNN (3 tohum ort.) | 2,34 | 0,871 |
| GRU topluluğu (3 tohum) | 2,09 | 0,895 |

- Derin modelin kazancı gerçek (bootstrap güven aralıkları sıfırı içermiyor) ama küçük: Ridge'e göre ≈ 0,17 °C.
- İklimsel ortalama süreklilikten kötü: sadece mevsim ve saat yetmez, güncel gözlem gerekir.
- Hata kış aylarında büyür; saatler arası fark azdır.

## Çalıştırma
```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace jena_sicaklik_tahmini.ipynb
```
Veri: Max Planck Biyojeokimya Enstitüsü, Jena hava istasyonu. https://www.bgc-jena.mpg.de/wetter/
