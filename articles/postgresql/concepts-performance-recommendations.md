---
title: Performans Önerileri - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'ndaki Performans Önerisi özelliği açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: a30af0c8bef47a37fe3439e885d3895a2c826225
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74768478"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanında Performans Önerileri - Tek Sunucu

**Aşağıdakiler için geçerlidir:** PostgreSQL için Azure Veritabanı - Tek Sunucu sürümleri 9.6, 10, 11

Performans Önerileri özelliği, gelişmiş performans için özelleştirilmiş öneriler oluşturmak için veritabanlarınızı analiz eder. Önerileri üretmek için, analiz şema da dahil olmak üzere çeşitli veritabanı özellikleri bakar. Performans Önerileri özelliğini tam olarak kullanmak için sunucunuzdaki [Sorgu Mağazası'nı](concepts-query-store.md) etkinleştirin. Herhangi bir performans önerisi uyguladıktan sonra, bu değişikliklerin etkisini değerlendirmek için performansı test etmeniz gerekir. 

## <a name="permissions"></a>İzinler
Performans Önerileri özelliğini kullanarak analiz çalıştırmak için **Sahip** veya **Katkıda bulunan** izinleri gereklidir.

## <a name="performance-recommendations"></a>Performans önerileri
[Performans Önerileri](concepts-performance-recommendations.md) özelliği, performansı iyileştirme potansiyeli olan dizinleri tanımlamak için sunucunuzdaki iş yüklerini analiz eder.

PostgreSQL sunucunuz için Azure portal sayfasındaki menü çubuğunun **Akıllı Performans** bölümünden **Performans Önerileri** açın.

![Performans Önerileri giriş sayfası](./media/concepts-performance-recommendations/performance-recommendations-page.png)

**Çözümle'yi** seçin ve çözümlemeye başlayacak bir veritabanı seçin. İş yükünüze bağlı olarak, th çözümlemesi birkaç dakika sürebilir. Analiz tamamlanınca portalda bir bildirim olur. Analiz veritabanınızın derin bir incelemesini gerçekleştirir. Yoğun olmayan dönemlerde analiz yapmanızı öneririz. 

**Varsa Öneriler** penceresi bir öneri listesi gösterir.

![Performans Önerileri yeni sayfa](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Öneriler otomatik olarak uygulanmaz. Öneriyi uygulamak için sorgu metnini kopyalayın ve seçtiğiniz istemciden çalıştırın. Öneriyi değerlendirmek için test etmeyi ve izlemeyi unutmayın. 

## <a name="recommendation-types"></a>Öneri türleri

Şu anda, iki tür öneri desteklenir: *Dizin Oluştur* ve *Bırak Dizini.*

### <a name="create-index-recommendations"></a>Dizin önerileri oluşturma
*Dizin oluşturma* önerileri, iş yükünde en sık çalışan veya zaman alan sorguları hızlandırmak için yeni dizinler önerir. Bu öneri türü, [Sorgu Deposu'nun](concepts-query-store.md) etkinleştirilmesini gerektirir. Sorgu Deposu sorgu bilgilerini toplar ve çözümlemenin öneriyi yapmak için kullandığı ayrıntılı sorgu çalışma zamanı ve sıklık istatistiklerini sağlar.

### <a name="drop-index-recommendations"></a>Drop Index önerileri
PostgreSQL için Azure Veritabanı, eksik dizinleri algılamanın yanı sıra varolan dizinlerin performansını analiz eder. Bir dizin nadiren kullanılırsa veya gereksiz ise, çözümleyici onu düşürmenizi önerir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Performans [Önerileri, okundu yinelemeler](concepts-read-replicas.md)için kullanılamaz.
## <a name="next-steps"></a>Sonraki adımlar
- PostgreSQL için Azure Veritabanı’nda [izleme ve ayarlama](concepts-monitoring.md) hakkında daha fazla bilgi edinin.

