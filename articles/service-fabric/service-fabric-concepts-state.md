---
title: Azure Service Fabric hizmetlerinde durumu yönetme
description: Service Fabric hizmetlerinde hizmet durumunun nasıl tanımlanabileceğiniz ve yönetilenler de dahil olmak üzere Azure Hizmet Kumaşı'ndaki durum hakkında bilgi edinin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9acd3031d1071d1822791b333976aaf76161600f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614579"
---
# <a name="service-state"></a>Hizmet durumu
**Hizmet durumu,** bir hizmetin çalışması için gereken bellek içi veya disk verilerine başvurur. Örneğin, hizmetin okuduğu ve iş yapmak için yazdığı veri yapılarını ve üye değişkenleri içerir. Hizmetin nasıl tasarlanabileceğine bağlı olarak, diskte depolanan dosyaları veya diğer kaynakları da içerebilir. Örneğin, veritabanının veri ve işlem günlüklerini depolamak için kullanacağı dosyalar.

Örnek bir hizmet olarak, bir hesap makinesi düşünelim. Temel bir hesap makinesi hizmeti iki sayı alır ve toplamlarını döndürür. Bu hesaplamayı gerçekleştirmek üye değişken veya başka bilgiler gerektirmez.

Şimdi aynı hesap makinesi düşünün, ancak depolama küçülme ve hesapladığı son toplamı iade etmek için ek bir yöntem ile. Bu hizmet artık devlet. Stateful, yeni bir toplamı hesapladığında yazdığı ve son hesaplanan toplamı döndüretmesini istediğinizde okuduğu bazı durumları içerdiği anlamına gelir.

Azure Hizmet Kumaşı'nda ilk hizmet, devletsiz hizmet olarak adlandırılır. İkinci hizmete devlet hizmeti denir.

## <a name="storing-service-state"></a>Hizmet durumunu depolama
Durum dışsallaştırılmış veya durumu manipüle eden kodla birlikte bulunabilir. Durum dışsallaştırma genellikle ağ üzerinden farklı makinelerde çalışan bir dış veritabanı veya diğer veri deposu kullanılarak veya aynı makinede işlem dışı yapılır. Hesap makinesi örneğimizde, veri deposu bir SQL veritabanı veya Azure Tablo Deposu örneği olabilir. Toplamı hesaplamak için her istek bu veriler üzerinde bir güncelleştirme gerçekleştirir ve mağazadan getirilen geçerli değer sonucu döndürmek için hizmet istekleri. 

Durum, durumu manipüle eden kodla da birlikte bulunabilir. Service Fabric'teki duruma hizmet genellikle bu model kullanılarak oluşturulur. Service Fabric, bu devletin yüksek kullanılabilir, tutarlı ve dayanıklı olmasını ve bu şekilde oluşturulmuş hizmetlerin kolayca ölçeklendirilebilmesini sağlamak için altyapı sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hizmet Kumaş ı hizmetlerinin kullanılabilirliği](service-fabric-availability-services.md)
* [Hizmet Kumaş ı hizmetlerinin ölçeklenebilirliği](service-fabric-concepts-scalability.md)
* [Bölümleme Servisi Kumaş hizmetleri](service-fabric-concepts-partitioning.md)
* [Servis Kumaşı Güvenilir Servisler](service-fabric-reliable-services-introduction.md)
