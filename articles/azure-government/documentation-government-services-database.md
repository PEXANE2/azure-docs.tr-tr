---
title: Azure Kamu veritabanları | Microsoft Docs
description: Bu, Azure Kamu için uygulama geliştirmeye yönelik özellikler ve yönergeler karşılaştırması sağlar
services: azure-government
cloud: gov
documentationcenter: ''
author: zakramer
manager: liki
ms.assetid: a1e173a9-996a-4091-a2e3-6b1e36da9ae1
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 03/06/2019
ms.author: zakramer
ms.openlocfilehash: cbf3d59bafa5b11be0fa037487dc64fdb9075619
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78357461"
---
# <a name="azure-government-databases"></a>Azure Kamu veritabanları
## <a name="sql-database"></a>SQL Database
Daha fazla bilgi için, meta veri görünürlük yapılandırması ve koruma en iyi uygulamaları hakkında ek yönergeler için<a href="https://msdn.microsoft.com/library/bb510589.aspx"> SQL veritabanı altyapısı Için Microsoft Güvenlik Merkezi</a> ve [Azure SQL veritabanı belgeleri](../sql-database/index.yml) bölümüne bakın.

### <a name="variations"></a>Farklılıkları
SQL V12 veritabanı, Azure Kamu 'da genel kullanıma sunulmuştur.

Azure Kamu 'daki SQL Azure sunucularının adresi farklıdır:

| Hizmet türü | Azure genel | Azure Kamu |
| --- | --- | --- |
| SQL Database |*. database.windows.net |*. database.usgovcloudapi.net |

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Aşağıdaki bilgiler Azure SQL için Azure Kamu sınırını tanımlar:

| Düzenlenen/denetlenen veriler izin verildi | Düzenlenen/denetlenen verilere izin verilmiyor |
| --- | --- |
| Microsoft Azure SQL 'de depolanan ve işlenen tüm veriler, Azure Kamu tarafından düzenlenen verileri içerebilir. Azure Kamu tarafından düzenlenen verilerin veri aktarımı için veritabanı araçlarını kullanın. |Azure SQL meta verilerinin dışarı aktarma denetimli verileri içermesine izin verilmez. Bu meta veriler, depolama ürününüzü oluştururken ve sürdürsırada girilen tüm yapılandırma verilerini içerir.  Şu alanlara düzenlenmiş/denetlenen verileri girmeyin: veritabanı adı, abonelik adı, kaynak grupları, sunucu adı, Sunucu Yöneticisi oturum açma, dağıtım adları, kaynak adları, kaynak etiketleri |

## <a name="sql-data-warehouse"></a>SQL Veri Ambarı
Bu hizmet ve nasıl kullanılacağı hakkında ayrıntılı bilgi için bkz. [Azure SQL veri ambarı belgeleri](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md).

## <a name="sql-server-stretch-database"></a>SQL Server Stretch Database
Bu hizmetle ilgili ayrıntılar ve nasıl kullanılacağı hakkında bilgi edinmek için bkz. [Azure SQL Server Stretch Database belgeleri](../sql-server-stretch-database/index.md)

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
Bu hizmetle ilgili ayrıntılar ve nasıl kullanılacağı hakkında bilgi için bkz. [Azure Cosmos DB belgeleri](../cosmos-db/index.yml).

### <a name="variations"></a>Farklılıkları
Azure Cosmos DB Azure Kamu 'da genel sürüme eşlik eden genel kullanıma sunulmuştur. Tek istisna, Azure Kamu için Cosmos DB kullanılamayan Şu anda **azure bilişsel arama Ekle** işlevdir.

Ayrıca, Azure Kamu 'daki Cosmos DB erişim URL 'Leri farklıdır:

| Hizmet türü | Azure genel | Azure Kamu |
| --- | --- | --- |
| Cosmos DB | *. documents.azure.com | *. documents.azure.us |


### <a name="considerations"></a>Dikkat edilmesi gerekenler
Aşağıdaki bilgiler Azure Cosmos DB için Azure Kamu sınırını tanımlar:

| Düzenlenen/denetlenen veriler izin verildi | Düzenlenen/denetlenen verilere izin verilmiyor |
| --- | --- |
| Azure Cosmos DB içinde depolanan ve işlenen tüm veriler, Azure Kamu tarafından düzenlenen verileri içerebilir. |Azure Cosmos DB meta verilerin dışarı aktarma denetimli verileri içermesine izin verilmez. Şu alanlara düzenlenmiş/denetlenen verileri girmeyin: **db adı, abonelik adı, kaynak grupları, kaynak etiketleri**. |


## <a name="azure-cache-for-redis"></a>Redis için Azure Önbelleği
Bu hizmet ve nasıl kullanılacağı hakkında ayrıntılı bilgi için bkz. [redsıs belgeleri Için Azure önbelleği](../azure-cache-for-redis/index.yml).

### <a name="variations"></a>Farklılıkları
Azure 'da redin Azure önbelleğine erişme ve bunları yönetmeye yönelik URL 'Ler farklıdır:

| Hizmet türü | Azure genel | Azure Kamu |
| --- | --- | --- |
| Önbellek uç noktası |*.redis.cache.windows.net |*.redis.cache.usgovcloudapi.net |

> [!NOTE]
> Uygun uç noktalara ve ortamlara yönelik tüm betiklerin ve kodların hesaba ihtiyacı vardır. Daha fazla bilgi için bkz. [diğer bulutlara bağlanma](../azure-cache-for-redis/cache-how-to-manage-redis-cache-powershell.md#how-to-connect-to-other-clouds).
>
>

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Aşağıdaki bilgiler Redsıs için Azure önbelleği için Azure Kamu sınırını tanımlar:

| Düzenlenen/denetlenen veriler izin verildi | Düzenlenen/denetlenen verilere izin verilmiyor |
| --- | --- |
| Redde Azure önbelleğinde depolanan ve işlenen tüm veriler, Azure Kamu tarafından düzenlenen verileri içerebilir. |Redsıs meta verileri için Azure önbelleğinin dışa aktarma denetimli veri içermesine izin verilmez. Şu alanlara düzenlenmiş/denetlenen verileri girmeyin: **önbellek adı, abonelik adı, kaynak grupları, kaynak etiketleri, redsıs özellikleri**. |

## <a name="azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı
Bu hizmet ve nasıl kullanılacağı hakkında ayrıntılı bilgi için bkz. [PostgreSQL Için Azure veritabanı belgeleri](../postgresql/index.yml).

### <a name="variations"></a>Farklılıkları
Gelişmiş tehdit koruması, PostgreSQL için Azure veritabanı 'nda sorgu Performans öngörüleri ve performans önerileri Azure Kamu **'da kullanılamaz.**

Azure Kamu 'da PostgreSQL için Azure veritabanı 'na erişmek ve bunları yönetmek için URL 'Ler farklıdır:

| Hizmet türü | Azure genel | Azure Kamu |
| --- | --- | --- |
| PostgreSQL uç noktası |*. postgres.database.azure.com |*. postgres.database.usgovcloudapi.net |

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Aşağıdaki bilgiler, PostgreSQL için Azure veritabanı Azure Kamu sınırını tanımlar:

| Düzenlenen/denetlenen veriler izin verildi | Düzenlenen/denetlenen verilere izin verilmiyor |
| --- | --- |
| PostgreSQL için Azure veritabanı 'nda depolanan ve işlenen tüm veriler, Azure Kamu tarafından düzenlenen verileri içerebilir. Azure Kamu tarafından düzenlenen verilerin veri aktarımı için veritabanı araçlarını kullanın. |PostgreSQL için Azure veritabanı meta verilerinin dışarı aktarma denetimli verileri içermesine izin verilmez. Bu meta veriler, depolama ürününüzü oluştururken ve sürdürsırada girilen tüm yapılandırma verilerini içerir.  Şu alanlara düzenlenmiş/denetlenen verileri girmeyin: veritabanı adı, abonelik adı, kaynak grupları, sunucu adı, Sunucu Yöneticisi oturum açma, dağıtım adları, kaynak adları, kaynak etiketleri. |

## <a name="azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı
Bu hizmet ve nasıl kullanılacağı hakkında ayrıntılı bilgi için bkz. [MariaDB Için Azure veritabanı belgeleri](../mariadb/index.yml).

### <a name="variations"></a>Farklılıkları
MariaDB için Azure veritabanı 'nda sorgu Performans öngörüleri ve performans önerileri Azure Kamu **'da kullanılamaz.**

Azure Kamu 'daki MariaDB için Azure veritabanı 'na erişmek ve bunları yönetmek için URL 'Ler farklıdır:

| Hizmet türü | Azure genel | Azure Kamu |
| --- | --- | --- |
| MariaDB uç noktası |*. mariadb.database.azure.com |*. mariadb.database.usgovcloudapi.net |

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Aşağıdaki bilgiler, MariaDB için Azure veritabanı için Azure Kamu sınırını tanımlar:

| Düzenlenen/denetlenen veriler izin verildi | Düzenlenen/denetlenen verilere izin verilmiyor |
| --- | --- |
| MariaDB için Azure veritabanı 'nda depolanan ve işlenen tüm veriler, Azure Kamu tarafından düzenlenen verileri içerebilir. Azure Kamu tarafından düzenlenen verilerin veri aktarımı için veritabanı araçlarını kullanın. |MariaDB meta verileri için Azure veritabanı 'nın dışarı aktarma denetimli verileri içerme izni yok. Bu meta veriler, depolama ürününüzü oluştururken ve sürdürsırada girilen tüm yapılandırma verilerini içerir.  Şu alanlara düzenlenmiş/denetlenen verileri girmeyin: veritabanı adı, abonelik adı, kaynak grupları, sunucu adı, Sunucu Yöneticisi oturum açma, dağıtım adları, kaynak adları, kaynak etiketleri. |

## <a name="azure-database-for-mysql"></a>MySQL için Azure Veritabanı
Bu hizmetle ilgili ayrıntılar ve nasıl kullanılacağı için bkz. [MySQL Için Azure veritabanı belgeleri](../mysql/index.yml).

### <a name="variations"></a>Farklılıkları
Gelişmiş tehdit koruması, MySQL için Azure veritabanı 'nda sorgu Performans öngörüleri ve performans önerileri Azure Kamu **'da kullanılamaz.**

Azure Kamu 'da MySQL için Azure veritabanı 'na erişmek ve bunları yönetmek için URL 'Ler farklıdır:

| Hizmet türü | Azure genel | Azure Kamu |
| --- | --- | --- |
| MySQL uç noktası |*. mysql.database.azure.com |*. mysql.database.usgovcloudapi.net |

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Aşağıdaki bilgiler MySQL için Azure veritabanı 'nın Azure Kamu sınırını tanımlar:

| Düzenlenen/denetlenen veriler izin verildi | Düzenlenen/denetlenen verilere izin verilmiyor |
| --- | --- |
| MySQL için Azure veritabanı 'nda depolanan ve işlenen tüm veriler, Azure Kamu tarafından düzenlenen verileri içerebilir. Azure Kamu tarafından düzenlenen verilerin veri aktarımı için veritabanı araçlarını kullanın. |MySQL meta verileri için Azure veritabanı 'nın dışarı aktarma denetimli verileri içerme izni yok. Bu meta veriler, depolama ürününüzü oluştururken ve sürdürsırada girilen tüm yapılandırma verilerini içerir.  Şu alanlara düzenlenmiş/denetlenen verileri girmeyin: veritabanı adı, abonelik adı, kaynak grupları, sunucu adı, Sunucu Yöneticisi oturum açma, dağıtım adları, kaynak adları, kaynak etiketleri. |

## <a name="next-steps"></a>Sonraki adımlar
Ek bilgi ve güncelleştirmeler için <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Kamu bloguna</a> abone olun.
