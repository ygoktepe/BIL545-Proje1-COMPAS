# COMPAS verisinde sızıntısız model değerlendirme

BIL545 · Veri Madenciliğinde İleri Konular · 2026–2027 Güz  
Üsküdar Üniversitesi · Dr. Selma Güzel  
Proje 1 · Konu 9.16: Yeniden suç işleme risk skorları (COMPAS)

Bu projede COMPAS verisini inceleyerek veri hazırlama, sınıflandırma, veri küpü ve birliktelik analizi çalışmalarını bir araya getirdim. Modelleri ortak bir değerlendirme düzeniyle karşılaştırdım; veri sızıntısının sonuçları nasıl etkilediğini ayrı deneylerle gösterdim.

## Grup ve çalışma kapsamı

| Üye | Öğrenci no |
|---|---|
| Yasin Göktepe | 264327007 |

- **Grup adı:** COMPAS — Yasin Göktepe bireysel çalışma
- **İletişim sorumlusu:** Yasin Göktepe
- **Kapsam:** 1–3 kişilik gruplar için belirlenen temel kapsam; Proje 1 Bölüm 1.1–3.4.
- **Proje defteri:** [Çalıştırılmış notebook](proje1/BIL545_Proje1_COMPAS_Yasin_Goktepe.ipynb)

## Araştırma sorusu

İlk COMPAS değerlendirmesi sırasında bilinebilen özelliklerle iki yıl içinde kayda geçen yeniden tutuklanma ne ölçüde öngörülebilir ve veri sızıntısı model değerlendirmesini nasıl değiştirir?

Hedef değişkeni, suç davranışının doğrudan ölçümü olarak değerlendirmedim. Yeniden tutuklanma kayıtlarının oluşmasında takip ve kayıt süreçlerinin de etkili olduğunu dikkate aldım. Bu çalışmada Proje 1'in veri ve model değerlendirme kapsamına odaklandım; grup hata oranları ve kalibrasyon gibi ayrıntılı adillik incelemelerini Proje 2 için bıraktım.

## Veri kümesi

| Alan | Bilgi |
|---|---|
| Veri kümesi | ProPublica — `compas-scores-two-years.csv` |
| Kaynak | [ProPublica COMPAS analiz deposu](https://github.com/propublica/compas-analysis) |
| İndirme bağlantısı | [CSV dosyası](https://raw.githubusercontent.com/propublica/compas-analysis/master/compas-scores-two-years.csv) |
| Lisans | Kaynak verinin açık lisans bilgisi doğrulanamadı; ham veri bu depoya eklenmedi. |
| Ham boyut | 7.214 kayıt × 53 sütun |
| Analiz örneklemi | ProPublica filtreleri sonrasında 6.172 kişi |
| Model girdileri | Değerlendirme anında bilinebilen 8 özellik |
| Hedef değişken | `two_year_recid`: iki yıllık yeniden tutuklanma kaydı vekili |
| Dosya SHA-256 | `c451db85908b2f7fef1d83203bedf6b71ecda0d5af468d82ae62178f91d0cc7d` |

Veri, defter çalıştırıldığında resmî kaynaktan indirilir ve dosya özeti kontrol edilir. Model girdilerini seçerken kimlik bilgilerini, sonraki olaylara ait alanları, ham tarihleri ve mevcut COMPAS skorlarını çıkardım. Irk bilgisini model girdisi olarak kullanmadım; veri küpündeki betimsel incelemede ayrı değerlendirdim.

Kişi adları, doğum tarihleri ve kişi düzeyindeki örnekler defter çıktılarında gösterilmez. Ham veri, temizlenmiş kişi tablosu, ARFF dosyaları ve eğitilmiş model dosyaları depoda bulunmaz.

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
        ├── protokol.json
        ├── ortam_colab.json
        ├── dogrulama.json
        ├── kat_skorlari.csv
        ├── model_karsilastirma.csv
        ├── hedef_kurallari.csv
        ├── ozellik_denetimi.csv
        ├── weka_ZeroR.txt
        ├── weka_J48.txt
        ├── weka_Apriori.txt
        └── analiz grafikleri (PNG)
```

Kodlar, yazılı yanıtlar ve sonuç yorumları tek defterdedir. `kanitlar/` klasörü, aynı Colab çalıştırmasından dışa aktardığım toplu sonuçları ve Weka çıktılarını içerir.

## Çalıştırma

1. [Google Colab](https://colab.research.google.com/) üzerinde **Dosya → Not defteri aç → GitHub** yolunu izleyin. Depo adresi olarak `https://github.com/ygoktepe/BIL545-Proje1-COMPAS` girip `proje1/BIL545_Proje1_COMPAS_Yasin_Goktepe.ipynb` dosyasını seçin. Dosyayı indirip **Not defteri yükle** seçeneğiyle de açabilirsiniz.
2. Standart Python CPU çalışma zamanını kullanın. GPU gerekmez; veri ve Weka indirmeleri için internet bağlantısı gerekir.
3. Temiz bir oturumda **Çalışma Zamanı → Tümünü çalıştır** seçeneğini kullanın. İlk hücre sürümleri ve rastgelelik tohumunu yazdırır. Oturumda daha önce farklı bir scikit-learn sürümü yüklendiyse ilk hücredeki yönlendirmeye göre oturumu yeniden başlatın.
4. Weka Java üzerinden çalışır. Colab'da Java bulunur; yerel çalıştırmada Java 8 veya üzeri gerekir.

Yerel Python 3.13 ortamı için kurulum:

```bash
python -m pip install -r requirements.txt
python -m pip install --no-deps mlxtend==0.25.0
```

mlxtend'i ders duyurusundaki gibi bağımlılıkları değiştirmeden ayrı kurdum. Bu çalışmada kullandığım FP-Growth ve işlem kodlama işlevlerini aşağıdaki ortamda doğruladım.

### Çalıştırma kaydı

Defteri 15 Kasım 2026'da Google Colab üzerinde çalıştırdım. Depodaki sürümde 19 kod hücresinin çıktıları ve 7 grafik bulunuyor; hata çıktısı yok. JSON, CSV, PNG ve Weka kayıtlarını bu çalıştırmadan dışa aktardım.

| Bileşen | Sürüm |
|---|---|
| Python | 3.13.18 |
| NumPy | 2.4.6 |
| pandas | 2.2.3 |
| scikit-learn | 1.6.1 |
| SciPy | 1.16.3 |
| Matplotlib | 3.10.0 |
| mlxtend | 0.25.0 |

Sürüm kayıtları `proje1/kanitlar/ortam_colab.json` dosyasındadır. Çalıştırma süreleri kullanılan ortama göre değişebilir.

Çıktılarda scikit-learn/SciPy kaynaklı `disp`/`iprint` kullanımdan kaldırma uyarıları ve Java Cgroup/MTJ uyarıları görülebilir. Bu uyarılar çalıştırmayı durdurmadı; yakınsama veya geçersiz sayı uyarısı oluşmadı. `jupyter_client.session` kaynaklı `datetime.utcnow()` uyarısı için sınırlı bir filtre kullandım.

## Değerlendirme düzeni

- Rastgelelik tohumu: **42**.
- Katmanlı bölme: **%80 geliştirme (4.937 kişi)** ve **%20 test (1.235 kişi)**.
- Geliştirme verisinde bütün modeller için aynı **5 kat × 3 tekrar** bölmeleri.
- Ana ölçüt: **dengeli doğruluk (BA)**. ROC AUC, F1, kesinlik ve duyarlılık ikincil ölçütler.
- Eksik değer doldurma, ölçekleme ve kodlama adımları her eğitim katında **Pipeline** içinde öğrenildi.
- Karar ağacı parametre aramasında **3 iç / 5 dış katlı iç içe çapraz doğrulama** kullanıldı.
- Geliştirme sonuçlarına göre seçilen iki modelin ayrılmış testteki hataları **exact McNemar testiyle** karşılaştırıldı. Bu test BA veya AUC farkını doğrudan sınamaz.
- Birliktelik analizinde **B seçeneği** kullanıldı: minimum destek **0,03**, minimum güven **0,60**, en fazla **iki öncül**.

Yanlış global ön işleme ve yanlış global hedef kodlama kurulumlarını sızıntıyı incelemek için ayrı deneyler olarak kullandım; seçilen modele taşımadım.

## Sonuç özeti

| Model | Geliştirme BA ortalama ± standart sapma |
|---|---|
| Gradyan artırma | 0.6632 ± 0.0175 |
| Lojistik regresyon | 0.6616 ± 0.0168 |
| Rastgele orman | 0.6607 ± 0.0193 |
| Karar ağacı | 0.6559 ± 0.0199 |
| k-NN | 0.6433 ± 0.0143 |
| Dummy | 0.5000 ± 0.0000 |

Ana ölçüte göre gradyan artırmayı seçtim. İlk üç modelin sonuçları birbirine yakındı. Tablodaki standart sapmalar katlar arasındaki değişkenliği gösterir; güven aralığı değildir.

- **Hedef kodlama:** Doğru kurulumda BA `0.6575 ± 0.0187`, yanlış global hedef kodlamada `0.6857 ± 0.0147` oldu. Değerlendirme etiketleri kodlamaya karıştığı için yanlış kurulumdaki artışı geçerli bir iyileşme olarak değerlendirmedim.
- **Ön işleme:** Pipeline içindeki doğru kurulum `0.6616 ± 0.0168`, yanlış global kurulum `0.6588 ± 0.0173` verdi. Yanlış kurulumun daha düşük skor vermesi, bilgi akışındaki sorunu ortadan kaldırmıyor.
- **İstatistiksel karşılaştırma:** Ayrılmış testte gradyan artırmanın doğru, lojistiğin yanlış olduğu 75; tersinin geçerli olduğu 79 kayıt vardı. McNemar p değeri `0.8091` bulundu. Anlamlı fark bulunmaması, modellerin eşit olduğunu kanıtlamaz.
- **Weka:** ZeroR BA `0.5000`, J48 BA `0.6577` verdi. J48/C4.5 ile Python'daki CART farklı algoritmalar olduğu için farkları bu çerçevede yorumladım.
- **Birliktelik kuralları:** Python ve Weka karşılaştırmasında **12/12 kural** eşleşti; ortak kayıt sayımlarında fark çıkmadı.
- **Veri küpü:** İncelenen altı yeterli örneklemli tabakanın ikisinde toplam farkın yönü değişti. Bütün tabakalarda terslenme olmadığı için tam bir Simpson paradoksu olarak yorumlamadım.

## Sınırlılıklar

- Veri belirli bir bölgeye ve geçmiş döneme ait; farklı nüfuslara doğrudan genellenemez.
- Hedef, yeniden tutuklanma kaydı vekilidir. Takip ve kayıt süreçleri sonuçları etkileyebilir.
- Ham `age` alanı yerine değerlendirme anındaki yaşı hesapladım. Türetilmiş yaş 10 kayıtta 18'in altında olduğundan ilk yaş grubunu **25 yaş altı** olarak adlandırdım.
- 1 Nisan 2014 sonrasındaki 868 kaydın tamamı pozitif etiketlidir. Erken dönem duyarlılık kontrolünde BA `0.6480 ± 0.0087` oldu; bu kontrol seçilim sorununu tamamen çözmez.
- Irk bilgisini modelden çıkarmak tek başına adillik sağlamaz. Diğer özellikler geçmiş kayıt süreçleriyle ilişkili olabilir.

## Sunum videosu

**[Sunum videosunu izle](https://drive.google.com/file/d/1JcfYduPzdD2myVyBZvJINpqfuLIE4u_0/view?usp=sharing)**  
**Süre:** 17 dakika 14 saniye.

Sunumu kendi anlatımımla kaydettim ve Google Drive'da paylaştım. Bağlantıyı gizli pencerede açarak erişimi kontrol ettim.

## Katkı beyanı

| Üye | Çalışma ve sunum kapsamı |
|---|---|
| Yasin Göktepe | Proje 1 Bölüm 1.1–3.4 kapsamındaki bireysel çalışmayı yürüttüm. Defteri Colab'da çalıştırdım, çıktıları kontrol edip dışa aktardım. Sunumu kendi anlatımımla kaydettim ve proje dosyalarını GitHub'a yükledim. |

## Yapay zekâ beyanı

Bu projede OpenAI Codex'ten kaynak araştırması, kod oluşturma ve hata giderme, analiz sonuçlarını yorumlama ve sunum metni hazırlama aşamalarında destek aldım. Defteri Colab'da çalıştırdım, çıktıları kontrol ettim ve sunumu kendi anlatımımla kaydettim.

## Kaynaklar

- [ProPublica COMPAS veri ve kod deposu](https://github.com/propublica/compas-analysis)
- [ProPublica: How We Analyzed the COMPAS Recidivism Algorithm](https://www.propublica.org/article/how-we-analyzed-the-compas-recidivism-algorithm)
- [Kaufman vd. (2012): Leakage in Data Mining](https://doi.org/10.1145/2382577.2382579)
- [Dietterich (1998): Approximate Statistical Tests for Comparing Supervised Classification Learning Algorithms](https://doi.org/10.1162/089976698300017197)
- [Wu, Chen ve Han (2010): Re-examination of Interestingness Measures in Pattern Mining](https://doi.org/10.1007/s10618-009-0161-2)
- [Barenstein (2019): ProPublica's COMPAS Data Revisited](https://arxiv.org/abs/1906.04711)
- [scikit-learn TargetEncoder belgesi](https://scikit-learn.org/1.6/modules/generated/sklearn.preprocessing.TargetEncoder.html)
- [Weka Apriori kaynak kodu](https://github.com/Waikato/weka-3.8/blob/master/weka/src/main/java/weka/associations/Apriori.java)
- BIL545 güncel yönerge, değerlendirme esasları, README ve notebook şablonları (20261007); Hafta 2–7 ders notları; 29 Ekim ve 4 Kasım 2026 ders duyuruları.
