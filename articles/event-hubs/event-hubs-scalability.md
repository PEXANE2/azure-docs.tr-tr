---
title: Ölçeklenebilirlik - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, bölümler ve iş verme birimleri kullanarak Azure Olay Hub'ları ölçeklendirme hakkında bilgi sağlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280956"
---
# <a name="scaling-with-event-hubs"></a>Olay Hub'ları ile ölçekleme

Olay Hub'ları ile ölçekleme etkileyen iki faktör vardır.
*   İşleme birimleri
*   Bölümler

## <a name="throughput-units"></a>İşleme birimleri

Event Hubs işleme kapasitesi, *işleme birimleri* tarafından denetlenir. İşleme birimleri önceden satın alınan kapasite birimleridir. Tek bir iş tamsayısı şunları sağlar:

* Giriş: Saniyede en fazla 1 MB veya saniyede 1000 olay (hangisi önce gelirse).
* Çıkış: Saniyede 2 MB'a kadar veya saniyede 4096 olay.

Satın alınan işleme birimlerinin kapasitesi aşıldığında giriş azaltılır ve [ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) döndürülür. Çıkış, azaltma özel durumları oluşturmaz, ancak yine de satın alınan işleme birimlerinin kapasitesiyle sınırlıdır. Yayımlama hızı özel durumları alırsanız veya daha yüksek çıkış görmeyi bekliyorsanız ad alanı için kaç tane işleme birimi satın aldığınızı denetlediğinizden emin olun. İşleme birimlerini [Azure portal](https://portal.azure.com) ad alanlarının **Ölçek** dikey penceresinde yönetebilirsiniz. [Ayrıca, Olay Hub'ları API'lerini](event-hubs-api-overview.md)kullanarak iş birimi birimlerini programlı olarak yönetebilirsiniz.

İşlem birimleri önceden satın alınır ve saat başına faturalandırılır. Satın alındıktan sonra işleme birimleri en az bir saat için faturalandırılır. Bir Olay Hub'ları ad alanı için en fazla 20 iş birimi satın alınabilir ve bu ad alanındaki tüm etkinlik hub'ları arasında paylaşılır.

Olay Hub'larının **Otomatik şişirme** özelliği, kullanım gereksinimlerini karşılamak için iş birimi sayısını artırarak otomatik olarak ölçeklendirilir. İş birimi birimlerinin artırılması, aşağıdakisenaryoların daraltılmasını önler:

- Veri giriş hızları ayarlanan iş birimi birimlerini aşıyor.
- Veri çıkış isteği oranları, ayarlanan iş birimi birimlerini aşar.

Olay Hub'ları hizmeti, yük minimum eşiğin dışına çıktığında, ServerBusy hatalarıyla ilgili herhangi bir istek başarısız olmadan iş verisini artırır. 

Otomatik şişirme özelliği hakkında daha fazla bilgi için [bkz.](event-hubs-auto-inflate.md)

## <a name="partitions"></a>Bölümler
[!INCLUDE [event-hubs-partitions](../../includes/event-hubs-partitions.md)]

### <a name="partition-key"></a>Bölüm anahtarı

Gelen olay verilerini veri düzenleme amacıyla belirli bölümlere eşlemek için [bölüm anahtarı](event-hubs-programming-guide.md#partition-key) kullanabilirsiniz. Bölüm anahtarı, gönderen tarafından belirtilip bir olay hub'ına geçirilen değerdir. Statik karma işlevi ile işlenir ve sonuçta bölüm ataması oluşturulur. Bir olayı yayımlarken bölüm anahtarı belirtmezseniz hepsini bir kez deneme ataması kullanılır.

Olay yayımcısı yalnızca bölüm anahtarını bilir, olayların yayımlandığı bölümü bilmez. Anahtar ile bölümün bu şekilde ayrılması göndereni aşağı akış işleme hakkında çok fazla bilgi sahibi olma gereksiniminden kurtarır. Cihaz veya kullanıcı başına benzersiz bir kimlik iyi bir bölüm anahtarı oluşturur, ancak ilgili olayları tek bir bölümde gruplandırmak için coğrafi bölge gibi diğer öznitelikler de kullanılabilir.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

- [İşleme birimlerini otomatik ölçeklendirme](event-hubs-auto-inflate.md)
- [Olay Hub'ları hizmetine genel bakış](event-hubs-what-is-event-hubs.md)
