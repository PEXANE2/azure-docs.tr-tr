---
title: Örnek API yönetimi ilkesi-Application Gateway kullanırken IP adresine Filtre Uygula
titleSuffix: Azure API Management
description: Azure API Management ilkesi örneği-bir Application Gateway kullanırken istek IP adresine nasıl filtre ekleneceğini gösterir.
services: api-management
documentationcenter: ''
author: jftl6y
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/13/2020
ms.author: joscot
ms.custom: fasttrack-new
ms.openlocfilehash: 45e16c9aa9e4b04e7225320951e9f839fae75ba3
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75942483"
---
# <a name="filter-on-request-ip-address-when-using-an-application-gateway"></a>Application Gateway kullanırken istek IP adresini filtrele

Bu makalede, API Management örneğine bir Application Gateway ya da başka bir aracı aracılığıyla erişildiğinde istek IP adresi üzerinde nasıl filtre yapıldığını gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Filter on IP Address when using Application Gateway.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Erişim kısıtlama ilkeleri](../api-management-access-restriction-policies.md)
+ [İlke örnekleri](../policy-samples.md)
