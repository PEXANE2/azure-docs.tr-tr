---
title: Sorgu istatistikleri koleksiyonunu optimize edin - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için bir Azure Veritabanı'nda sorgu istatistikleri koleksiyonunu nasıl optimize edebileceğiniz açıklanmaktadır
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: f467f01118470eb51f7decf3bd6457917c566723
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74770178"
---
# <a name="optimize-query-statistics-collection-on-an-azure-database-for-postgresql---single-server"></a>PostgreSQL için Bir Azure Veritabanında sorgu istatistikleri koleksiyonunu optimize edin - Tek Sunucu
Bu makalede, PostgreSQL sunucusu için bir Azure Veritabanı'nda sorgu istatistikleri koleksiyonunun nasıl optimize edilebildiğini açıklanmaktadır.

## <a name="use-pg_stats_statements"></a>pg_stats_statements kullanın
**Pg_stat_statements,** PostgreSQL için Azure Veritabanı'nda varsayılan olarak etkinleştirilen bir PostgreSQL uzantısıdır. Uzantı, bir sunucu tarafından yürütülen tüm SQL deyimleri için yürütme istatistiklerini izlemek için bir araç sağlar. Bu modül her sorgu yürütme içine kanca ve önemsiz olmayan bir performans maliyeti ile birlikte gelir. **Pg_stat_statements'nin** sorgu metnini diskteki dosyalara yazmalarını zorlamasını etkinleştirme.

Uzun sorgu metnine sahip benzersiz sorgularınız varsa veya **pg_stat_statements**etkin olarak izlemiyorsanız, en iyi performans için **pg_stat_statements** devre dışı kılabilir. Bunu yapmak için, ayarı ' yı ' ' olarak `pg_stat_statements.track = NONE`değiştirin

Bazı müşteri iş **yükleri, pg_stat_statements** devre dışı bırakıldığında yüzde 50'ye kadar performans artışı görmüştür. devre dışı pg_stat_statements yaptığınız denge, performans sorunlarını giderme yükümlülüğüdür.

Ayarlamak `pg_stat_statements.track = NONE`için:

- Azure portalında, [PostgreSQL kaynak yönetimi sayfasına gidin ve sunucu parametreleri blade'i seçin.](howto-configure-server-parameters-using-portal.md)

  ![PostgreSQL sunucu parametre blade](./media/howto-optimize-query-stats-collection/pg_stats_statements_portal.png)

- Azure [CLI](howto-configure-server-parameters-using-cli.md) az postgres sunucu `--name pg_stat_statements.track --resource-group myresourcegroup --server mydemoserver --value NONE`yapılandırmasını '' olarak ayarlayın.

## <a name="use-the-query-store"></a>Sorgu Deposunu Kullanma 
PostgreSQL için Azure Veritabanı'ndaki [Sorgu Deposu](concepts-query-store.md) özelliği, sorgu istatistiklerini izlemek için daha etkili bir yöntem sağlar. Bu özelliği *pg_stats_statements*kullanmaya alternatif olarak öneriyoruz. 

## <a name="next-steps"></a>Sonraki adımlar
Azure `pg_stat_statements.track = NONE` [portalında](howto-configure-server-parameters-using-portal.md) veya [Azure CLI'yi](howto-configure-server-parameters-using-cli.md)kullanarak ayarlamayı düşünün.

Daha fazla bilgi için bkz. 
- [Sorgu Mağaza kullanım senaryoları](concepts-query-store-scenarios.md) 
- [Sorgu Deposu en iyi yöntemleri](concepts-query-store-best-practices.md) 
