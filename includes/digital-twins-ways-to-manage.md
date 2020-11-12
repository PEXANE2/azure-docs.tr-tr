---
author: baanders
description: Azure dijital TWINS için dosya ekleme-örneği yönetme yolları
ms.service: digital-twins
ms.topic: include
ms.date: 11/11/2020
ms.author: baanders
ms.openlocfilehash: 47de00780fb259e7476c16dea9b7c63bc512a211
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94533914"
---
Bu makalede, [Azure Digital TWINS .net (C#) **SDK 'sını**](/dotnet/api/overview/azure/digitaltwins/management?view=azure-dotnet&preserve-view=true)kullanarak farklı yönetim işlemlerinin nasıl tamamlanacağı vurgulanmıştır. Ayrıca, [*nasıl yapılır: Azure Digital TWINS API 'leri ve SDK 'Ları kullanma*](../articles/digital-twins/how-to-use-apis-sdks.md)bölümünde açıklanan diğer dil SDK 'larını kullanarak aynı yönetim çağrılarını da oluşturabilirsiniz.

> [!TIP] 
> Tüm SDK yöntemlerinin zaman uyumlu ve zaman uyumsuz sürümlerde yer aldığını unutmayın. Disk belleği çağrılarında zaman uyumsuz yöntemler, `AsyncPageable<T>` zaman uyumlu sürümler geri dönirken döndürülür `Pageable<T>` .

Başka bir yönetim seçeneği, bu konu için doğrudan Azure Digital TWINS [**REST API 'lerini**](/rest/api/azure-digitaltwins/) çağırmak için kullanılır.

Son olarak, Azure Digital TWINS **CLI** kullanarak aynı yönetim işlemlerini tamamlayabilirsiniz. CLı kullanma hakkında daha fazla bilgi edinmek için bkz. [*nasıl yapılır: Azure dijital TWINS CLI 'Yi kullanma*](../articles/digital-twins/how-to-use-cli.md).