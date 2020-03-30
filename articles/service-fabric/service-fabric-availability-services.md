---
title: Hizmet Kumaş ı hizmetlerinin kullanılabilirliği
description: Azure Hizmet Kumaşı uygulamasında bir hizmetin hata algılama, hata algılama ve kurtarma durumu açıklanır.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 5306439184561e8dec8303a7b149f51d6c2f6e08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75551871"
---
# <a name="availability-of-service-fabric-services"></a>Hizmet Kumaş ı hizmetlerinin kullanılabilirliği
Bu makalede, Azure Hizmet Dokusu'nun bir hizmetin kullanılabilirliğini nasıl koruduğuna genel bir bakış sunulmaktadır.

## <a name="availability-of-service-fabric-stateless-services"></a>Hizmet Kumaş stateless hizmetlerin durumu
Service Fabric hizmetleri durumlu veya devletsiz olabilir. Devletsiz hizmet, kullanılabilir veya güvenilir olması gereken yerel bir [durumu](service-fabric-concepts-state.md) olmayan bir uygulama hizmetidir.

Devletsiz bir hizmet oluşturmak `InstanceCount`için bir . Örnek sayısı, kümede çalışan devletsiz hizmetin uygulama mantığının örnek sayısını tanımlar. Örnek sayısını artırmak, devletsiz bir hizmeti ölçeklemenin önerilen yoludur.

Bir durum dışı adlandırılmış hizmet örneği başarısız olduğunda, kümedeki uygun bir düğümde yeni bir örnek oluşturulur. Örneğin, devlet siz bir hizmet örneği Düğüm1'de başarısız olabilir ve Düğüm5'te yeniden oluşturulabilir.

## <a name="availability-of-service-fabric-stateful-services"></a>Hizmet Kumaş ı devlet hizmetlerinin mevcudiyeti
Bir devlet hizmetinin bununla ilişkili bir durumu vardır. Service Fabric'te, devlethizmeti bir yineleme kümesi olarak modellenir. Her yineleme hizmet kodu çalışan bir örneğidir. Yineleme de bu hizmet için devletin bir kopyası vardır. Okuma ve yazma işlemleri *Birincil*adı verilen tek bir yinelemede gerçekleştirilir. Yazma işlemlerinden durum değişiklikleri, *Etkin İkinciler*olarak adlandırılan çoğaltma kümesindeki diğer yinelemelere *çoğaltılır* ve uygulanır. 

Yalnızca bir Birincil yineleme olabilir, ancak birden çok Etkin İkincil yineleme olabilir. Etkin İkincil yinelemelerin sayısı yapılandırılabilir ve daha fazla sayıda yineleme daha fazla eşzamanlı yazılım ve donanım hatalarını tolere edebilir.

Birincil yineleme aşağı giderse, Hizmet Kumaşı Etkin İkincil yinelemelerden birini yeni Birincil yineleme yapar. Bu Etkin İkincil yineleme zaten *çoğaltma*yoluyla, devletin güncelleştirilmiş sürümü vardır ve daha fazla okuma/yazma işlemleri işleme devam edebilirsiniz. Bu işlem *yeniden yapılandırma* olarak bilinir ve [yeniden yapılandırma](service-fabric-concepts-reconfiguration.md) makalesinde daha fazla açıklanmıştır.

Birincil veya Etkin İkincil olmak çoğaltma kavramı, *çoğaltma rolü*olarak bilinir. Bu [yinelemeler, Yinelemeler ve örnekler](service-fabric-concepts-replica-lifecycle.md) makalesinde daha fazla açıklanmıştır. 

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Ölçekleme Servisi Kumaş hizmetleri](service-fabric-concepts-scalability.md)
- [Bölümleme Servisi Kumaş hizmetleri](service-fabric-concepts-partitioning.md)
- [Devleti tanımlama ve yönetme](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)

