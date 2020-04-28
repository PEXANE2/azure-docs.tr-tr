---
title: Azure Service Fabric hizmetlerinde durumu yönetme
description: Service Fabric hizmetlerinde hizmet durumunu tanımlama ve yönetme dahil olmak üzere Azure Service Fabric durum hakkında bilgi edinin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9acd3031d1071d1822791b333976aaf76161600f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75614579"
---
# <a name="service-state"></a>Hizmet durumu
**Hizmet durumu** , bir hizmetin çalışması için gerekli olan bellek içi veya disk verileri anlamına gelir. Bu, örneğin, hizmetin iş yaptığı ve yazdığı veri yapılarını ve üye değişkenlerini içerir. Hizmetin nasıl tanımlandığına bağlı olarak, ayrıca diskte depolanan dosyaları veya diğer kaynakları da içerebilir. Örneğin, bir veritabanının veri ve işlem günlüklerini depolamak için kullanacağı dosyalar.

Örnek bir hizmet olarak, bir Hesaplayıcı ele alalım. Temel bir Hesaplayıcı hizmeti iki sayı alır ve toplamlarını döndürür. Bu hesaplamanın gerçekleştirilmesi, hiçbir üye değişkeni veya başka bilgiler içermez.

Şimdi aynı hesaplayıcıyı göz önünde bulundurun, ancak hesaplanan son toplamı depolamak ve döndürmek için ek bir yöntem ile. Bu hizmet artık durum bilgisi. Durum bilgisi, yeni bir toplam hesaplandıktan sonra yazdığı ve son hesaplanan toplamı döndürmesini istediğinizde okuduğu bir durum içerdiği anlamına gelir.

Azure Service Fabric 'de ilk hizmet durum bilgisi olmayan bir hizmet olarak adlandırılır. İkinci hizmet durum bilgisi olmayan bir hizmet olarak adlandırılır.

## <a name="storing-service-state"></a>Hizmet durumunu depolama
Durum externalized veya durumu işleyen kodla birlikte bulunabilir. Dışsallaştırılması of State, genellikle ağ üzerinden veya aynı makinede bulunan farklı makinelerde çalışan bir dış veritabanı veya başka bir veri deposu kullanılarak yapılır. Hesaplayıcı örneğimizde, veri deposu bir SQL veritabanı veya Azure Tablo deposu örneği olabilir. Toplam hesaplama isteği bu verilerde bir güncelleştirme gerçekleştirir ve depolama alanından getirilen geçerli değerde değer sonucunu döndürmek için hizmete istekler. 

Durum, durumu işleyen kodla birlikte da bulunabilir. Service Fabric ' de durum bilgisi olan hizmetler genellikle bu model kullanılarak oluşturulmuştur. Service Fabric, bu durumun yüksek oranda kullanılabilir, tutarlı ve dayanıklı olduğundan ve bu şekilde oluşturulan hizmetlerin kolayca ölçeklenebilmesini sağlamak için altyapıyı sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Fabric hizmetlerinin kullanılabilirliği](service-fabric-availability-services.md)
* [Service Fabric Hizmetleri ölçeklenebilirliği](service-fabric-concepts-scalability.md)
* [Service Fabric Hizmetleri bölümlendirme](service-fabric-concepts-partitioning.md)
* [Service Fabric Reliable Services](service-fabric-reliable-services-introduction.md)
