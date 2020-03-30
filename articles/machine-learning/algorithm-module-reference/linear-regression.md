---
title: 'Lineer Regresyon: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Bir boru hattında kullanılmak üzere doğrusal bir regresyon modeli oluşturmak için Azure Machine Learning'deki Doğrusal Regresyon modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 602553637e21b17aa4f9bc7402753af024c697c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477570"
---
# <a name="linear-regression-module"></a>Lineer Regresyon modülü
Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Bir boru hattında kullanılmak üzere doğrusal bir regresyon modeli oluşturmak için bu modülü kullanın.  Doğrusal regresyon, bir veya daha fazla bağımsız değişken ile sayısal bir sonuç veya bağımlı değişken arasında doğrusal bir ilişki kurmaya çalışır. 

Bu modülü doğrusal bir regresyon yöntemi tanımlamak ve ardından etiketli bir veri kümesi kullanarak bir model eğitmek için kullanırsınız. Eğitimli model daha sonra öngörülerde bulunmak için kullanılabilir.

## <a name="about-linear-regression"></a>Doğrusal regresyon hakkında

Lineer regresyon, makine öğreniminde benimsenen ve çizginin montajı ve hatayı ölçmek için birçok yeni yöntemle geliştirilmiş yaygın bir istatistiksel yöntemdir. En temel anlamda, regresyon sayısal bir hedefin tahmin anlamına gelir. Temel bir tahmin görevi için basit bir model istediğinizde doğrusal regresyon hala iyi bir seçimdir. Doğrusal regresyon da karmaşıklık eksik yüksek boyutlu, seyrek veri setleri üzerinde iyi çalışma eğilimindedir.

Azure Machine Learning, doğrusal regresyona ek olarak çeşitli regresyon modellerini destekler. Ancak, "regresyon" terimi gevşek yorumlanabilir ve diğer araçlarda sağlanan bazı gerileme türleri desteklenmez.

+ Klasik regresyon sorunu tek bir bağımsız değişken ve bağımlı değişken içerir. Buna *basit regresyon*denir.  Bu modül basit regresyon destekler.

+ *Birden çok doğrusal regresyon,* tek bir bağımlı değişkene katkıda bulunan iki veya daha fazla bağımsız değişken içerir. Tek bir sayısal sonucu tahmin etmek için birden çok girdinin kullanıldığı problemlere *çok değişkenli doğrusal regresyon*da denir.

    **Doğrusal Regresyon** modülü, diğer regresyon modüllerinin çoğu gibi bu sorunları çözebilir.

+ *Çok etiketli regresyon,* tek bir model içinde birden çok bağımlı değişkeni tahmin etme görevidir. Örneğin, çok etiketli lojistik regresyonda, bir örnek birden çok farklı etikete atanabilir. (Bu, tek bir sınıf değişkeni içinde birden çok düzeyi tahmin etme görevinden farklıdır.)

    Bu tür bir gerileme Azure Machine Learning'de desteklenmez. Birden çok değişkeni tahmin etmek için, tahmin etmek istediğiniz her çıktı için ayrı bir öğrenci oluşturun.

Yıllardır istatistikçiler regresyon için giderek daha gelişmiş yöntemler geliştirmektedir. Bu doğrusal regresyon için bile geçerlidir. Bu modül, hatayı ölçmek ve regresyon çizgisine sığdırmak için iki yöntemi destekler: sıradan en az kareler yöntemi ve degrade iniş.

- **Degrade iniş** modeli eğitim sürecinin her adımında hata miktarını en aza indiren bir yöntemdir. Degrade iniş birçok varyasyonları vardır ve çeşitli öğrenme sorunları için optimizasyon kapsamlı incelenmiştir. **Çözüm yöntemi**için bu seçeneği seçerseniz, adım boyutunu, öğrenme hızını vb. denetlemek için çeşitli parametreler ayarlayabilirsiniz. Bu seçenek, tümleşik parametre süpürme kullanımını da destekler.

- **Sıradan en küçük kareler** doğrusal regresyonda en sık kullanılan tekniklerden biridir. Örneğin, en az kareler Microsoft Excel için Çözümleme Toolpak kullanılan yöntemdir.

    Normal en küçük kareler, gerçek değerden tahmin edilen satıra uzaklık karesinin toplamı olarak hatayı hesaplayan ve karehatasını en aza indirerek modele uyan kayıp işlevini ifade eder. Bu yöntem, girişler ve bağımlı değişken arasında güçlü bir doğrusal ilişki varsayar.

## <a name="configure-linear-regression"></a>Doğrusal Regresyon Yapıla

Bu modül, farklı seçeneklerle bir regresyon modeli takmak için iki yöntemi destekler:

+ [Sıradan en küçük kareleri kullanarak bir regresyon modeli sığdırın](#create-a-regression-model-using-ordinary-least-squares)

    Küçük veri kümeleri için en iyi sıradan en küçük kareleri seçmektir. Bu, Excel'e benzer sonuçlar vermelidir.
    
+ [Çevrimiçi degrade inişi kullanarak bir regresyon modeli oluşturma](#create-a-regression-model-using-online-gradient-descent)

    Degrade iniş, daha karmaşık veya değişken sayısı göz önüne alındığında çok az eğitim verisi olan modeller için daha iyi bir kayıp işlevidir.

### <a name="create-a-regression-model-using-ordinary-least-squares"></a>Sıradan en küçük kareleri kullanarak bir regresyon modeli oluşturma

1. Doğrusal **Regresyon Modeli** modüllerini tasarımcıda ardınıza ekleyin.

    Bu modülü **Machine Learning** kategorisinde bulabilirsiniz. **Initialize Model'i**genişletin, **Regresyon'u**genişletin ve **ardından Doğrusal Regresyon Modeli** modüllerini boru hattınıza sürükleyin.

2. **Özellikler** bölmesinde, Çözüm **yöntemi** açılır **listesinde, Olağan En Küçük Kareler'i**seçin. Bu seçenek, regresyon satırını bulmak için kullanılan hesaplama yöntemini belirtir.

3. **L2 düzenlileştirme ağırlığında, L2 düzenlileştirme**için ağırlık olarak kullanılacak değeri yazın. Aşırı yakışmayan bir değer kullanmanızı öneririz.

     Düzenlileştirmenin model montajını nasıl etkilediği hakkında daha fazla bilgi edinmek için şu makaleye bakın: [Makine Öğrenimi için L1 ve L2 Düzenlileştirme](https://msdn.microsoft.com/magazine/dn904675.aspx)

4. Kesme terimini görüntülemek istiyorsanız, **durdurma terimini dahil**etme seçeneğini seçin.

    Regresyon formülünü gözden geçirmeniz gerekmiyorsa bu seçeneği seçin.

5. **Rasgele sayı tohumu**için, isteğe bağlı olarak model tarafından kullanılan rasgele sayı üreteci tohumiçin bir değer yazabilirsiniz.

    Aynı ardışık ardışık ardışık farklı çalışır arasında aynı sonuçları korumak istiyorsanız, bir tohum değeri kullanmak yararlıdır. Aksi takdirde, varsayılan sistem saatinden bir değer kullanmaktır.


7. Tren [Modeli](./train-model.md) modülünü boru hattınıza ekleyin ve etiketli bir veri kümesibağlayın.

8. Boru hattını gönderin.

### <a name="results-for-ordinary-least-squares-model"></a>Sıradan en az kareler modeli için sonuçlar

Eğitim tamamlandıktan sonra:


+ Öngörülerde bulunmak için, eğitilmiş modeli yeni değerlerden oluşan bir veri kümesiyle birlikte [Puan Modeli](./score-model.md) modülüne bağlayın. 


### <a name="create-a-regression-model-using-online-gradient-descent"></a>Çevrimiçi degrade inişi kullanarak bir regresyon modeli oluşturma

1. Doğrusal **Regresyon Modeli** modüllerini tasarımcıda ardınıza ekleyin.

    Bu modülü **Machine Learning** kategorisinde bulabilirsiniz. **Initialize Modelini**Genişletin, **Regresyon'u**genişletin ve **Lineer Regresyon Modeli** modüllerini boru hattınıza sürükleyin

2. **Özellikler** bölmesinde, Çözüm **yöntemi** açılır listesinde, regresyon çizgisini bulmak için kullanılan hesaplama yöntemi olarak **Çevrimiçi Gradyan İniş'i** seçin.

3. **Eğitmen oluştur modu**için, modeli önceden tanımlanmış bir parametre kümesiyle mi eğitmek istediğinizi veya bir parametre süpürme kullanarak modeli optimize etmek isteyip istemediğinizi belirtin.

    + **Tek Parametre**: Doğrusal regresyon ağını nasıl yapılandırmak istediğinizi biliyorsanız, bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz.
    
    + **Parametre Aralığı**: En iyi parametrelerden emin değilseniz ve parametre süpürmesini çalıştırmak istiyorsanız bu seçeneği seçin. Üzerinde çoğaltmak için bir dizi değer seçin ve [Tune Model Hiperparametreleri,](tune-model-hyperparameters.md) en iyi sonuçları üreten hiperparametreleri belirlemek için sağladığınız ayarların olası tüm kombinasyonları üzerinde yinelenir.  

   
4. **Öğrenme oranı**için, stokastik gradyan iniş optimize ediciiçin ilk öğrenme oranını belirtin.

5. Eğitim çağı sayısı için, **algoritmanın**örnekler aracılığıyla kaç kez yinelemesi gerektiğini gösteren bir değer yazın. Az sayıda örnek içeren veri kümeleri için yakınsama ulaşmak için bu sayının büyük olması gerekir.

6. **Özellikleri normalleştirme**: Modeli eğitmek için kullanılan sayısal verileri zaten normalleştirdiyseniz, bu seçeneği niçin seçebilirsiniz. Varsayılan olarak, modül tüm sayısal girişleri 0 ile 1 arasında bir aralıkta normalleştirir.

    > [!NOTE]
    > 
    > Puanlama için kullanılan yeni verilere aynı normalleştirme yöntemini uygulamayı unutmayın.

7. **L2 düzenlileştirme ağırlığında, L2 düzenlileştirme**için ağırlık olarak kullanılacak değeri yazın. Aşırı yakışmayan bir değer kullanmanızı öneririz.

    Düzenlileştirmenin model montajını nasıl etkilediği hakkında daha fazla bilgi edinmek için şu makaleye bakın: [Makine Öğrenimi için L1 ve L2 Düzenlileştirme](https://msdn.microsoft.com/magazine/dn904675.aspx)


9. Yinelemeler ilerledikçe **öğrenme oranının**düşmesini istiyorsanız, öğrenme oranını düşür seçeneğini seçin.  

10. **Rasgele sayı tohumu**için, isteğe bağlı olarak model tarafından kullanılan rasgele sayı üreteci tohumiçin bir değer yazabilirsiniz. Aynı ardışık ardışık ardışık farklı çalışır arasında aynı sonuçları korumak istiyorsanız, bir tohum değeri kullanmak yararlıdır.


12. Etiketli bir veri seti ve eğitim modüllerinden birini ekleyin.

    Parametre taraması kullanmıyorsanız, [Tren Modeli](train-model.md) modüllerini kullanın.

13. Boru hattını gönderin.

### <a name="results-for-online-gradient-descent"></a>Çevrimiçi degrade iniş sonuçları

Eğitim tamamlandıktan sonra:

+ Öngörülerde bulunmak için, eğitilmiş modeli yeni giriş verileriyle birlikte [Puan Modeli](./score-model.md) modülüne bağlayın.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 