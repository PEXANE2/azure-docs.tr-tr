---
title: PostgreSQL için Azure veritabanı 'nda sunucu günlükleri-tek sunucu
description: Bu makalede, PostgreSQL için Azure veritabanı 'nın, tek bir sunucunun sorgu ve hata günlükleri oluşturma ve günlük saklama işlemlerinin nasıl yapılandırıldığı açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: 17083029f2377037b99abfa3ce8371661eccb957
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029979"
---
# <a name="server-logs-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda sunucu günlükleri-tek sunucu
PostgreSQL için Azure Veritabanı sorgu ve hata günlükleri oluşturur. Sorgu ve hata günlükleri yapılandırma hatalarını belirlemek, sorunlarını gidermek ve onarmak ve performansı düzeltmek için kullanılabilir. (İşlem günlüklerine erişim dahil değildir). 

## <a name="configure-logging"></a>Günlüğe kaydetmeyi yapılandırma 
Günlüğe kaydetme sunucusu parametrelerini kullanarak sunucunuzda günlüğe kaydetmeyi yapılandırabilirsiniz. Her yeni sunucu için **log_checkpoints** ve **log_connections** varsayılan olarak açık. Günlük gereksinimlerinize uyacak şekilde ayarlayabileceğiniz ek parametreler vardır: 

![PostgreSQL için Azure veritabanı-günlük parametreleri](./media/concepts-server-logs/log-parameters.png)

Bu parametreler hakkında daha fazla bilgi için bkz. PostgreSQL 'in [hata raporlama ve günlüğe kaydetme](https://www.postgresql.org/docs/current/static/runtime-config-logging.html) belgeleri. PostgreSQL için Azure veritabanı parametrelerini yapılandırma hakkında bilgi edinmek için bkz. [Portal belgeleri](howto-configure-server-parameters-using-portal.md) veya [CLI belgeleri](howto-configure-server-parameters-using-cli.md).

## <a name="access-server-logs-through-portal-or-cli"></a>Portal veya CLı aracılığıyla sunucu günlüklerine erişin
Günlükleri etkinleştirdiyseniz, [Azure Portal](howto-configure-server-logs-in-portal.md), [Azure CLı](howto-configure-server-logs-using-cli.md)ve Azure REST API 'Lerini kullanarak PostgreSQL için Azure veritabanı günlük depolama alanından erişebilirsiniz. Günlük dosyaları saatte bir veya 100 MB boyuta ulaşıldığında (hangisi önce gerçekleşirse) döndürülür. Bu günlük depolama alanı için saklama süresini, sunucunuz ile ilişkili **günlük @ no__t-1bekletme @ no__t-2period** parametresini kullanarak ayarlayabilirsiniz. Varsayılan değer 3 gündür; en büyük değer 7 gündür. Sunucunuzun günlük dosyalarını tutmak için yeterli ayrılmış depolama alanı olmalıdır. (Bu bekletme parametresi Azure tanılama günlüklerini yönetmez.)


## <a name="diagnostic-logs"></a>Tanılama günlükleri
PostgreSQL için Azure veritabanı, Azure Izleyici tanılama günlükleriyle tümleşiktir. PostgreSQL sunucunuzda günlükleri etkinleştirdikten sonra [Azure izleyici günlüklerine](../azure-monitor/log-query/log-query-overview.md), Event Hubs veya Azure depolama 'ya yayılmasını seçebilirsiniz. 

> [!IMPORTANT]
> Sunucu günlükleri için bu tanılama özelliği yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş [fiyatlandırma katmanlarında](concepts-pricing-tiers.md)kullanılabilir.

Tanılama günlüklerini Azure portal kullanarak etkinleştirmek için:

   1. Portalda, Postgres sunucunuzun gezinti menüsünde *Tanılama ayarları* ' na gidin.
   2. *Tanılama ayarı Ekle*' yi seçin.
   3. Bu ayarı adlandırın. 
   4. Tercih edilen aşağı akış konumunuzu (depolama hesabı, Olay Hub 'ı, Log Analytics) seçin. 
   5. İstediğiniz veri türlerini seçin.
   6. Ayarınızı kaydedin.

Aşağıdaki tabloda her günlükte neler olduğu açıklanmaktadır. Seçtiğiniz çıkış uç noktasına bağlı olarak, dahil edilen alanlar ve göründükleri sıralama farklılık gösterebilir. 

|**Alan** | **Açıklama** |
|---|---|
| Değerine | Kiracı KIMLIĞINIZ |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Günlük kaydedildiği zaman damgası (UTC) |
| Tür | Günlüğün türü. Her zaman `AzureDiagnostics` |
| kaynak grubundaki | Sunucunun ait olduğu abonelik için GUID |
| adlı yönetilen örnek, | Sunucunun ait olduğu kaynak grubunun adı |
| ResourceProvider | Kaynak sağlayıcının adı. Her zaman `MICROSOFT.DBFORPOSTGRESQL` |
| ResourceType | `Servers` |
| ResourceId | Kaynak URI 'SI |
| Kaynak | Sunucunun adı |
| Kategori | `PostgreSQLLogs` |
| için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir | `LogEvent` |
| Düzeyi | Günlüğe kaydetme düzeyi, örnek: günlük, hata, BILDIRIM |
| İleti | Birincil günlük iletisi | 
| Domain | Sunucu sürümü, örnek: Postgres-10 |
| Ayrıntılarını | İkincil günlük iletisi (varsa) |
| Tation | Sütunun adı (varsa) |
| SchemaName | Şemanın adı (varsa) |
| DatatypeName | Veri türünün adı (varsa) |
| LogicalServerName | Sunucunun adı | 
| _Resourceıd | Kaynak URI 'SI |
| Alan kodu | Günlük satırının öneki |



## <a name="next-steps"></a>Sonraki adımlar
- [Azure Portal](howto-configure-server-logs-in-portal.md) veya [Azure CLI](howto-configure-server-logs-using-cli.md)'dan günlüklere erişme hakkında daha fazla bilgi edinin.
- [Azure izleyici fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/)hakkında daha fazla bilgi edinin.
