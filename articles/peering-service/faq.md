---
title: Azure eşleme hizmeti hakkında SSS
description: Microsoft Azure eşleme hizmeti SSS hakkında bilgi edinin
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 55c5e6c5b718dc2de295b9b4418ddc8607a69f8f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "84872053"
---
# <a name="peering-service-faq"></a>Eşleme hizmeti hakkında SSS

Bu makalede, Azure eşleme hizmeti bağlantıları hakkında en sık sorulan sorular açıklanmaktadır.


**Ç. Hedef müşteriler kim?**

A. Hedef müşteriler, Microsoft bulut 'a taşıma olarak internet kullanarak bağlanan kuruluşlardır.

**Ç. Müşteriler birden çok sağlayıcı ile eşleme hizmetine kaydolabilir mi?** 

A. Evet, müşteriler aynı bölgede veya farklı bölgelerde birden fazla sağlayıcı ile eşleme hizmetine kaydolabilir, ancak aynı önek için kayıt yapabilir.

**Ç. Müşteriler, coğrafi bölge başına siteleri için benzersiz bir ISS seçebilir mi?**

A. Evet, müşteriler bunu yapabilir. İş ve işletimsel gereksinimlerinize uyan bölge başına iş ortağı ISS 'sini seçin.

**Ç. Microsoft Edge PoP nedir?**

A. Bu, Microsoft 'un diğer ağlarla bağlanacağı fiziksel bir konumdur. Microsoft Edge PoP konumunda, Azure ön kapısı ve Azure CDN gibi hizmetler barındırılır. Daha fazla bilgi için bkz. [Azure CDN](https://docs.microsoft.com/azure/cdn/cdn-features).

## <a name="peering-service-unique-characteristics"></a>Eşleme hizmeti: benzersiz özellikler

**Ç. Eşleme hizmeti normal internet erişiminizden nasıl farklıdır?**

A. Microsoft eşleme hizmeti 'ne kayıtlı olan iş ortakları, Microsoft hizmetlerine iyileştirilmiş yönlendirme ve güvenilir bağlantı sunmak üzere Microsoft ile çalışmaktadır.  

**Ç. Eşleme hizmeti ExpressRoute 'dan farklı midir?**

A. Azure ExpressRoute, bir veya daha fazla müşteri konumundan özel, adanmış bir bağlantıdır. Eşleme hizmeti iyileştirilmiş ortak bağlantı sağlarken ve herhangi bir özel bağlantıyı desteklemediğinden, yerel internet ayırıcıları için iyileştirilmiş bağlantı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Eşleme hizmeti hakkında bilgi edinmek için bkz. [eşleme hizmetine genel bakış](about.md).
- Bir hizmet sağlayıcısı bulmak için bkz. [eşleme hizmeti ortakları ve konumları](location-partners.md).
- Bir eşleme hizmeti bağlantısı eklemek için bkz. [eşleme hizmeti ekleme](onboarding-model.md).
- Bir eşleme hizmeti bağlantısını kaydetmek için bkz. [eşleme hizmeti bağlantısını kaydetme-Azure Portal](azure-portal.md).
- Telemetriyi ölçmek için bkz. [ölçüm bağlantı telemetrisi](measure-connection-telemetry.md).