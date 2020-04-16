---
title: Synapse Analytics SQL kaynak tüketimi
description: Azure Synapse Analytics'teki Synapse SQL tüketim modelleri hakkında bilgi edinin.
services: synapse analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: e078893b3bbe0ef5661cd87bad62b320f78ceb5d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424863"
---
# <a name="azure-synapse-analytics-sql-resource-consumption"></a>Azure Synapse Analytics SQL kaynak tüketimi

Bu makalede, Synapse SQL (önizleme) kaynak tüketim modelleri açıklanmaktadır.

## <a name="sql-on-demand"></a>İsteğe bağlı SQL

İsteğe bağlı SQL, sorgu hizmeti başına doğru boyutu seçmenizi gerektirmeyen bir ödemedir. Sistem gereksinimlerinize göre otomatik olarak ayarlanır, altyapınızı yönetmekten ve çözümünüz için doğru boyutu seçmekten sizi kurtarır.

## <a name="sql-pool---data-warehouse-units-dwus-and-compute-data-warehouse-units-cdwus"></a>SQL havuzu - Veri Ambarı Birimleri (DWUs) ve bilgi işlem Veri Ambarı Birimleri (cDWUs)

Fiyat ve performansı optimize etmek için ideal veri ambarı birimi (DWUs) sayısının seçilmesi ve birim sayısının nasıl değiştirilene ilişkin öneriler.

### <a name="what-are-data-warehouse-units"></a>Veri Ambarı Birimleri nedir

Synapse SQL havuzu, sağlanan analitik kaynaklar koleksiyonunu temsil eder. Analitik kaynaklar CPU, bellek ve IO'nun bir kombinasyonu olarak tanımlanır. Bu üç kaynak, Veri Ambarı Birimleri (DWUs) adı verilen bilgi işlem ölçeği birimlerine paketlenir. DWU, hesaplama kaynakları ve performansının soyut, normalleştirilmiş bir ölçümünü temsil eder. Hizmet düzeyinizdeki bir değişiklik, sisteminizin performansını ve maliyetini ayarlayan, sistemin kullanabileceği DW'lerin sayısını değiştirir.

Daha yüksek performans için veri ambarı birimlerinin sayısını artırabilirsiniz. Daha az performans için veri ambarı birimlerini azaltın. Depolama ve hesaplama maliyetleri ayrıca faturalandırılır, yani veri ambarı birimlerini değiştirmek depolama maliyetlerini etkilemez.

Veri ambarı birimlerinin performansı şu veri ambarı iş yükü ölçümlerini temel alar:

- Standart bir veri ambarı sorgusunun çok sayıda satırı tarayıp karmaşık bir toplama gerçekleştirmesi ne kadar hızlı olabilir. Bu işlem G/Ç ve CPU yoğundur.
- Veri ambarı, Azure Depolama Blobs veya Azure Veri Gölü'nden ne kadar hızlı veri alabilir. Bu işlem ağ ve CPU yoğundur.
- T-SQL [`CREATE TABLE AS SELECT`](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) komutu bir tabloyu ne kadar hızlı kopyalayabilir. Bu işlem, depolamadan gelen verileri okumayı, cihazın düğümlerine dağıtmayı ve yeniden depolamaya yazmayı içerir. Bu işlem CPU, IO ve ağ yoğundur.

Artan DWUs:

- Sistemin taramalar, toplamalar ve CTAS ifadeleri için performansını doğrusal olarak değiştirir
- PolyBase yük işlemleri için okuyucu ve yazar sayısını artırır
- En fazla eşzamanlı sorgu ve eşzamanlılık yuvası sayısını artırır.

### <a name="service-level-objective"></a>Hizmet Düzeyi Hedefi

Hizmet Düzeyi Hedefi (SLO), veri ambarınızın maliyet ve performans düzeyini belirleyen ölçeklenebilirlik ayarıdır. Gen2'nin hizmet düzeyleri, örneğin DW2000c gibi bilgi işlem veri ambarı birimlerinde (cDWU) ölçülür. Gen1 hizmet düzeyleri DWUs cinsinden ölçülür, örneğin DW2000.

Hizmet Düzeyi Hedefi (SLO), veri ambarınızın maliyet ve performans düzeyini belirleyen ölçeklenebilirlik ayarıdır. Gen2 SQL havuzunun hizmet düzeyleri, dw2000c gibi veri ambarı birimlerinde (DWU) ölçülür.

> [!NOTE]
> Azure SQL Veri Ambarı Gen2, 100 cDWU'ya kadar düşük bilgi işlem katmanlarını desteklemek için ek ölçek özellikleri ekledi. Şu anda Gen1'de daha düşük bilgi işlem katmanları gerektiren varolan veri ambarları, şu anda ek ücret ödemeden kullanılabilen bölgelerde Gen2'ye yükseltilebilir.  Bölgeniz henüz desteklenmiyorsa, desteklenen bir bölgeye yükseltmeyapmaya devam edebilirsiniz. Daha fazla bilgi [için Gen2'ye Yükseltme'ye](../sql-data-warehouse/upgrade-to-latest-generation.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

T-SQL'de SERVICE_OBJECTIVE ayarı, SQL havuzunuzun hizmet düzeyini ve performans katmanını belirler.

```sql
CREATE DATABASE mySQLDW
(Edition = 'Datawarehouse'
 ,SERVICE_OBJECTIVE = 'DW1000c'
)
;
```

### <a name="performance-tiers-and-data-warehouse-units"></a>Performans Katmanları ve Veri Ambarı Birimleri

Her performans katmanı, veri ambarı birimleri için biraz farklı bir ölçü birimi kullanır. Bu fark, ölçek birimi doğrudan faturalandırmaya çevrildiği için faturaya yansıtılır.

- Gen1 veri ambarları Veri Ambarı Birimleri (DWUs) cinsinden ölçülür.
- Gen2 veri ambarları bilgi işlem veri ambarı birimlerinde (cDWUs) ölçülür.

Hem DWUs'lar hem de cDWUs'lar, veri ambarını kullanmanız gerekmediğinde hesaplamayı yukarı veya aşağı ölçekleme ve hesaplamayı duraklatmayı destekler. Bu işlemlerin hepsi isteğe bağlı. Gen2, performansı artırmak için bilgi işlem düğümlerinde yerel bir disk tabanlı önbellek kullanır. Sistemi ölçeklediğinizde veya duraklattığınızda, önbellek geçersiz kılınır ve böylece en iyi performans elde edilmeden önce önbellek ısınması dönemi gerekir.  

Veri ambarı birimlerini artırdıkça, bilgi işlem kaynaklarını doğrusal olarak artırırsınız. Gen2 en iyi sorgu performansını ve en yüksek ölçeği sağlar. Gen2 sistemleri de önbellekten en iyi şekilde yararlanır.

#### <a name="capacity-limits"></a>Kapasite sınırları

Her SQL sunucusunun (örneğin, myserver.database.windows.net) belirli sayıda veri ambarı birimi sağlayan bir [Veritabanı İşlem Birimi (DTU)](../../sql-database/sql-database-service-tiers-dtu.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) kotası vardır. Daha fazla bilgi için [iş yükü yönetimi kapasite sınırlarına](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#workload-management)bakın.

### <a name="how-many-data-warehouse-units-do-i-need"></a>Kaç veri ambarı birimine ihtiyacım var

İdeal veri ambarı birimi sayısı, iş yükünüze ve sisteme yüklediğiniz veri miktarına bağlıdır.

İş yükünüz için en iyi DWU'yu bulma adımları:

1. Daha küçük bir DWU seçerek başlayın.
2. Gözlemlediğiniz performansa göre seçilen DW'lerin sayısını gözlemleyerek, sistemdeki veri yüklerini test ederken uygulama performansınızı izleyin.
3. Yoğun aktivitenin periyodik dönemleri için ek gereksinimleri belirleyin. Etkinlikte önemli zirveler ve çukurlar gösteren iş yüklerinin sık sık ölçeklendirilmesi gerekebilir.

SQL havuzu, büyük miktarda bilgi işlem ve sorgu lanabilir miktarda veri sağlayan ölçeklendirilebilir bir sistemdir. Ölçeklendirme için gerçek yeteneklerini görmek için, özellikle büyük DWUs'larda, CPU'ları beslemek için yeterli veriye sahip olduğundan emin olmak için ölçeklendikçe veri kümesini ölçeklendirmenizi öneririz. Ölçek testi için en az 1 TB kullanmanızı öneririz.

> [!NOTE]
>
> Sorgu performansı yalnızca işlem düğümleri arasında bölünebilir, daha fazla paralelleştirme ile artar. Ölçeklendirmenin performansınızı değiştirmediğini fark ederseniz, tablo tasarımınızı ve/veya sorgularınızı ayarlamanız gerekebilir. Sorgu alamı kılavuzu için [bkz.](../overview-cheat-sheet.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

### <a name="permissions"></a>İzinler

Veri ambarı birimlerinin değiştirilmesi [alter DATABASE'te](/sql/t-sql/statements/alter-database-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)açıklanan izinleri gerektirir.

SQL DB Katılımcısı ve SQL Server Katılımcısı gibi Azure kaynaklarının yerleşik rolleri DWU ayarlarını değiştirebilir.

#### <a name="view-current-dwu-settings"></a>Geçerli DWU ayarlarını görüntüleme

Geçerli DWU ayarını görüntülemek için:

1. Visual Studio'da SQL Server Object Explorer'ı açın.
2. Mantıksal SQL Veritabanı sunucusuyla ilişkili ana veritabanına bağlanın.
3. sys.database_service_objectives dinamik yönetim görünümünden seçim belirleyin. Örnek aşağıda verilmiştir:

```sql
SELECT  db.name [Database]
,        ds.edition [Edition]
,        ds.service_objective [Service Objective]
FROM    sys.database_service_objectives   AS ds
JOIN    sys.databases                     AS db ON ds.database_id = db.database_id
;
```

### <a name="change-data-warehouse-units"></a>Veri ambarı birimlerini değiştirme

#### <a name="azure-portal"></a>Azure portal

DWUs'ları değiştirmek için:

1. Azure [portalını](https://portal.azure.com)açın, veritabanınızı açın ve **Ölçekle'yi**tıklatın.

2. **Ölçek**altında, DWU ayarını değiştirmek için kaydırıcıyı sola veya sağa taşıyın.

3. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Onaylamak için **evet**’e, iptal etmek için **hayır**’a tıklayın.

#### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

DWUs'ları değiştirmek için [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) PowerShell cmdlet'i kullanın. Aşağıdaki örnek, sunucu MyServer'da barındırılan MySQLDW veritabanı için hizmet düzeyi hedefini DW1000 olarak ayarlar.

```Powershell
Set-AzSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000c"
```

Daha fazla bilgi için [SQL Veri Ambarı için PowerShell cmdlets'e](../sql-data-warehouse/sql-data-warehouse-reference-powershell-cmdlets.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) bakın

#### <a name="t-sql"></a>T-SQL

T-SQL ile geçerli DWUayar'ı görüntüleyebilir, ayarları değiştirebilir ve ilerlemeyi denetleyebilirsiniz.

DWUs'u değiştirmek için:

1. Mantıksal SQL Veritabanı sunucunuzla ilişkili ana veritabanına bağlanın.
2. ALTER [DATABASE](/sql/t-sql/statements/alter-database-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) TSQL deyimini kullanın. Aşağıdaki örnek, MySQLDW veritabanı için dw1000c hizmet düzeyi hedefini ayarlar.

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000c')
;
```

#### <a name="rest-apis"></a>REST API'leri

DWUs'ları değiştirmek için Veritabanı OLUŞTURMA [veya Güncelleştir](/sql/t-sql/statements/alter-database-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) EKSAP'sini kullanın. Aşağıdaki örnek, sunucu MyServer'da barındırılan MySQLDW veritabanı için hizmet düzeyi hedefini DW1000c olarak ayarlar. Sunucu, ResourceGroup1 adlı bir Azure kaynak grubundadır.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

Daha fazla REST API örneği [için, SQL Veri Ambarı için REST API'leri'ne](../sql-data-warehouse/sql-data-warehouse-manage-compute-rest-api.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

### <a name="check-status-of-dwu-changes"></a>DWU değişikliklerinin durumunu denetleme

DWU değişikliklerinin tamamlanması birkaç dakika sürebilir. Otomatik olarak ölçekleme alıyorsanız, başka bir eyleme geçmeden önce belirli işlemlerin tamamlandığından emin olmak için mantık uygulamayı düşünün.

Veritabanı durumunu çeşitli uç noktalardan denetlemek, otomasyonu doğru bir şekilde uygulamanızı sağlar. Portal, bir işlem tamamlandıktan sonra bildirim sağlar ve veritabanları geçerli durum, ancak durum programlı denetimi için izin vermez.

Azure portalı ile ölçeklendirme işlemleri için veritabanı durumunu denetleyemezsiniz.

DWU değişikliklerinin durumunu denetlemek için:

1. Mantıksal SQL Veritabanı sunucunuzla ilişkili ana veritabanına bağlanın.
2. Veritabanı durumunu denetlemek için aşağıdaki sorguyu gönderin.

```sql
SELECT    *
FROM      sys.databases
;
```

1. İşlem durumunu denetlemek için aşağıdaki sorguyu gönderme

```sql
SELECT    *
FROM      sys.dm_operation_status
WHERE     resource_type_desc = 'Database'
AND       major_resource_id = 'MySQLDW'
;
```

Bu DMV, SQL havuzunuzdaki işlem ve işlemin durumu gibi IN_PROGRESS veya TAMAMLANAN gibi çeşitli yönetim işlemleri hakkında bilgi verir.

### <a name="the-scaling-workflow"></a>Ölçekleme iş akışı

Bir ölçek işlemini başlattığınızda, sistem önce tüm açık oturumları öldürür ve tutarlı bir durum sağlamak için tüm açık hareketleri geri alar. Ölçek işlemleri için ölçeklendirme yalnızca bu işlemgeri geri alma tamamlandıktan sonra oluşur.  

- Ölçeklendirme işlemi için sistem tüm işlem düğümlerini ayırır, ek işlem düğümlerini hükümler ve ardından depolama katmanına yeniden bağlar.
- Küçültülmüş bir işlem için sistem tüm işlem düğümlerini ayırır ve yalnızca gerekli düğümleri depolama katmanına yeniden bağlar.

## <a name="next-steps"></a>Sonraki adımlar

Performansı yönetme hakkında daha fazla bilgi edinmek için iş yükü yönetimi ve [Bellek ve eşzamanlılık sınırları](../sql-data-warehouse/memory-concurrency-limits.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)için Kaynak [sınıflarına](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) bakın.
