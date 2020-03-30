---
title: Model Hiper Parametrelerini Ayarlama
titleSuffix: Azure Machine Learning
description: En uygun parametre ayarlarını belirlemek için bir model üzerinde parametre taraması yapmak için Azure Machine Learning'de Tune Model Hiperparametreleri modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: ff0ccbf201f2b83dd446859d8054d115a70f402e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064169"
---
# <a name="tune-model-hyperparameters"></a>Model Hiper Parametrelerini Ayarlama

Bu makalede, Azure Machine Learning tasarımcısında Tune Model Hiperparametreleri modülünün nasıl kullanılacağı açıklanmaktadır (önizleme). Amaç, bir makine öğrenme modeli için en uygun hiperparametreleri belirlemektir. Modül, farklı ayar kombinasyonlarını kullanarak birden çok modeli oluşturur ve sınar. Ayarların birleşimlerini elde etmek için tüm modellerdeki ölçümleri karşılaştırır. 

Terim *parametresi* ve *hiperparametre* kafa karıştırıcı olabilir. Modelin *parametreleri,* modülün sağ bölmesine ayarladığınız parametrelerdir. Temel olarak, bu modül belirtilen parametre ayarları üzerinde bir *parametre süpürme* gerçekleştirir. Her belirli karar ağacı, veri kümesi veya regresyon yöntemi için farklı olabilecek en uygun _hiperparametre_kümesini öğrenir. En iyi yapılandırmayı bulma işlemi bazen *ayar*olarak adlandırılır. 

Modül, bir model için en uygun ayarları bulmak için aşağıdaki yöntemi destekler: *entegre tren ve ayar.* Bu yöntemde, kullanılacak bir dizi parametreyi yapılandırırsınız. Daha sonra modülün birden çok kombinasyon üzerinde tekrarlamasına izin verebilirsiniz. Modül, "en iyi" modeli bulana kadar doğruluğu ölçer. Çoğu öğrenci modülünde, eğitim sürecinde hangi parametrelerin değiştirilmesi ve hangilerinin sabit kalması gerektiğini seçebilirsiniz.

Tuning işleminin ne kadar süre yle çalışmasını istediğinize bağlı olarak, tüm kombinasyonları kapsamlı bir şekilde sınamaya karar verebilirsiniz. Veya parametre kombinasyonları bir ızgara oluşturarak ve parametre ızgarası randomize bir alt kümesini test ederek işlemi kısaltabilirsiniz.

Bu yöntem, yeniden kullanmak için kaydedebilirsiniz eğitimli bir model oluşturur.  

> [!TIP] 
> İlgili bir görevi yapabilirsiniz. Atolandırmaya başlamadan önce, en yüksek bilgi değerine sahip sütunları veya değişkenleri belirlemek için özellik seçimini uygulayın.

## <a name="how-to-configure-tune-model-hyperparameters"></a>Tune Model Hiperparametreleri nasıl yapılandırılır?  

Bir makine öğrenme modeli için en uygun hiperparametrelerin öğrenilmesi, boru hatlarının önemli ölçüde kullanılmasını gerektirir.

### <a name="train-a-model-by-using-a-parameter-sweep"></a>Parametre süpürme kullanarak bir modeli eğitin  

Bu bölümde, Tune Model Hiperparametreleri modüllerini kullanarak bir modeli eğiten temel parametre süpürmesinin nasıl gerçekleştirildiği açıklanmaktadır.

1.  Tasarımcıda ardışık hattınıza Tune Model Hiperparametrelermodüle ekleyin.

2.  Eğitimsiz bir modeli en soldaki girişe bağlayın. 



4.  Eğitim için kullanmak istediğiniz veri kümesini ekleyin ve Tune Model Hyperparameters'un orta girişine bağlayın.  

    İsteğe bağlı olarak, etiketli bir veri setiniz varsa, onu en doğru giriş bağlantı noktasına **(İsteğe bağlı doğrulama veri kümesi)** bağlayabilirsiniz. Bu, eğitim ve atoalma sırasında doğruluğu ölçmenizi sağlar.

5.  Tune Model Hyperparameters'un sağ panelinde, **Parametre süpürme modu**için bir değer seçin. Bu seçenek parametrelerin nasıl seçili olduğunu denetler.

    - **Tüm ızgara**: Bu seçeneği seçtiğinizde, modül farklı kombinasyonları denemek ve en iyi öğreneni belirlemek için sistem tarafından önceden tanımlanmış bir ızgara üzerinde döngüler. Bu seçenek, en iyi parametre ayarlarının ne olabileceğini bilmiyorsanız ve tüm olası değer birleşimlerini denemek istediğinizde yararlıdır.

    - **Rastgele süpürme**: Bu seçeneği seçtiğinizde, modül sistem tanımlı bir aralık üzerinden parametre değerlerini rasgele seçer. Modülün yürütmesini istediğiniz maksimum çalıştırma sayısını belirtmeniz gerekir. Bu seçenek, seçtiğiniz ölçümleri kullanarak model performansını artırmak ancak yine de bilgi işlem kaynaklarını korumak istediğinizde yararlıdır.    

6.  **Etiket sütunu**için, tek bir etiket sütunu seçmek için sütun seçiciyi açın.

7.  Çalıştırma sayısını seçin:

    1. **Rasgele süpürmede maksimum çalıştırma sayısı**: Rasgele bir süpürme seçerseniz, parametre değerlerinin rasgele bir birleşimini kullanarak modelin kaç kez eğitilmesi gerektiğini belirtebilirsiniz.

8.  **Sıralama**için, modelleri sıralamak için kullanmak üzere tek bir metrik seçin.

    Bir parametre süpürme çalıştırdığınızda, modül model türü için geçerli tüm ölçümleri hesaplar ve **bunları Süpür sonuç** raporunda döndürür. Modül, regresyon ve sınıflandırma modelleri için ayrı ölçümler kullanır.

    Ancak, seçtiğiniz metrik modellerin nasıl sıralanır belirler. Yalnızca seçilen metrik tarafından sıralanan en iyi model, puanlama için kullanılacak eğitimli bir model olarak çıktıdır.

9.  **Rastgele tohum**için, parametre süpürme başlatmak için kullanmak için bir sayı girin. 

10. Boru hattını gönderin.

## <a name="results-of-hyperparameter-tuning"></a>Hiperparametre amısonuçları

Eğitim tamamlandığında:

+ En iyi model için doğruluk ölçümleri kümesini görüntülemek için modülü sağ tıklatın ve ardından **Visualize'ı**seçin.

    Çıktı, model türüne uygulanan tüm doğruluk ölçümlerini içerir, ancak sıralama için seçtiğiniz metrik hangi modelin "en iyi" olarak kabul edilir olduğunu belirler.

+ Eğitilen modelin anlık görüntüsünü kaydetmek **için, Tren modeli** modülünün sağ panelindeki **Çıktılar** sekmesini seçin. Modeli yeniden kullanılabilir bir modül olarak kaydetmek için **Kayıt veri kümesi** simgesini seçin.


## <a name="technical-notes"></a>Teknik notlar

Bu bölümde uygulama ayrıntıları ve ipuçları içerir.

### <a name="how-a-parameter-sweep-works"></a>Parametre süpürme nasıl çalışır?

Bir parametre süpürme ayarladığınızda, aramanızın kapsamını tanımlarsınız. Arama rasgele seçilen parametrelerin sınırlı sayıda kullanabilirsiniz. Ya da tanımladığınız bir parametre alanı üzerinde kapsamlı bir arama olabilir.

+ **Rastgele süpürme**: Bu seçenek, belirli sayıda yineleme kullanarak bir modeli eğitir. 

  Üzerinde yinelemek için bir dizi değer belirtirsiniz ve modül bu değerlerin rasgele seçilmiş bir alt kümesini kullanır. Değerler yedekle seçilir, yani daha önce rasgele seçilen sayılar kullanılabilir sayılar havuzundan kaldırılmaz. Bu nedenle herhangi bir değerin seçilme olasılığı tüm geçişlerde aynı kalır.  

+ **Tüm ızgara**: Tüm ızgarayı kullanma seçeneği, her kombinasyonun test edildiği anlamına gelir. Bu seçenek en kapsamlı, ancak en çok zaman gerektirir. 

### <a name="controlling-the-length-and-complexity-of-training"></a>Eğitimin uzunluk ve karmaşıklığını kontrol etme

Birçok ayar bileşimi üzerinde yeniden işleme zaman alabilir, bu nedenle modül işlemi kısıtlamak için çeşitli yollar sağlar:

+ Bir modeli sınamak için kullanılan yineleme sayısını sınırlayın.
+ Parametre alanını sınırlayın.
+ Hem yineleme sayısını hem de parametre alanını sınırlayın.

Belirli bir veri kümesi ve modeli üzerinde en verimli eğitim yöntemini belirlemek için ayarları düzenlemenizi öneririz.

### <a name="choosing-an-evaluation-metric"></a>Değerlendirme metrik seçimi

Testin sonunda, model, metrik sonuçları gözden geçirebilmeniz için her modelin doğruluğunu içeren bir rapor sunar:

- Tüm ikili sınıflandırma modelleri için tek tip bir metrik kümesi kullanılır.
- Doğruluk tüm çok sınıflı sınıflandırma modelleri için kullanılır.
- Regresyon modelleri için farklı bir ölçüm kümesi kullanılır. 

Ancak, eğitim sırasında, atoklama işlemi sırasında oluşturulan modelleri sıralamada kullanmak üzere *tek* bir metrik seçmeniz gerekir. En iyi metrik, iş sorunu ve yanlış pozitif ve yanlış negatiflerin maliyetine bağlı olarak değişir bulabilirsiniz.

#### <a name="metrics-used-for-binary-classification"></a>İkili sınıflandırma için kullanılan ölçümler

-   **Doğruluk,** doğru sonuçların toplam olgulara oranıdır.  

-   **Kesinlik,** doğru sonuçların olumlu sonuçlara oranıdır.  

-   **Geri çağırma,** tüm sonuçlar üzerinde tüm doğru sonuçların kesir olduğunu.  

-   **F-skoru,** hassasiyeti ve hatırlamayı dengeleyen bir ölçüdür.  

-   **AUC,** x ekseninde yanlış pozitifler çizildiğinde ve y ekseninde gerçek pozitifler çizildiğinde eğrinin altındaki alanı temsil eden bir değerdir.  

-   **Ortalama Günlük Kaybı** iki olasılık dağılımı arasındaki farktır: gerçek olan ve modeldeki.  

#### <a name="metrics-used-for-regression"></a>Regresyon için kullanılan ölçümler

-   **Ortalama mutlak hata,** *hatanın* öngörülen değerin gerçek değerden uzaklığı anlamına geldiği modeldeki tüm hataların ortalamasını verir. Genellikle *MAE*olarak kısaltılır.  

-   **Ortalama kare hata kökü** hataların karelerinin ortalamasını ölçer ve sonra bu değerin kökünü alır. Genellikle *RMSE*olarak kısaltılır.  

-   **Göreli mutlak hata,** hatayı gerçek değerin yüzdesi olarak temsil eder.  

-   **Göreli karehata,** öngörülen değerlerin toplam kare hatasına bölünerek toplam kare hatasını normalleştirir.  

-   **Belirleme katsayısı,** verilerin bir modele ne kadar iyi uyduğunu gösteren tek bir sayıdır. Bir değer, modelin verilerle tam olarak eşleştiğini anlamına gelir. Sıfır değeri, verilerin rastgele olduğu veya modele başka bir şekilde sığamayacağı anlamına gelir. Genellikle *r<sup>2</sup>*, *R<sup>2</sup>*, veya *r-kare*denir.  

### <a name="modules-that-dont-support-a-parameter-sweep"></a>Parametre süpürmeyi desteklemeyen modüller

Azure Machine Learning'deki hemen hemen tüm öğrenciler, ile boru hattı için parametreleri seçmenize olanak tanıyan tümleşik parametre süpürmesiyle çapraz doğrulamayı destekler. Öğrenci bir dizi değer ayarlamayı desteklemiyorsa, bunu çapraz doğrulamada kullanmaya devam edebilirsiniz. Bu durumda, süpürme için izin verilen bir dizi değer seçilir. 


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 

