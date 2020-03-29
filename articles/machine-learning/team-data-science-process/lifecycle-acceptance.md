---
title: Ekip Veri Bilimi Süreci yaşam döngüsünün müşteri kabul aşaması
description: Veri bilimi projelerinizin müşteri kabul aşaması için hedefler, görevler ve teslim edilebilir ler
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
ms.openlocfilehash: 7224a7bb26ef491915df9fcb9b6b84ff171a9fc0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720529"
---
# <a name="customer-acceptance-stage-of-the-team-data-science-process-lifecycle"></a>Ekip Veri Bilimi Süreci yaşam döngüsünün müşteri kabul aşaması

Bu makalede, Ekip Veri Bilimi Süreci'nin (TDSP) müşteri kabul aşamasıyla ilişkili hedefler, görevler ve teslim edilebilir ler sıralanmıştır. Bu işlem, veri bilimi projelerinizi yapılandırmak için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü, projelerin genellikle yineleyici olarak yürüttüğü başlıca aşamaları özetler:

   1. **Kurumsal yaklaşım**
   2. **Veri edinme ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsünün görsel bir gösterimi aşağıdavelvettir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Hedef
**Proje teslim edilebilirlerini sonuçlandırmak**: Boru hattının, modelin ve bunların üretim ortamında dağıtımLarının müşterinin hedeflerini karşıladığını onaylayın.

## <a name="how-to-do-it"></a>Nasıl yapılacağını
Bu aşamada ele verilen iki ana görev vardır:

   * **Sistem doğrulama**: Dağıtılan modelin ve boru hattının müşterinin ihtiyaçlarını karşıladığını doğrulayın.
   * **Proje teslimi**: Projeyi, sistemi üretimde çalıştıracak varlığa teslim edin.

Müşteri, sistemin iş gereksinimlerini karşıladığını ve istemcinin uygulaması tarafından kullanılmak üzere sistemi üretime dağıtmak için kabul edilebilir bir doğrulukla soruları yanıtladığını doğrulamalıdır. Tüm belgeler sonuçlandırılır ve gözden geçirilir. Proje, operasyonlardan sorumlu kuruluşa devredilir. Bu varlık, örneğin, bir BT veya müşteri veri bilimi ekibi veya üretimde sistemi çalıştırmaktan sorumlu müşterinin temsilcisi olabilir. 

## <a name="artifacts"></a>Yapıtlar
Bu son aşamada üretilen ana eser **müşteri için projenin Çıkış raporudur.** Bu teknik rapor, sistemin nasıl işletilmesi hakkında bilgi edinmek için yararlı olan projenin tüm ayrıntılarını içerir. TDSP bir [Çıkış raporu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) şablonu sağlar. Şablonu olduğu gibi kullanabilir veya belirli istemci gereksinimlerine göre özelleştirebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

TDSP'nin yaşam döngüsündeki her adıma bağlantılar aşağıda vereb

   1. [Kurumsal yaklaşım](lifecycle-business-understanding.md)
   2. [Veri edinme ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımları gösteren tam izlenme ler sağlarız. [Örnek walkthroughs](walkthroughs.md) makalesi, bağlantılar ve küçük resim açıklamaları içeren senaryoların bir listesini sağlar. İzler, bulut, şirket içi araçlar ve hizmetlerin akıllı bir uygulama oluşturmak için iş akışı veya ardışık ardışık yollarla nasıl birleştirilebildiğini gösterir. 

Azure Machine Learning Studio kullanan TDSP'lerde adımların nasıl yürütüldüne ilgili örnekler [için](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bkz.
