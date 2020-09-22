---
title: Sorgu gereksinimleri
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 23da214f4ffc9e589523f7ada00a77494183eb15
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941930"
---
Sorgu içinde, belirli bir `@StartTime` zaman damgasına yönelik ölçüm verilerini almak için parametresini kullanın. Bu, bir `yyyy-MM-ddTHH:mm:ss` Biçim dizesiyle değiştirilmelidir. 

> [!IMPORTANT]
> Sorgu tarafından yalnızca **tek bir zaman damgasından** alınan ölçüm verilerinin döndürüldüğünden emin olun. Ölçüm Danışmanı, karşılık gelen ölçüm verilerini almak için sorguyu her zaman damgasıyla çalıştırır. Örneğin, *günlük* ayrıntı düzeyi olan bir ölçüm sorgusu yalnızca bir kez sorgu çalıştırırken olduğu gibi tek bir zaman damgası içermelidir `2020-06-21T00:00:00Z` . 
