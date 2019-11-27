---
title: Team Data Science Işlem yaşam döngüsünün modelleme aşaması
description: Veri bilimi projelerinizin modelleme aşamasına yönelik hedefler, görevler ve teslim edilebilirler
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: d72d39a2a59e06954c36473083af2d2b4689a7b6
ms.sourcegitcommit: 36eb583994af0f25a04df29573ee44fbe13bd06e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74538217"
---
# <a name="modeling-stage-of-the-team-data-science-process-lifecycle"></a>Team Data Science Işlem yaşam döngüsünün modelleme aşaması

Bu makalede, Team Data Science Işleminin (TDSP) modelleme aşamasına ilişkin hedefler, görevler ve teslim edilebilirler özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapılandırmak için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü, projelerin tipik olarak yürütülen ana aşamaları özetler, genellikle yinelemeli olarak:

   1. **İş anlama**
   2. **Veri alma ve anlama**
   3. **Oluşturmanın**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsünün görsel bir gösterimi aşağıda verilmiştir:

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* Machine Learning modeli için en iyi veri özelliklerini saptayın.
* En doğru hedefi tahmin eden bilgilendirici bir makine öğrenimi modeli oluşturun.
* Üretime uygun bir makine öğrenimi modeli oluşturun.

## <a name="how-to-do-it"></a>Nasıl yapılır?
Bu aşamada listelenen üç ana görev vardır:

  * **Özellik Mühendisliği**: model eğitimini kolaylaştırmak için ham verilerden veri özellikleri oluşturun.
  * **Model eğitimi**: başarı ölçümlerini karşılaştırarak soruyu en doğru şekilde yanıtlayan modeli bulun.
  * Modelinizin **üretime uygun** olup olmadığını belirleme.

### <a name="feature-engineering"></a>Özellik mühendisliği
Özellik Mühendisliği, çözümlemede kullanılan özellikleri oluşturmak için ham değişkenlerin dahil edilmesi, toplanmasını ve dönüştürülmesini içerir. Bir modeli yönlendiren Öngörüler hakkında bilgi edinmek istiyorsanız, özelliklerin birbirleriyle nasıl ilişkilendirildiğini ve makine öğrenimi algoritmalarının bu özellikleri nasıl kullandığını anlamanız gerekir. 

Bu adım, etki alanı uzmanlığından oluşan yaratıcı bir bileşim ve veri araştırma adımından elde edilen Öngörüler gerektirir. Özellik Mühendisliği, bilgilendirici değişkenleri bulmayı ve dahil etme ve çok fazla ilgisiz değişkenden kaçınmaya çalışan aynı zamanda bir işlem işlemidir. Bilgilendirici değişkenler sonucu geliştirir; ilişkisiz değişkenler, modele gereksiz gürültü sunar. Ayrıca, Puanlama sırasında elde edilen yeni veriler için bu özellikleri oluşturmanız gerekir. Sonuç olarak, bu özelliklerin oluşturulması yalnızca Puanlama sırasında bulunan verilere bağlı olabilir. 

Çeşitli Azure veri teknolojilerini kullanırken Özellik Mühendisliği hakkında teknik yönergeler için bkz. [veri bilimi Işlemindeki Özellik Mühendisliği](create-features.md). 

### <a name="model-training"></a>Model eğitimi
Yanıtlamaya çalıştığınız soru türüne bağlı olarak, kullanılabilir birçok modelleme algoritması vardır. Algoritmaları seçme hakkında yönergeler için bkz. [Microsoft Azure Machine Learning algoritmaları seçme](../studio/algorithm-choice.md). Bu makalede Azure Machine Learning kullanılsa da, sağladığı rehberlik tüm makine öğrenimi projeleri için yararlıdır. 

Model eğitimi işlemi aşağıdaki adımları içerir: 

   * Modelleme verileri kümesine ve bir test veri kümesine modelleme için **giriş verilerini rastgele Böl** .
   * Eğitim veri kümesini kullanarak **modeller oluşturun** .
   * Eğitimi ve test veri kümesini **değerlendirin** . Geçerli verilerle ilgili soruya yanıt verme sorusunu karşılayan çeşitli ilişkili ayarlama parametreleri ( *parametre tarama*olarak bilinir) ile birlikte, bir dizi rekabet makinesi öğrenme algoritması kullanın.
   * Diğer Yöntemler arasındaki başarı ölçümlerini karşılaştırarak soruyu yanıtlamak için **"en iyi" çözümü belirleme** .

> [!NOTE]
> **Sızıntıdan kaçının**: bir model veya makine öğrenimi algoritmasının, gerçekçi bir şekilde sağlam tahminler yapmasına izin veren eğitim verileri kümesinin dışından veri eklerseniz veri sızıntılarına neden olabilirsiniz. Sızıntı, veri bilimcilerinin, doğru olması için çok iyi sonuç veren tahmine dayalı sonuçlar edindiklerinde nervous elde ettikleri yaygın bir nedendir. Bu bağımlılıkların algılanması zor olabilir. Sızıntı olmaması için genellikle analiz veri kümesi oluşturma, model oluşturma ve sonuçların doğruluğunu değerlendirme arasında yineleme yapılması gerekir. 
> 
> 

TDSP ile, bir temel model oluşturmak için birden çok algoritmadan ve parametre süpürmeleri aracılığıyla çalışabilecek [otomatik bir modelleme ve Raporlama Aracı](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) sağlıyoruz. Ayrıca, değişken önem dahil her modelin ve parametre birleşiminin performansını özetleyen bir temel modelleme raporu da üretir. Bu işlem, daha fazla özellik Mühendisliği için de kullanılabilir. 

## <a name="artifacts"></a>Artifacts
Bu aşamada oluşturulan yapıtlar şunlardır:

   * [Özellik kümeleri](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Data_Report/Data%20Defintion.md): modelleme için geliştirilen özellikler, **veri tanımı** raporunun **özellik kümeleri** bölümünde açıklanmaktadır. Özellik oluşturmak için kodun işaretçilerini ve özelliğin nasıl oluşturulduğunu gösteren bir açıklama içerir.
   * [Model raporu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): denenen her bir model için, her bir denemeye ilişkin ayrıntıları sağlayan standart, şablon tabanlı bir rapor oluşturulur.
   * **Kontrol noktası kararı**: modelin bir üretim sistemine dağıtmak için yeterince iyi performans yapıp yapmadığını değerlendirin. Sorabileceğiniz bazı önemli sorular şunlardır:
     * Model, test verileri için yeterli güvenle sorusuna yanıt veriyor mu? 
     * Alternatif yaklaşımlar denemeli mi? Ek verileri toplamanız, daha fazla özellik mühendisi mi, yoksa diğer algoritmalardan mi denemeler yapmanız gerekir?

## <a name="next-steps"></a>Sonraki adımlar

TDSP yaşam döngüsüyle her adımın bağlantıları aşağıda verilmiştir:

   1. [İş anlama](lifecycle-business-understanding.md)
   2. [Veri alma ve anlama](lifecycle-data.md)
   3. [Oluşturmanın](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımları gösteren eksiksiz bir uçtan uca izlenecek yol sunuyoruz. [Örnek yönergeler](walkthroughs.md) makalesi, bağlantılar ve küçük resim açıklamaları olan senaryoların bir listesini sağlar. İzlenecek yollar, akıllı bir uygulama oluşturmak için bulut, şirket içi araçların ve hizmetlerin bir iş akışı veya işlem hattına nasıl birleştirileceğini gösterir. 

Azure Machine Learning Studio kullanan TDSPs 'de adımların nasıl yürütüleceği hakkında örnekler için, bkz. [Azure Machine Learning Ile TDSP kullanma](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/). 
