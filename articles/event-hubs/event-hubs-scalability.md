---
title: Ölçeklenebilirlik-Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs bölümler ve üretilen iş birimleri kullanarak ölçeklendirmeye yönelik bilgiler sağlanmaktadır.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 06/18/2019
ms.author: shvija
ms.openlocfilehash: 2b36faef8c39a8e9b02a056576ae7f5a77b1f6bf
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76309532"
---
# <a name="scaling-with-event-hubs"></a>Event Hubs ile ölçeklendirme

Event Hubs ölçeklendirmeyi etkileyen iki etken vardır.
*   Aktarım hızı birimleri
*   Bölümler

## <a name="throughput-units"></a>Aktarım hızı birimleri

Event Hubs işleme kapasitesi, *işleme birimleri* tarafından denetlenir. İşleme birimleri önceden satın alınan kapasite birimleridir. Tek bir üretilen iş şunları yapmanızı sağlar:

* Giriş: Kadar ikinci veya 1000 olaya (hangisi önce gerçekleşirse) saniye başına saniyede 1 MB.
* Çıkış: Saniye başına ikinci veya 4096 olay başına 2 MB'a kadar.

Satın alınan işleme birimlerinin kapasitesi aşıldığında giriş azaltılır ve [ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) döndürülür. Çıkış, azaltma özel durumları oluşturmaz, ancak yine de satın alınan işleme birimlerinin kapasitesiyle sınırlıdır. Yayımlama hızı özel durumları alırsanız veya daha yüksek çıkış görmeyi bekliyorsanız ad alanı için kaç tane işleme birimi satın aldığınızı denetlediğinizden emin olun. Üretilen iş birimleri yönetebileceğiniz **ölçek** alanlarının dikey [Azure portalında](https://portal.azure.com). Üretilen iş birimleri program aracılığıyla kullanarak da yönetebilirsiniz [olay hub'ları API](event-hubs-api-overview.md).

İşleme birimleri önceden satın alınır ve saat başına faturalandırılır. Satın alındıktan sonra işleme birimleri en az bir saat için faturalandırılır. En fazla 20 işleme birimi bir Event Hubs ad alanı için satın alınabilir ve bu ad alanındaki tüm event hubs arasında paylaşılır.

**Otomatik şişme** özellik Event Hubs'ın otomatik olarak ölçeklendirilebilir üretilen iş birimi sayısını artırarak, kullanım karşılaması gerekir. Üretilen iş birimleri artırma engeller azaltma senaryoları, burada:

- Veri giriş hızlarını kümesi işleme birimleri en fazla.
- Veri çıkışı istek hızları kümesi işleme birimleri en fazla.

Tüm istekleri ServerBusy hatalarla başarısız olmadan en düşük eşikten yüksek yük artırdığı durumlarda Event Hubs hizmeti verimliliğini artırır. 

Otomatik Şişir özelliği hakkında daha fazla bilgi için bkz. [üretilen iş birimlerini otomatik olarak ölçeklendirme](event-hubs-auto-inflate.md).

## <a name="partitions"></a>Bölümler
[!INCLUDE [event-hubs-partitions](../../includes/event-hubs-partitions.md)]

### <a name="partition-key"></a>Bölüm anahtarı

Gelen olay verilerini veri düzenleme amacıyla belirli bölümlere eşlemek için [bölüm anahtarı](event-hubs-programming-guide.md#partition-key) kullanabilirsiniz. Bölüm anahtarı, gönderen tarafından belirtilip bir olay hub'ına geçirilen değerdir. Statik karma işlevi ile işlenir ve sonuçta bölüm ataması oluşturulur. Bir olayı yayımlarken bölüm anahtarı belirtmezseniz hepsini bir kez deneme ataması kullanılır.

Olay yayımcısı yalnızca bölüm anahtarını bilir, olayların yayımlandığı bölümü bilmez. Anahtar ile bölümün bu şekilde ayrılması göndereni aşağı akış işleme hakkında çok fazla bilgi sahibi olma gereksiniminden kurtarır. Cihaz veya kullanıcı başına benzersiz bir kimlik iyi bir bölüm anahtarı oluşturur, ancak ilgili olayları tek bir bölümde gruplandırmak için coğrafi bölge gibi diğer öznitelikler de kullanılabilir.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

- [İşleme birimlerini otomatik ölçeklendirme](event-hubs-auto-inflate.md)
- [Event Hubs hizmetine genel bakış](event-hubs-what-is-event-hubs.md)
