---
title: Performans önerileri - MariaDB için Azure Veritabanı
description: Bu makalede, MariaDB için Azure Veritabanı'ndaki Performans Önerisi özelliği açıklanmaktadır
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 838a4123bd5007f987f27674862409445967a2d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528107"
---
# <a name="performance-recommendations-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı’ndaki Performans Önerileri

**Aşağıdakiler için geçerlidir:** MariaDB 10.2 için Azure Veritabanı

Performans Önerileri özelliği, gelişmiş performans için özelleştirilmiş öneriler oluşturmak için veritabanlarınızı analiz eder. Önerileri üretmek için, analiz şema da dahil olmak üzere çeşitli veritabanı özellikleri bakar. Performans Önerileri özelliğini tam olarak kullanmak için sunucunuzdaki [Sorgu Mağazası'nı](concepts-query-store.md) etkinleştirin. Performans şeması KAPALI ise, Sorgu Mağazası'nı açmak performance_schema ve özellik için gereken performans şeması araçlarının bir alt kümesini sağlar. Herhangi bir performans önerisi uyguladıktan sonra, bu değişikliklerin etkisini değerlendirmek için performansı test etmeniz gerekir.

## <a name="permissions"></a>İzinler

Performans Önerileri özelliğini kullanarak analiz çalıştırmak için **Sahip** veya **Katkıda bulunan** izinleri gereklidir.

## <a name="performance-recommendations"></a>Performans önerileri

[Performans Önerileri](concepts-performance-recommendations.md) özelliği, performansı iyileştirme potansiyeli olan dizinleri tanımlamak için sunucunuzdaki iş yüklerini analiz eder.

MariaDB sunucunuz için Azure portal sayfasındaki menü çubuğunun **Akıllı Performans** bölümünden **Performans Önerileri** açın.

![Performans Önerileri giriş sayfası](./media/concepts-performance-recommendations/performance-recommendations-page.png)

**Çözümle'yi** seçin ve çözümlemeye başlayacak bir veritabanı seçin. İş yükünüze bağlı olarak, çözümlemenin tamamlanması birkaç dakika sürebilir. Analiz tamamlanınca portalda bir bildirim olur. Analiz veritabanınızın derin bir incelemesini gerçekleştirir. Yoğun olmayan dönemlerde analiz yapmanızı öneririz.

**Öneriler** penceresi, varsa önerilerin listesini ve bu öneriyi oluşturan ilgili sorgu kimliğini gösterir. Sorgu kimliği ile, sorgu hakkında daha fazla bilgi edinmek için [mysql.query_store](concepts-query-store.md#mysqlquery_store) görünümünü kullanabilirsiniz.

![Performans Önerileri yeni sayfa](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Öneriler otomatik olarak uygulanmaz. Öneriyi uygulamak için sorgu metnini kopyalayın ve seçtiğiniz istemciden çalıştırın. Öneriyi değerlendirmek için test etmeyi ve izlemeyi unutmayın.

## <a name="recommendation-types"></a>Öneri türleri

Şu anda yalnızca *Diziliş Oluştur* önerileri desteklenir.

### <a name="create-index-recommendations"></a>Dizin önerileri oluşturma

*Dizin oluşturma* önerileri, iş yükünde en sık çalışan veya zaman alan sorguları hızlandırmak için yeni dizinler önerir. Bu öneri türü, [Sorgu Deposu'nun](concepts-query-store.md) etkinleştirilmesini gerektirir. Sorgu Deposu sorgu bilgilerini toplar ve çözümlemenin öneriyi yapmak için kullandığı ayrıntılı sorgu çalışma zamanı ve sıklık istatistiklerini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- MariaDB için Azure Veritabanı'nda [izleme ve aetme](concepts-monitoring.md) hakkında daha fazla bilgi edinin.