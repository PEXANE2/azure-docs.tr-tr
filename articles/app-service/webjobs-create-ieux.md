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
ms.openlocfilehash: 4737f0f19acf199190df02386ecb2ece65fa571e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101746683"
---
# <a name="run-background-tasks-with-webjobs-in-azure-app-service"></a>Azure App Service Web Işleri ile arka plan görevleri çalıştırma

Azure 'da runn [arka plan görevlerinin](./webjobs-create-ieux-conceptual.md) kavramı, Azure App Service Web işleri ile sunulmaktadır. Dağıtma hakkında bilgi edinin <abbr title="Web uygulaması, API uygulaması veya mobil uygulama ile aynı örnekteki bir program veya komut dosyası.">Web İşleri</abbr> [Azure Portal](https://portal.azure.com) kullanarak bir yürütülebilir dosyayı veya betiği karşıya yükleyin. 

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