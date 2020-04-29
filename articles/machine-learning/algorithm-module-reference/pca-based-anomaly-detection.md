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
ms.openlocfilehash: 0672b9769feae65c73a6f752a268968a7bad9e4b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79502990"
---
# <a name="pca-based-anomaly-detection"></a>PCA Tabanlı Anomali Algılama

Bu makalede, sorumlu bileşen analizine (PCA) dayalı bir anomali algılama modeli oluşturmak için Azure Machine Learning tasarımcısında (Önizleme) **PCA tabanlı anomali algılama** modülünün nasıl kullanılacağı açıklanır.

Bu modül, geçerli işlemler gibi bir sınıftan eğitim verileri elde etmek, ancak hedeflenen bozukluklar için yeterli örnek elde etmek zor olan senaryolarda bir model oluşturmanıza yardımcı olur. 

Örneğin, sahte işlemleri algılamak için çok daha fazla sahtekarlık konusunda eğitmeniz gerekmez, ancak çok sayıda iyi işlem örneği vardır. **PCA tabanlı anomali algılama** modülü, "normal" bir sınıfı neyin oluşturduğunu belirlemek için kullanılabilir özellikleri çözümleyerek sorunu çözer ve anormallikleri temsil eden durumları belirlemek için mesafe ölçümleri uygulama. Bu, mevcut imdenli verileri kullanarak bir modeli eğmenizi sağlar.

## <a name="more-about-principal-component-analysis"></a>Sorumlu bileşen analizi hakkında daha fazla bilgi

Genelde PCA için kısaltılmış olan *sorumlu bileşen analizi*, Machine Learning 'de belirlenmiş bir tekniktir. PCA, verilerin iç yapısını açığa çıkardığından ve verilerdeki varyansı anlatan araştırmacı veri analizinde sıklıkla kullanılır.

PCA birden çok değişken içeren verileri analiz ederek işe yarar. Değişkenler arasında bağıntılar arar ve sonuçların farklarını en iyi şekilde yakalayan değerlerin birleşimini belirler. Bu Birleşik özellik değerleri, *sorumlu bileşenleri*olarak adlandırılan daha kompakt bir özellik alanı oluşturmak için kullanılır.

Anomali algılama için, her yeni giriş çözümlenir ve anomali algılama algoritması, bir normalleştirilmiş yeniden oluşturma hatasıyla birlikte eigenvektörlerin projeksiyonunu hesaplar. Normalleştirilmiş hata, anomali puanı olarak kullanılır. Hatanın ne kadar yüksekse, örnek de o kadar fazla olur.

PCA 'nın nasıl çalıştığı hakkında daha fazla bilgi edinmek ve anomali algılama için uygulama hakkında daha fazla bilgi için şu İncelemeleri inceleyin:

- [Sorumlu bileşen analizi için rastgele bir algoritma](https://arxiv.org/abs/0809.2274). Rokhlin, Szlan ve Tygert

- [Rastgele olan yapıyı bulma: yaklaşık matris Dekompozisyonlarını (PDF indirmesi) oluşturmak Için dayalı algoritmaları](http://users.cms.caltech.edu/~jtropp/papers/HMT11-Finding-Structure-SIREV.pdf) . Halko, Martinsson ve Tropp.

## <a name="how-to-configure-pca-anomaly-detection"></a>PCA anomali algılamasını yapılandırma

1. Tasarımcıdaki işlem hattınızla **PCA tabanlı anomali algılama** modülünü ekleyin. Bu modülü **anomali algılama** kategorisinde bulabilirsiniz.

2. **PCA tabanlı anomali algılama** modülünün sağ panelinde, **eğitim modu** seçeneğine tıklayın ve belirli bir parametre kümesini kullanarak modeli eğmek isteyip istemediğinizi belirtin veya en iyi parametreleri bulmak için bir parametre süpürme kullanın.

    - **Tek parametre**: modeli nasıl yapılandırmak istediğinizi biliyorsanız ve bağımsız değişken olarak belirli bir değer kümesi sağlamak için bu seçeneği belirleyin.

3. **PCA 'da kullanılacak bileşen sayısı**: çıktı özelliklerinin sayısını veya çıkış yapmak istediğiniz bileşenleri belirtin.

    Kaç bileşen ekleneceğini gösteren karar, PCA kullanarak deneme tasarımının önemli bir parçasıdır. Genel rehberlik, değişkenlerle aynı sayıda PCA bileşenini içermemelidir. Bunun yerine, bazı ölçütlere göre daha az sayıda bileşen ile başlamalı ve bunları artırmanız gerekir.

    En iyi sonuçlar, çıktı bileşenleri sayısı, veri kümesinde bulunan özellik sütunlarının sayısından **az** olduğunda elde edilir.

4. Rastgele PCA eğitimi sırasında gerçekleştirilecek fazla örnekleme miktarını belirtin. Anomali algılama sorunlarında, imdengeli veriler standart PCA tekniklerini uygulamayı zorlaştırır. Bir miktar fazla örnekleme belirterek, hedef örneklerin sayısını artırabilirsiniz.

    1 belirtirseniz, aşırı örnekleme yapılmaz. 1 ' den yüksek bir değer belirtirseniz, modele eğitim içinde kullanmak için ek örnekler üretilir.

    Bir parametre süpürme kullanılmasına bağlı olarak iki seçenek vardır:

    - **Rastgele PCA Için aşırı örnekleme parametresi**: normal sınıf üzerinde minınlik sınıfının fazla örneklemenin oranını temsil eden tek bir tam sayı yazın. ( **Tek parametreli** eğitim yöntemi kullanılırken kullanılabilir.)

    > [!NOTE]
    > Aşırı örneklenmiş veri kümesini görüntüleyemezsiniz. Eksik örnekleme 'nın PCA ile nasıl kullanıldığı hakkında daha fazla bilgi için bkz. [Teknik notlar](#technical-notes).

5. **Giriş özelliği ortalama normalleştirmeyi etkinleştir**: tüm giriş özelliklerini sıfır ortalaması ile normalleştirmek için bu seçeneği belirleyin. PCA 'nın hedefi, değişkenler arasındaki varyansı en üst düzeye çıkarmaktır.

     Bu seçenek varsayılan olarak seçilidir. Değerler farklı bir yöntem veya ölçek kullanılarak normalleştirilmeye alınmış ise bu seçeneğin seçimini kaldırın.

6. Etiketli eğitim veri kümesini ve eğitim modüllerden birini bağlayın:

    - **Tek parametre**olarak bir Itme **modu oluştur** seçeneğini ayarlarsanız, [anomali algılama modeli](train-anomaly-detection-model.md) modülünü kullanın.

7. İşlem hattını gönderme.

## <a name="results"></a>Sonuçlar

Eğitim tamamlandığında, eğitilen modeli kaydedebilir veya anomali puanları Tahmin etmek için bunu [puan modeli](score-model.md) modülüne bağlayabilirsiniz.

Anomali algılama modelinin sonuçlarını değerlendirmek için bazı ek adımlar gerekir:

1. Bir puan sütununun her iki veri kümelerinde da kullanılabilir olduğundan emin olun

    Anomali algılama modelini değerlendirmeye çalışırsanız ve hatayı elde ediyorsanız, "Karşılaştırılacak bir veri kümesinde puan sütunu yok" olarak, bir etiket sütunu içeren, ancak olasılık puanı içermeyen tipik bir değerlendirme veri kümesi kullandığınız anlamına gelir. Bir Puanlanı algılama modelinin şema çıkışıyla eşleşen bir veri kümesi seçmeniz gerekir. Bu, bir **puanlanmış Etiketler** ve **puanlanmış olasılıkların** sütunu içerir.

2. Etiket sütunlarının işaretlendiğinden emin olun

    Bazen, etiket sütunuyla ilişkili meta veriler ardışık düzen grafiğinde kaldırılır. Bu durumda, iki anomali algılama modelinin sonuçlarını karşılaştırmak için [modeli değerlendir](evaluate-model.md) modülünü kullandığınızda, "puanlanmış veri kümesinde etiket sütunu yok" veya "Karşılaştırılacak bir veri kümesinde etiket sütunu yok" hatasını alabilirsiniz.

    [Modeli değerlendir](evaluate-model.md) modülünden önce [meta veri düzenleme](edit-metadata.md) modülünü ekleyerek bu hatadan kaçınabilirsiniz. Sütun seçiciyi kullanarak sınıf sütununu seçin ve **alanlar** açılan listesinde **etiket**' i seçin.

3. Etiket sütunu kategorilerini 1 (pozitif, normal) ve 0 (negatif, olağan dışı) olarak ayarlamak için [Python betiğini Yürüt](execute-python-script.md) ' ü kullanın.

    ````
    label_column_name = 'XXX'
    anomaly_label_category = YY
    dataframe1[label_column_name] = dataframe1[label_column_name].apply(lambda x: 0 if x == anomaly_label_category else 1)
    ````

    
## <a name="technical-notes"></a>Teknik notlar

Bu algoritma, normal sınıfını içeren alt boşluğu yaklaşık olarak almak için PCA kullanır. Alt boşluk, veri Kovaryans matrisin en üst eigendeğerleriyle ilişkili eigenvektörler tarafından yayıldır. Anomali algılayıcısı, her yeni giriş için ilk olarak eigenvektörlerin projeksiyonunu hesaplar ve normalleştirilmiş yeniden oluşturma hatasını hesaplar. Bu hata anomali puandır. Hatanın ne kadar yüksekse, örnek de daha fazla olur. Normal alanın hesaplanma hakkında daha fazla bilgi için bkz. Vikipedi: [Principal bileşen analizi](https://wikipedia.org/wiki/Principal_component_analysis) 


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 

Tasarımcı modüllerine özgü hataların listesi için bkz. [Tasarımcı Için özel durumlar ve hata kodları (Önizleme)](designer-error-codes.md) . ' '