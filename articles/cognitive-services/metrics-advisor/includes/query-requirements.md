---
title: Sorgu gereksinimleri
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 68b5946cb21bef1c8979a94c8780682cfe9defa6
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376739"
---
Sorgu içinde, belirli bir `@StartTime` zaman damgasına yönelik ölçüm verilerini almak için parametresini kullanın. Bu, bir `yyyy-MM-ddTHH:mm:ss` Biçim dizesiyle değiştirilmelidir. 

> [!IMPORTANT]
> Sorgu tarafından yalnızca **tek bir zaman damgasından** alınan ölçüm verilerinin döndürüldüğünden emin olun. Ölçüm Danışmanı, karşılık gelen ölçüm verilerini almak için sorguyu her zaman damgasıyla çalıştırır. Örneğin, *günlük* ayrıntı düzeyi olan bir ölçüm sorgusu yalnızca bir kez sorgu çalıştırırken olduğu gibi tek bir zaman damgası içermelidir `2020-06-21T00:00:00Z` . 
