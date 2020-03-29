---
title: Ekip Veri Bilimi Süreci yaşam döngüsünün dağıtım aşaması
description: Veri bilimi projelerinizin dağıtım aşaması için hedefler, görevler ve teslim edilebilir ler
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
ms.openlocfilehash: 1138c95274c769186a9a29aa4d35517e378baeae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720495"
---
# <a name="deployment-stage-of-the-team-data-science-process-lifecycle"></a>Ekip Veri Bilimi Süreci yaşam döngüsünün dağıtım aşaması

Bu makalede, Ekip Veri Bilimi Süreci (TDSP) dağıtımıyla ilişkili hedefler, görevler ve teslim edilebilir ler sıralanmıştır. Bu işlem, veri bilimi projelerinizi yapılandırmak için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü, projelerin genellikle yineleyici olarak yürüttüğü başlıca aşamaları özetler:

   1. **Kurumsal yaklaşım**
   2. **Veri edinme ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsünün görsel bir gösterimi aşağıdavelvettir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Hedef
Son kullanıcı kabulü için veri aktarıcısına sahip modelleri üretim veya üretim benzeri bir ortama dağıtın. 

## <a name="how-to-do-it"></a>Nasıl yapılacağını
Bu aşamada ele alınacak ana görev:

**Modeli operasyonel hale getirmek**: Modeli ve boru hattını uygulama tüketimi için üretim veya üretim benzeri bir ortama dağıtın.

### <a name="operationalize-a-model"></a>Bir modeli kullanıma hazır hale getirme
İyi performans gösteren bir dizi modele sahip olduktan sonra, bunları diğer uygulamaların tüketebileceği şekilde işlevsel hale getirebilirsiniz. İş gereksinimlerine bağlı olarak, tahminler gerçek zamanlı olarak veya toplu olarak yapılır. Modelleri dağıtmak için, açık bir API arabirimi ile bunları ortaya çıkarırsınız. Arabirim, modelin aşağıdakiler gibi çeşitli uygulamalardan kolayca tüketilmesini sağlar:

   * Çevrimiçi web siteleri
   * Elektronik tablolar 
   * Panolar
   * İş yeri uygulamaları 
   * Arka uç uygulamaları 

Azure Machine Learning web hizmetiyle model operasyonelleştirme örnekleri [için](../studio/deploy-a-machine-learning-web-service.md)bkz. Telemetri oluşturmak ve dağıtım yaptığınız veri ardışık yapısına izleme yapmak en iyi yöntemdir. Bu uygulama sonraki sistem durumu raporlama ve sorun giderme ile yardımcı olur.  

## <a name="artifacts"></a>Yapıtlar

* Sistem durumunu ve temel ölçümleri görüntüleyen bir durum panosu
* Dağıtım ayrıntılarını içeren son modelleme raporu
* Nihai çözüm mimarisi belgesi


## <a name="next-steps"></a>Sonraki adımlar

TDSP'nin yaşam döngüsündeki her adıma bağlantılar aşağıda vereb

   1. [Kurumsal yaklaşım](lifecycle-business-understanding.md)
   2. [Veri Toplama ve Anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımları gösteren tam izlenme ler sağlarız. [Örnek walkthroughs](walkthroughs.md) makalesi, bağlantılar ve küçük resim açıklamaları içeren senaryoların bir listesini sağlar. İzler, bulut, şirket içi araçlar ve hizmetlerin akıllı bir uygulama oluşturmak için iş akışı veya ardışık ardışık yollarla nasıl birleştirilebildiğini gösterir. 

Azure Machine Learning Studio kullanan TDSP'lerde adımların nasıl yürütüldüne ilgili örnekler [için](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bkz.
