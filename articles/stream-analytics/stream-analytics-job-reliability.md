---
title: Azure Akış Analizi işlerinde hizmet kesintilerinden kaçının
description: Bu makalede, Akış Analizi işlerinizi esnek hale getirme kılavuzu açıklanmaktadır.
author: jseb225
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.custom: seodec18
ms.openlocfilehash: d2d21e081b274bd985c48dac91fff5203a6b5b8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75425982"
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Hizmet güncellemeleri sırasında Stream Analytics iş güvenilirliğini garanti edin

Tam olarak yönetilen bir hizmet olmanın bir parçası da yeni hizmet işlevselliğini ve iyileştirmelerini hızlı bir şekilde sunabilme yeteneğidir. Sonuç olarak, Stream Analytics bir hizmet güncelleştirmesi haftalık (veya daha sık) olarak dağıtılabilir. Ne kadar sınama yapılırsa yapılsın, bir hatanın sunulması nedeniyle varolan, çalışan bir işin kopma riski vardır. Görev açısından kritik işler de yürütüyorsanız, bu risklerden kaçınılması gerekir. Azure'un **[eşleştirilmiş bölge](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** modelini izleyerek bu riski azaltabilirsiniz. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Azure eşleştirilmiş bölgeler bu sorunu nasıl giderir?

Akış Analizi, eşleştirilmiş bölgelerdeki işlerin ayrı gruplar halinde güncelleştirilmelerini garanti eder. Sonuç olarak, olası sorunları belirlemek ve bunları düzeltmek için güncelleştirmeler arasında yeterli bir zaman farkı vardır.

Orta Hindistan (eşleştirilmiş bölgesi Güney Hindistan, Stream Analytics varlığı olmayan) _dışında,_ Stream Analytics'e bir güncelleştirmenin dağıtımı eşleştirilmiş bölgeler kümesinde aynı anda gerçekleşmez. **Aynı gruptaki** birden çok bölgede dağıtımlar **aynı anda**oluşabilir.

**[Kullanılabilirlik ve eşleştirilmiş bölgelerle](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** ilgili makale, hangi bölgelerin eşlendiğine ilişkin en güncel bilgilere sahiptir.

Aynı işleri her iki eşlenen bölgeye dağıtmak önerilir. Daha sonra beklenmedik bir şey olduğunda haberdar olmak için [bu işleri izlemek](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor) gerekir. Bu işlerden biri, Akış Analizi hizmeti güncelleştirmesi sonrasında [Başarısız](https://docs.microsoft.com/azure/stream-analytics/job-states) durumda kalırsa, temel nedeni belirlemeye yardımcı olmak için müşteri desteğine başvurabilirsiniz. Ayrıca sağlıklı iş çıktısı için herhangi bir downstream tüketiciler üzerinde başarısız olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Akış Analizine Giriş](stream-analytics-introduction.md)
* [Akış Analizi ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Akış Analizi sorgu dili başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Stream Analytics yönetimi REST API referansı](https://msdn.microsoft.com/library/azure/dn835031.aspx)
