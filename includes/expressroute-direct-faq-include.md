---
title: include dosyası
description: include dosyası
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: include
ms.date: 07/09/2019
ms.author: jaredro
ms.custom: include file
ms.openlocfilehash: 43d1942b1413569d77c6c17fee9fff14dc83c924
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68362650"
---
### <a name="what-is-expressroute-direct"></a>ExpressRoute Direct nedir?

ExpressRoute Direct, müşterilere Microsoft’un dünya çapında stratejik noktalara yerleştirilmiş eşleme konumlarından oluşan küresel ağına doğrudan bağlanabilme olanağı sağlar. ExpressRoute Direct, ölçek üzerinde etkin/etkin bağlantıyı destekleyen iki 100 veya 10 Gbps bağlantı sağlar. 

### <a name="how-do-customers-connect-to-expressroute-direct"></a>Müşteriler ExpressRoute Direct 'e nasıl bağlanır? 

ExpressRoute Direct 'ten faydalanmak için müşterilerin ExpressRoute yönlendiricilerine bağlantı alması için yerel taşıyıcılar ve ortak konum sağlayıcılarıyla birlikte çalışması gerekecektir.

### <a name="what-locations-currently-support-expressroute-direct"></a>Şu anda ExpressRoute Direct 'i hangi konumlarda destekliyor? 

Kullanılabilir bağlantı noktaları dinamik olacak ve bu kapasiteyi görüntülemek için PowerShell tarafından kullanılabilecek. Konumlar ve *kullanılabilirlik temelinde değişebilir*:

* Amsterdam
* Amsterdam2
* Auckland 
* Chicago
* Dallas
* Dublin
* Hong Kong SAR
* Londra
* Melbourne
* New York City
* Perth
* San Antonio
* Seattle
* Seoul
* Silikon Vadisi
* Singapur2 
* Sidney
* Taipei
* Tokyo
* Toronto
* Washington
* Washington DC2

### <a name="what-is-the-sla-for-expressroute-direct"></a>ExpressRoute Direct için SLA nedir?

ExpressRoute Direct, [ExpressRoute 'un kurumsal sınıf](https://azure.microsoft.com/support/legal/sla/expressroute/v1_3/)aynısını kullanır. 

### <a name="what-scenarios-should-customers-consider-with-expressroute-direct"></a>Müşterilerin ExpressRoute Direct ile hangi senaryolar dikkate alınmalıdır?  

ExpressRoute Direct, müşterilere Microsoft Global omurgaya doğrudan 100 veya 10 Gbps bağlantı noktası çiftleri sağlar. Müşterilerimize en büyük avantajları sağlayacak senaryolar şunlardır: Büyük veri alımı, düzenlenen pazarlar için fiziksel yalıtım ve işleme gibi veri bloğu senaryosu için adanmış kapasite. 

### <a name="what-is-the-billing-model-for-expressroute-direct"></a>ExpressRoute Direct için faturalandırma modeli nedir? 

ExpressRoute Direct, bağlantı noktası çifti için sabit bir miktar olarak faturalandırılır. Standart devreler ek bir saat olmaksızın dahil edilir ve Premium, hafif bir eklenti ücretine sahip olur. Çıkış, eşleme konumunun bölgesine bağlı olarak her bir devre temelinde faturalandırılır.

### <a name="when-does-billing-start-for-the-expressroute-direct-port-pairs"></a>ExpressRoute doğrudan bağlantı noktası çiftleri için faturalandırma ne zaman başlar?

ExpressRoute Direct 'in bağlantı noktası çiftleri 45 gün boyunca ExpressRoute doğrudan kaynağının oluşturulmasına veya bağlantıların 1 veya her ikisinin de etkinleştirilmediğinde, hangisi önce geldiğini faturalandırılır. 45 günlük yetkisiz kullanım süresi, müşterilerin çapraz bağlantı sürecini birlikte bulundurma sağlayıcısıyla tamamçalıştırmasına izin vermek için verilir.
