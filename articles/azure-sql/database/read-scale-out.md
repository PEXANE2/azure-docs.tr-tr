---
title: Çoğaltmalarda sorguları okuyun
description: Azure SQL veritabanı, okuma ölçeği genişletme adlı salt okuma çoğaltmalarının kapasitesini kullanarak salt okuma iş yüklerini Yük Dengeleme olanağı sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
titleSuffix: Azure SQL Database & SQL Managed Instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
ms.date: 06/03/2019
ms.openlocfilehash: 3883e56108c71d46465dbce94876724fc5664bcf
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84737827"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads"></a>Salt okunur sorgu iş yüklerinde yük dengelemesi için salt okurun çoğaltmaları kullanma
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[Yüksek kullanılabilirlik mimarisinin](high-availability-sla.md#premium-and-business-critical-service-tier-availability)bir parçası olarak, Premium ve iş açısından kritik hizmet katmanlarındaki her bir veritabanı, birincil çoğaltma ve birkaç ikincil çoğaltmayla otomatik olarak sağlanır. İkincil çoğaltmalar, birincil çoğaltmayla aynı işlem boyutuyla sağlanır. Okuma *ölçeği genişletme* özelliği, okuma-yazma çoğaltmasını paylaşmak yerine salt okuma Çoğaltmalarından birinin KAPASITESINI kullanarak SQL veritabanı salt okuma iş yüklerini yük dengelemenize olanak tanır. Bu şekilde, salt okuma iş yükü, ana okuma/yazma iş yükünün yalıtılarak ve performansını etkilemez. Özelliği, analiz gibi mantıksal olarak ayrılmış salt okunurdur iş yüklerini içeren uygulamalara yöneliktir. Premium ve İş Açısından Kritik hizmet katmanlarında uygulamalar, ek ücret ödemeden bu ek kapasiteyi kullanarak performans avantajları elde edebilir.

En az bir ikincil çoğaltma oluşturulduğunda, hiper ölçek hizmeti katmanında okuma ölçeği genişletme özelliği de kullanılabilir. Salt okuma iş yükleri bir ikincil çoğaltmada kullanılabilir olandan daha fazla kaynak gerektiriyorsa birden çok ikincil çoğaltma kullanılabilir. Temel, standart ve Genel Amaçlı hizmet katmanlarındaki yüksek kullanılabilirlik mimarisi herhangi bir çoğaltma içermez. Bu hizmet katmanlarında okuma ölçeği genişletme özelliği kullanılamaz.

Aşağıdaki diyagramda bir İş Açısından Kritik veritabanı kullanılarak gösterilmektedir.

![Salt okuma çoğaltmaları](./media/read-scale-out/business-critical-service-tier-read-scale-out.png)

Yeni Premium, İş Açısından Kritik ve hiper ölçekli veritabanlarında, okuma ölçeği genişletme özelliği varsayılan olarak etkindir. Hiper ölçek için, varsayılan olarak yeni veritabanları için bir ikincil çoğaltma oluşturulur. SQL bağlantı dizeniz ile yapılandırıldıysa `ApplicationIntent=ReadOnly` , uygulama ağ geçidi tarafından bu veritabanının salt okunurdur. Özelliğini kullanma hakkında daha fazla bilgi için `ApplicationIntent` bkz. [uygulama hedefini belirtme](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Uygulamanın SQL bağlantı dizesindeki ayarından bağımsız olarak birincil çoğaltmaya bağlanmasını sağlamak isterseniz `ApplicationIntent` , veritabanını oluştururken veya yapılandırmasını değiştirmeksizin, okuma ölçeğini açıkça devre dışı bırakmanız gerekir. Örneğin, veritabanınızı standart veya Genel Amaçlı katmanından Premium, İş Açısından Kritik veya hiper ölçekli katmana yükseltiyorsanız ve tüm bağlantılarınızın birincil çoğaltmaya gitmeye devam etmesini sağlamak istiyorsanız, okuma ölçeğini devre dışı bırakın. Devre dışı bırakma hakkında daha fazla bilgi için bkz. [okuma ölçeğini etkinleştirme ve devre dışı bırakma](#enable-and-disable-read-scale-out).

> [!NOTE]
> Sorgu veri deposu, genişletilmiş olaylar ve SQL Profiler özellikleri salt yazılır çoğaltmalar üzerinde desteklenmez.

## <a name="data-consistency"></a>Veri tutarlılığı

Çoğaltmaların avantajlarından biri, çoğaltmaların her zaman işlemsel olarak tutarlı durumda olması, ancak farklı noktalarda, farklı çoğaltmalar arasında bazı küçük bir gecikme süresi olabilir. Okuma ölçeği genişletme, oturum düzeyi tutarlılığını destekler. Bu, salt okuma oturumunun çoğaltma kullanılamamasından kaynaklanan bir bağlantı hatasından sonra yeniden bağlanmasının ardından, okuma/yazma çoğaltmasındaki %100 ' a kadar güncel bir kopyaya yönlendirildiğini gösterir. Benzer şekilde, bir uygulama bir okuma-yazma oturumu kullanarak veri yazarsa ve salt okunur bir oturum kullanarak bunu hemen okuduğunda, en son güncelleştirmelerin çoğaltma üzerinde hemen görünür olması mümkündür. Gecikme süresi, zaman uyumsuz bir işlem günlüğü yineleme işlemi nedeniyle oluşur.

> [!NOTE]
> Bölge içindeki çoğaltma gecikmeleri düşüktür ve bu durum nadir olarak belirlenir.

## <a name="connect-to-a-read-only-replica"></a>Salt okunurdur bir çoğaltmaya bağlanma

Bir veritabanı için okuma ölçeğini etkinleştirdiğinizde, `ApplicationIntent` istemci tarafından belirtilen bağlantı dizesindeki seçenek bağlantının yazma çoğaltmasına mı yoksa salt bir salt bir kopyaya mı yönlendirildiğini belirler. Özellikle, `ApplicationIntent` değer `ReadWrite` (varsayılan değer) ise, bağlantı veritabanının okuma/yazma çoğaltmasına yönlendirilir. Bu, varolan davranışla aynıdır. `ApplicationIntent`Değer ise `ReadOnly` , bağlantı salt okunurdur ve bir kopyaya yönlendirilir.

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

Aşağıdaki sorguyu çalıştırarak salt tanımlı bir çoğaltmaya bağlanıp bağlanmadığını doğrulayabilirsiniz. Salt okunurdur bir kopyaya bağlandığınızda READ_ONLY döndürülür.

```sql
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```

> [!NOTE]
> Belirli bir zamanda, yalnızca bir AlwaysON Çoğaltmalarından birine salt okunur oturumlar erişebilir.

## <a name="monitor-and-troubleshoot-a-read-only-replica"></a>Salt okuma çoğaltmasını izleme ve sorun giderme

Salt okunurdur bir çoğaltmaya bağlanıldığında, DMV kullanarak performans ölçümlerine erişebilirsiniz `sys.dm_db_resource_stats` . Sorgu planı istatistiklerine erişmek için, `sys.dm_exec_query_stats` `sys.dm_exec_query_plan` ve `sys.dm_exec_sql_text` DMVs 'yi kullanın.

> [!NOTE]
> `sys.resource_stats`Mantıksal ana veritabanındaki DMV, birincil ÇOĞALTMANıN CPU kullanımını ve depolama verilerini döndürür.

## <a name="enable-and-disable-read-scale-out"></a>Okuma ölçeğini etkinleştirme ve devre dışı bırakma

Okuma ölçeği genişletme, Premium, İş Açısından Kritik ve hiper ölçek hizmeti katmanlarında varsayılan olarak etkindir. Okuma ölçeği genişletme, temel, standart veya Genel Amaçlı Hizmet katmanlarında etkinleştirilemez. Okuma ölçeği, sıfır çoğaltmalarla yapılandırılmış hiper ölçekli veritabanlarında otomatik olarak devre dışıdır.

Aşağıdaki yöntemleri kullanarak, Premium veya İş Açısından Kritik hizmet katmanındaki tek veritabanlarında ve esnek havuz veritabanlarında okuma ölçeğini devre dışı bırakıp yeniden etkinleştirebilirsiniz.

> [!NOTE]
> Okuma ölçeğini devre dışı bırakma özelliği, geriye dönük uyumluluk için sağlanır.

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

## <a name="using-tempdb-on-a-read-only-replica"></a>Bir salt okuma çoğaltmasında TempDB kullanma

TempDB veritabanı salt okunurdur çoğaltmalara çoğaltılmaz. Her çoğaltmanın, çoğaltma oluşturulduğunda oluşturulan kendi TempDB veritabanı sürümü vardır. TempDB 'nin sorgu yürütme sırasında güncelleştirilmesini ve değiştirilmesini sağlar. Salt okuma iş yükünüz TempDB nesnelerini kullanmaya bağımlıysa, bu nesneleri sorgu betiğinizin bir parçası olarak oluşturmanız gerekir.

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Coğrafi olarak çoğaltılan veritabanları ile okuma ölçeğini kullanma

Coğrafi olarak çoğaltılan bir veritabanında (örneğin, bir yük devretme grubunun üyesi olarak) salt okuma iş yüklerini yük dengelemesi için okuma ölçeğini kullanıyorsanız, hem birincil hem de coğrafi olarak çoğaltılan ikincil veritabanlarında okuma ölçeği 'nin etkinleştirildiğinden emin olun. Bu yapılandırma, uygulamanız yük devretmeden sonra yeni birincil ağa bağlanırsa aynı yük dengeleme deneyiminin devam etmesini sağlar. 

Coğrafi olarak çoğaltılan ikincil veritabanına okuma ölçeğinde bir şekilde bağlanıyorsanız, ile olan oturumlarınız, `ApplicationIntent=ReadOnly` birincil veritabanında bağlantıları yönlendirdiğimiz şekilde çoğaltmalardan birine yönlendirilir.  Olmadan oturumlar, `ApplicationIntent=ReadOnly` coğrafi olarak çoğaltılan ikincil kopyanın birincil çoğaltmasına yönlendirilir ve bu da salt okunurdur. Coğrafi olarak çoğaltılan ikincil veritabanının birincil veritabanından farklı bir uç noktası olduğundan, geçmişte, ikinciye erişmek için gerekli değildir `ApplicationIntent=ReadOnly` . Geriye dönük uyumluluk sağlamak için, `sys.geo_replication_links` DMV `secondary_allow_connections=2` 'nin gösterdiği (istemci bağlantısına izin verilir).

> [!NOTE]
> Hepsini bir kez deneme veya ikincil veritabanının yerel çoğaltmaları arasındaki diğer yük dengeli yönlendirme desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

SQL veritabanı hiper ölçek teklifi hakkında daha fazla bilgi için bkz. [hyperscale hizmet katmanı](service-tier-hyperscale.md).
