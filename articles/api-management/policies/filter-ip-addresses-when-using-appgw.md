---
title: Örnek API yönetim ilkesi - Uygulama Ağ Geçidi kullanırken IP Adresine filtre uygulayın
titleSuffix: Azure API Management
description: Azure API yönetim ilkesi örneği - Uygulama Ağ Geçidi kullanırken istek IP adresine nasıl filtre uygulandığını gösterir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75942483"
---
# <a name="filter-on-request-ip-address-when-using-an-application-gateway"></a>Uygulama Ağ Geçidi kullanırken istek teki IP Adresine filtre uygulayın

Bu makalede, API Yönetimi örneğine bir Uygulama Ağ Geçidi veya başka bir aracı aracılığıyla erişildiğinde istek IP adresine nasıl filtre uygulandığını gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. İlke kodunu ayarlamak veya ayarlamak için, Ayarla'da açıklanan adımları izleyin [veya bir ilkeyi ayarlayın.](../set-edit-policies.md) Diğer örnekleri görmek için [ilke örneklerine](../policy-samples.md)bakın.

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştır.

[!code-xml[Main](../../../api-management-policy-samples/examples/Filter on IP Address when using Application Gateway.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM politikaları hakkında daha fazla bilgi edinin:

+ [Erişim kısıtlamaları ilkeleri](../api-management-access-restriction-policies.md)
+ [İlke örnekleri](../policy-samples.md)
