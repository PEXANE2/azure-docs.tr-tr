---
title: PostgreSQL için Azure veritabanı 'nda performans önerileri-tek sunucu
description: Bu makalede PostgreSQL için Azure veritabanı-tek sunucu ' da performans önerisi özelliği açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: e1e9e998c2ac4695d955a546d0f02fbc2b517d5e
ms.sourcegitcommit: beb34addde46583b6d30c2872478872552af30a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69907486"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda performans önerileri-tek sunucu

**Şunlara uygulanır:** PostgreSQL için Azure veritabanı-tek sunucu 9,6 ve 10

Performans önerileri özelliği, gelişmiş performans için özel öneriler oluşturmak üzere veritabanlarınızı analiz edin. Bu önerileri oluşturmak için analiz, şema dahil çeşitli veritabanı özelliklerine bakar. Performans önerileri özelliğini tam olarak kullanmak için sunucunuzda [sorgu deposunu](concepts-query-store.md) etkinleştirin. Herhangi bir performans önerisi uygulandıktan sonra, bu değişikliklerin etkisini değerlendirmek için performansı sınamalısınız. 

## <a name="permissions"></a>İzinler
Performans Önerileri özelliğini kullanarak analiz çalıştırmak için **Sahip** veya **Katkıda bulunan** izinleri gereklidir.

## <a name="performance-recommendations"></a>Performans önerileri
[Performans Önerileri](concepts-performance-recommendations.md) özelliği, performansı iyileştirme potansiyeli olan dizinleri tanımlamak için sunucunuzdaki iş yüklerini analiz eder.

PostgreSQL sunucunuz için Azure portal sayfasındaki menü çubuğunun **akıllı performans** bölümünde **performans önerilerini** açın.

![Performans Önerileri giriş sayfası](./media/concepts-performance-recommendations/performance-recommendations-page.png)

**Çözümle** ' yi seçin ve analiz işlemini başlatmak için bir veritabanı seçin. İş yükünüze bağlı olarak, önce çözümlemenin tamamlanması birkaç dakika sürebilir. Analiz tamamlanınca portalda bir bildirim olur. Analiz, veritabanınızı ayrıntılı bir şekilde inceleme işlemini gerçekleştirir. Yoğun olmayan dönemler sırasında analiz gerçekleştirmenizi öneririz. 

**Öneriler** penceresi, bulunursa önerilerin bir listesini gösterir.

![Performans önerileri yeni sayfa](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Öneriler otomatik olarak uygulanmaz. Öneriyi uygulamak için, sorgu metnini kopyalayın ve tercih ettiğiniz istemciden çalıştırın. Öneriyi değerlendirmek için sınamayı ve izlemeyi unutmayın. 

## <a name="recommendation-types"></a>Öneri türleri

Şu anda iki tür öneri desteklenir: *Dizin* ve *bırakma dizini*oluşturun.

### <a name="create-index-recommendations"></a>Dizin önerileri oluşturma
*Dizin önerilerini oluşturma* , iş yükünde en sık çalıştırılan veya zaman alan sorguları hızlandırmak için yeni dizinler önerir. Bu öneri türü, [sorgu deposunun](concepts-query-store.md) etkinleştirilmesini gerektirir. Sorgu deposu sorgu bilgilerini toplar ve çözümlemenin öneriyi yapmak için kullandığı ayrıntılı sorgu çalışma zamanı ve sıklık istatistiklerini sağlar.

### <a name="drop-index-recommendations"></a>Dizin önerilerini bırak
Eksik dizinlerin algılanmasının yanı sıra PostgreSQL için Azure veritabanı, mevcut dizinlerin performansını analiz eder. Bir dizin nadiren kullanılıyorsa veya yedekli ise, çözümleyici bunu bırakmayı önerir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* [Okuma çoğaltmaları](concepts-read-replicas.md)Için performans önerileri kullanılamaz.
## <a name="next-steps"></a>Sonraki adımlar
- PostgreSQL için Azure Veritabanı’nda [izleme ve ayarlama](concepts-monitoring.md) hakkında daha fazla bilgi edinin.

