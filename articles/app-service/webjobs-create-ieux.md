---
title: Web Işleri ile arka plan görevleri çalıştırma
description: Azure App Service arka plan görevlerini çalıştırmak için WebJobs kullanmayı öğrenin. Çeşitli betik biçimlerinden birini seçin ve bunları CRON ifadeleriyle çalıştırın.
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: glenga
ms.reviewer: msangapu;suwatch;pbatum;naren.soni
ms.custom: seodec18
zone_pivot_groups: app-service-webjob
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 0d49323e2bc3c0522b1fb9ad49ffcc14f476e2dc
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102452807"
---
# <a name="run-background-tasks-with-webjobs-in-azure-app-service"></a>Azure App Service Web Işleri ile arka plan görevleri çalıştırma

Azure 'da [arka plan görevleri](./webjobs-create-ieux-conceptual.md) çalıştırma kavramı, Azure App Service Web işleri ile sunulmaktadır. Dağıtma hakkında bilgi edinin <abbr title="Web uygulaması, API uygulaması veya mobil uygulama ile aynı örnekteki bir program veya komut dosyası.">Web İşleri</abbr> [Azure Portal](https://portal.azure.com) kullanarak bir yürütülebilir dosyayı veya betiği karşıya yükleyin. 

Desteklenen üç Web Işi şunları içerir:

* **Sürekli**: hemen başlar ve tipik olarak sonsuz bir döngüde çalışır.
* **Zamanlandı**: zamanlanan tetikleyiciden başlar
* **El ile**: el ile tetikleyiciden başlar

::: zone pivot="webjob-type-continuous"

[!INCLUDE [Continuous](./includes/webjobs-create-ieux-continuous.md)]

::: zone-end

::: zone pivot="webjob-type-scheduled"

[!INCLUDE [Scheduled](./includes/webjobs-create-ieux-scheduled.md)]

::: zone-end

::: zone pivot="webjob-type-manual"

[!INCLUDE [Manual](./includes/webjobs-create-ieux-manual.md)]

::: zone-end
   
## <a name="next-steps"></a><a name="NextSteps"></a> Sonraki adımlar

* [Web işleri olarak arka plan görevleri hakkında daha fazla bilgi edinin](./webjobs-create-ieux-conceptual.md)
* [Web Işlerinin günlük geçmişini görüntüle](./webjobs-create-ieux-view-log.md)

* Birçok programlama görevini basitleştirmek için [WebJobs SDK 'sını](https://github.com/Azure/azure-webjobs-sdk/wiki) kullanın

* [Visual Studio Ile Web işleri geliştirmeyi ve dağıtmayı](webjobs-dotnet-deploy-vs.md) öğrenin
