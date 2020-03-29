---
title: Azure Akış Analizi iş durumları
description: Bu makalede, bir Stream Analytics işinin dört farklı durumu açıklanmaktadır; çalışan, durdurulan, bozulan ve başarısız olan.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 454474333cac94dc25deae8196e9ba45bcb3a574
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75359775"
---
# <a name="azure-stream-analytics-job-states"></a>Azure Akış Analizi iş durumları

Bir Akış Analizi işi herhangi bir zamanda dört eyaletten birinde olabilir: çalıştırma, durdurma, alçaltılmış veya başarısız. İşinizin durumunu Azure portalında Akış Analizi işinin Genel Bakış sayfasında bulabilirsiniz. 

| Durum | Açıklama | Önerilen eylemler |
| --- | --- | --- |
| **Çalışıyor** | İşiniz, tanımlanan giriş kaynaklarından gelen Azure okuma etkinlikleriüzerinde çalışmak, bunları işlemek ve sonuçları yapılandırılmış çıktı lavabolarına yazmaktır. | Önemli [ölçümleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor)izleyerek işinizin performansını izlemek en iyi yöntemdir. |
| **Durduruldu** | İşiniz durdurulur ve olayları işlemez. | NA | 
| **Düzeyi düşürüldü** | Giriş ve çıktı bağlantılarınızla ilgili aralıklı sorunlar olabilir. Bu hatalar, işinizin bozulmuş bir duruma girmesine neden olabilecek geçici hatalar olarak adlandırılır. Akış Analizi, bu tür hatalardan hemen kurtulmayı ve Çalışan durumuna (birkaç dakika içinde) dönmeye çalışır. Bu hatalar ağ sorunları, diğer Azure kaynaklarının kullanılabilirliği, deserialization hataları vb nedeniyle olabilir. İşinizin performansı bozulmuş durumda olduğunda etkilenebilir.| Bu geçici hataların nedeni hakkında daha fazla bilgi edinmek için [tanılama veya etkinlik günlüklerine](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) bakabilirsiniz. Deserialization hataları gibi durumlarda, olayların yanlış biçimlendirilmiş olmadığından emin olmak için düzeltici eylemde bulunmaları önerilir. İş kaynak kullanım sınırına ulaşmaya devam ederse, SU numarasını artırmayı veya [işinizi paralelleştirmeyi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization)deneyin. Herhangi bir eylemde bulunamadığınız diğer durumlarda, Akış Analizi *Çalışan* durumuna geri kazanmaya çalışır. <br> [Filigran gecikme](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor) ölçümü kullanarak bu geçici hataların işinizin performansını etkiedip etkilemediğinizi anlayabilirsiniz.|
| **Başarısız** | İşiniz, başarısız bir durumla sonuçlanan kritik bir hatayla karşılaştı. Olaylar okunmaz ve işlenmez. Çalışma zamanı hataları, başarısız durumda biten işler için sık karşılaşılan bir nedendir. | İş Başarısız durumuna geçtiğinde bilgilendirilmeniz için [uyarıları yapılandırabilirsiniz.](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#set-up-alerts-in-the-azure-portal) <br> <br>Kök nedenini belirlemek ve sorunu gidermek için [etkinlik ve tanılama günlüklerini](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) kullanarak hata ayıklama yapabilirsiniz.|

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizi işleri için kurulum uyarıları](stream-analytics-set-up-alerts.md)
* [Akış Analizi'nde bulunan ölçümler](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics)
* [Etkinlik ve tanılama günlüklerini kullanarak sorun giderme](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)
