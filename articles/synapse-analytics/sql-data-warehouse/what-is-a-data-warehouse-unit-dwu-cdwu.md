---
title: Azure SYNAPSE Analytics 'te (eski adıyla SQL DW) veri ambarı birimleri (DWU)
description: Fiyat ve performansı iyileştirmek için ideal sayıda veri ambarı birimi (DWU) seçmeye ve birim sayısını değiştirmeye yönelik öneriler.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 11/22/2019
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: e96fc9af0aa4f362256a5f0fee9465441a1c3928
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85210635"
---
# <a name="data-warehouse-units-dwus"></a>Veri ambarı birimleri (DWU 'Lar)

Fiyat ve performansı iyileştirmek için ideal sayıda veri ambarı birimi (DWU) seçmeye ve birim sayısını değiştirmeye yönelik öneriler.

## <a name="what-are-data-warehouse-units"></a>Veri ambarı birimleri nedir?

[SYNAPSE SQL havuzu](sql-data-warehouse-overview-what-is.md#synapse-sql-pool-in-azure-synapse) , sağlanmakta olan analitik kaynakların koleksiyonunu temsil eder. Analitik kaynaklar CPU, bellek ve GÇ birleşimi olarak tanımlanır.

Bu üç kaynak, veri ambarı birimleri (DWU) adlı bilgi işlem ölçeği birimlerine paketlenmiştir. DWU, hesaplama kaynakları ve performansının soyut, normalleştirilmiş bir ölçümünü temsil eder.

Hizmet düzeyinizdeki değişiklik, sistem tarafından kullanılabilen DWU sayısını değiştirir ve bu da sisteminizin performansını ve maliyetini ayarlar.

Daha yüksek performans için, veri ambarı birimlerinin sayısını artırabilirsiniz. Daha az performans için veri ambarı birimlerini azaltın. Depolama ve hesaplama maliyetleri ayrıca faturalandırılır, yani veri ambarı birimlerini değiştirmek depolama maliyetlerini etkilemez.

Veri ambarı birimlerinin performansı, bu veri ambarı iş yükü ölçümlerini temel alır:

- Standart bir SQL havuzu sorgusunun çok sayıda satırı tarayabilmesi ve ardından karmaşık bir toplama işlemi gerçekleştirebilmesi. Bu işlem g/ç ve CPU yoğun.
- SQL havuzunun verileri Azure depolama Bloblarından veya Azure Data Lake alabilir. Bu işlem ağ ve CPU yoğun.
- [`CREATE TABLE AS SELECT`](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)T-SQL komutunun bir tabloyu ne kadar hızlı kopyalayabilirler. Bu işlem, depolama alanından veri okumayı, gerecin düğümlerine dağıtmayı ve depolama alanına yeniden yazmayı içerir. Bu işlem CPU, GÇ ve ağ yoğunluğu.

DWUs 'yi artırma:

- , Taramalar, Toplamalar ve CTAS deyimleri için sistemin performansını yeniden değiştirir
- PolyBase yük işlemleri için okuyucu ve yazıcı sayısını artırır
- En fazla eşzamanlı sorgu ve eşzamanlılık yuvası sayısını artırır.

## <a name="service-level-objective"></a>Hizmet Düzeyi Hedefi

Hizmet düzeyi hedefi (SLO), veri ambarınızın maliyet ve performans düzeyini belirleyen ölçeklenebilirlik ayarıdır. Gen2 için hizmet düzeyleri, işlem verileri ambarı birimlerinde (cDWU) ölçülür (örneğin, DW2000c). Gen1 hizmet düzeyleri DWUs içinde ölçülür, örneğin DW2000.

Hizmet düzeyi hedefi (SLO), SQL havuzunuzun maliyet ve performans düzeyini belirleyen ölçeklenebilirlik ayarıdır. Gen2 SQL havuzu için hizmet düzeyleri, veri ambarı birimleri (DWU) ile ölçülür (örneğin, DW2000c).

> [!NOTE]
> Azure SQL veri ambarı Gen2, son zamanlarda 100 cDWU kadar düşük bilgi işlem katmanlarını desteklemek için ek ölçek özellikleri ekledi. Daha düşük işlem katmanları gerektiren Gen1 üzerinde şu anda mevcut olan veri ambarları artık ek bir ücret ödemeden mevcut olan bölgelerde Gen2 'ye yükseltilebilir.  Bölgeniz henüz desteklenmiyorsa desteklenen bir bölgeye de yükseltme yapabilirsiniz. Daha fazla bilgi için bkz. [Gen2 sürümüne yükseltme](../sql-data-warehouse/upgrade-to-latest-generation.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

T-SQL ' de SERVICE_OBJECTIVE ayarı, SQL havuzunuzun hizmet düzeyini ve performans katmanını belirler.

```sql
CREATE DATABASE mySQLDW
(Edition = 'Datawarehouse'
 ,SERVICE_OBJECTIVE = 'DW1000c'
)
;
```

## <a name="performance-tiers-and-data-warehouse-units"></a>Performans katmanları ve veri ambarı birimleri

Her performans katmanı, veri ambarı birimleri için biraz farklı bir ölçü birimi kullanır. Ölçek birimi doğrudan faturalandırmaya çevirilmesi halinde bu fark faturaya yansıtılır.

- Gen1 veri ambarları, veri ambarı birimlerinde (DWU) ölçülür.
- Gen2 veri ambarları, işlem verileri ambarı birimlerinde (cDWUs) ölçülür.

Hem DWUs hem de cDWUs desteği, işlem ölçeğini artırma veya azaltma ve veri ambarını kullanmanıza gerek olmadığında işlem duraklatma. Bu işlemler isteğe bağlıdır. Gen2, performansı artırmak için işlem düğümlerinde yerel bir disk tabanlı önbellek kullanır. Sistemi ölçeklendirerek veya duraklatdığınızda, önbellek geçersiz kılınır ve en iyi performans elde etmeden önce bir süre sonra bir önbellek ısınmanız gerekir.  

Her SQL Server (örneğin, myserver.database.windows.net), belirli sayıda veri ambarı birimine izin veren bir [veritabanı Işlem birimi (DTU)](../../azure-sql/database/service-tiers-dtu.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) kotasına sahiptir. Daha fazla bilgi için bkz. [iş yükü yönetim kapasitesi sınırları](sql-data-warehouse-service-capacity-limits.md#workload-management).

## <a name="capacity-limits"></a>Kapasite sınırları

Her SQL Server (örneğin, myserver.database.windows.net), belirli sayıda veri ambarı birimine izin veren bir [veritabanı Işlem birimi (DTU)](../../sql-database/sql-database-what-is-a-dtu.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) kotasına sahiptir. Daha fazla bilgi için bkz. [iş yükü yönetim kapasitesi sınırları](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#workload-management).

## <a name="how-many-data-warehouse-units-do-i-need"></a>Kaç veri ambarı birimine ihtiyacım var?

İdeal veri ambarı birimi sayısı, iş yükünüze ve sisteme yüklediğiniz veri miktarına çok fazla bağlıdır.

İş yükünüz için en iyi DWU bulma adımları:

1. Daha küçük bir DWU seçerek başlayın.
2. Veri yüklerini sisteme test ettiğiniz için uygulama performansınızı izleyin ve seçilen DWU sayısını gözlemlediğiniz performansa göre gözlemleyin.
3. Dönemsel etkinlik dönemlerine yönelik tüm ek gereksinimleri belirler. Etkinliğin önemli ve Troughs bir şekilde gösterilmesi için iş yüklerinin sık ölçeklendirilmesi gerekebilir.

SQL havuzu, büyük miktarlarda işlem ve sorgu boyutlandırılabilir veri miktarları sağlayabilen bir genişleme sistemidir.

Özellikle de daha büyük DWU 'Larda ölçeklendirmeye yönelik doğru özellikleri görmek için, CPU 'Ları akışa almak için yeterli veriniz olduğundan emin olmak üzere ölçeği ölçeklendirirken veri kümesini ölçeklendirmeniz önerilir. Ölçek testi için en az 1 TB kullanmanızı öneririz.

> [!NOTE]
>
> Sorgu performansı yalnızca, işin işlem düğümleri arasında bölüneceği durumlarda daha fazla paralelleştirme ile artar. Ölçeklendirmenin performansınızı değiştirmediğinden, tablo tasarımınızı ve/veya sorgularınızı ayarlamanız gerekebilir. Sorgu ayarlama Kılavuzu için bkz. [Kullanıcı sorgularını yönetme](cheat-sheet.md).

## <a name="permissions"></a>İzinler

Veri ambarı birimlerinin değiştirilmesi, [alter database](/sql/t-sql/statements/alter-database-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)bölümünde açıklanan izinleri gerektirir.

SQL DB katılımcısı ve SQL Server katılımcısı gibi Azure kaynakları için yerleşik roller DWU ayarlarını değiştirebilir.

## <a name="view-current-dwu-settings"></a>Geçerli DWU ayarlarını görüntüle

Geçerli DWU ayarını görüntülemek için:

1. Visual Studio 'da SQL Server Nesne Gezgini açın.
2. Mantıksal SQL Server ile ilişkili ana veritabanına bağlanın.
3. Sys. database_service_objectives dinamik yönetim görünümünden seçin. Aşağıda bir örnek verilmiştir:

```sql
SELECT  db.name [Database]
,        ds.edition [Edition]
,        ds.service_objective [Service Objective]
FROM    sys.database_service_objectives   AS ds
JOIN    sys.databases                     AS db ON ds.database_id = db.database_id
;
```

## <a name="change-data-warehouse-units"></a>Veri ambarı birimlerini değiştirme

### <a name="azure-portal"></a>Azure portal

DWUs 'yi değiştirmek için:

1. [Azure Portal](https://portal.azure.com)açın, veritabanınızı açın ve **Ölçek**' e tıklayın.

2. **Ölçek**altında, DWU ayarını değiştirmek için kaydırıcıyı sola veya sağa taşıyın.

3. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Onaylamak için **evet**’e, iptal etmek için **hayır**’a tıklayın.

#### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

DWU 'ları değiştirmek için [set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) PowerShell cmdlet 'ini kullanın. Aşağıdaki örnek, sunucu sunucum üzerinde barındırılan MySQLDW veritabanı için hizmet düzeyi hedefini DW1000 olarak ayarlar.

```Powershell
Set-AzSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000c"
```

Daha fazla bilgi için bkz. [SQL veri ambarı Için PowerShell cmdlet 'leri](../sql-data-warehouse/sql-data-warehouse-reference-powershell-cmdlets.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

### <a name="t-sql"></a>T-SQL

T-SQL ile geçerli DWUsettings ayarlarını görüntüleyebilir, ayarları değiştirebilir ve ilerleme durumunu kontrol edebilirsiniz.

DWUs 'yi değiştirmek için:

1. Sunucunuz ile ilişkili ana veritabanına bağlanın.
2. [Alter database](/sql/t-sql/statements/alter-database-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) TSQL deyimini kullanın. Aşağıdaki örnek, hizmet düzeyi hedefini MySQLDW veritabanı için DW1000c olarak ayarlar.

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000c')
;
```

### <a name="rest-apis"></a>REST API'leri

DWU 'ları değiştirmek için [Create veya Update Database](/rest/api/sql/databases/createorupdate) REST API kullanın. Aşağıdaki örnek, sunucu sunucum üzerinde barındırılan MySQLDW veritabanı için hizmet düzeyi hedefini DW1000c olarak ayarlar. Sunucu, ResourceGroup1 adlı bir Azure Kaynak grubunda bulunur.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

Daha fazla REST API örnek için bkz. [SQL veri ambarı Için REST API 'leri](../sql-data-warehouse/sql-data-warehouse-manage-compute-rest-api.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

## <a name="check-status-of-dwu-changes"></a>DWU değişikliklerinin durumunu denetle

DWU değişikliklerinin tamamlanması birkaç dakika sürebilir. Otomatik olarak ölçeklendirmeye devam ediyorsanız, başka bir eyleme geçmeden önce belirli işlemlerin tamamlandığından emin olmak için mantığı uygulamayı düşünün.

Veritabanı durumunun çeşitli uç noktalarla denetlenmesi, Otomasyonu doğru bir şekilde uygulamanıza olanak tanır. Portal bir işlemin tamamlanmasını ve veritabanlarının geçerli durumunu tamamladıktan sonra durum denetimi için izin vermez.

Azure portal genişleme işlemleri için veritabanı durumunu kontrol edebilirsiniz.

DWU değişikliklerinin durumunu denetlemek için:

1. Sunucunuz ile ilişkili ana veritabanına bağlanın.
2. Veritabanı durumunu denetlemek için aşağıdaki sorguyu gönder.

```sql
SELECT    *
FROM      sys.databases
;
```

1. İşlemin durumunu denetlemek için aşağıdaki sorguyu gönder

    ```sql
    SELECT    *
    FROM      sys.dm_operation_status
    WHERE     resource_type_desc = 'Database'
    AND       major_resource_id = 'MySQLDW'
    ;
    ```

Bu DMV, SQL havuzunuzdaki işlem ve işlem durumu gibi IN_PROGRESS ya da tamamlanmış olan çeşitli yönetim işlemleri hakkında bilgi döndürür.

## <a name="the-scaling-workflow"></a>Ölçeklendirme iş akışı

Bir ölçeklendirme işlemi başlattığınızda, sistem ilk olarak tüm açık oturumları ve tüm açık işlemleri geri alarak, tutarlı bir durum olmasını sağlar. Ölçeklendirme işlemleri için ölçekleme yalnızca bu işlem geri alma işlemi tamamlandıktan sonra oluşur.  

- Genişleme işlemi için, sistem tüm işlem düğümlerini ayırır, ek işlem düğümlerini sağlar ve ardından depolama katmanına yeniden ekler.
- Bir ölçek azaltma işlemi için, sistem tüm işlem düğümlerini ayırır ve depolama katmanına yalnızca gerekli düğümleri yeniden ekler.

## <a name="next-steps"></a>Sonraki adımlar

Performansı yönetme hakkında daha fazla bilgi edinmek için bkz. [iş yükü yönetimi Için kaynak sınıfları](resource-classes-for-workload-management.md) ve [bellek ve eşzamanlılık sınırları](memory-concurrency-limits.md).
