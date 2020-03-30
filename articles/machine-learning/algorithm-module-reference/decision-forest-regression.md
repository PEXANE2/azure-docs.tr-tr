---
title: 'Karar Orman Regresyon: Modül Referans'
titleSuffix: Azure Machine Learning
description: Karar ağaçları topluluğuna dayalı bir regresyon modeli oluşturmak için Azure Machine Learning'deki Karar Ormanı Regresyon modülünün nasıl kullanılacağını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 63d90a5239e6bf350d8a6b66f35157e4c7d15aee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456548"
---
# <a name="decision-forest-regression-module"></a>Karar Orman Regresyon modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Karar ağaçları nın bir topluluğuna dayalı bir regresyon modeli oluşturmak için bu modülü kullanın.

Modeli yapılandırdıktan sonra, modeli etiketli bir veri kümesi ve [Tren Modeli](./train-model.md) modüllerini kullanarak eğitmenniz gerekir. Eğitimli model daha sonra öngörülerde bulunmak için kullanılabilir. 

## <a name="how-it-works"></a>Nasıl çalışır?

Karar ağaçları, her örnek için bir dizi basit test gerçekleştiren ve bir yaprak düğümü (karar) ulaşılına kadar ikili ağaç veri yapısında geçiş yapan parametrik olmayan modellerdir.

Karar ağaçlarının şu avantajları vardır:

- Eğitim ve tahmin sırasında hem hesaplama hem de bellek kullanımında etkilidirler.

- Doğrusal olmayan karar sınırlarını temsil edebilirler.

- Entegre özellik seçimi ve sınıflandırması yaparlar ve gürültülü özelliklerin varlığında dirençlidirler.

Bu regresyon modeli karar ağaçlarının bir topluluk oluşur. Bir gerileme kararı orman her ağaç bir tahmin olarak gaussdağılımı çıkar. Modeldeki tüm ağaçlar için birleşik dağılıma en yakın Gaussian dağılımını bulmak için ağaçların topluluğu üzerinde bir toplama gerçekleştirilir.

Bu algoritmanın teorik çerçevesi ve uygulanması hakkında daha fazla bilgi için şu makaleye bakınız: [Karar Ormanları: Sınıflandırma, Regresyon, Yoğunluk Tahmini, Manifold Öğrenme ve Yarı Denetimli Öğrenme için Birleşik Bir Çerçeve](https://www.microsoft.com/en-us/research/publication/decision-forests-a-unified-framework-for-classification-regression-density-estimation-manifold-learning-and-semi-supervised-learning/?from=http%3A%2F%2Fresearch.microsoft.com%2Fapps%2Fpubs%2Fdefault.aspx%3Fid%3D158806#)

## <a name="how-to-configure-decision-forest-regression-model"></a>Karar Ormanı Regresyon Modeli nasıl yapılandırılır?

1. Karar **Ormanı Regresyon** modüllerini boru hattına ekleyin. **Sen Machine Learning**altında tasarımcı modülü bulabilirsiniz , Model **Initialize**, ve **Regresyon**.

2. Modül özelliklerini açın ve **Yeniden Örnekleme yöntemi**için, tek tek ağaçları oluşturmak için kullanılan yöntemi seçin.  **Torbalama** veya **Çoğaltma'dan**seçim yapabilirsiniz.

    - **Torbalama**: Torbalama da *bootstrap toplama*denir. Bir gerileme kararı orman her ağaç tahmin yolu ile bir Gaussian dağılımı çıkar. Toplama olan ilk iki dakika tek tek ağaçlar tarafından döndürülen tüm dağılımları birleştirerek verilen Gaussian dağılımlarının karışımı anları eşleşen bir Gaussian bulmaktır.

         Daha fazla bilgi için [Bootstrap toplama](https://wikipedia.org/wiki/Bootstrap_aggregating)vikipedi girişine bakın.

    - **Çoğaltma**: Çoğaltma, her ağaç tam olarak aynı giriş verileri üzerinde eğitilir. Her ağaç düğümü için hangi bölünmüş yüklemin kullanıldığının belirlenmesi rasgele kalır ve ağaçlar farklı olacaktır.

         **Çoğaltma** seçeneği ile eğitim süreci hakkında daha fazla bilgi için, Bilgisayarlı Görme ve Tıbbi Görüntü Analizi için Karar Ormanları bölümüne [bakın. Criminisi ve J. Shotton. Springer 2013. .](https://research.microsoft.com/projects/decisionforests/)

3. **Eğitmen modu oluştur** seçeneğini ayarlayarak modelin nasıl eğitilmek istediğini belirtin.

    - **Tek Parametre**

      Modeli nasıl yapılandırmak istediğinizi biliyorsanız, bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz. Bu değerleri deneme yaparak öğrenmiş veya kılavuz olarak almış olabilirsiniz.

    - **Parametre Aralığı**: En iyi parametrelerden emin değilseniz ve parametre süpürmesini çalıştırmak istiyorsanız bu seçeneği seçin. Üzerinde çoğaltmak için bir dizi değer seçin ve [Tune Model Hiperparametreleri,](tune-model-hyperparameters.md) en iyi sonuçları üreten hiperparametreleri belirlemek için sağladığınız ayarların olası tüm kombinasyonları üzerinde yinelenir. 



4. **Karar ağaçlarının sayısı**için, toplulukiçinde oluşturulacak toplam karar ağacı sayısını belirtin. Daha fazla karar ağaçları oluşturarak, potansiyel olarak daha iyi kapsama alabilirsiniz, ancak eğitim süresi artacaktır.

    > [!TIP]
    > Bu değer, eğitilen modeli görselleştirerken görüntülenen ağaç sayısını da denetler. tek bir ağacı görmek veya yazdırmak istiyorsanız, değeri 1 olarak ayarlayabilirsiniz; ancak, bu yalnızca bir ağaç üretilecek (ilk parametreler kümesine sahip ağaç) ve başka yinelemeler yapılmayaceği anlamına gelir.

5. **Karar ağaçlarının maksimum derinliği**için, herhangi bir karar ağacının maksimum derinliğini sınırlamak için bir sayı yazın. Ağacın derinliğinin artırılması, bazı aşırı uyum ve artan eğitim süresi riski, hassasiyeti artırabilir.

6. **Düğüm başına rasgele bölme sayısı**için, ağacın her düğüm inşa ederken kullanılacak bölme sayısını yazın. *Bölme,* ağacın her düzeyindeki özelliklerin (düğüm) rasgele bölündüğü anlamına gelir.

7. **Yaprak düğümü başına en az sayıda örnek**için, bir ağaçta herhangi bir terminal düğümü (yaprak) oluşturmak için gereken en az sayıda servis talebi belirtin.

     Bu değeri artırarak, yeni kurallar oluşturmak için eşiği artırırsınız. Örneğin, varsayılan değeri 1 olan tek bir büyük/küçük harf bile yeni bir kuralın oluşturulmasına neden olabilir. Değeri 5'e yükselterseniz, eğitim verilerinin aynı koşulları karşılayan en az beş servis alayı içermesi gerekir.


9. Etiketli bir veri kümesini bağlayın, en fazla iki sonuç içeren tek bir etiket sütunu seçin ve [Train Model'e](./train-model.md)bağlanın.

    - Tek **Parametre** **için eğitmen modu oluştur** seçeneğini ayarlarsanız, Tren [Modeli](./train-model.md) modüllerini kullanarak modeli çalıştırın.

   

10. Boru hattını gönderin.

### <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Eğitilmiş modelin anlık görüntüsünü kaydetmek için eğitim modülünü seçin ve ardından sağ paneldeki **Çıktılar** sekmesine geçin. Simge Kayıt **modeline**tıklayın.  Kaydedilen modeli modül ağacında modül olarak bulabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 