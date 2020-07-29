---
title: Örnek Azure API Yönetimi İlkesi-ağ geçidi ve arka uç arasında yetkilendirme için OAuth2 kullanın
titleSuffix: Azure API Management
description: Azure API Management ilkesi örneği-ağ geçidi ile arka uç arasında yetkilendirme için OAuth2 kullanmayı gösterir. AAD'den erişim belirtecini alma ve bunu arka uca iletme işlemini de gösterir.
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
ms.openlocfilehash: 09d51759c07e7dacc25d5b5ffce9698831c37a7d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75442362"
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>Ağ Geçidi ile arka uç arasında yetkilendirme için OAuth2 kullanma

Bu makalede, ağ geçidi ile arka uç arasında yetkilendirme için OAuth2 kullanmayı gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. AAD'den erişim belirtecini alma ve bunu arka uca iletme işlemini de gösterir. 

Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-samples.md).

Aşağıdaki betik {{Property}} içinde görünen özellikleri kullanır. Özellikler hakkında bilgi edinmek ve bunları API Management ilkelerinde kullanmayı öğrenmek için [Bu](../api-management-howto-properties.md) konuya bakın.
 
## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get OAuth2 access token from AAD and forward it to the backend.policy.xml)]
  
## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

