---
title: Veri şeması gereksinimleri
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 09/10/2020
ms.author: mbullwin
ms.openlocfilehash: c754efef02cdad6edbf047c5de9f1af6d758f137
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92043229"
---
Ölçüm Izleyicisi, zaman serisi anomali algılama, tanılama ve analize yönelik bir hizmettir. AI destekli bir hizmet olarak, kullanılan modeli eğitebilmeniz için verilerinizi kullanır. Hizmet, toplanan verilerin tablolarını aşağıdaki sütunlara sahip olarak kabul eder:

* **Ölçü** (zorunlu): sayısal değerler içeren bir veya daha fazla sütun.
* **Zaman damgası** (isteğe bağlı): sıfır veya türü olan bir `DateTime` sütun `String` . Bu sütun ayarlanmamışsa, zaman damgası her alma döneminin başlangıç saati olarak ayarlanır. Zaman damgasını şu şekilde biçimlendirin: `yyyy-MM-ddTHH:mm:ssZ` . 
  * **Zaman Damgalarınız, ölçümün ayrıntı düzeyi ile eşleşmelidir. Örneğin, günlük ölçüm, zaman damgasında olarak `00:00:00` etiketlenen saat, dakika ve saniyeyi sağlamalıdır **.
* **Boyut** (isteğe bağlı): sütunlar herhangi bir veri türünde olabilir. Fazla sayıda boyutun işlenmesini engellemek için büyük miktarda sütun ve değerle çalışırken dikkatli olun.

> [!Note]
> Her ölçüm için, tek bir boyut birleşimine karşılık gelen, ölçü başına yalnızca bir zaman damgası olmalıdır. Verilerinizi ekleme öncesinde toplayın veya alınacak verileri belirtmek için sorguyu kullanın.