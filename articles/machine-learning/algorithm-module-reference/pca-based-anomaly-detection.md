---
title: 'PCA Tabanlı Anomali Algılama: Modül Referans'
titleSuffix: Azure Machine Learning
description: Temel Bileşen Analizine (PCA) dayalı bir anomali algılama modeli oluşturmak için PCA Tabanlı Anomali Algılama modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 0672b9769feae65c73a6f752a268968a7bad9e4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502990"
---
# <a name="pca-based-anomaly-detection"></a>PCA Tabanlı Anomali Algılama

Bu makalede, Temel Bileşen Analizi'ne (PCA) dayalı bir anormallik algılama modeli oluşturmak için Azure Machine Learning tasarımcısında (önizleme) **PCA Tabanlı Anomali Algılama** modülünün nasıl kullanılacağı açıklanmaktadır.

Bu modül, geçerli işlemler gibi bir sınıftan eğitim verilerini almanın kolay olduğu, ancak hedeflenen anomalilerden yeterli örnek almanın zor olduğu senaryolarda bir model oluşturmanıza yardımcı olur. 

Örneğin, sahte işlemleri algılamak için, çoğu zaman yeterli dolandırıcılık örneğine sahip değilsiniz, ancak iyi işlemlere ilişkin birçok örneğiniz vardır. **PCA Tabanlı Anomali Algılama** modülü, "normal" bir sınıfın ne olduğunu belirlemek için mevcut özellikleri analiz ederek ve anomalileri temsil eden durumları belirlemek için mesafe ölçümleri uygulayarak sorunu çözer. Bu, varolan dengesiz verileri kullanarak bir model eğitmenizini sağlar.

## <a name="more-about-principal-component-analysis"></a>Ana Bileşen Analizi hakkında daha fazla bilgi

Sık sık PCA olarak kısaltılır *Ana Bileşen Analizi,* makine öğreniminde kurulmuş bir tekniktir. PCA, verilerin iç yapısını ortaya çıkardığı ve verilerdeki varyansı açıkladığı için araştırmacı veri analizinde sık lıkla kullanılır.

PCA, birden çok değişken içeren verileri analiz ederek çalışır. Değişkenler arasındaki bağıntıları arar ve sonuçlardaki farklılıkları en iyi yakalayan değerlerin birleşimini belirler. Bu birleştirilmiş özellik *değerleri, ana bileşenler*olarak adlandırılan daha kompakt bir özellik alanı oluşturmak için kullanılır.

Anomali tespiti için her yeni giriş analiz edilir ve anomali algılama algoritması normalleştirilmiş bir yeniden yapılandırma hatasıyla birlikte eigenvectors üzerindeki projeksiyonunu hesaplar. Normalleştirilmiş hata anomali skoru olarak kullanılır. Hata ne kadar yüksekse, örnek o kadar anormaldir.

PCA'nın nasıl çalıştığı ve anomali tespiti uygulaması hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Ana bileşen çözümlemesi için randomize](https://arxiv.org/abs/0809.2274)algoritma. Rokhlin, Szlan ve Tygert

- [Rasgelelik ile Yapı Bulma: Yaklaşık Matris Ayrıştırma oluşturma için Olasılıksal Algoritmalar](http://users.cms.caltech.edu/~jtropp/papers/HMT11-Finding-Structure-SIREV.pdf) (PDF indir). Halko, Martinsson ve Tropp.

## <a name="how-to-configure-pca-anomaly-detection"></a>PCA Anomali Algılama nasıl yapılandırılır

1. PcA **Tabanlı Anomali Algılama** modüllerini tasarımcıdaki boru hattınıza ekleyin. Bu modülü **Anomali Algılama** kategorisinde bulabilirsiniz.

2. **PCA Tabanlı Anomali Algılama** modülünün sağ panelinde, **Eğitim modu** seçeneğini tıklayın ve modeli belirli bir parametre kümesini kullanarak eğitmek mi istediğinizi veya en iyi parametreleri bulmak için bir parametre süpürme kullanmak isteyip istemediğinizi belirtin.

    - **Tek Parametre**: Modeli nasıl yapılandırmak istediğinizi biliyorsanız bu seçeneği seçin ve bağımsız değişken olarak belirli bir değer kümesi sağlayın.

3. **PCA'da kullanılacak bileşen sayısı**: Çıktı yapmak istediğiniz çıktı özelliklerinin veya bileşenlerin sayısını belirtin.

    Kaç bileşenin dahil edilemeyekarar verilebilen karar, PCA kullanarak deneme tasarımının önemli bir parçasıdır. Genel kılavuz, değişkenler olduğu gibi aynı sayıda PCA bileşeni eklememeniz gerektiğidir. Bunun yerine, bazı daha az sayıda bileşenle başlayıp bazı ölçütler karşılanana kadar bunları artırmalısınız.

    Çıktı bileşenlerinin sayısı veri kümesinde bulunan özellik sütunlarının sayısından **az** olduğunda en iyi sonuçlar elde edilir.

4. Randomize PCA eğitimi sırasında gerçekleştirecek aşırı örnekleme miktarını belirtin. Anomali algılama problemlerinde, dengesiz veriler standart PCA tekniklerinin uygulanmasını zorlaştırır. Bir miktar aşırı örnekleme belirterek, hedef örneklerin sayısını artırabilirsiniz.

    1 belirtirseniz, aşırı örnekleme yapılmaz. 1'den yüksek bir değer belirtirseniz, modeli eğitmek için ek örnekler oluşturulur.

    Parametre süpürme kullanıp kullanmadığınıza bağlı olarak iki seçenek vardır:

    - **Randomize PCA için aşırı örnekleme parametresi**: Azınlık sınıfının normal sınıfa göre aşırı örnekleme oranını temsil eden tek bir tam sayı yazın. (Tek **parametre** eğitim yöntemini kullanırken kullanılabilir.)

    > [!NOTE]
    > Aşırı örneklenmiş veri kümesini görüntüleyemezsiniz. PCA ile aşırı örneklemenin nasıl kullanıldığı hakkında daha fazla bilgi için [teknik notlara](#technical-notes)bakın.

5. **Giriş özelliğini normalleştirme ortalamasını etkinleştir**: Tüm giriş özelliklerini ortalama sıfıra normalleştirmek için bu seçeneği seçin. PCA'nın amacı değişkenler arasındaki farkı en üst düzeye çıkarmak olduğundan, normalleştirme veya sıfıra kadar ölçeklendirme genellikle PCA için önerilir.

     Bu seçenek varsayılan olarak seçilidir. Değerler zaten farklı bir yöntem veya ölçek kullanılarak normalleştirilmişse bu seçeneği seçin.

6. Etiketli bir eğitim veri kümesini ve eğitim modüllerinden birini bağlayın:

    - Tek **Parametre** **için eğitmen oluştur seçeneğini** ayarlarsanız, Tren [Anomali Algılama Modeli](train-anomaly-detection-model.md) modüllerini kullanın.

7. Boru hattını gönderin.

## <a name="results"></a>Sonuçlar

Eğitim tamamlandığında, ya eğitilmiş modeli kaydedebilir veya anomali puanlarını tahmin etmek için [Puan Modeli](score-model.md) modülüne bağlayabilirsiniz.

Bir anomali algılama modelinin sonuçlarının değerlendirilmesi bazı ek adımlar gerektirir:

1. Her iki veri kümesinde de bir puan sütunu olduğundan emin olun

    Bir anomali algılama modelini değerlendirmeye ve "Karşılaştırmak için puanlı veri kümesinde puan sütunu yok" hatasını almaya çalışırsanız, bu, etiket sütunu içeren ancak olasılık puanı içermeyen tipik bir değerlendirme veri kümesi kullandığınız anlamına gelir. Anormal algılama modelleri için şema çıktısı ile eşleşen ve **Puanlı Etiketler** ve **Puanlı Olasılıklar** sütunu içeren bir veri kümesi seçmeniz gerekir.

2. Etiket sütunlarının işaretli olduğundan emin olun

    Bazen etiket sütunuyla ilişkili meta veriler ardışık etki hattı grafiğinde kaldırılır. Bu durumda, iki anomali algılama modelinin sonuçlarını karşılaştırmak için [Model'i değerlendir](evaluate-model.md) modülünü kullandığınızda, "Puanlı veri kümesinde etiket sütunu yok" veya "Karşılaştırılması gereken veri kümesinde etiket sütunu yoktur" hatasını alabilirsiniz.

    [Modeli Değerlendir](evaluate-model.md) modülünden önce Meta veri modüllerini [edit](edit-metadata.md) ekleyerek bu hatayı önleyebilirsiniz. Sınıf sütununu seçmek için sütun seçiciyi kullanın ve **Alanlar** açılır listesinde **Etiket'i**seçin.

3. Etiket sütun kategorilerini 1 (pozitif, normal) ve 0 (negatif, anormal) olarak ayarlamak için [Execute Python Script'i](execute-python-script.md) kullanın.

    ````
    label_column_name = 'XXX'
    anomaly_label_category = YY
    dataframe1[label_column_name] = dataframe1[label_column_name].apply(lambda x: 0 if x == anomaly_label_category else 1)
    ````

    
## <a name="technical-notes"></a>Teknik notlar

Bu algoritma, normal sınıfı içeren alt uzayı yaklaşık olarak pca kullanır. Altuzay, veri covariance matrisinin üst özdeğerleri ile ilişkili özvektörler tarafından yayılılır. Her yeni giriş için, anomali dedektörü önce eigenvectors üzerindeki projeksiyonunu hesaplar ve sonra normalleştirilmiş yeniden yapılandırma hatasını hesaplar. Bu hata anomali skorudur. Hata ne kadar yüksekse, örnek o kadar anormaldir. Normal alanın nasıl hesaplandırılabildiğini öğrenmek [Principal Component Analysis](https://wikipedia.org/wiki/Principal_component_analysis) için bkz. 


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 

Tasarımcı modüllerine özgü hataların listesi [için tasarımcının özel durumları ve hata kodlarına bakın (önizleme).'](designer-error-codes.md)