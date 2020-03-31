---
title: SMOTE
titleSuffix: Azure Machine Learning
description: Aşırı örnekleme kullanarak veri kümesindeki düşük insidanslı örneklerin sayısını artırmak için Azure Machine Learning'deki SMOTE modülünün nasıl kullanılacağını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: ed6d9e86143c3a5d6c97c4bd92a07c258bbd1bbc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477468"
---
# <a name="smote"></a>SMOTE

Bu makalede, makine öğrenimi için kullanılan bir veri kümesinde yeterince temsil edilmeyen servis talebi sayısını artırmak için Azure Machine Learning tasarımcısındaki (önizleme) SMOTE modülünün nasıl kullanılacağı açıklanmaktadır. SMOTE, nadir vakaların sayısını artırmanın sadece varolan vakaları çoğaltmaktan daha iyi bir yoludur.  

SMOTE modülünü *dengesiz*bir veri kümesine bağlarsınız. Bir veri kümesinin dengesiz olmasının birçok nedeni olabilir. Örneğin, hedeflediğiniz kategori popülasyonda nadir olabilir veya verileri toplamak zor olabilir. Genellikle, çözümlemek istediğiniz *sınıf* yeterince temsil edilmiyorsa SMOTE'yi kullanırsınız. 
  
Modül, özgün örnekleri içeren bir veri kümesi döndürür. Ayrıca, belirttiğiniz yüzdebağlı olarak bir dizi sentetik azınlık numunesi de döndürür.  
  
## <a name="more-about-smote"></a>SMOTE hakkında daha fazla

Sentetik Azınlık Örnekleme Tekniği (SMOTE), veri setinizdeki vaka sayısını dengeli bir şekilde artırmak için istatistiksel bir tekniktir. Modül, girdi olarak sağladığınız mevcut azınlık durumlardan yeni örnekler oluşturarak çalışır. SMOTE'nin bu uygulaması çoğunluk durumlarının sayısını *değiştirmez.*

Yeni örnekler sadece mevcut azınlık davalarının kopyaları değil. Bunun yerine, algoritma her hedef sınıf ve en yakın komşuları için *özellik alanı* örnekleri alır. Algoritma daha sonra, hedef servis talebinin özelliklerini komşularının özellikleriyle birleştiren yeni örnekler oluşturur. Bu yaklaşım, her sınıfın kullanabileceği özellikleri artırır ve örnekleri daha genel hale getirir.
  
SMOTE tüm veri kümesini bir girdi olarak alır, ancak yalnızca azınlık vakalarının yüzdesini artırır. Örneğin, vakaların yalnızca yüzde 1'inin hedef değer A'ya (azınlık sınıfı) ve vakaların yüzde 99'unun B değerine sahip olduğu dengesiz bir veri kümeniz olduğunu varsayalım. Azınlık vakalarının yüzdesini önceki yüzdenin iki katına çıkarmak için, modülün özelliklerine **SMOTE yüzdesi** için **200** girersiniz.  
  
## <a name="examples"></a>Örnekler  

Nasıl çalıştığını görmek için küçük bir veri kümesiyle SMOTE'u kullanmayı denemenizi öneririz. Aşağıdaki örnekte, Azure Machine Learning tasarımcısında bulunan Kan Bağışı veri kümesi kullanılır.
  
Veri kümesini bir ardışık sıraya ekler ve veri kümesinin çıktısında **Görselleştir'i** seçerseniz, veri kümesindeki 748 satır veya servis genelliklenin 570 örneğinin (yüzde 76) Sınıf 0 ve 178 servis örneğinin (yüzde 24) Sınıf 1'den olduğunu görebilirsiniz. Bu sonuç son derece dengesiz olmasa da, Sınıf 1 kan bağışlayan kişileri temsil eder, bu nedenle bu satırlar modellemek istediğiniz *özellik alanını* içerir.
 
Servis taleplerini artırmak için, 100'ün katlarını kullanarak **SMOTE yüzdesinin**değerini aşağıdaki gibi ayarlayabilirsiniz:

||Sınıf 0|Sınıf 1|toplam|  
|-|-------------|-------------|-----------|  
|Orijinal veri kümesi<br /><br /> **(SMOTE yüzdesi** = **0'a**eşdeğer )|570<br /><br /> 76%|178<br /><br /> 24%|748|  
|**SMOTE yüzdesi** = **100**|570<br /><br /> 62%|356<br /><br /> 38%|926|  
|**SMOTE yüzdesi** = **200**|570<br /><br /> %52|534<br /><br /> 48%|1,104|  
|**SMOTE yüzdesi** = **300**|570<br /><br /> 44%|712<br /><br /> 56%|1,282|  
  
> [!WARNING]
> SMOTE kullanarak servis taleplerini artırmak daha doğru modeller üretmek için garanti edilmez. Servis talepleri eklemenin modelinizi nasıl etkilediğini görmek için farklı yüzdelerle, farklı özellik kümeleriyle ve farklı sayıda en yakın komşuyla pipelining yapmayı deneyin.  
  
## <a name="how-to-configure-smote"></a>SMOTE nasıl yapılandırılır?
  
1.  SMOTE modüllerini boru hattınıza ekleyin. Modülü Veri Dönüşümü **modülleri**altında, **Manipülasyon** kategorisinde bulabilirsiniz.

2. Artırmak istediğiniz veri kümesini bağlayın. Yalnızca belirli sütunlar kullanarak veya bazılarını hariç tutarak yeni servis taleplerini oluşturmak için özellik alanını belirtmek istiyorsanız, [Dataset modülündeki Sütunları Seç'i](select-columns-in-dataset.md) kullanın. Daha sonra SMOTE kullanmadan önce kullanmak istediğiniz sütunları yalıtabilirsiniz.
  
    Aksi takdirde, SMOTE aracılığıyla yeni servis talepleri nin oluşturulması, girdi olarak sağladığınız *tüm* sütunları temel alar. Özellik sütunlarının en az bir sütunu sayısaldır.
  
3.  Etiket veya hedef sınıfı içeren sütunun seçildiğinden emin olun. SMOTE yalnızca ikili etiketleri kabul eder.
  
4.  SMOTE modülü, etiket sütunundaki azınlık sınıfını otomatik olarak tanımlar ve azınlık sınıfı için tüm örnekleri alır. Tüm sütunlar NaN değerlerine sahip olamaz.
  
5.  **SMOTE yüzdesi** seçeneğinde, çıktı veri kümesindeki azınlık durumlarının hedef yüzdesini gösteren bir tam sayı girin. Örnek:  
  
    - **0**girersiniz. SMOTE modülü, girdi olarak sağladığınız veri kümesini tam olarak döndürür. Yeni azınlık davaları da ekliyor. Bu veri kümesinde sınıf oranı değişmedi.  
  
    - **100 girersin.** SMOTE modülü yeni azınlık vakaları oluşturur. Bu orijinal veri kümesinde olan azınlık durumlarda aynı sayıda ekler. SMOTE çoğunluk durumlarının sayısını artırmadığından, her sınıfın servis taleplerini n oranı değişti.  
  
    - **200 girersin.** Modül, orijinal veri kümesine kıyasla azınlık vakalarının yüzdesini iki katına çıkar. Bu da öncekinin iki katı kadar azınlık davasına yol *alamayıyor.* Bunun yerine, veri kümesinin boyutu, çoğunluk durum larının sayısının aynı kalacağı şekilde artar. İstenilen yüzde değeriyle eşleşene kadar azınlık vakalarının sayısı artırılır.  
  
    > [!NOTE]
    > SMOTE yüzdesi için 100'ün yalnızca katlarını kullanın.

6.  SMOTE **algoritmasının** yeni servis talepleri oluşturmak için kullandığı özellik alanının boyutunu belirlemek için en yakın komşu sayısı seçeneğini kullanın. En yakın komşu, hedef servis talebine benzer bir veri satırıdır (servis talebi). Herhangi iki durum arasındaki mesafe, tüm özelliklerin ağırlıklı vektörleri birleştirilerek ölçülür.  
  
    + En yakın komşuların sayısını artırarak, daha fazla durumda özellikler elde elabilirsiniz.
    + En yakın komşu sayısını düşük tutarak, orijinal örnektekilere daha çok benzeyen özellikler kullanırsınız.  
  
7. Aynı veri ile aynı ardışık ardışık çalışır üzerinde aynı sonuçları sağlamak istiyorsanız **Random tohum** kutusuna bir değer girin. Aksi takdirde, boru hattı dağıtıldığında modül işlemci saat değerlerine dayalı rasgele bir tohum oluşturur. Rasgele bir tohum üretimi çalışır üzerinde biraz farklı sonuçlara neden olabilir.

8. Boru hattını gönderin.  
  
   Modülün çıktısı, özgün satırların yanı sıra azınlık durumları olan bir dizi ek satır içeren bir veri kümesidir.  

## <a name="technical-notes"></a>Teknik notlar

+ **SMOTE** modüllerini kullanan bir model yayımlarken, web hizmeti olarak yayımlanmadan önce **SMOTE'yi** tahmin edilen ardışık kaynaktan kaldırın. Bunun nedeni, SMOTE'nin eğitim sırasında bir modeli geliştirmek için tasarlanmış olmasıdır, puanlama için değil. Yayınlanmış bir tahmin hattı SMOTE modüllerini içeriyorsa hata alabilirsiniz.

+ SMOTE'u uygulamadan önce eksik değerleri temizlerseniz veya verileri düzeltmek için başka dönüşümler uygularsanız genellikle daha iyi sonuçlar elde edebilirsiniz. 

+ Bazı araştırmacılar SMOTE'nin metin sınıflandırmasında veya genomik veri kümelerinde kullanılan veriler gibi yüksek boyutlu veya seyrek veriler üzerinde etkili olup olmadığını araştırmış. Bu yazı etkileri ve bu gibi durumlarda SMOTE uygulama teorik geçerliliği iyi bir özeti vardır: [Blagus ve Lusa: Yüksek boyutlu sınıf dengesiz veri için SMOTE](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-14-106).

+ SMOTE veri setinizde etkili değilse, düşünebilirsiniz diğer yaklaşımlar şunlardır:
  + Azınlık davalarını aşırı örnekleme veya çoğunluk vakalarının alt örnekleme yöntemleri.
  + Kümeleme, torbalama veya uyarlamalı güçlendirme kullanarak doğrudan öğrenciye yardımcı olan topluluk teknikleri.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 

