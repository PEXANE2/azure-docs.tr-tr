---
title: Tek veritabanı kaynaklarını ölçeklendirme
description: Bu makalede, Azure SQL Veritabanı'nda tek bir veritabanı için kullanılabilir işlem ve depolama kaynaklarının nasıl ölçeklendirilenaçıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/10/2020
ms.openlocfilehash: 84846e642fa102045b89eb12dbc85b0995867a3e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061592"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Azure SQL Veritabanı'nda tek veritabanı kaynaklarını ölçeklendirin

Bu makalede, sağlanan işlem katmanında bir Azure SQL Veritabanı için kullanılabilir işlem ve depolama kaynaklarının nasıl ölçeklendirilenanlatılmaktadır. Alternatif olarak, [sunucusuz bilgi işlem katmanı](sql-database-serverless.md) kullanılan işlem için saniyede otomatik ölçekleme ve faturalar sağlar.

Başlangıçta vCores veya DT'lerin sayısını seçerek sonra, [azure portalı](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update)veya [REST API](https://docs.microsoft.com/rest/api/sql/databases/update)kullanarak gerçek deneyime dayalı dinamik olarak tek bir veritabanı ölçeklendirebilirsiniz.

Aşağıdaki video, tek bir veritabanı için kullanılabilir DUS'ları artırmak için hizmet katmanını ve bilgi işlem boyutunu dinamik olarak değiştirdiğini gösterir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri almak için bir veritabanını küçültmeniz gerekebilir. Daha fazla bilgi için Azure [SQL Veritabanı'nda dosya alanını yönet'e](sql-database-file-space-management.md)bakın.

## <a name="impact"></a>Etki

Hizmet katmanının veya işlem boyutunun değiştirilmesi esas olarak aşağıdaki adımları gerçekleştiren hizmeti içerir:

1. Veritabanı için yeni işlem örneği oluşturma  

    İstenen hizmet katmanı ve bilgi işlem boyutuyla yeni bir bilgi işlem örneği oluşturulur. Bazı hizmet katmanı ve bilgi işlem boyutu değişiklikleri kombinasyonları için, veri kopyalamayı içeren ve genel gecikmeyi güçlü bir şekilde etkileyebilen yeni bilgi işlem örneğinde veritabanının bir kopyası oluşturulmalıdır. Ne olursa olsun, veritabanı bu adım sırasında çevrimiçi kalır ve bağlantılar özgün bilgi işlem örneğinde veritabanına yönlendirilmeye devam eder.

2. Bağlantıların yönlendirmesini yeni işlem örneğine değiştirme

    Özgün işlem örneğinde veritabanına varolan bağlantılar bırakılır. Yeni işlem örneğinde veritabanına yeni bağlantılar kurulur. Bazı hizmet katmanı ve işlem boyutu değişiklikleri kombinasyonları için veritabanı dosyaları değiştirilir ve geçiş sırasında yeniden eklenir.  Ne olursa olsun, veritabanı genellikle 30 saniyeden kısa ve genellikle yalnızca birkaç saniye için kullanılamadığında anahtar kısa bir hizmet kesintisine neden olabilir. Bağlantılar bırakıldığında uzun süre çalışan hareketler varsa, iptal edilen hareketleri kurtarmak için bu adımın süresi daha uzun sürebilir. [Hızlandırılmış Veritabanı Kurtarma](sql-database-accelerated-database-recovery.md) uzun çalışan hareketleri iptal etkisini azaltabilir.

> [!IMPORTANT]
> İş akışındaki herhangi bir adımda veri kaybolmaz. Hizmet katmanı değiştirilirken Azure SQL Veritabanı'nı kullanan uygulamalarda ve bileşenlerde bazı [yeniden deneme mantığı](sql-database-connectivity-issues.md) uyguladığınıza emin olun.

## <a name="latency"></a>Gecikme süresi 

Hizmet katmanını değiştirmek veya tek bir veritabanının veya elastik havuzun işlem boyutunu yeniden ölçeklendirmek için tahmini gecikme sayısı aşağıdaki gibi parametrelendirilir:

|Hizmet katmanı|Temel tek veritabanı,</br>Standart (S0-S1)|Temel elastik havuz,</br>Standart (S2-S12), </br>Hiper ölçek, </br>Genel Amaçlı tek veritabanı veya elastik havuz|Premium veya Business Critical tek veritabanı veya esnek havuz|
|:---|:---|:---|:---|
|**Temel tek</br> veritabanı, Standart (S0-S1)**|&bull;&nbsp;Kullanılan alandan bağımsız sabit zaman gecikmesi</br>&bull;&nbsp;Genellikle, 5 dakikadan az|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|
|**Temel elastik </br>havuz, Standart (S2-S12), </br>Hiperölçek, </br>Genel Amaçlı tek veritabanı veya elastik havuz**|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|&bull;&nbsp;Kullanılan alandan bağımsız sabit zaman gecikmesi</br>&bull;&nbsp;Genellikle, 5 dakikadan az|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|
|**Premium veya Business Critical tek veritabanı veya esnek havuz**|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|

> [!TIP]
> Devam eden işlemleri izlemek için bkz: [SQL REST API'yi kullanarak işlemleri yönetin,](https://docs.microsoft.com/rest/api/sql/operations/list) [CLI kullanarak işlemleri yönetin,](/cli/azure/sql/db/op) [T-SQL'i kullanarak işlemleri izleyin, t-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) ve bu iki PowerShell komutunu kullanarak operasyonları izleyin: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) ve [Stop-AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

## <a name="cancelling-changes"></a>Değişiklikleri iptal etme

Bir hizmet katmanı değişikliği veya hesaplama rescaling işlemi iptal edilebilir.

#### <a name="azure-portal"></a>Azure portalında

Veritabanına genel bakış bıçağında, **Bildirimler'e** gidin ve devam eden bir işlem olduğunu belirten döşemeyi tıklatın:

![Devam eden operasyon](media/sql-database-single-database-scale/ongoing-operations.png)

Ardından, **bu işlemi iptal**et etiketine tıklayın.

![Devam eden işlemi iptal etme](media/sql-database-single-database-scale/cancel-ongoing-operation.png)

#### <a name="powershell"></a>PowerShell

PowerShell komut isteminden `$resourceGroupName`, `$serverName`, `$databaseName`ve , 'yi ayarlayın ve ardından aşağıdaki komutu çalıştırın:

```azurecli
$operationName = (az sql db op list --resource-group $resourceGroupName --server $serverName --database $databaseName --query "[?state=='InProgress'].name" --out tsv)
if (-not [string]::IsNullOrEmpty($operationName)) {
    (az sql db op cancel --resource-group $resourceGroupName --server $serverName --database $databaseName --name $operationName)
        "Operation " + $operationName + " has been canceled"
}
else {
    "No service tier change or compute rescaling operation found"
}
```

## <a name="additional-considerations"></a>Diğer konular

- Daha yüksek bir hizmet katmanına veya bilgi işlem boyutuna yükseltiyorsanız, daha büyük bir boyutu (maxsize) açıkça belirtmediğiniz sürece veritabanı nın maksimum boyutu artmaz.
- Veritabanını düşürmek için veritabanı kullanılan alanın, hedef hizmet katmanı ve işlem boyutu için izin verilen maksimum boyuttan daha küçük olması gerekir.
- **Premium'dan** **Standart** katmana düşürüldüğünde, her ikisi de (1) veritabanının maksimum boyutu hedef işlem boyutunda desteklenirse ve (2) maksimum boyut hedef işlem boyutuna dahil edilen depolama miktarını aşıyorsa, ekstra bir depolama maliyeti uygulanır. Örneğin, maksimum boyutu 500 GB olan bir P1 veritabanı S3'e küçültülürse, S3 maksimum 1 TB boyutu desteklediği ve dahil edilen depolama miktarı yalnızca 250 GB olduğundan ek depolama maliyeti uygulanır. Yani, ekstra depolama miktarı 500 GB - 250 GB = 250 GB. Ek depolama fiyatlandırması için [SQL Veritabanı fiyatlandırmasına](https://azure.microsoft.com/pricing/details/sql-database/)bakın. Kullanılan alan gerçek miktarı dahil depolama miktarından daha azsa, veritabanı nın maksimum boyutunu dahil edilen tutara indirgeyerek bu ekstra maliyetönlenebilir.
- Bir veritabanını coğrafi [çoğaltma](sql-database-geo-replication-portal.md) etkinleştirilmiş olarak yükseltirken, birincil veritabanını yükseltmeden önce ikincil veritabanlarını istenen hizmet katmanına ve işlem boyutuna yükseltin (en iyi performans için genel kılavuz). Farklı bir baskıya yükseltilirken, önce ikincil veritabanının yükseltilmesi gereksinimidir.
- Bir veritabanını coğrafi [çoğaltma](sql-database-geo-replication-portal.md) etkinleştirilmiş olarak küçültterken, ikincil veritabanını (en iyi performans için genel kılavuz) düşürmeden önce birincil veritabanlarını istenen hizmet katmanına ve işlem boyutuna düşürün. Farklı bir baskıya indirgendiğinde, önce birincil veritabanının indirgenmiş olması gereksinimidir.
- Geri yükleme hizmeti teklifleri, çeşitli hizmet katmanları için farklılık gösterir. **Temel** katmana indiriyorsanız, daha düşük bir yedekleme bekletme süresi vardır. Bkz. [Azure SQL Veritabanı Yedeklemeleri.](sql-database-automated-backups.md)
- Veritabanının yeni özellikleri, değişiklikler tamamlanana kadar uygulanmaz.

## <a name="billing"></a>Faturalandırma 

Kullanımdan veya veritabanının bir saatten kısa bir süre için etkin olup olmadığına bakılmaksızın, o saat içinde uygulanan en yüksek hizmet katmanı + bilgi işlem boyutu kullanılarak bir veritabanının var olduğu her saat için faturalandırılırsınız. Örneğin, tek bir veritabanı oluşturur ve beş dakika sonra silerseniz, faturanız bir veritabanı saati için bir ücret yansıtır.

## <a name="change-storage-size"></a>Depolama boyutunu değiştirme

### <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli

- Depolama, 1 GB'lık artışlar kullanılarak veri depolama maksimum boyut sınırına kadar sağlanabilir. En az yapılandırılabilir veri depolama 1 GB'dır. Her hizmet hedefinde veri depolama max boyut sınırları için tek veritabanları ve elastik [havuzlar](sql-database-vcore-resource-limits-single-databases.md) için kaynak sınırı belgeleme [sayfalarına](sql-database-vcore-resource-limits-elastic-pools.md) bakın.
- Tek bir veritabanı için veri depolama, [Azure portalı](https://portal.azure.com), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update)veya [REST API'yi](https://docs.microsoft.com/rest/api/sql/databases/update)kullanarak maksimum boyutunu artırarak veya azaltarak sağlanabilir. Maksimum boyut değeri baytcinsinden belirtilirse, 1 GB'ın (1073741824 bayt) katı olmalıdır.
- Bir veritabanının veri dosyalarında depolanabilecek veri miktarı, yapılandırılan veri depolama max boyutuyla sınırlıdır. Bu depolamaya ek olarak, SQL Veritabanı işlem günlüğü için kullanılmak üzere otomatik olarak %30 daha fazla depolama alanı ayırır.
- SQL Database, `tempdb` vCore başına 32 GB'ı veritabanı için otomatik olarak ayırır. `tempdb`tüm hizmet katmanlarındaki yerel SSD depolama alanında yer alır.
- Tek bir veritabanı veya elastik bir havuz için depolama fiyatı, hizmet katmanının depolama birimi fiyatıyla çarpılarak veri depolama ve işlem günlüğü depolama tutarlarının toplamıdır. Maliyeti `tempdb` fiyata dahildir. Depolama fiyatı yla ilgili ayrıntılar için [SQL Veritabanı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/sql-database/)bakın.

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri almak için bir veritabanını küçültmeniz gerekebilir. Daha fazla bilgi için Azure [SQL Veritabanı'nda dosya alanını yönet'e](sql-database-file-space-management.md)bakın.

### <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

- Tek bir veritabanı için DTU fiyatı, ek ücret ödemeden belirli bir depolama miktarı içerir. Dahil edilen miktarın ötesinde ki ekstra depolama alanı, 250 GB'dan 1 TB'ye kadar artışlarla ve ardından 1 TB'nin ötesinde 256 GB'lık artışlarla maksimum boyut sınırına kadar ek bir maliyet için sağlanabilir. Dahil edilen depolama tutarları ve maksimum boyut sınırları için [bkz: Tek veritabanı: Depolama boyutları ve işlem boyutları.](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes)
- Azure portalı, [Transact-SQL,](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1) [PowerShell,](/powershell/module/az.sql/set-azsqldatabase) [Azure CLI](/cli/azure/sql/db#az-sql-db-update)veya [REST API'yi](https://docs.microsoft.com/rest/api/sql/databases/update)kullanarak maksimum boyutunu artırarak tek bir veritabanı için ekstra depolama alanı sağlanabilir.
- Tek bir veritabanı için ekstra depolama fiyatı, hizmet katmanının ekstra depolama birim fiyatıyla çarpılan ekstra depolama tutarıdır. Ek depolama fiyatı yla ilgili ayrıntılar için [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/)bölümüne bakın.

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri almak için bir veritabanını küçültmeniz gerekebilir. Daha fazla bilgi için Azure [SQL Veritabanı'nda dosya alanını yönet'e](sql-database-file-space-management.md)bakın.

### <a name="geo-replicated-database"></a>Coğrafi olarak çoğaltılmış veritabanı

Çoğaltılan ikincil veritabanının veritabanı boyutunu değiştirmek için birincil veritabanının boyutunu değiştirin. Bu değişiklik daha sonra çoğaltılır ve ikincil veritabanında da uygulanır. 

## <a name="p11-and-p15-constraints-when-max-size-greater-than-1-tb"></a>Maksimum boyutu 1 TB'den büyük olduğunda P11 ve P15 kısıtlamaları

Premium katmanında 1 TB'den fazla depolama şu anda Çin Doğu, Çin Kuzey, Almanya Orta, Almanya Kuzeydoğu, Batı Orta ABD, ABD DoD bölgeleri ve ABD Hükümeti Merkezi hariç tüm bölgelerde kullanılabilir. Bu bölgelerde, Premium katmandaki depolama limiti 1 TB ile sınırlıdır. En fazla boyutu 1 TB'den büyük olan P11 ve P15 veritabanları için aşağıdaki hususlar ve sınırlamalar geçerlidir:

- Bir P11 veya P15 veritabanının maksimum boyutu 1 TB'den büyük bir değere ayarlanmışsa, yalnızca p11 veya P15 veritabanına geri yüklenebilir veya kopyalanabilir.  Daha sonra, yeniden ölçeklendirme işlemi sırasında ayrılan alan miktarı yeni bilgi işlem boyutunun maksimum boyut sınırlarını aşmaması koşuluyla veritabanı farklı bir bilgi işlem boyutuna yeniden ölçeklenebilir.
- Etkin coğrafi çoğaltma senaryoları için:
  - Coğrafi çoğaltma ilişkisi kurma: Birincil veritabanı P11 veya P15 ise, ikincil veritabanı da P11 veya P15 olmalıdır; 1 TB'den fazla destek yeteneğine sahip olmadıklarından, daha düşük işlem boyutu ikinci harf olarak reddedilir.
  - Bir coğrafi çoğaltma ilişkisinde birincil veritabanını yükseltme: Birincil veritabanında en büyük boyutu 1 TB'den fazlasına değiştirmek ikincil veritabanında aynı değişikliği tetikler. Birincil değişikliğin etkili olabilmesi için her iki yükseltmenin de başarılı olması gerekir. 1-TB'den fazla seçenek için bölge sınırlamaları geçerlidir. İkincil, 1 TB'den fazla desteği olmayan bir bölgedeyse, birincil yükseltilmez.
- P11/P15 veritabanlarını 1 TB'den fazla yükleme için Alma/Dışa Aktarma hizmetinin kullanılması desteklenmez. Verileri [almak](sql-database-import.md) ve [aktarmak](sql-database-export.md) için SqlPackage.exe'yi kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Genel kaynak sınırları için, [SQL Database vCore tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve SQL Veritabanı [DTU tabanlı kaynak sınırları - elastik havuzları](sql-database-dtu-resource-limits-single-databases.md)bakın.
