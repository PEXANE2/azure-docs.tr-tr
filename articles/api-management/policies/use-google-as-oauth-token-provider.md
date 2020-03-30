---
title: Örnek API yönetim ilkesi - Google OAuth belirteci kullanarak erişimi yetkilendirme
titleSuffix: Azure API Management
description: Azure API yönetim ilkesi örneği - Google'ı OAuth belirteç sağlayıcısı olarak kullanarak uç noktalarınıza erişimin nasıl onayverilebildiğini gösterir.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: d606d29d84cd5917c74efe188ae02627ad55d4ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75442383"
---
# <a name="authorize-access-using-google-oauth-token"></a>Google OAuth belirtecini kullanarak erişim yetkisi verme

Bu makalede, Google'ı OAuth belirteç sağlayıcısı olarak kullanarak uç noktalarınıza erişimyetkisinin nasıl verilebildiğini gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. İlke kodunu ayarlamak veya ayarlamak için, Ayarla'da açıklanan adımları izleyin [veya bir ilkeyi ayarlayın.](../set-edit-policies.md) Diğer örnekleri görmek için [ilke örneklerine](../policy-samples.md)bakın.

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştır.

[!code-xml[Main](../../../api-management-policy-samples/examples/Simple Google OAuth validate-jwt.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM politikaları hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

