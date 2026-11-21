# COMPAS verisinde sızıntısız model değerlendirme

BIL545 Veri Madenciliğinde İleri Konular · 2026–2027 Güz · Üsküdar Üniversitesi  
Dr. Selma Güzel · Proje 1 · Konu 9.16 Yeniden suç işleme risk skorları COMPAS

## Grup ve çalışma kapsamı

| Üye | Öğrenci no |
|---|---|
| Yasin Göktepe | 264327007 |

- Grup adı: COMPAS Yasin Göktepe bireysel çalışma
- İletişim sorumlusu: Yasin Göktepe
- Grup tipi: 1 kişi, 1–3 kişilik temel kapsam. Bölüm 1–3 tamamlandı.
- Proje 1 defteri: [Çalıştırılmış notebook](proje1/BIL545_Proje1_COMPAS_Yasin_Goktepe.ipynb).

## Soru

Tarama anında bilinebilen özelliklerle iki yıllık yeniden suç kaydı vekili ne kadar öngörülebilir ve veri sızıntısı değerlendirmeyi nasıl değiştirir?

Bu akademik çalışma COMPAS'ın kapalı algoritmasını yeniden üretmez. Kişi hakkında karar vermek için kullanılamaz. Hedef suç davranışının doğrudan ölçümü değildir. Irksal grup hata oranlarının tam adillik analizi Proje 2 için araştırma yönüdür; Proje 1'de yalnız temel küp ve yöntem sınırlamaları incelenir.

## Veri

| Alan | Bilgi |
|---|---|
| Veri kümesi | ProPublica compas-scores-two-years.csv |
| Resmi kaynak | https://github.com/propublica/compas-analysis |
| Doğrudan indirme | https://raw.githubusercontent.com/propublica/compas-analysis/master/compas-scores-two-years.csv |
| Lisans | Resmi depoda açık lisans doğrulanamadı. Açık erişim yeniden dağıtım hakkı varsayımı değildir. |
| Ham boyut | 7.214 kayıt × 53 sütun |
| Analiz kohortu | Kanonik ProPublica filtreleri sonrası 6.172 kişi |
| Model girdisi | 8 güvenli özellik; kimlik, gelecek olay, tarih ve COMPAS skorları hariç |
| Hedef | two_year_recid, iki yıllık yeniden suç kaydı vekili |
| SHA-256 | c451db85908b2f7fef1d83203bedf6b71ecda0d5af468d82ae62178f91d0cc7d |

Veri depoda yoktur. Notebook resmi kaynaktan indirir ve SHA-256 kontrolü yapar. Ham ad, doğum tarihi veya kişi düzeyinde örnekler notebook çıktısında gösterilmez. Yerel olarak üretilen temiz veri, ARFF ve model dosyaları da bu pakete alınmadı.

## Depo yapısı

```text
.
├── README.md
├── requirements.txt
├── .gitignore
└── proje1/
    ├── BIL545_Proje1_COMPAS_Yasin_Goktepe.ipynb
    └── kanitlar/
        ├── sonuclar.json
        ├── ortam_colab.json
        ├── dogrulama.json
        ├── kat_skorlari.csv
        ├── model_karsilastirma.csv
        ├── weka_ZeroR.txt
        ├── weka_J48.txt
        ├── weka_Apriori.txt
        └── grafikler ve toplu denetim tabloları
```

Proje 2 bu pakette yapılmadı. Tüm resmi yazılı cevaplar, kodlar ve çıktılar tek notebook içindedir. `kanitlar/` aynı çalıştırmanın toplu tekrar üretim kayıtlarıdır, ayrı rapor değildir.

## Çalıştırma

1. [Google Colab](https://colab.research.google.com/) açın. Dosya → Not defteri yükle ile `proje1/BIL545_Proje1_COMPAS_Yasin_Goktepe.ipynb` dosyasını seçin. GitHub'a yayımlandıktan sonra Dosya → Not defteri aç → GitHub yolu da kullanılabilir.
2. Standart Python CPU çalışma zamanını kullanın. GPU gerekmez. İnternet, veri ve Weka indirmesinde gereklidir.
3. Temiz oturumda Çalışma Zamanı → Tümünü çalıştır seçin. İlk hücre tohum ve sürümleri yazdırır, scikit-learn 1.6.1 / mlxtend 0.25.0 sürümlerini sabitler. Eski bir sklearn sürümü önceden import edilmişse sürüm mesajındaki gibi oturumu yeniden başlatıp tümünü tekrar çalıştırın.
4. Weka Java üzerinden çalışır. Colab'da Java vardır. Yerel çalıştırmada Java 8 veya üzeri gerekir; Python paketleri aşağıdaki iki aşamalı kurulumla hazırlanır. Windows'ta Java yolu otomatik denetlenir.
5. Dosyalar çalışma klasöründeki `calisma_ciktilari/` içinde yeniden üretilir. `yerel_ozel/` altındaki veri/model/ARFF dosyalarını GitHub'a yüklemeyin.

Notebook 15 Kasım 2026 tarihinde Google Colab üzerinde öğrenci tarafından çalıştırıldı. İndirilen `.ipynb` dosyasında 19 kod hücresinin tamamının çıktıları ve 7 grafik bulunuyor; çalıştırma sayıları 20–38 arasında ardışık, hata çıktısı yok. Çalıştırma sayacının 1'den başlaması gerekmez; sayıların özgün kayıtları korunmuştur. Kanıt klasöründeki JSON, CSV, PNG ve Weka metinleri aynı Colab çalıştırmasından dışa aktarıldı. 90 kat skorunun özet istatistikleri yeniden hesaplanarak kontrol edildi ve Weka ile 12/12 kural eşleşmesi doğrulandı.

### Doğrulanan Colab ortamı

| Bileşen | Sürüm |
|---|---|
| Python | 3.13.18 |
| NumPy | 2.4.6 |
| pandas | 2.2.3 |
| scikit-learn | 1.6.1 |
| SciPy | 1.16.3 |
| Matplotlib | 3.10.0 |
| mlxtend | 0.25.0 |

Sürümler ilk hücrenin gerçek çıktısından ve `proje1/kanitlar/ortam_colab.json` dosyasından alınmıştır. İlk yerel çalıştırmadaki raporlanan sayısal sonuçlarla Colab sonuçları aynıdır; çalışma süreleri ortama göre değişir. `requirements.txt` bu Colab çalıştırmasının temel paket sürümlerini içerir.

Yerel bir Python 3.13 ortamı kurulacaksa sırasıyla `python -m pip install -r requirements.txt` ve `python -m pip install --no-deps mlxtend==0.25.0` çalıştırılır. mlxtend, ders duyurusundaki gibi bağımlılıkları değiştirmeden ayrı kurulur; bu çalışmada kullanılan FP-Growth ve işlem kodlama işlevleri kaydedilen ortamda doğrulanmıştır. Bu yöntem bütün mlxtend işlevleri için genel uyumluluk iddiası değildir.

### Çalıştırma uyarıları

İlk hücredeki dar filtre, yalnız `jupyter_client.session` kaynaklı `datetime.utcnow()` deprecation mesajını hedefler. `frequent_patterns` ve `TransactionEncoder` importları filtreden önce yapıldığı için sonraki import uyarı ayarını tekrar değiştirmez. Colab dosyasında bu uyarı yoktur.

Çıktılarda scikit-learn 1.6.1'in SciPy L-BFGS-B çağrısındaki `disp`/`iprint` seçeneklerine ilişkin deprecation mesajları ile Java'nın Cgroup/MTJ kaynak uyarıları korunmuştur. Bunlar çalıştırmayı durdurmamıştır; hata çıktısı, yakınsama uyarısı veya geçersiz sayı uyarısı saptanmadı. Hesaplama uyarılarını topluca gizleyen bir filtre kullanılmadı. Dosya tamamen uyarısız olarak sunulmaz.

## Değerlendirme protokolü

- Tohum 42; katmanlı %80 geliştirme (4.937), %20 ayrılmış test (1.235).
- Geliştirmede aynı 5 kat × 3 tekrar. Bütün geçerli ön işleme Pipeline içinde.
- Ana ölçüt balanced accuracy (BA). AUC, F1, precision ve recall ikincil.
- Model sıralaması yalnız geliştirmede sabitlendi. İlk iki model aynı bağımsız testte exact McNemar ile karşılaştırıldı. McNemar BA/AUC farkını doğrudan sınamaz.
- Ayrı karar ağacı araması: 3 iç kat / 5 dış kat. Yanlış global ön işleme ve yanlış global hedef kodlama yalnız öğretici deneylerdir.
- Birliktelik B: destek .03 (CAR için gerçek alt adet 149), güven .60, en fazla iki öncül. Weka CAR `-A -N 2147483647 -U 1.0 -M .03 -C .60`; U=M bu sürümde yüksek destekli kuralları silebildiğinden kullanılmaz.

## Proje 1 sonuç özeti

| Model | Geliştirme BA ortalama ± std |
|---|---|
| Gradyan artırma | 0.6632 ± 0.0175 |
| Lojistik | 0.6616 ± 0.0168 |
| Rastgele orman | 0.6607 ± 0.0193 |
| Karar ağacı | 0.6559 ± 0.0199 |
| k-NN | 0.6433 ± 0.0143 |
| Dummy | 0.5000 ± 0.0000 |

- Seçilen model: **Gradyan artırma**. Standart sapma katlar arası değişkenliktir, güven aralığı değildir.
- Doğru hedef kodlama: 0.6575 ± 0.0187; **geçersiz** global hedef kodlama: 0.6857 ± 0.0147.
- Doğru ön işleme: 0.6616 ± 0.0168; **geçersiz** global ön işleme: 0.6588 ± 0.0173. Farkın küçük veya ters yönde olması yanlış yöntemi geçerli yapmaz.
- Ayrılmış testte en iyi iki modelin uyuşmayan hata sayıları 75 ve 79, exact McNemar p=0.8091. Anlamlı fark bulunmadı; eşitlik kanıtı değildir.
- Weka ZeroR BA=0.5000; J48 BA=0.6577. J48/C4.5 ve Python CART farklı algoritmalardır.
- Python ve Weka Apriori ortak kapsamda **12/12 kuralda**, ortak kayıt adetleriyle eşleşti.
- `age` yerine tarama anından hesaplanan yaş kullanıldı. İlk yaş dilimi “25 altı”dır; 10 kayıtta türetilmiş yaş 18'in altında olduğundan “18–24” denmedi.
- 1 Nisan 2014 sonrası 868 kaydın tamamı pozitif. Tarihler model girdisinden çıkarıldı; erken dönem duyarlılığı BA=0.6480 ± 0.0087. Bu kontrol veri seçilim problemini tamamen düzeltmez.

## Sunum videosu ve teslim durumu

**[Sunum videosunu izle](https://drive.google.com/file/d/1JcfYduPzdD2myVyBZvJINpqfuLIE4u_0/view?usp=sharing)**  
**Süre:** 17 dakika 14 saniye.

Yasin Göktepe sunum videosunu kendi anlatımıyla kaydedip bölümleri birleştirdi ve Google Drive'a yükledi. Video bağlantısı ve süre 21 Kasım 2026 tarihinde README'ye eklendi.

4 Kasım 2026 duyurusuna göre son teslim **21 Kasım 2026 Cumartesi 23.59**, yalnız GitHub deposudur. Notebook `proje1/` altında, güncel README kökte olmalıdır. Son commit zamanı teslim zamanıdır. STIX'e ayrıca yükleme gerekmiyor. Geç teslimde günlük %5 kesinti ve 3 gün sınırı duyuruda belirtilmiştir.

## Katkı beyanı

| Kişi | Kapsam ve işler | Sunum |
|---|---|---|
| Yasin Göktepe | Bireysel Proje 1 Bölüm 1.1–3.4. Colab hücrelerini sırasıyla çalıştırma, çıktıları inceleme ve paylaşma, yeniden çalıştırma, uyarı filtresini uygulama ve çıktı dosyalarını dışa aktarma. Kaynak, kod, analiz, yorum ve sunum hazırlığında kapsamlı yapay zekâ desteği kullanıldı. | Proje 1 sunumunu kendi anlatımıyla kaydetti, video bölümlerini birleştirdi ve 17 dakika 14 saniyelik videoyu Google Drive'da paylaştı. |

Gerçek GitHub commit geçmişiyle tutarlı katkı kaydı tutulmalıdır. Geriye dönük veya sahte commit oluşturulmadı.

## Yapay zekâ beyanı

OpenAI Codex; ders belgelerini gereksinimlerle eşleştirme, resmi kaynakları araştırma, kod üretme/düzeltme, deneyleri yerel çalıştırma, öğrenciye Colab çalıştırması ve dışa aktarmada adım adım rehberlik etme, sonuç yorumları ve sunum/açıklama taslaklarında kapsamlı kullanıldı. Bu içerik insan tarafından sıfırdan üretilmiş olarak sunulmaz. Öğrenci Colab çalıştırması ve dışa aktarma adımlarını tamamladı; sunumu kendi anlatımıyla kaydedip video bölümlerini birleştirdi. Konuşma metninin ilk ve daha doğal anlatımlı taslakları Codex desteğiyle hazırlandı.

## Kaynaklar

- ProPublica veri ve kod: https://github.com/propublica/compas-analysis
- ProPublica yöntem: https://www.propublica.org/article/how-we-analyzed-the-compas-recidivism-algorithm
- Kaufman vd. 2012: https://doi.org/10.1145/2382577.2382579
- Dietterich 1998: https://doi.org/10.1162/089976698300017197
- Wu, Chen ve Han 2010: https://doi.org/10.1007/s10618-009-0161-2
- Barenstein 2019: https://arxiv.org/abs/1906.04711
- scikit-learn TargetEncoder: https://scikit-learn.org/1.6/modules/generated/sklearn.preprocessing.TargetEncoder.html
- Weka Apriori kaynak kodu: https://github.com/Waikato/weka-3.8/blob/master/weka/src/main/java/weka/associations/Apriori.java
- BIL545 güncel yönerge/değerlendirme/README/notebook şablonları (20261007), Hafta 2–7 ders notları ve 29 Ekim / 4 Kasım 2026 STIX duyuruları.
