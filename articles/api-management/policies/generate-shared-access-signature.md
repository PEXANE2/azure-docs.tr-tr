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
ms.openlocfilehash: 62a6e8c9fdec3b30bd193e9887d7e0cb7926e73e
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86243010"
---
# <a name="generate-shared-access-signature"></a>Paylaşılan erişim Imzası oluştur

Bu makalede, ifadeleri kullanarak [paylaşılan erişim imzasının](../../storage/common/storage-sas-overview.md) nasıl oluşturulacağı ve isteğin yeniden yazma-URI Ilkesi ile Azure depolama 'ya nasıl iletebileceği gösterilir. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)
