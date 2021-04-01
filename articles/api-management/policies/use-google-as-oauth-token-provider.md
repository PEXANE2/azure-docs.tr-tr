---
title: Örnek API yönetimi ilkesi-Google OAuth belirtecini kullanarak erişimi yetkilendir
titleSuffix: Azure API Management
description: Azure API Management ilkesi örneği-bir OAuth belirteç sağlayıcısı olarak Google kullanarak uç noktalarınıza erişim yetkisi verme işlemlerini gösterir.
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
ms.openlocfilehash: 0f6c9fe2146414f78e90d6ade1a00045cdf3a04f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92078026"
---
# <a name="authorize-access-using-google-oauth-token"></a>Google OAuth belirtecini kullanarak erişim yetkisi verme

Bu makalede, bir OAuth belirteç sağlayıcısı olarak Google kullanarak uç noktalarınıza erişim yetkisi verme işlemlerini gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-reference.md).

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Simple Google OAuth validate-jwt.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-reference.md)