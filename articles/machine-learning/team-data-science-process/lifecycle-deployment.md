---
title: Team Data Science Process yaşam döngüsü dağıtım aşaması
description: Hedefleri, görevleri ve teslim edilebilirler için veri bilimi projelerinizi dağıtım aşaması
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
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720495"
---
# <a name="deployment-stage-of-the-team-data-science-process-lifecycle"></a>Team Data Science Process yaşam döngüsü dağıtım aşaması

Bu makalede, hedeflerinizi, görevleri ve teslim edilebilirler ile Team Data Science işlem (TDSP) dağıtımı ilişkili özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü projeleri genellikle genellikle yinelemeli olarak yürütme, önemli aşamalar açıklanmaktadır:

   1. **İş anlama**
   2. **Veri alma ve anlama**
   3. **Oluşturmanın**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsü görsel bir temsilini şu şekildedir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Hedef
Bir üretim ya da üretim ortamına benzer son kullanıcı onay için bir işlem hattıyla veri modelleri dağıtın. 

## <a name="how-to-do-it"></a>Nasıl yapılır
Bu aşamada ele ana görev:

**Modeli**çalıştırın: model ve işlem hattını, uygulama tüketimi için bir üretime veya üretime benzer bir ortama dağıtın.

### <a name="operationalize-a-model"></a>Bir modeli kullanıma hazır hale getirme
Bir dizi iyi gerçekleştirilip modelleri oluşturduktan sonra bunları kullanmak, diğer uygulamalar için çalışır hale getirebilirsiniz. İş gereksinimlerine bağlı olarak, Öngörüler, gerçek zamanlı olarak veya toplu olarak yapılır. Modelleri dağıtmak için bunları açık bir API arabirimi ile kullanıma gerekir. Arabirim modelinin çeşitli uygulamalardan gibi kolayca kullanılabilmesini sağlar:

   * Çevrimiçi Web siteleri
   * Elektronik tablolar 
   * Panolar
   * Satır iş kolu uygulamaları 
   * Arka uç uygulamaları 

Azure Machine Learning Web hizmeti ile model işlemleştirme örnekleri için bkz. [Azure Machine Learning Web hizmeti dağıtma](../studio/deploy-a-machine-learning-web-service.md). Telemetri ve üretim modeli ve dağıttığınız veri işlem hattını izleme oluşturmak için en iyi bir uygulamadır. Bu yöntem, raporlama ve sorun giderme sonraki sistem durumu ile yardımcı olur.  

## <a name="artifacts"></a>Yapıtlar

* Sistem durumunu ve anahtar ölçümlerin gösteren bir durum Panosu
* Dağıtım ayrıntıları son modelleme raporla
* Nihai çözüm mimarisi belge


## <a name="next-steps"></a>Sonraki adımlar

TDSP yaşam döngüsü içinde her adım için bağlantılar şunlardır:

   1. [İş anlama](lifecycle-business-understanding.md)
   2. [Veri alma ve anlama](lifecycle-data.md)
   3. [Oluşturmanın](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımları gösteren tam izlenecek yollar sunuyoruz. [Örnek yönergeler](walkthroughs.md) makalesi, bağlantılar ve küçük resim açıklamaları olan senaryoların bir listesini sağlar. İzlenecek bir iş akışı veya işlem hattı akıllı bir uygulama oluşturmak için bulut, şirket içi araçları ve Hizmetleri birleştirme işlemini göstermektedir. 

Azure Machine Learning Studio kullanan TDSPs 'de adımların nasıl yürütüleceği hakkında örnekler için, bkz. [Azure Machine Learning Ile TDSP kullanma](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).
