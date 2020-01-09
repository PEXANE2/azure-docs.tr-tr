---
title: Azure Stream Analytics işlerinde hizmet kesintilerini önleme
description: Bu makalede, yükseltme, Stream Analytics işleri dayanıklı hale yönergeler açıklanmaktadır.
author: jseb225
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.custom: seodec18
ms.openlocfilehash: d2d21e081b274bd985c48dac91fff5203a6b5b8a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75425982"
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Stream Analytics işi güvenilirlik garanti sırasında hizmet güncelleştirmeleri

Tam olarak yönetilen bir hizmet olan yeni hizmet işlevlerini ve geliştirmeleri hızlı bir hızda özelliği parçasıdır. Sonuç olarak, Stream Analytics, bir hizmet güncelleştirmesinden tek haftalık (veya daha sık) temelinde dağıtma olabilir. Ne kadarlık testi bitti ne olursa olsun yine de, var olan, çalışan bir iş sunulmasıyla bir hata nedeniyle kesilebilir sokması mümkündür. Görev açısından kritik işler çalıştırıyorsanız, bu risklerin kaçınılması gerekir. Azure 'un **[eşleştirilmiş bölge](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** modelini izleyerek bu riski azaltabilirsiniz. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Azure eşleştirilmiş bölgeleri başlıklarla bu nasıl karşılayabileceği?

Stream Analytics, eşleştirilmiş bölgelerin işlerinde ayrı toplu işler üzerinde güncelleştirilir garanti eder. Sonuç olarak, olası sorunları belirlemek ve düzeltmek için güncelleştirmeler arasında yeterli zaman aralığı vardır.

_Orta Hindistan dışında_ (eşleştirilmiş olan bölge, Güney Hindistan, Stream Analytics durum yok), Stream Analytics için bir güncelleştirme dağıtımı, eşleştirilmiş bölgelerin kümesinde aynı anda oluşmaz. Birden çok bölgede dağıtımları **aynı gruptaki** oluşabilir **aynı anda**.

Makale **[kullanılabilirlik ve eşleştirilmiş bölgelerin](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** bölgeleri halindedir en güncel bilgilere sahip.

Aynı işlerin her ikisi de eşleştirilmiş bölgelere dağıtılması önerilir. Sonra [Bu işleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor) , beklenmeyen bir şeyler gerçekleştiğinde bildirim almak için izlemeniz gerekir. Stream Analytics bir hizmet güncelleştirmesinden sonra bu işlerden biri [başarısız durumunda](https://docs.microsoft.com/azure/stream-analytics/job-states) sonlanıyorsa, kök nedeni belirlemenize yardımcı olması için müşteri desteğiyle iletişim kurun. Ayrıca, herhangi bir aşağı akış tüketicilerinin yükünü sağlıklı iş çıktısına devreder.

## <a name="next-steps"></a>Sonraki adımlar

* [Stream analytics'e giriş](stream-analytics-introduction.md)
* [Stream Analytics ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Stream Analytics sorgu dili başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
