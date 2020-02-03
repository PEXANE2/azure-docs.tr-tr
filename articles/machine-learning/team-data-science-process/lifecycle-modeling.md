---
title: Team Data Science Process yaşam döngüsü aşaması modelleme
description: Hedefleri, görevleri ve teslim edilebilirler için veri bilimi projelerinizi modelleme aşaması
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 1d3cd61ea3da88c4c5231f22c0e127508591fb8d
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720478"
---
# <a name="modeling-stage-of-the-team-data-science-process-lifecycle"></a>Team Data Science Process yaşam döngüsü aşaması modelleme

Bu makalede, hedeflerinizi, görevleri ve teslim edilebilirler ile Team Data Science işlem (TDSP) modelleme aşama ilişkili özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü projeleri genellikle genellikle yinelemeli olarak yürütme, önemli aşamalar açıklanmaktadır:

   1. **İş anlama**
   2. **Veri alma ve anlama**
   3. **Oluşturmanın**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsü görsel bir temsilini şu şekildedir:

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* Machine learning modelinin en iyi veri özelliklerini belirler.
* Hedef en doğru bir şekilde tahmin eden bilgilendirici bir makine öğrenimi modeli oluşturun.
* Üretim için uygun olan machine learning modeli oluşturun.

## <a name="how-to-do-it"></a>Nasıl yapılır
Bu aşamada yönelik üç ana görev vardır:

  * **Özellik Mühendisliği**: model eğitimini kolaylaştırmak için ham verilerden veri özellikleri oluşturun.
  * **Model eğitimi**: başarı ölçümlerini karşılaştırarak soruyu en doğru şekilde yanıtlayan modeli bulun.
  * Modelinizin **üretime uygun** olup olmadığını belirleme.

### <a name="feature-engineering"></a>Özellik mühendisliği
Özellik Mühendisliği, ekleme, toplama ve analizde kullanılan özellikler oluşturmak için ham değişkenleri dönüştürmeyi içerir. Bir model önünü açıyor Öngörüler isterseniz, özellikleri birbirleriyle nasıl ilişki kuracağını ve makine öğrenme algoritmalarını söz konusu özellikleri kullanır şeklini anlamanız gerekir. 

Bu adım veri araştırma adımda elde edilen içgörüleri ve etki alanı uzmanlığı yaratıcı bir birleşimini gerektirir. Özellik Mühendisliği bir bulma ve bilgilendirici değişkenleri dahil, ancak aynı anda çok fazla ilgisiz değişkenleri önlemek çalışırken Dengeleme işidir. Bilgilendirici değişkenleri sonuç artırın; İlişkisiz değişkenleri gereksiz bir gürültü modele tanıtır. Ayrıca, Puanlama sırasında elde edilen tüm yeni veriler için bu özellikleri oluşturmak gerekir. Sonuç olarak, bu özelliklerin oluşturma yalnızca Puanlama sırasında kullanılabilir olan veri bağlı olabilir. 

Çeşitli Azure veri teknolojilerini kullanırken Özellik Mühendisliği hakkında teknik yönergeler için bkz. [veri bilimi Işlemindeki Özellik Mühendisliği](create-features.md). 

### <a name="model-training"></a>Model eğitimi
Yanıtlamaya çalıştığınız sorunun türüne bağlı olarak, kullanılabilir birçok modelleme algoritması vardır. Algoritmaları seçme hakkında yönergeler için bkz. [Microsoft Azure Machine Learning algoritmaları seçme](../studio/algorithm-choice.md). Bu makalede, Azure Machine Learning kullansa da, herhangi bir makine öğrenimi projeleri için sağladığı Kılavuzu yararlıdır. 

İşlem modeli eğitimi için aşağıdaki adımları içerir: 

   * Modelleme verileri kümesine ve bir test veri kümesine modelleme için **giriş verilerini rastgele Böl** .
   * Eğitim veri kümesini kullanarak **modeller oluşturun** .
   * Eğitimi ve test veri kümesini **değerlendirin** . Geçerli verilerle ilgili soruya yanıt verme sorusunu karşılayan çeşitli ilişkili ayarlama parametreleri ( *parametre tarama*olarak bilinir) ile birlikte, bir dizi rekabet makinesi öğrenme algoritması kullanın.
   * Diğer Yöntemler arasındaki başarı ölçümlerini karşılaştırarak soruyu yanıtlamak için **"en iyi" çözümü belirleme** .

> [!NOTE]
> **Sızıntıdan kaçının**: bir model veya makine öğrenimi algoritmasının, gerçekçi bir şekilde sağlam tahminler yapmasına izin veren eğitim verileri kümesinin dışından veri eklerseniz veri sızıntılarına neden olabilirsiniz. Sızıntısına neden veri uzmanları, Tahmine dayalı sonuçları alın, tedirgin doğru olması abartılı yaygın bir nedenidir. Bu bağımlılıklar algılanması zor olabilir. Genellikle sızıntısını önlemek için bir analysis veri kümesi oluşturma, bir model oluşturma ve değerlendirme sonuçları doğruluğunu arasında yineleme gerektirir. 
> 
> 

TDSP ile, bir temel model oluşturmak için birden çok algoritmadan ve parametre süpürmeleri aracılığıyla çalışabilecek [otomatik bir modelleme ve Raporlama Aracı](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) sağlıyoruz. Ayrıca, değişken önem dahil olmak üzere her model ve parametre birleşimi performansını özetleyen rapor modelleme bir temel oluşturur. Bu işlem, ayrıca daha fazla özellik Mühendisliği yönlendirebilirsiniz gibi yinelemelidir. 

## <a name="artifacts"></a>Yapıtlar
Bu aşamada üretilen yapıtları içerir:

   * [Özellik kümeleri](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Data_Report/Data%20Defintion.md): modelleme için geliştirilen özellikler, **veri tanımı** raporunun **özellik kümeleri** bölümünde açıklanmaktadır. Bu özellikler ve özellik nasıl oluşturulduğunu tanımını oluşturmak için kod işaretçileri içerir.
   * [Model raporu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): denenen her bir model için, her bir denemeye ilişkin ayrıntıları sağlayan standart, şablon tabanlı bir rapor oluşturulur.
   * **Kontrol noktası kararı**: modelin üretim için yeterince performans yapıp gerçekleştirmediğini değerlendirin. Bazı temel sorular sormak için şunlardır:
     * Modeli belirtilen test veri yeterli güvenle sorusunu mu? 
     * Herhangi bir alternatif yaklaşımlar çalışmanız gerekir? Ek veri topla, daha fazla özellik Mühendisliği yapın veya diğer algoritmalar ile denemeler?

## <a name="next-steps"></a>Sonraki adımlar

TDSP yaşam döngüsü içinde her adım için bağlantılar şunlardır:

   1. [İş anlama](lifecycle-business-understanding.md)
   2. [Veri alma ve anlama](lifecycle-data.md)
   3. [Oluşturmanın](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

İşlemin belirli senaryolar için tüm adımları gösteren uçtan uca tam talimatlara sunuyoruz. [Örnek yönergeler](walkthroughs.md) makalesi, bağlantılar ve küçük resim açıklamaları olan senaryoların bir listesini sağlar. İzlenecek bir iş akışı veya işlem hattı akıllı bir uygulama oluşturmak için bulut, şirket içi araçları ve Hizmetleri birleştirme işlemini göstermektedir. 

Azure Machine Learning Studio kullanan TDSPs 'de adımların nasıl yürütüleceği hakkında örnekler için, bkz. [Azure Machine Learning Ile TDSP kullanma](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/). 
