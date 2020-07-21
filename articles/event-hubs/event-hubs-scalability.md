---
title: Ölçeklenebilirlik-Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs bölümler ve üretilen iş birimleri kullanarak ölçeklendirmeye yönelik bilgiler sağlanmaktadır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 4dacb24ace2332f590db54959cbf1f06694b982b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86521964"
---
# <a name="scaling-with-event-hubs"></a>Event Hubs ile ölçeklendirme

Event Hubs ölçeklendirmeyi etkileyen iki etken vardır.
*   İşleme birimleri
*   Bölümler

## <a name="throughput-units"></a>İşleme birimleri

Event Hubs işleme kapasitesi, *işleme birimleri* tarafından denetlenir. İşleme birimleri önceden satın alınan kapasite birimleridir. Tek bir üretilen iş şunları yapmanızı sağlar:

* Giriş: saniye başına 1 MB veya saniyede 1000 olay (hangisi önce gelirse).
* Çıkış: saniye başına 2 MB veya 4096 olay.

Satın alınan işleme birimlerinin kapasitesi aşıldığında giriş azaltılır ve [ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) döndürülür. Çıkış, azaltma özel durumları oluşturmaz, ancak yine de satın alınan işleme birimlerinin kapasitesiyle sınırlıdır. Yayımlama hızı özel durumları alırsanız veya daha yüksek çıkış görmeyi bekliyorsanız ad alanı için kaç tane işleme birimi satın aldığınızı denetlediğinizden emin olun. İşleme birimlerini [Azure portal](https://portal.azure.com) ad alanlarının **Ölçek** dikey penceresinde yönetebilirsiniz. Ayrıca, [Event Hubs API 'lerini](./event-hubs-samples.md)kullanarak işleme birimlerini programlı bir şekilde yönetebilirsiniz.

Üretilen iş birimleri önceden satın alınır ve saat başına faturalandırılır. Satın alındıktan sonra işleme birimleri en az bir saat için faturalandırılır. Event Hubs ad alanı için en fazla 20 işleme birimi satın alınabilir ve bu ad alanındaki tüm olay hub 'ları arasında paylaşılır.

Event Hubs **Otomatik Şişir** özelliği, kullanım ihtiyaçlarını karşılamak için üretilen iş birimi sayısını artırarak otomatik olarak ölçeklendirilir. Üretilen iş birimlerinin artırılması, azaltma senaryolarına engel olur; bu:

- Veri giriş ücretleri, üretilen iş birimlerini aşıyor.
- Veri çıkış isteği ücretleri, üretilen iş birimlerini aşıyor.

Event Hubs hizmeti, yük en düşük eşiğin ötesinde, sunucu meşgul hatalarıyla başarısız olan istekler olmadan aktarım hızını artırır. 

Otomatik Şişir özelliği hakkında daha fazla bilgi için bkz. [üretilen iş birimlerini otomatik olarak ölçeklendirme](event-hubs-auto-inflate.md).

## <a name="partitions"></a>Bölümler
[!INCLUDE [event-hubs-partitions](../../includes/event-hubs-partitions.md)]

### <a name="partition-key"></a>Bölüm anahtarı

Gelen olay verilerini veri düzenleme amacıyla belirli bölümlere eşlemek için [bölüm anahtarı](event-hubs-programming-guide.md#partition-key) kullanabilirsiniz. Bölüm anahtarı, gönderen tarafından belirtilip bir olay hub'ına geçirilen değerdir. Statik karma işlevi ile işlenir ve sonuçta bölüm ataması oluşturulur. Bir olayı yayımlarken bölüm anahtarı belirtmezseniz hepsini bir kez deneme ataması kullanılır.

Olay yayımcısı yalnızca bölüm anahtarını bilir, olayların yayımlandığı bölümü bilmez. Anahtar ile bölümün bu şekilde ayrılması göndereni aşağı akış işleme hakkında çok fazla bilgi sahibi olma gereksiniminden kurtarır. Cihaz veya kullanıcı başına benzersiz bir kimlik iyi bir bölüm anahtarı oluşturur, ancak ilgili olayları tek bir bölümde gruplandırmak için coğrafi bölge gibi diğer öznitelikler de kullanılabilir.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

- [İşleme birimlerini otomatik ölçeklendirme](event-hubs-auto-inflate.md)
- [Event Hubs hizmete genel bakış](./event-hubs-about.md)
