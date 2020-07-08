---
title: Örnek API yönetimi ilkesi-paylaşılan erişim Imzası oluştur
titleSuffix: Azure API Management
description: Azure API Management ilkesi örneği-ifadeleri kullanarak paylaşılan erişim Imzasının nasıl oluşturulacağını ve isteği yeniden yazma-URI ilkesiyle Azure depolama 'ya iletmeyi gösterir.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75442455"
---
# <a name="generate-shared-access-signature"></a>Paylaşılan erişim Imzası oluştur

Bu makalede, ifadeleri kullanarak [paylaşılan erişim imzasının](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) nasıl oluşturulacağı ve isteğin yeniden yazma-URI Ilkesi ile Azure depolama 'ya nasıl iletebileceği gösterilir. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

