---
title: SMOTE
titleSuffix: Azure Machine Learning service
description: Fazla örnekleme kullanarak bir veri kümesindeki düşük olay örneklerinin sayısını artırmak için Azure Machine Learning hizmetinde SMOTE modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: 072268af52ebf7d3bede564d8c949eec9fc9f625
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73516194"
---
# <a name="smote"></a>SMOTE

Bu makalede, makine öğrenimi için kullanılan bir veri kümesindeki eksik temsil edilen durumların sayısını artırmak için Azure Machine Learning tasarımcısında (Önizleme) **Smote** modülünün nasıl kullanılacağı açıklanır. IÇGÖRÜME, yalnızca mevcut durumları çoğaltmaktan nadir durum sayısını artırmanın daha iyi bir yoludur.  

 SMOTE modülünü *imdengeli*bir veri kümesine bağlayabilirsiniz. Bir veri kümesinin dengesde görünmesinin pek çok nedeni vardır: hedeflediğiniz kategori nüfusda nadir olabilir veya verilerin toplanması zor olabilir. Genellikle, analiz etmek istediğiniz *sınıf* bölümünde temsil edildiğinde Smote kullanırsınız. 
  
 Modül, Belirttiğiniz yüzdeye bağlı olarak orijinal örnekleri içeren bir veri kümesi ve ek sayıda yapay azlık örneği döndürür.  
  
### <a name="more-about-smote"></a>SMOTE hakkında daha fazla bilgi

**Smote** , *yapay Azınma fazla örnekleme tekniği*için temsil eder. Bu, veri kümenizdeki durum sayısını dengeli bir şekilde artırmak için istatistiksel bir tekniktir.  Modül, giriş olarak sağladığınız mevcut minınlık çalışmalarından yeni örnekler oluşturarak işe yarar. Bu duman uygulamasının çoğunluğu büyük/ veya büyük/büyük

Yeni örnekler yalnızca mevcut minlık durumlarının kopyaları değildir; Bunun yerine, algoritma her bir hedef sınıf ve en yakın komşuları için *özellik alanının* örneklerini alır ve hedef durumunun özelliklerini komşunilerin özellikleriyle birleştiren yeni örnekler oluşturur. Bu yaklaşım her sınıf için kullanılabilen özellikleri artırır ve örnekleri daha genel hale getirir.
  
DUMAN, tüm veri kümesini giriş olarak alır, ancak yalnızca azlık durumlarının yüzdesini artırır. Örneğin, yalnızca %1 ' in hedef değerine sahip olduğu (minınlık sınıfı) ve servis taleplerinin %99 ' si B değerine sahip olan bir imdengeli veri kümeniz olduğunu varsayalım. Minınlık durumlarının yüzdesini önceki yüzdenin iki katına yükseltmek için, modülün özelliklerindeki **Smote yüzdesi** olarak 200 girersiniz.  
  
## <a name="examples"></a>Örnekler  

Nasıl çalıştığını görmek için küçük bir veri kümesiyle **Smote** kullanmayı denemeniz önerilir. Aşağıdaki örnek, Azure Machine Learning tasarımcısında bulunan kan bağış veri kümesini kullanır.
  
Veri kümesini bir işlem hattına ekler ve veri kümesinin çıktısında **Görselleştir** ' e tıklarsanız, 748 satır veya veri kümesindeki durum, 570 durum (%76) olduğunu görebilirsiniz. Sınıf 0 ve 178 durum (%24) 1. sınıf. Bu, korursız olmasa da, 1. sınıf, kanlı olan kişileri temsil eder ve bu satırlar, modellemek istediğiniz *özellik alanını* içerir.
 
Durum sayısını artırmak için, 100 katları kullanarak **Smote yüzdesi**değerini aşağıdaki gibi ayarlayabilirsiniz:

||Sınıf 0|Sınıf 1|Toplamda|  
|-|-------------|-------------|-----------|  
|Özgün veri kümesi<br /><br /> ( = **Smolik yüzdesine** eşdeğerdir)|570<br /><br /> %76|178<br /><br /> 24|748|  
|**Smote yüzdesi** = **100**|570<br /><br /> %62|356<br /><br /> %38|926|  
|**Smote yüzdesi** = **200**|570<br /><br /> %52|534<br /><br /> %48|1104|  
|**Smote yüzdesi** = **300**|570<br /><br /> %44|712<br /><br /> %56|1282|  
  
> [!WARNING]
>  Daha doğru modeller oluşturmak için, SMOTE kullanan servis taleplerinin sayısının artırılması garanti edilmez. Farklı yüzdeleri, farklı özellik kümelerini ve farklı sayıda komşu komşular kullanarak, servis taleplerinin modelinizi nasıl etkilediği hakkında bilgi almanız gerekir.  
  
## <a name="how-to-configure-smote"></a>SMOTE yapılandırma
  
1.  Ardışık düzene SMOTE modülünü ekleyin. Modül, veri dönüştürme modülleri altında, düzenleme kategorisinde bulunabilir.

2. Artırmak istediğiniz veri kümesini bağlayın. Yeni durumları oluşturmak için, yalnızca belirli sütunları kullanarak veya bazılarını dışlayarak özellik alanını belirtmek istiyorsanız, **Smote**'yı kullanmadan önce kullanmak istediğiniz sütunları yalıtmak Için [veri kümesinde sütunları seç](select-columns-in-dataset.md) modülünü kullanın.
  
    Aksi takdirde, **Smote** kullanarak yeni durumların oluşturulması, giriş olarak sağladığınız **Tüm** sütunları temel alır. Özellik sütunlarının en az bir sütunu sayısaldır.
  
3.  Etiketi veya hedef sınıfı içeren sütunun seçili olduğundan emin olun.  **Smote** yalnızca ikili Etiketler kabul eder.
  
4.  **Smote** modülü etiket sütunundaki minınlık sınıfını otomatik olarak tanımlar ve ardından minınlık sınıfı için tüm örnekleri alır. Tüm sütunlarda NaN olamaz.
  
5.  **Smote yüzdesi** seçeneğinde, çıkış veri kümesindeki minınlık durumlarının hedef yüzdesini gösteren bir tamsayı yazın. Örneğin:  
  
    - **0** (%) yazın. SMOTE modülü, giriş olarak verdiğiniz aynı veri kümesini, yeni bir minpom durumu ekleyerek de döndürür. Bu veri kümesinde, sınıf oranı değişmemiştir.  
  
    -   **100** (%) yazın. SMOTE modülü, özgün veri kümesindeki aynı sayıda minınlık durumu ekleyerek yeni minınlık durumları oluşturur. SMOTE, büyük/büyük/büyük/büyük/veya her bir sınıfın servis talebi oranını değiştirmediği için  
  
    -   **200** (%) yazın. Modül, özgün veri kümesiyle karşılaştırıldığında minınlık durumlarının yüzdesini iki katına çıkarır. Bu **,** daha önce olduğu gibi birçok minlık durumuna iki kez sahip olmaya neden olmaz.  Bunun yerine, veri kümesinin boyutu çok sayıda artar ve bu da, büyük/küçük bir servis talebi sayısı, istenen yüzde değeri ile eşleşene kadar artar.  
  
    > [!NOTE]
    > SMOTE yüzdesi için yalnızca 100 katları kullanın.

6.  Yeni durumlar oluştururken, SMOTE algoritmasının kullandığı özellik alanının boyutunu belirtmek için **en yakın komşu sayısı** seçeneğini kullanın. *En yakın komşu* , bazı hedef çalışmasına benzer bir veri satırıdır (bir durumdur). İki durum arasındaki mesafe, tüm özelliklerin ağırlıklı vektörlerini birleştirerek ölçülür.  
  
     + En yakın komşu sayısını artırarak, daha fazla durumda özellikler alırsınız.
     + En yakın komşuların sayısını düşük tutarak, özgün örnekteki gibi olan özellikleri kullanabilirsiniz.  
  
7. Aynı işlem hattının çalıştırılmasıyla aynı sonuçları aynı verilerle aynı şekilde sağlamak istiyorsanız **rastgele çekirdek** metin kutusuna bir değer yazın. Aksi takdirde modül, işlem hattı dağıtıldığında, çalışma sırasında biraz farklı sonuçlara neden olabilecek bir rastgele çekirdek üretir.

8. İşlem hattını çalıştırma.  
  
     Modülün çıktısı, özgün satırları içeren bir veri kümesidir ve azlık durumlarına sahip bazı eklenmiş satırları içerir.  

## <a name="technical-notes"></a>Teknik notlar

+ **Smote** modülünü kullanan bir model yayımlarken, bir Web hizmeti olarak yayınlanmadan önce, tahmine dayalı işlem hattından **Smote** 'yı kaldırın.  Bunun nedeni, SMOME 'nin eğitim sırasında bir modeli geliştirmesinin amaçlandığı ve Puanlama için tasarlanmamıştır.  Yayımlanan bir tahmine dayalı işlem hattı, SMOTE modülünü içeriyorsa bir hata alabilirsiniz.

+ Daha fazla sonuç elde etmek için eksik değer Temizleme veya diğer dönüştürmeleri, SMOTE uygulamadan önce verileri onarmak üzere uygularsanız daha iyi sonuçlar elde edebilirsiniz. 

+ Bazı araştırmacılar, SMOTE 'ın metin sınıflandırması veya genomikler veri kümelerinde kullanılanlar gibi yüksek boyutlu veya seyrek veriler üzerinde etkin olup olmadığını araştırmıştır. Bu raporda, bu gibi durumlarda SMOTE uygulamanın teorik ve teorik doğruluğuna ilişkin iyi bir Özet vardır: [Blagus ve lusa: yüksek boyutlu sınıf tarafından imlenen veriler IÇIN SMOTE](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-14-106)

    SMOTE, veri kümeniz için geçerli değilse, minlık durumlarını fazla örneklemek veya büyük/küçük durumların temelini oluşturmak için çeşitli yöntemler, kümeleme kullanarak doğrudan öğrenme yardımcı olan bir teknik bilgi de dahil olmak üzere, Bama veya Uyarlamalı yükseltme.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 

