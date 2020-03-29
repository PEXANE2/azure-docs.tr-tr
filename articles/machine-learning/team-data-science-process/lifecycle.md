---
title: Ekip Veri Bilimi Süreci yaşam döngüsü
description: Ekip Veri Bilimi Süreci (TDSP), veri bilimi projelerinizi yapılandırmak için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar.
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
ms.openlocfilehash: a043a1655950f3ed7688e59352f8a912146e12c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720461"
---
# <a name="the-team-data-science-process-lifecycle"></a>Ekip Veri Bilimi Süreci yaşam döngüsü

Ekip Veri Bilimi Süreci (TDSP), veri bilimi projelerinizi yapılandırmak için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü, başarılı projelerin izlediği tüm adımları özetler. Veri Madenciliği için Çapraz Endüstri Standart Süreci [(CRISP-DM)](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), Veritabanlarında Bilgi Bulma [(KDD)](https://wikipedia.org/wiki/Data_mining#Process)veya kuruluşunuzun kendi özel işlemi gibi başka bir veri bilimi yaşam döngüsü kullanıyorsanız, yine de görev tabanlı TDSP'yi kullanabilirsiniz. 

Bu yaşam döngüsü, akıllı uygulamaların bir parçası olarak gönderilmeyi amaçlayan veri bilimi projeleri için tasarlanmıştır. Bu uygulamalar, tahmine dayalı analitik için makine öğrenimi veya yapay zeka modellerini devreye sokar. Araştırmacı veri bilimi projeleri ve doğaçlama analiz projeleri de bu sürecin kullanımından yararlanabilir. Ancak bu projeler için, burada açıklanan bazı adımlara gerek olmayabilir. 

## <a name="five-lifecycle-stages"></a>Beş yaşam döngüsü aşaması

TDSP yaşam döngüsü yinelemeli olarak yürütülen beş ana aşamadan oluşur. Bu aşamalar şunlardır:

   1. [Kurumsal yaklaşım](lifecycle-business-understanding.md)
   2. [Veri edinme ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

TDSP yaşam döngüsünün görsel bir gösterimi aşağıdavelvettir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


TDSP yaşam döngüsü, tahmine dayalı modelleri kullanmak için gereken görevler hakkında rehberlik sağlayan bir dizi yeniden biçimlendirilmiş adım olarak modellenir. Akıllı uygulamalar oluşturmak için kullanmayı planladığınız üretim ortamında tahmine dayalı modelleri dağıtMış olursunuz. Bu işlem yaşam döngüsünün amacı, bir veri bilimi projesini net bir etkileşim bitiş noktasına taşımaya devam etmektir. Veri bilimi araştırma ve keşif bir egzersizdir. Standart şablonlar kullanan iyi tanımlanmış bir yapı tonu kullanarak görevleri ekibinize ve müşterilerinize iletebilme özelliği yanlış anlamaları önlemeye yardımcı olur. Bu şablonları kullanmak, karmaşık bir veri bilimi projesinin başarıyla tamamlanması olasılığını da artırır.

Her aşama için aşağıdaki bilgileri sağlarız:

   * **Hedefler**: Belirli hedefler.
   * **Nasıl yapılacağını**: Belirli görevlerin ana hatlarını ve bunların nasıl tamamlanılabildiğini anlatan kılavuzlar.
   * **Eserler**: Teslim ve bunları üretmek için destek.

## <a name="next-steps"></a>Sonraki adımlar

Belirli senaryolar için işlemdeki tüm adımları gösteren tam uçtan uca izlenme ler salıyoruz. [Örnek walkthroughs](walkthroughs.md) makalesi, bağlantılar ve küçük resim açıklamaları içeren senaryoların bir listesini sağlar. İzler, bulut, şirket içi araçlar ve hizmetlerin akıllı bir uygulama oluşturmak için iş akışı veya ardışık ardışık yollarla nasıl birleştirilebildiğini gösterir. 

Azure Machine Learning Studio kullanan TDSP'lerde adımların nasıl yürütüldüne ilgili örnekler [için](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bkz.
