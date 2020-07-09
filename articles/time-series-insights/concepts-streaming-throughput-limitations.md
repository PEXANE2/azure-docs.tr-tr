---
title: Akış alma işleme sınırlamaları-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights giriş aktarım hızı sınırları hakkında bilgi edinin.
author: lyrana
ms.author: lyhughes
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: baa132589f5242ab480c9caa55ca793e0652ad6e
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135017"
---
# <a name="streaming-ingestion-throughput-limits"></a>Akış alma performansı sınırları

Azure Time Series Insights akış veri giriş sınırlamaları aşağıda açıklanmıştır.

> [!TIP]
> Önizleme [ortamınızı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-plan#review-preview-limits) , tüm önizleme limitlerinin kapsamlı bir listesi için planlayın.

## <a name="per-environment-limitations"></a>Ortam başına sınırlamalar

Genel olarak giriş fiyatları, kuruluşunuzda bulunan cihazların sayısı, olay egörev sıklığı ve her olayın boyutu olarak görüntülenir:

*  **Cihazların sayısı** × **olay emisi sıklığı** **her olayın boyutu**.

Time Series Insights, varsayılan olarak, **Time Series Insights ortamları başına 1 megabaylarca (Mbps)** bir hızda gelen verileri alabilir. [Hub bölümü başına](concepts-streaming-throughput-limitations.md#hub-partitions-and-per-partition-limits)ek sınırlamalar vardır.

> [!TIP]
>
> * 16 MBps 'e kadar hızlara kadar olan ortam desteği istek tarafından sağlanarak temin edilebilir.
> * Azure portal aracılığıyla bir destek bileti göndererek daha yüksek aktarım hızı gerekiyorsa bizimle iletişim kurun.
 
* **Örnek 1:**

    Contoso Shipping, dakikada üç kez bir olay sunan 100.000 cihaza sahiptir. Bir olayın boyutu 200 bayttır. Time Series Insights olay kaynağı olarak dört bölümden oluşan bir IoT Hub kullanıyor.

    * Time Series Insights ortamları için alım oranı: **100.000 cihaz * 200 bayt/olay * (3/60 olay/sn) = 1 MB**/sn olmalıdır.
    * Bölüm başına alım oranı 0,25 MBps olur.
    * Contoso sevkiyat alım oranı, ölçek kısıtlaması dahilinde olacaktır.

* **Örnek 2:**

    Contoso Fleet Analytics, her saniye bir olay sunan 60.000 cihaza sahiptir. Time Series Insights olay kaynağı olarak 4 bölüm sayısıyla bir olay hub 'ı kullanıyor. Bir olayın boyutu 200 bayttır.

    * Ortam alma hızı şu şekilde olacaktır: **60.000 cihaz * 200 bayt/olay * 1 olay/sn = 12 Mbps**.
    * Bölüm başına hız 3 MBps olur.
    * Contoso Fleet Analizi ' alım oranı, ortam ve bölüm sınırlarının üzerinde. Time Series Insights, ortamları için alma hızını artırmak üzere Azure portal aracılığıyla bir istek gönderebilirler ve önizleme sınırları dahilinde daha fazla bölüme sahip bir olay hub 'ı oluşturabilir.

## <a name="hub-partitions-and-per-partition-limits"></a>Merkez bölümleri ve bölüm sınırları başına

Time Series Insights ortamınızı planlarken, Time Series Insights bağlanacağınız olay kaynakları 'nın yapılandırılmasını göz önünde bulundurmanız önemlidir. Hem Azure IoT Hub hem de Event Hubs, olay işleme için yatay ölçeklendirmeyi etkinleştirmek üzere bölümleri kullanır. 

*Bölüm* , bir hub 'da tutulan olayların sıralı dizisidir. Bölüm sayısı, hub oluşturma aşamasında ayarlanır ve değiştirilemez.

Event Hubs bölümlendirme en iyi uygulamaları için [kaç bölümden Ihtiyacım olduğunu](https://docs.microsoft.com/azure/event-hubs/event-hubs-faq#how-many-partitions-do-i-need) gözden geçirin.

> [!NOTE]
> Azure Time Series Insights en çok kullanılan IoT Hub 'Larının yalnızca dört bölüm olması gerekir.

Time Series Insights ortamınız için yeni bir hub oluşturuyor veya var olanı kullanarak, sınırlar dahilinde olup olmadığını anlamak için bölüm başına alma hızınızı hesaplamanız gerekir. 

Azure Time Series Insights Preview şu anda **0,5 Mbps 'lik bölüm sınırına göre**genel bir.

### <a name="iot-hub-specific-considerations"></a>IoT Hub özgü konular

Bir cihaz IoT Hub oluşturulduğunda, kalıcı olarak bir bölüme atanır. Bunu yaparken, IoT Hub olay sıralamasını garanti edebilir (atama hiçbir şekilde değişmeyeceğinden).

Sabit bölüm ataması Ayrıca, IoT Hub aşağı akış 'dan gönderilen verilerin Time Series Insights örnekleri de etkiler. Birden çok cihazdan gelen iletiler hub 'a aynı ağ geçidi cihaz KIMLIĞI kullanılarak iletildiğinde, bu, bölüm başına ölçek limitlerini aşan büyük olasılıkla aynı anda aynı bölüme ulaşabilirler.

**Etki**:

* Tek bir bölüm önizleme sınırı üzerinden sürekli alma oranı yaşıyorsa, IoT Hub veri saklama süresi aşılmadan önce Time Series Insights tüm cihaz telemetrisini eşitlememesi mümkündür. Sonuç olarak, giriş sınırları sürekli olarak aşılırsa gönderilen veriler kaybolabilir.

Bu durumda, aşağıdaki en iyi yöntemleri öneririz:

* Çözümünüzü dağıtmaya başlamadan önce ortamınızı ve bölüm başına giriş oranlarını hesaplayın.
* IoT Hub cihazlarınızın mümkün olan en yüksek ölçüde yük dengelemesi yapıldığından emin olun.

> [!IMPORTANT]
> Bir olay kaynağı olarak IoT Hub kullanan ortamlarda, oran 'nin bölüm sınırlaması başına 0,5 MBps 'in altına düştüğünü sağlamak için kullanımdaki hub cihazı sayısını kullanarak alım oranını hesaplayın.
>
> * Aynı anda birkaç olay geldikçe bile, önizleme sınırı aşılmaz.

  ![IoT Hub bölüm diyagramı](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

Hub aktarım hızını ve bölümleri iyileştirme hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

* [IoT Hub ölçeği](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [Olay Hub 'ı ölçeği](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [Olay Hub 'ı bölümleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

## <a name="next-steps"></a>Sonraki adımlar

* Veri [depolama](concepts-storage.md) hakkında bilgi edinin