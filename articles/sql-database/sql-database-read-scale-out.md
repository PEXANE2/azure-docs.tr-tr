---
title: Yinelemelerle ilgili sorguları okuma
description: Azure SQL Veritabanı, salt okunur çoğaltma ların kapasitesini kullanarak salt okunur iş yüklerini yükleme olanağı sağlar - Ölçeklendirme oku adı verilir.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
ms.date: 06/03/2019
ms.openlocfilehash: 1a1b9907cd931716949d92d948a7d541fd2d5057
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206954"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads"></a>Salt okunur sorgu iş yüklerinde yük dengelemesi için salt okurun çoğaltmaları kullanma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Yüksek Kullanılabilirlik mimarisinin](./sql-database-high-availability.md#premium-and-business-critical-service-tier-availability)bir parçası olarak, Premium ve İş Açısından Kritik hizmet katmanındaki her veritabanı otomatik olarak birincil yineleme ve birkaç ikincil yinelemeyle birlikte verilir. İkincil yinelemeler birincil yinelemeyle aynı işlem boyutuyla birlikte verilir. **Okuma Ölçeği-Out** özelliği, oku-yazma yinelemesini paylaşmak yerine yalnızca okundu yinelemelerinden birinin kapasitesini kullanarak SQL Veritabanı salt okunur iş yüklerini yüklemenize olanak tanır. Bu şekilde salt okunur iş yükü ana okuma-yazma iş yükünden yalıtılır ve bu iş yükünün performansını etkilemez. Bu özellik, analitik gibi mantıksal olarak ayrılmış salt okunur iş yüklerini içeren uygulamalar için tasarlanmıştır. Premium ve İş Açısından Kritik hizmet katmanlarında, uygulamalar bu ek kapasiteyi hiçbir ek ücret ödemeden kullanarak performans avantajları ndan kazanç sağlayabilir.

En az bir ikincil yineleme **oluşturulduğunda, Okuma Ölçeği-Out** özelliği Hyperscale hizmet katmanında da kullanılabilir. Salt okunur iş yükleri bir ikincil yinelemede kullanılabilirden daha fazla kaynak gerektiriyorsa, birden çok ikincil yineleme kullanılabilir. Temel, Standart ve Genel Amaçlı hizmet katmanlarının Yüksek Kullanılabilirlik mimarisinde yinelemeler yer almaz. Bu hizmet katmanlarında **Okuma Ölçeği-Out** özelliği kullanılamaz.

Aşağıdaki diyagram, bir İş Açısından Kritik veritabanı nı kullanarak bunu göstermektedir.

![Yalnızca yinelemeleri okuma](media/sql-database-read-scale-out/business-critical-service-tier-read-scale-out.png)

Yeni Premium, Business Critical ve Hyperscale veritabanlarında Varsayılan olarak Ölçeklendirme Oku özelliği etkinleştirilir. Hyperscale için, yeni veritabanları için varsayılan olarak ikincil bir yineleme oluşturulur. SQL bağlantı dizeniz ile `ApplicationIntent=ReadOnly`yapılandırılırsa, uygulama ağ geçidi tarafından bu veritabanının salt okunur yinelemesine yönlendirilir. `ApplicationIntent` Özelliğin nasıl kullanılacağı hakkında bilgi için [bkz.](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent)

Uygulamanın SQL bağlantı dizesindeki `ApplicationIntent` ayarı ne olursa olsun birincil yinelemeye bağlanmasını sağlamak istiyorsanız, veritabanını oluştururken veya yapılandırmasını değiştirirken okuma ölçeğini açıkça devre dışı bmelisiniz. Örneğin, veritabanınızı Standart veya Genel Amaçlı katmandan Premium, İş Açısından Kritik veya Hiper ölçekli katmana yükseltiyorsanız ve tüm bağlantılarınızın birincil yinelemeye gitmeye devam etmesini istiyorsanız, Okuma Ölçeği'ni devre dışı bırakın. Nasıl devre dışı atılanın, [Oku Ölçeğini Etkinleştir ve devre dışı etme](#enable-and-disable-read-scale-out)bilgisi için bkz.

> [!NOTE]
> Sorgu Veri Deposu, Genişletilmiş Olaylar ve SQL Profiler özellikleri salt okunur yinelemelerde desteklenmez.

## <a name="data-consistency"></a>Veri tutarlılığı

Yinelemelerin avantajlarından biri, yinelemelerin her zaman işlem açısından tutarlı durumda olmasıdır, ancak zaman içinde farklı noktalarda farklı yinelemeler arasında küçük bir gecikme olabilir. Read Scale-Out oturum düzeyinde tutarlılığı destekler. Bunun anlamı, yineleme kullanılabilirliğinden kaynaklanan bir bağlantı hatasından sonra salt okunur oturumu yeniden bağlanırsa, okuma yazma yinelemesiyle %100 güncel olmayan bir yinelemeye yönlendirilebilir. Aynı şekilde, bir uygulama bir okuma-yazma oturumu kullanarak veri yazar ve hemen salt okunur oturumu kullanarak okursa, en son güncelleştirmelerin yinelemede hemen görünmemesi mümkündür. Gecikme, eşzamanlı işlem günlüğü yeniden işlem den kaynaklanır.

> [!NOTE]
> Bölge içindeki çoğaltma gecikmeleri düşüktür ve bu durum nadirdir.

## <a name="connect-to-a-read-only-replica"></a>Salt okunur yinelemeye bağlanma

Bir veritabanı için Oku Ölçeklendirme'yi `ApplicationIntent` etkinleştirdiğinizde, istemci tarafından sağlanan bağlantı dizesinde seçenek, bağlantının yazma yinelemesine mi yoksa yalnızca okunur yinelemeye mi yönlendirildiğini belirler. Özellikle, `ApplicationIntent` değer (varsayılan değer) ise, `ReadWrite` bağlantı veritabanının okuma yazma yinelemesine yönlendirilir. Bu, varolan davranışla aynıdır. `ApplicationIntent` Değer ise, `ReadOnly`bağlantı salt okunur yinelemeye yönlendirilir.

Örneğin, aşağıdaki bağlantı dizesi istemciyi salt okunur yinelemeye bağlar (açı parantezindeki öğeleri ortamınız için doğru değerlerle değiştirme ve açı braketlerini düşürme):

```sql
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Aşağıdaki bağlantı dizelerinden biri istemciyi okuma-yazma yinelemesine bağlar (açı parantezindeki öğeleri ortamınız için doğru değerlerle değiştirme ve açı braketlerini düşürme):

```sql
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="verify-that-a-connection-is-to-a-read-only-replica"></a>Bağlantının salt okunur yinelemeye ait olduğunu doğrulama

Aşağıdaki sorguyu çalıştırarak salt okunur yinelemeye bağlanıp bağlanmadığınızı doğrulayabilirsiniz. Salt okunur bir yinelemeye bağlandığında READ_ONLY döndürecek.

```sql
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```

> [!NOTE]
> Herhangi bir zamanda, ReadOnly oturumları tarafından yalnızca bir AlwaysON yinelemesi erişilebilir.

## <a name="monitoring-and-troubleshooting-read-only-replica"></a>Yalnızca okunan yinelemeyi izleme ve sorun giderme

Salt okunur yinelemeye bağlandığınızda, `sys.dm_db_resource_stats` DMV'yi kullanarak performans ölçümlerine erişebilirsiniz. Sorgu planı istatistiklerine erişmek `sys.dm_exec_query_stats` `sys.dm_exec_query_plan` için, ve `sys.dm_exec_sql_text` DMV'leri kullanın.

> [!NOTE]
> Mantıksal ana `sys.resource_stats` veritabanındaki DMV, birincil yinelemenin CPU kullanımını ve depolama verilerini döndürür.

## <a name="enable-and-disable-read-scale-out"></a>Okuma Ölçeğini Etkinleştirme ve Devre Dışı

Premium, Business Critical ve Hyperscale hizmet katmanlarında Varsayılan olarak Ölçeklendirme Oku etkinleştirilir. Temel, Standart veya Genel Amaçlı hizmet katmanlarında Ölçeklendirme Okuma etkinleştirilemez. Read Scale-Out, 0 çoğaltmayla yapılandırılan Hiper ölçekli veritabanlarında otomatik olarak devre dışı bırakılır.

Aşağıdaki yöntemleri kullanarak Premium veya Business Critical hizmet katmanındaki tek veritabanlarında ve esnek havuz veritabanlarında Okuma Ölçeğini devre dışı bırakıp yeniden etkinleştirebilirsiniz.

> [!NOTE]
> Read Scale-Out'u devre dışı etme olanağı geriye dönük uyumluluk için sağlanır.

### <a name="azure-portal"></a>Azure portalında

**Yapılandırılan** veritabanı bıçaklarında Okuma Ölçeği-out ayarını yönetebilirsiniz.

### <a name="powershell"></a>PowerShell

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi (RM) modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirmeler Az.Sql modülü içindir. AzureRM modülü en az Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecektir.  Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumlulukları hakkında daha fazla bilgi için [yeni Azure PowerShell Az modüllerini tanıtın.](/powershell/azure/new-azureps-module-az)

Azure PowerShell'de Okuma Ölçeğini yönetme için Aralık 2016 Azure PowerShell sürümü veya daha yeni sürümü gerekiyor. En yeni PowerShell sürümü için [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)bölümüne bakın.

[Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) cmdlet'i çağırarak ve parametre için istenen değerde (veya) `Enabled` geçerek Azure PowerShell'de Okuma Ölçeğini `Disabled` `-ReadScale` devre dışı bırakabilir veya yeniden etkinleştirebilirsiniz.

Varolan bir veritabanında okuma ölçeğini devre dışı bırakmak için (açı parantezindeki öğeleri ortamınız için doğru değerlerle değiştirme ve açı braketlerini düşürme):

```powershell
Set-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Disabled
```

Yeni bir veritabanında okuma ölçeğini devre dışı bırakmak için (açı parantezindeki öğeleri ortamınız için doğru değerlerle değiştirme ve açı braketlerini düşürme):

```powershell
New-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Disabled -Edition Premium
```

Varolan bir veritabanında okuma ölçeğini yeniden etkinleştirmek için (açı parantezindeki öğeleri ortamınız için doğru değerlerle değiştirme ve açı braketlerini düşürme):

```powershell
Set-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Enabled
```

### <a name="rest-api"></a>REST API

Okuma ölçeklendirme devre dışı bırakılmış bir veritabanı oluşturmak veya varolan bir veritabanının `readScale` ayarını `Enabled` değiştirmek `Disabled` için aşağıdaki yöntemi aşağıdaki örnek istekte ayarlanan özellikle birlikte kullanın.

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body: {
   "properties": {
      "readScale":"Disabled"
   }
}
```

Daha fazla bilgi için [Bkz. Veritabanları - Oluştur veya Güncelleştir.](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)

## <a name="using-tempdb-on-read-only-replica"></a>TempDB'yi salt okunur yinelemede kullanma

TempDB veritabanı salt okunur yinelemelere çoğaltılamaz. Her yineleme, yineleme oluşturulduğunda oluşturulan TempDB veritabanının kendi sürümüne sahiptir. TempDB'nin güncelleştirilmesini ve sorgu yürütmeniz sırasında değiştirilebilmesini sağlar. Salt okunur iş yükünüz TempDB nesnelerini kullanmaya bağlıysa, sorgu komut dosyanızın bir parçası olarak bu nesneleri oluşturmanız gerekir.

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Coğrafi olarak çoğaltılan veritabanlarıyla Okuma Ölçeğini Kullanma

Coğrafi olarak çoğaltılmış bir veritabanında salt okunur iş yüklerini yüklemek için Oku Ölçeği-Out kullanıyorsanız (örneğin, bir başarısız grubun üyesi olarak), hem birincil hem de coğrafi olarak çoğaltılan ikincil veritabanlarında okuma ölçeklendirmesinin etkin olduğundan emin olun. Bu yapılandırma, uygulamanız başarısız olduktan sonra yeni birincil ebağlandığında aynı yük dengeleme deneyiminin devam etmesini sağlar. Okuma ölçeği etkinleştirilmiş coğrafi çoğaltılan ikincil veritabanına bağlanıyorsanız, `ApplicationIntent=ReadOnly` oturumlarınız birincil veritabanındaki bağlantıları yönlendirdiğimiz şekilde yinelemelerden birine yönlendirilir.  Olmayan `ApplicationIntent=ReadOnly` oturumlar, yalnızca okunur olan coğrafi çoğaltılan ikincil sekonderin birincil kopyasına yönlendirilir. Coğrafi olarak çoğaltılan ikincil veritabanı birincil veritabanından farklı bir bitiş noktasına sahip olduğundan, tarihsel `ApplicationIntent=ReadOnly`olarak ikincil veritabanına erişmek için ayarlamak için gerekli değildi. Geriye dönük uyumluluk `sys.geo_replication_links` sağlamak için `secondary_allow_connections=2` DMV gösterir (herhangi bir istemci bağlantısına izin verilir).

> [!NOTE]
> İkincil veritabanının yerel yinelemeleri arasında round-robin veya başka bir yük dengeli yönlendirme desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanı Hiperölçek teklifi hakkında daha fazla bilgi [için, Bkz. Hyperscale hizmet katmanı.](./sql-database-service-tier-hyperscale.md)
