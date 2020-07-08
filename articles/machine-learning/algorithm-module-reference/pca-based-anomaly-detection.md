---
title: 'PCA tabanlı anomali algılama: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Birincil bileşen analizine (PCA) dayalı bir anomali algılama modeli oluşturmak için PCA tabanlı anomali algılama modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 0498823e1b730db6425d255b6de4b826dd05a6a4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84749495"
---
# <a name="pca-based-anomaly-detection-module"></a>PCA tabanlı anomali algılama modülü

Bu makalede, sorumlu bileşen analizine (PCA) dayalı bir anomali algılama modeli oluşturmak için Azure Machine Learning tasarımcısında (Önizleme) PCA tabanlı anomali algılama modülünün nasıl kullanılacağı açıklanır.

Bu modül, geçerli işlemler gibi bir sınıftan eğitim verilerini almanın ve hedeflenen anomali örneklere yeterli örnek almak zor olan senaryolarda bir model oluşturmanıza yardımcı olur. 

Örneğin, sahte işlemleri algılamak için, üzerinde eğecek çok sayıda sahtekarlık örneği yoktur. Ancak iyi işlemlere çok sayıda örnek olabilir. PCA tabanlı anomali algılama modülü, "normal" sınıfının ne olduğunu belirlemek için kullanılabilir özellikleri çözümleyerek sorunu çözer. Modül daha sonra, anormallikleri temsil eden durumları belirlemek için mesafe ölçümleri uygular. Bu yaklaşım, mevcut imdenli verileri kullanarak bir modeli eğmenize imkan tanır.

## <a name="more-about-principal-component-analysis"></a>Sorumlu bileşen analizi hakkında daha fazla bilgi

PCA, Machine Learning 'de belirlenmiş bir tekniktir. Verilerin iç yapısını açığa çıkardığından ve verilerdeki varyansı açıkladığından, keşif verileri analizinde sıklıkla kullanılır.

PCA birden çok değişken içeren verileri analiz ederek işe yarar. Değişkenler arasında bağıntılar arar ve sonuçların farklarını en iyi şekilde yakalayan değerlerin birleşimini belirler. Bu Birleşik özellik değerleri, *sorumlu bileşenleri*olarak adlandırılan daha kompakt bir özellik alanı oluşturmak için kullanılır.

Anomali algılama için, her yeni giriş çözümlenir. Anomali algılama algoritması, bir normalleştirilmiş yeniden oluşturma hatasıyla birlikte eigenvektörlerin projeksiyonunu hesaplar. Normalleştirilmiş hata, anomali puanı olarak kullanılır. Hatanın ne kadar yüksekse, örnek de o kadar fazla olur.

PCA 'nın nasıl çalıştığı hakkında daha fazla bilgi edinmek ve anomali algılama için uygulama hakkında daha fazla bilgi için şu İncelemeleri inceleyin:

- Rokhlin, Szlan ve Tygert tarafından [sorumlu bileşen analizi için rastgele bir algoritma](https://arxiv.org/abs/0809.2274)

- [Rastgele bir yapıyı bulma: yaklaşık matris deklerini (PDF indirmesi) oluşturmak Için dayalı algoritmaları](http://users.cms.caltech.edu/~jtropp/papers/HMT11-Finding-Structure-SIREV.pdf) , Halko, Martinsson ve Tropp tarafından

## <a name="how-to-configure-pca-based-anomaly-detection"></a>PCA tabanlı anomali algılamayı yapılandırma

1. Tasarımcıdaki işlem hattınızla **PCA tabanlı anomali algılama** modülünü ekleyin. Bu modülü **anomali algılama** kategorisinde bulabilirsiniz.

2. Modülün sağ bölmesinde **eğitim modu** seçeneğini belirleyin. Modeli belirli bir parametre kümesini kullanarak eğmek isteyip istemediğinizi belirtin veya en iyi parametreleri bulmak için bir parametre süpürme kullanın.

    Modeli nasıl yapılandırmak istediğinizi biliyorsanız, **tek parametre** seçeneğini belirleyin ve bağımsız değişken olarak belirli bir değer kümesi sağlayın.

3. **PCA 'da kullanılacak bileşen sayısı**için, istediğiniz çıkış özelliklerinin veya bileşenlerinin sayısını belirtin.

    Kaç bileşen ekleneceğini gösteren karar, PCA kullanan deneme tasarımının önemli bir parçasıdır. Genel rehberlik, değişkenlerle aynı sayıda PCA bileşenini içermemelidir. Bunun yerine, daha az sayıda bileşen ile başlamalı ve bazı kriterler karşılanana kadar bunları artırmanız gerekir.

    En iyi sonuçlar, çıktı bileşenleri sayısı, veri kümesinde bulunan özellik sütunlarının sayısından *az* olduğunda elde edilir.

4. Rastgele PCA eğitimi sırasında gerçekleştirilecek fazla örnekleme miktarını belirtin. Anomali algılama sorunlarında, imdengeli veriler standart PCA tekniklerini uygulamayı zorlaştırır. Bir miktar fazla örnekleme belirterek, hedef örneklerin sayısını artırabilirsiniz.

    **1**belirtirseniz, aşırı örnekleme yapılmaz. **1**' den yüksek bir değer belirtirseniz, modele eğitim içinde kullanmak için ek örnekler üretilir.

    Bir parametre süpürme kullanılmasına bağlı olarak iki seçenek vardır:

    - **Rastgele PCA Için aşırı örnekleme parametresi**: normal sınıf üzerinde minınlik sınıfının fazla örneklemenin oranını temsil eden tek bir tam sayı yazın. (Bu seçenek, **tek parametreli** eğitim yöntemini kullanırken kullanılabilir.)

    > [!NOTE]
    > Aşırı örneklenmiş veri kümesini görüntüleyemezsiniz. PCA için fazla örnekleme kullanma hakkında daha fazla bilgi için bkz. [Teknik notlar](#technical-notes).

5. Tüm giriş özelliklerini sıfır ortalaması olacak şekilde normalleştirmek için **giriş özelliğini etkinleştir ortalama normalleştirme** seçeneğini belirleyin. PCA 'nın hedefi, değişkenler arasındaki varyansı en üst düzeye çıkarmaktır.

    Bu seçenek varsayılan olarak seçilidir. Değerler farklı bir yöntem veya ölçeğe göre normalleştirildiğinden, bu seçimi kaldırın.

6. Etiketli eğitim veri kümesini ve eğitim modülleriyle bir tane bağlayın.

   **Tek parametre**olarak bir Itme **modu oluştur** seçeneğini ayarlarsanız, [anomali algılama modeli](train-anomaly-detection-model.md) modülünü kullanın.

7. İşlem hattını gönderme.

## <a name="results"></a>Sonuçlar

Eğitim tamamlandığında, eğitilen modeli kaydedebilirsiniz. Ya da anomali puanları Tahmin etmek için bunu [puan modeli](score-model.md) modülüne bağlayabilirsiniz.

Anomali algılama modelinin sonuçlarını değerlendirmek için:

1. Bir puan sütununun her iki veri kümesi için de kullanılabilir olduğundan emin olun.

    Anomali algılama modelini değerlendirmeye çalışırsanız ve "Karşılaştırılacak veri kümesinde puan sütunu yok" hatasını alırsanız, bir etiket sütunu içeren, ancak olasılık puanı içermeyen tipik bir değerlendirme veri kümesi kullanıyorsunuz. Bir veri kümesi seçin; Bu, **puanlanmış** ve **puanlanmış olasılıkların** sütunları içeren anomali algılama modellerinin şema çıkışıyla eşleşir.

2. Etiket sütunlarının işaretlendiğinden emin olun.

    Bazen, etiket sütunuyla ilişkili meta veriler ardışık düzen grafiğinde kaldırılır. Bu durumda, iki anomali algılama modelinin sonuçlarını karşılaştırmak için [modeli değerlendir](evaluate-model.md) modülünü kullandığınızda, "puanlanmış veri kümesinde etiket sütunu yok" hatasını alabilirsiniz. Ya da "Karşılaştırılacak bir başlık sütunu yok" değerini alabilirsiniz.

    [Modeli değerlendir](evaluate-model.md) modülünden önce [verileri Düzenle](edit-metadata.md) modülünü ekleyerek bu hatalardan kaçınabilirsiniz. Sütun seçiciyi kullanarak sınıf sütununu seçin ve **alanlar** listesinde **etiket**' i seçin.

3. Etiket sütunu kategorilerini **1 (pozitif, normal)** ve **0 (negatif, olağan dışı)** olarak ayarlamak için [Python betiği yürütme](execute-python-script.md) modülünü kullanın.

    ````
    label_column_name = 'XXX'
    anomaly_label_category = YY
    dataframe1[label_column_name] = dataframe1[label_column_name].apply(lambda x: 0 if x == anomaly_label_category else 1)
    ````

    
## <a name="technical-notes"></a>Teknik notlar

Bu algoritma, normal sınıfını içeren alt boşluğu yaklaşık olarak tahmin etmek için PCA kullanır. Alt boşluk, veri Kovaryans matrisin en üst eigendeğerleriyle ilişkili eigenvektörler tarafından yayıldır. 

Anomali algılayıcısı, her yeni giriş için ilk olarak eigenvektörlerin projeksiyonunu hesaplar ve normalleştirilmiş yeniden oluşturma hatasını hesaplar. Bu hata anomali puandır. Hatanın ne kadar yüksekse, örnek de daha fazla olur. Normal alanın hesaplanma hakkında daha fazla bilgi için bkz. Vikipedi: [Principal bileşen analizi](https://wikipedia.org/wiki/Principal_component_analysis). 


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 

Tasarımcı modüllerine özgü hataların listesi için bkz. [Tasarımcı Için özel durumlar ve hata kodları (Önizleme)](designer-error-codes.md) .