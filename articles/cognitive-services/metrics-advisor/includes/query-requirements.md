---
title: Sorgu gereksinimleri
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 09/30/2020
ms.author: aahi
ms.openlocfilehash: 518865f78c170f1fbe4e65b96dc149c1b449a88b
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91631400"
---
Sorgu içinde, `@StartTime` tek bir zaman damgasına yönelik ölçüm verilerini almak için parametresini kullanın. Ölçüm Danışmanı, `yyyy-MM-ddTHH:mm:ss` sorguyu çalıştırırken parametre bir biçim dizesiyle değiştirilir.

> [!IMPORTANT]
> Sorgu her bir zaman damgasında her bir boyut birleşimi için en çok bir kayıt döndürmelidir. Ve sorgu tarafından döndürülen tüm kayıtlar aynı zaman damgalarına sahip olmalıdır. Ölçüm Danışmanı, verilerinizi almak için bu sorguyu her zaman damgası için çalıştırır. Daha fazla bilgi ve örnek için [sorgular hakkında SSS bölümüne](../faq.md#how-do-i-write-a-valid-query-for-ingesting-my-data) bakın. 