---
title: Çoğaltmalarda sorguları okuyun
description: Azure SQL, okuma ölçeği genişletme olarak adlandırılan okuma iş yükleri için salt okuma çoğaltmalarının kapasitesini kullanma olanağı sunar.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
ms.date: 06/26/2020
ms.openlocfilehash: cf9f48b0907d3bfe1d07dcffcc0d0b9534f74c83
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135891"
---
# <a name="use-read-only-replicas-to-offload-read-only-query-workloads"></a>Salt okuma sorgusu iş yüklerini boşaltmak için salt okuma çoğaltmaları kullanın
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

[Yüksek kullanılabilirlik mimarisinin](high-availability-sla.md#premium-and-business-critical-service-tier-availability)bir parçası olarak, Premium ve iş açısından kritik hizmet katmanındaki her veritabanı ve yönetilen örnek, birincil bir okuma-yazma çoğaltması ve birkaç ikincil salt okuma çoğaltmalarıyla otomatik olarak sağlanır. İkincil çoğaltmalar, birincil çoğaltmayla aynı işlem boyutuyla sağlanır. *Okuma ölçeği genişletme* özelliği, salt yazılır çoğaltmalardan birinin işlem kapasitesini kullanarak, bunları okuma/yazma çoğaltmasında çalıştırmak yerine salt okunurdur. Bu şekilde, bazı salt okunurdur bazı iş yükleri, okuma-yazma iş yüklerinden yalıtılabilir ve performanslarını etkilemeyecektir. Özelliği, analiz gibi mantıksal olarak ayrılmış salt okunurdur iş yüklerini içeren uygulamalara yöneliktir. Premium ve İş Açısından Kritik hizmet katmanlarında uygulamalar, ek ücret ödemeden bu ek kapasiteyi kullanarak performans avantajları elde edebilir.

En az bir ikincil çoğaltma oluşturulduğunda, hiper ölçek hizmeti katmanında *okuma ölçeği* genişletme özelliği de kullanılabilir. Birden fazla ikincil çoğaltma, bir ikincil çoğaltmada bulunandan daha fazla kaynak gerektiren Yük Dengeleme salt okuma iş yükleri için kullanılabilir.

Temel, standart ve Genel Amaçlı hizmet katmanlarının yüksek kullanılabilirlik mimarisi herhangi bir çoğaltma içermez. Bu hizmet katmanlarında *okuma ölçeği genişletme* özelliği kullanılamaz.

Aşağıdaki diyagramda özelliği gösterilmektedir.

![Salt okunur çoğaltmalar](./media/read-scale-out/business-critical-service-tier-read-scale-out.png)

Yeni Premium, İş Açısından Kritik ve hiper ölçekli veritabanlarında, *okuma ölçeği genişletme* özelliği varsayılan olarak etkindir. Hiper ölçek için, varsayılan olarak yeni veritabanları için bir ikincil çoğaltma oluşturulur. 

> [!NOTE]
> Yönetilen örneğin İş Açısından Kritik hizmet katmanında okuma ölçeği genişletme her zaman etkindir.

SQL bağlantı dizeniz ile yapılandırıldıysa `ApplicationIntent=ReadOnly` , uygulama o veritabanının veya yönetilen örneğin salt okunurdur bir çoğaltmasına yönlendirilir. Özelliğini kullanma hakkında daha fazla bilgi için `ApplicationIntent` bkz. [uygulama hedefini belirtme](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Uygulamanın SQL bağlantı dizesindeki ayarından bağımsız olarak birincil çoğaltmaya bağlanmasını sağlamak isterseniz `ApplicationIntent` , veritabanını oluştururken veya yapılandırmasını değiştirmeksizin, okuma ölçeğini açıkça devre dışı bırakmanız gerekir. Örneğin, veritabanınızı standart veya Genel Amaçlı katmanından Premium, İş Açısından Kritik veya hiper ölçekli katmana yükseltirsiniz ve tüm bağlantılarınızın birincil çoğaltmaya gitmeye devam etmesini sağlamak istiyorsanız, okuma ölçeğini devre dışı bırakın. Devre dışı bırakma hakkında daha fazla bilgi için bkz. [okuma ölçeğini etkinleştirme ve devre dışı bırakma](#enable-and-disable-read-scale-out).

> [!NOTE]
> Sorgu deposu ve SQL Profiler özellikleri salt okuma çoğaltmalarda desteklenmez. 

## <a name="data-consistency"></a>Veri tutarlılığı

Çoğaltmaların avantajlarından biri, çoğaltmaların her zaman işlemsel olarak tutarlı durumda olması, ancak farklı noktalarda, farklı çoğaltmalar arasında bazı küçük bir gecikme süresi olabilir. Okuma ölçeği genişletme, oturum düzeyi tutarlılığını destekler. Bu, salt okuma oturumu, çoğaltma kullanım dışı olmasından kaynaklanan bir bağlantı hatasından sonra yeniden bağlanırsa, okuma-yazma çoğaltmasındaki %100 güncel olmayan bir çoğaltmaya yönlendirilebilir. Benzer şekilde, bir uygulama bir okuma-yazma oturumu kullanarak veri yazarsa ve salt okunur bir oturum kullanarak bunu hemen okuduğunda, en son güncelleştirmelerin çoğaltmada hemen görünür olmaması mümkündür. Gecikme süresi, zaman uyumsuz bir işlem günlüğü yineleme işlemi nedeniyle oluşur.

> [!NOTE]
> Bölge içindeki çoğaltma gecikmeleri düşüktür ve bu durum nadir olarak belirlenir. Çoğaltma gecikmesini izlemek için bkz. [salt okuma çoğaltmasını izleme ve sorun giderme](#monitoring-and-troubleshooting-read-only-replicas).

## <a name="connect-to-a-read-only-replica"></a>Salt okunurdur bir çoğaltmaya bağlanma

Bir veritabanı için okuma ölçeğini etkinleştirdiğinizde, `ApplicationIntent` istemci tarafından belirtilen bağlantı dizesindeki seçenek bağlantının yazma çoğaltmasına mı yoksa salt bir salt bir kopyaya mı yönlendirildiğini belirler. Özellikle, `ApplicationIntent` değer `ReadWrite` (varsayılan değer) ise, bağlantı okuma-yazma çoğaltmasına yönlendirilir. Bu, `ApplicationIntent` bağlantı dizesinde bulunmayan davranış ile aynıdır. `ApplicationIntent`Değer ise `ReadOnly` , bağlantı salt okunurdur ve bir kopyaya yönlendirilir.

Örneğin, aşağıdaki bağlantı dizesi istemciyi salt bir salt bir kopyaya bağlar (açılı ayraçlar içindeki öğeleri, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçları bırakarak):

```sql
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Aşağıdaki bağlantı dizelerinden biri, istemciyi bir okuma-yazma çoğaltmasına bağlar (açılı ayraçlar içindeki öğeleri, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçları bırakarak):

```sql
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="verify-that-a-connection-is-to-a-read-only-replica"></a>Bir bağlantının salt bir çoğaltma çoğaltması olduğunu doğrulama

Veritabanı bağlamında aşağıdaki sorguyu çalıştırarak salt bir salt okuma çoğaltmasına bağlanıp bağlanmadığını doğrulayabilirsiniz. Salt bir kopyaya bağlandığınızda READ_ONLY döndürülür.

```sql
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability');
```

> [!NOTE]
> Premium ve İş Açısından Kritik hizmet katmanlarında, belirli bir zamanda salt okuma Çoğaltmalarından yalnızca birine erişilebilir. Hiper ölçek birden çok salt okuma çoğaltmasını destekler.

## <a name="monitoring-and-troubleshooting-read-only-replicas"></a>Salt okuma çoğaltmaları izleme ve sorunlarını giderme

Bir salt okuma çoğaltmasına bağlanıldığında, dinamik yönetim görünümleri (DMVs) çoğaltmanın durumunu yansıtır ve izleme ve sorun giderme amacıyla sorgulanabilir. Veritabanı altyapısı, çok çeşitli izleme verilerini açığa çıkarmak için birden çok görünüm sağlar. 

Yaygın olarak kullanılan görünümler şunlardır:

| Name | Amaç |
|:---|:---|
|[sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Son saat için CPU, veri GÇ ve hizmet hedefi sınırlarına göre günlük yazma kullanımı dahil olmak üzere kaynak kullanım ölçümleri sağlar.|
|[sys. dm_os_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql)| Veritabanı altyapısı örneği için toplam bekleme istatistikleri sağlar. |
|[sys. dm_database_replica_states](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-replica-states-azure-sql-database)| Çoğaltma sistem durumu ve eşitleme istatistikleri sağlar. Sıra boyutunu Yinele ve yineleme oranı, salt okuma çoğaltmasında veri gecikmesi göstergesi olarak hizmeti sunar. |
|[sys. dm_os_performance_counters](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql)| Veritabanı altyapısı performans sayaçlarını sağlar.|
|[sys. dm_exec_query_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql)| Yürütmeler sayısı, kullanılan CPU süresi vb. gibi sorgu başına yürütme istatistikleri sağlar.|
|[sys. dm_exec_query_plan ()](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql)| Önbelleğe alınmış sorgu planları sağlar. |
|[sys. dm_exec_sql_text ()](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql)| Önbelleğe alınmış bir sorgu planı için sorgu metni sağlar.|
|[sys. dm_exec_query_profiles](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql)| Sorgular yürütülürken gerçek zamanlı sorgu ilerleme durumu sağlar.|
|[sys. dm_exec_query_plan_stats ()](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql)| Bir sorgu için çalışma zamanı İstatistikleri dahil olmak üzere, bilinen son gerçek yürütme planını sağlar.|
|[sys. dm_io_virtual_file_stats ()](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql)| Tüm veritabanı dosyaları için depolama ıOPS, aktarım hızı ve gecikme süresi istatistikleri sağlar. |

> [!NOTE]
> `sys.resource_stats` `sys.elastic_pool_resource_stats` Mantıksal ana veritabanındaki ve DMVs, birincil çoğaltmanın kaynak kullanım verilerini döndürür.

### <a name="monitoring-read-only-replicas-with-extended-events"></a>Genişletilmiş olaylarla salt okuma çoğaltmalarını izleme

Bir salt okuma çoğaltmasına bağlanıldığında genişletilmiş bir olay oturumu oluşturulamaz. Bununla birlikte, Azure SQL veritabanı 'nda, birincil çoğaltmada oluşturulup değiştirilen veritabanı kapsamındaki [genişletilmiş olay](xevent-db-diff-from-svr.md) oturumlarının tanımları, coğrafi çoğaltmalar ve salt okuma çoğaltmalarında olay yakalama dahil olmak üzere salt okuma çoğaltmalarına çoğaltılır.

Birincil çoğaltmadan bir oturum tanımına dayalı bir salt okuma çoğaltmasındaki genişletilmiş bir olay oturumu, birincil çoğaltmadan bağımsız olarak başlatılabilir ve durdurulabilir. Birincil çoğaltmaya genişletilmiş bir olay oturumu bırakıldığında, bu, tüm salt okunurdur çoğaltmalarda da bırakılır.

### <a name="transaction-isolation-level-on-read-only-replicas"></a>Salt okuma çoğaltmalarda işlem yalıtım düzeyi

Salt okuma çoğaltmaları üzerinde çalışan sorgular her zaman [anlık görüntü](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server) işlem yalıtım düzeyine eşlenir. Anlık görüntü yalıtımı, okuyucuların yazarları engellediği senaryolara engel olmak için satır sürümü oluşturmayı kullanır.

Nadir durumlarda, bir anlık görüntü yalıtım işlemi başka bir eşzamanlı işlemde değiştirilmiş nesne meta verilerine erişirse, "%. * ls" veritabanında anlık görüntü yalıtımı işlemi başarısız [3961](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-3961-database-engine-error)olabilir, çünkü deyimin eriştiği nesne, bu işlemin başlamasından bu yana başka bir eşzamanlı IŞLEMDE bir DDL ifadesiyle değiştirilmiştir. Meta verilerde sürüm bilgisi olmadığından işleme izin verilmedi. Anlık görüntü yalıtımıyla karıştırıldığında, meta verilere yönelik eşzamanlı güncelleştirme tutarsızlığa neden olabilir. "

### <a name="long-running-queries-on-read-only-replicas"></a>Salt okuma çoğaltmalarda uzun süre çalışan sorgular

Salt okuma çoğaltmalarda çalıştırılan sorguların sorguda başvurulan nesneler için meta verilere erişmesi gerekir (tablolar, dizinler, istatistikler, vb.) Nadir durumlarda, bir sorgu salt okuma çoğaltmasındaki aynı nesne üzerinde bir kilit tuttuğunda, birincil çoğaltmada bir meta veri nesnesi değiştirilirse, sorgu birincil çoğaltmadaki salt okuma çoğaltmasına değişiklikler uygulayan işlemi [engelleyebilir](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/troubleshoot-primary-changes-not-reflected-on-secondary#BKMK_REDOBLOCK) . Bu tür bir sorgu uzun bir süre çalıştırmak olsaydı, salt okuma çoğaltmasının birincil çoğaltmayla önemli ölçüde eşitlenmemesine neden olur. 

Salt okuma çoğaltması üzerinde uzun süre çalışan bir sorgu bu tür engellemeye neden oluyorsa, otomatik olarak sonlandırılır ve oturum, "yüksek öncelikli bir DDL işlemi nedeniyle oturumunuz kesildi" 1219 hatasını alır.

> [!NOTE]
> Sorguları salt bir çoğaltmada çalıştırırken 3961 hatası veya 1219 hatası alırsanız, sorguyu yeniden deneyin.

> [!TIP]
> Premium ve İş Açısından Kritik hizmet katmanlarında, salt okunurdur, `redo_queue_size` `redo_rate` [sys. dm_database_replica_states](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-replica-states-azure-sql-database) DMV içindeki ve sütunları, salt okuma çoğaltmasında veri gecikmesi göstergeleri olarak hizmet veren veri eşitleme işlemini izlemek için kullanılabilir.
> 

## <a name="enable-and-disable-read-scale-out"></a>Okuma ölçeğini etkinleştirme ve devre dışı bırakma

Okuma ölçeği genişletme, Premium, İş Açısından Kritik ve hiper ölçek hizmeti katmanlarında varsayılan olarak etkindir. Okuma ölçeği genişletme, temel, standart veya Genel Amaçlı Hizmet katmanlarında etkinleştirilemez. Okuma ölçeği, sıfır çoğaltmalarla yapılandırılmış hiper ölçekli veritabanlarında otomatik olarak devre dışıdır.

Aşağıdaki yöntemleri kullanarak, Premium veya İş Açısından Kritik hizmet katmanlarında tek veritabanlarında ve esnek havuz veritabanlarında okuma ölçeğini devre dışı bırakıp yeniden etkinleştirebilirsiniz.

> [!NOTE]
> Tek veritabanları ve elastik havuz veritabanları için, okuma ölçeğini devre dışı bırakma özelliği geriye dönük uyumluluk için sağlanır. İş Açısından Kritik yönetilen örneklerde okuma ölçeği genişletme devre dışı bırakılamaz.

### <a name="azure-portal"></a>Azure portal

Veritabanı **yapılandırma** dikey penceresinde okuma ölçeği genişletme ayarını yönetebilirsiniz.

### <a name="powershell"></a>PowerShell

> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Azure Resource Manager modülü, en az Aralık 2020 ' e kadar hata düzeltmeleri almaya devam edecektir.  Az Module ve Azure Resource Manager modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumluluklarını hakkında daha fazla bilgi için bkz. [new Azure PowerShell konusuna giriş az Module](/powershell/azure/new-azureps-module-az).

Azure PowerShell 'de okuma ölçeğini yönetme, Aralık 2016 Azure PowerShell yayını veya daha yenisini gerektirir. En yeni PowerShell sürümü için bkz. [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

[Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) cmdlet 'ini çağırarak ve parametresi için istenen değeri (veya) geçirerek Azure PowerShell okuma ölçeğini devre dışı bırakabilir veya yeniden etkinleştirebilirsiniz `Enabled` `Disabled` `-ReadScale` .

Var olan bir veritabanında okuma ölçeğini devre dışı bırakmak için (açılı ayraçlar içindeki öğeleri, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçları bırakarak):

```powershell
Set-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Disabled
```

Yeni bir veritabanında okuma ölçeğini devre dışı bırakmak için (açılı ayraçlar içindeki öğeleri, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçları bırakarak):

```powershell
New-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Disabled -Edition Premium
```

Var olan bir veritabanında okuma ölçeğini yeniden etkinleştirmek için (açılı ayraçlar içindeki öğeleri, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçları bırakarak):

```powershell
Set-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Enabled
```

### <a name="rest-api"></a>REST API

Okuma ölçeği genişletme devre dışı bırakılmış bir veritabanı oluşturmak veya var olan bir veritabanının ayarını değiştirmek için, `readScale` `Enabled` `Disabled` Aşağıdaki örnek istekte olduğu gibi özelliği veya olarak ayarlanan özelliği ile aşağıdaki yöntemi kullanın.

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body: {
   "properties": {
      "readScale":"Disabled"
   }
}
```

Daha fazla bilgi için bkz. [veritabanları-oluştur veya Güncelleştir](https://docs.microsoft.com/rest/api/sql/databases/createorupdate).

## <a name="using-the-tempdb-database-on-a-read-only-replica"></a>`tempdb`Veritabanını salt bir çoğaltma çoğaltması üzerinde kullanma

`tempdb`Birincil çoğaltmadaki veritabanı salt okunurdur çoğaltmalara çoğaltılmaz. Her çoğaltma `tempdb` , çoğaltma oluşturulduğunda oluşturulan kendi veritabanına sahiptir. Bu `tempdb` , güncelleştirilebilir olduğunu ve sorgu yürütme sırasında değiştirilmesini sağlar. Salt okuma iş yükünüz nesneleri kullanmaya bağımlıysa `tempdb` , bu nesneleri sorgu betiğinizin bir parçası olarak oluşturmanız gerekir.

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Coğrafi olarak çoğaltılan veritabanları ile okuma ölçeğini kullanma

Coğrafi olarak çoğaltılan ikincil veritabanlarının birincil veritabanlarıyla aynı yüksek kullanılabilirlik mimarisi vardır. Coğrafi olarak çoğaltılan ikincil veritabanına okuma ölçeği kullanımı etkinken bağlanıyorsanız, oturumunuz, `ApplicationIntent=ReadOnly` birincil yazılabilir veritabanına yönlendirildikleri gibi yüksek kullanılabilirlik Çoğaltmalarından birine yönlendirilir. Olmadan oturumlar, `ApplicationIntent=ReadOnly` coğrafi olarak çoğaltılan ikincil kopyanın birincil çoğaltmasına yönlendirilir ve bu da salt okunurdur. 

Bu şekilde, coğrafi çoğaltma oluşturmak, bir okuma-yazma birincil veritabanı için daha fazla salt okuma çoğaltması sağlar ve toplam üç salt okuma çoğaltmasıdır. Her ek coğrafi çoğaltma, farklı bir salt okuma çoğaltması sağlar. Coğrafi çoğaltmalar, birincil veritabanının bölgesi de dahil olmak üzere herhangi bir Azure bölgesinde oluşturulabilir.

> [!NOTE]
> Coğrafi olarak çoğaltılan bir ikincil veritabanının çoğaltmaları arasında otomatik hepsini bir kez deneme veya diğer yük dengeli yönlendirme yoktur.

## <a name="next-steps"></a>Sonraki adımlar

- SQL veritabanı hiper ölçek teklifi hakkında bilgi için bkz. [hyperscale hizmet katmanı](service-tier-hyperscale.md).
