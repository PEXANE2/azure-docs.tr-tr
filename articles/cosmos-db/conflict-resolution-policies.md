---
title: Azure Cosmos DB'de çakışma çözümleme türleri ve çözüm ilkeleri
description: Bu makalede, Azure Cosmos DB'deki çakışma kategorileri ve çakışma çözümlemesi ilkeleri açıklanmaktadır.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: a8ee72f46e1789088e779c10a0824262469ffde8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75441981"
---
# <a name="conflict-types-and-resolution-policies"></a>Çakışma türleri ve çözme ilkeleri

Azure Cosmos DB hesabınız birden çok yazma bölgesiyle yapılandırıldıysa, çakışma ve çakışma çözümilkeleri uygulanabilir.

Birden çok yazma bölgesiyle yapılandırılan Azure Cosmos hesapları için, yazarlar aynı anda aynı maddeyi birden çok bölgede güncelleştirdiğinde güncelleştirme çakışmaları oluşabilir. Güncelleştirme çakışmaları aşağıdaki üç tür olabilir:

* **Ek çakışmaları**: Bir uygulama aynı anda iki veya daha fazla maddeyi iki veya daha fazla bölgede aynı benzersiz dizinle eklediğinde bu çakışmalar oluşabilir. Örneğin, bu çakışma bir kimlik özelliği ile oluşabilir.

* **Çakışma Ları Değiştir**: Bir uygulama aynı öğeyi aynı anda iki veya daha fazla bölgede güncellediğinde bu çakışmalar oluşabilir.

* **Delete çakışmaları**: Bu çakışmalar, bir uygulama bir bölgedeki bir öğeyi aynı anda sildiğinde ve başka bir bölgede güncellediğinde oluşabilir.

## <a name="conflict-resolution-policies"></a>Çakışma çözümleme ilkeleri

Azure Cosmos DB, yazma çakışmalarını çözmek için esnek bir ilke odaklı mekanizma sunar. Bir Azure Cosmos kapsayıcıda iki çakışma çözümlemesi ilkesinden birini seçebilirsiniz:

* **Son Yazma Kazançları (LWW)**: Bu çözüm ilkesi, varsayılan olarak, sistem tanımlı bir zaman damgası özelliği kullanır. Zaman senkronizasyonu saat protokolüne dayanıyor. SQL API'yi kullanıyorsanız, çakışma çözümü için kullanılacak diğer özel sayısal özelliği (örneğin, kendi zaman damgası kavramınız) belirtebilirsiniz. Özel bir sayısal özellik, *çakışma çözümyolu*olarak da adlandırılır. 

  İki veya daha fazla öğe ekleme veya değiştirme işlemleriyle çakışırsa, çakışma çözümlemesi yolu için en yüksek değere sahip öğe kazanan olur. Birden çok öğe çakışma çözümyolu için aynı sayısal değere sahipse, sistem kazananı belirler. Tüm bölgelerin tek bir kazanana yakınsaması ve taahhüt edilen öğenin aynı sürümüyle sonuçlandığı garanti edilir. Silme çakışmaları söz konusu olduğunda, silinen sürüm her zaman çakışma ekleme veya değiştirme üzerinde kazanır. Bu sonuç, çakışma çözümlemesi yolunun değeri ne olursa olsun oluşur.

  > [!NOTE]
  > Son Yazma Kazançları varsayılan çakışma çözümleme ilkesidir ve aşağıdaki API'ler için zaman damgası `_ts` kullanır: SQL, MongoDB, Cassandra, Gremlin ve Tablo. Özel sayısal özellik yalnızca SQL API için kullanılabilir.

  Daha fazla bilgi edinmek için [LWW çakışma çözümlemesi ilkelerini kullanan örneklere](how-to-manage-conflicts.md)bakın.

* **Özel**: Bu çözüm ilkesi, çakışmaların mutabakatı için uygulama tanımlı semantik için tasarlanmıştır. Bu ilkeyi Azure Cosmos kapsayıcınızda ayarladığınızda, *birleştirilmiş depolanmış yordamı*da kaydetmeniz gerekir. Bu yordam, sunucudaki bir veritabanı hareketi altında çakışma algılandığında otomatik olarak çağrılır. Sistem, taahhüt protokolünün bir parçası olarak birleştirme yordamının yürütülmesi için tam olarak bir kez garanti sağlar.  

  Kapsayıcınızı özel çözümseçeneğiyle yapılandırıyorsanız ve kapsayıcıya birleştirme yordamı kaydediyseniz veya birleştirme yordamı çalışma zamanında bir özel durum oluşturursa, *çakışmalar çakışma beslemesine*yazılır. Uygulamanızın daha sonra çakışma akışındaki çakışmaları el ile çözmesi gerekir. Daha fazla bilgi edinmek için, [özel çözümleme ilkesinin nasıl kullanılacağı ve çakışma akışının nasıl kullanılacağına](how-to-manage-conflicts.md)ilişkin örneklere bakın.

  > [!NOTE]
  > Özel çakışma çözümlemesi ilkesi yalnızca SQL API hesapları için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Çakışma çözümlemesi ilkelerini nasıl yapılandırılacaya caizdir:

* [Uygulamalarınızda çoklu master nasıl yapılandırılatır](how-to-multi-master.md)
* [Çakışma çözümlemesi ilkeleri nasıl yönetilir?](how-to-manage-conflicts.md)
* [Çakışma akışından okuma](how-to-manage-conflicts.md#read-from-conflict-feed)
