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
ms.openlocfilehash: 8249cc543c6334841c8e5152d5d1ceb84d4097dc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92076122"
---
# <a name="filter-on-request-ip-address-when-using-an-application-gateway"></a>Application Gateway kullanırken istek IP adresini filtrele

Bu makalede, API Management örneğine bir Application Gateway ya da başka bir aracı aracılığıyla erişildiğinde istek IP adresi üzerinde nasıl filtre yapıldığını gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-reference.md).

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Filter on IP Address when using Application Gateway.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Erişim kısıtlama ilkeleri](../api-management-access-restriction-policies.md)
+ [İlke örnekleri](../policy-reference.md)