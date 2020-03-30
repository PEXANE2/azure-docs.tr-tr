---
title: Örnek API yönetim ilkesi - Paylaşılan Erişim İmzası Oluşturma
titleSuffix: Azure API Management
description: Azure API yönetim ilkesi örneği - İfadeleri kullanarak Paylaşılan Erişim İmzası'nın nasıl üretilmeye ve isteği yeniden yazma-uri ilkesiyle Azure depolama alanına nasıl iletiniz gösterir.
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
ms.openlocfilehash: 0f003bc268af6b7f8bd6b046ae84734dbefeac28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75442455"
---
# <a name="generate-shared-access-signature"></a>Paylaşılan Erişim İmzası Oluşturma

Bu makalede, ifadeler kullanılarak [Paylaşılan Erişim İmzası](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) nasıl üretilip isteği yeniden yazma ilkesiyle Azure depolama alanına ileteceklerini gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. İlke kodunu ayarlamak veya ayarlamak için, Ayarla'da açıklanan adımları izleyin [veya bir ilkeyi ayarlayın.](../set-edit-policies.md) Diğer örnekleri görmek için [ilke örneklerine](../policy-samples.md)bakın.

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştır.

[!code-xml[Main](../../../api-management-policy-samples/examples/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM politikaları hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

