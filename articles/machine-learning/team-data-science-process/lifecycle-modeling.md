---
title: Ekip Veri Bilimi Süreci yaşam döngüsünün modelleme aşaması
description: Veri bilimi projelerinizin modelleme aşaması için hedefler, görevler ve teslim edilebilir ler
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720478"
---
# <a name="modeling-stage-of-the-team-data-science-process-lifecycle"></a>Ekip Veri Bilimi Süreci yaşam döngüsünün modelleme aşaması

Bu makalede, Takım Veri Bilimi Süreci'nin (TDSP) modelleme aşamasıyla ilişkili hedefler, görevler ve teslim edilebilirler sıralanmıştır. Bu işlem, veri bilimi projelerinizi yapılandırmak için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü, projelerin genellikle yineleyici olarak yürüttüğü başlıca aşamaları özetler:

   1. **Kurumsal yaklaşım**
   2. **Veri edinme ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsünün görsel bir gösterimi aşağıdavelvettir:

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefler
* Makine öğrenimi modeli için en uygun veri özelliklerini belirleyin.
* Hedefi en doğru şekilde tahmin eden bilgilendirici bir makine öğrenme modeli oluşturun.
* Üretim için uygun bir makine öğrenme modeli oluşturun.

## <a name="how-to-do-it"></a>Nasıl yapılacağını
Bu aşamada ele üç ana görev vardır:

  * **Özellik mühendisliği**: Model eğitimini kolaylaştırmak için ham verilerden veri özellikleri oluşturun.
  * **Model eğitimi**: Başarı ölçümlerini karşılaştırarak soruyu en doğru şekilde yanıtlayan modeli bulun.
  * Modelinizin **üretim için uygun** olup olmadığını belirleyin.

### <a name="feature-engineering"></a>Özellik mühendisliği
Özellik mühendisliği, analizde kullanılan özellikleri oluşturmak için ham değişkenlerin dahil edilmesini, toplanması ve dönüşümünün kullanılmasını içerir. Bir modeli neyin yönlendirdiğinize dair bir fikir edinmek istiyorsanız, özelliklerin birbiriyle nasıl ilişkili olduğunu ve makine öğrenme algoritmalarının bu özellikleri nasıl kullandığını anlamanız gerekir. 

Bu adım, etki alanı uzmanlığının ve veri arama adımından elde edilen öngörülerin yaratıcı bir birleşimini gerektirir. Özellik mühendisliği, bilgilendirici değişkenleri bulmak ve içeren bir dengeleme eylemidir, ancak aynı zamanda çok fazla ilgisiz değişkenden kaçınmaya çalışır. Bilgilendirici değişkenler sonucunuzu geliştirir; ilgisiz değişkenler modele gereksiz gürültü ekler. Ayrıca puanlama sırasında elde edilen herhangi bir yeni veri için bu özellikleri oluşturmanız gerekir. Sonuç olarak, bu özelliklerin üretimi yalnızca puanlama sırasında kullanılabilen verilere bağlı olabilir. 

Çeşitli Azure veri teknolojilerinden yararlanırken özellik mühendisliği hakkında teknik rehberlik için [veri bilimi sürecinde Özellik mühendisliğibölümüne](create-features.md)bakın. 

### <a name="model-training"></a>Model eğitimi
Yanıtlamaya çalıştığınız sorunun türüne bağlı olarak, birçok modelleme algoritması vardır. Algoritmaları seçme kılavuzu için Microsoft [Azure Machine Learning için algoritmaları nasıl seçeceğiniz e](../studio/algorithm-choice.md)bakın. Bu makalede Azure Machine Learning kullanılsa da, sağladığı kılavuz tüm makine öğrenimi projeleri için yararlıdır. 

Model eğitimi süreci aşağıdaki adımları içerir: 

   * Bir eğitim veri kümesi ve test veri kümesi içine modelleme için giriş verilerini rasgele **bölün.**
   * Eğitim veri kümesini kullanarak **modelleri oluşturun.**
   * Eğitimi ve test veri kümesini **değerlendirin.** Geçerli verilerle ilgili soruyu yanıtlamaya yönelik çeşitli ilişkili aparat parametreleri *(parametre süpürmesi*olarak bilinir) ile birlikte bir dizi rakip makine öğrenme algoritması kullanın.
   * Alternatif yöntemler arasındaki başarı ölçümlerini karşılaştırarak soruyu yanıtlamak için **"en iyi" çözümü belirleyin.**

> [!NOTE]
> **Sızıntıyı önle**: Bir modelin veya makine öğrenme algoritmasının gerçekçi olmayan derecede iyi tahminlerde bulunamasına olanak tanıyan eğitim veri setinin dışından veri sızıntısına neden olabilirsiniz. Sızıntı, veri bilimciler gerçek olamayacak kadar iyi görünen tahmine dayalı sonuçlar elde ettiklerinde tedirgin olmalarının yaygın bir nedenidir. Bu bağımlılıkları tespit etmek zor olabilir. Sızıntıyı önlemek için genellikle bir analiz veri kümesi oluşturma, bir model oluşturma ve sonuçların doğruluğunu değerlendirmek arasında yineleyici gerektirir. 
> 
> 

TDSP ile bir temel model oluşturmak için birden çok algoritma ve parametre süpürmesi ile çalışan otomatik bir [modelleme ve raporlama aracı](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) salıyoruz. Ayrıca, değişken önemi de dahil olmak üzere her model ve parametre kombinasyonunun performansını özetleyen bir temel modelleme raporu da üretir. Bu süreç aynı zamanda daha fazla özellik mühendisliği sürücü olarak yinelemeli. 

## <a name="artifacts"></a>Yapıtlar
Bu aşamada üretilen eserler şunlardır:

   * [Özellik kümeleri](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Data_Report/Data%20Defintion.md): Modelleme için geliştirilen **özellikler, Veri tanım** raporunun **Özellik kümeleri** bölümünde açıklanmıştır. Özellikleri oluşturmak için kod işaretçileri ve özelliğin nasıl oluşturulduğuna ilgili bir açıklama içerir.
   * [Model raporu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): Denenen her model için, her deneme hakkında ayrıntılı bilgi sağlayan standart, şablon tabanlı bir rapor üretilir.
   * **Kontrol noktası kararı**: Modelin üretim için yeterli performans yapıp etmediğini değerlendirin. Sorması gereken bazı önemli sorular şunlardır:
     * Model, test verileri göz önüne alındığında soruyu yeterli güvenle yanıtlır mu? 
     * Herhangi bir alternatif yaklaşım denemeli misiniz? Ek veri toplamalı, daha fazla özellik mühendisliği yapmalı veya diğer algoritmalarla denemeniz mi gerekiyor?

## <a name="next-steps"></a>Sonraki adımlar

TDSP'nin yaşam döngüsündeki her adıma bağlantılar aşağıda vereb

   1. [Kurumsal yaklaşım](lifecycle-business-understanding.md)
   2. [Veri edinme ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımları gösteren tam uçtan uca izlenme ler salıyoruz. [Örnek walkthroughs](walkthroughs.md) makalesi, bağlantılar ve küçük resim açıklamaları içeren senaryoların bir listesini sağlar. İzler, bulut, şirket içi araçlar ve hizmetlerin akıllı bir uygulama oluşturmak için iş akışı veya ardışık ardışık yollarla nasıl birleştirilebildiğini gösterir. 

Azure Machine Learning Studio kullanan TDSP'lerde adımların nasıl yürütüldüne ilgili örnekler [için](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bkz. 
