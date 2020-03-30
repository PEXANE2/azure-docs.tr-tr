---
title: Elastik havuz kaynaklarını ölçeklendirme
description: Bu sayfada Azure SQL Veritabanı'ndaki esnek havuzlar için ölçekleme kaynakları açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
ms.date: 3/14/2019
ms.openlocfilehash: daca108cfc8bb2e5b2a068170a4a0244c72c9592
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77462607"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Azure SQL Veritabanı'nda esnek havuz kaynaklarını ölçeklendirin

Bu makalede, Azure SQL Veritabanı'nda elastik havuzlar ve havuzlu veritabanları için kullanılabilir bilgi işlem ve depolama kaynaklarının nasıl ölçeklendirilenanlatılmaktadır.

## <a name="change-compute-resources-vcores-or-dtus"></a>İşlem kaynaklarını değiştirme (vCores veya DTUs)

Başlangıçta vCores veya eDTUs sayısını seçerek sonra, [Azure portalı](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), veya [REST API](https://docs.microsoft.com/rest/api/sql/elasticpools/update)kullanarak gerçek deneyime dayalı dinamik bir yukarı veya aşağı elastik bir havuz ölçeklendirebilirsiniz.

### <a name="impact-of-changing-service-tier-or-rescaling-compute-size"></a>Hizmet katmanını değiştirme veya hesaplama boyutunu yeniden ölçekleme nin etkisi

Elastik bir havuzun hizmet katmanıveya bilgi işlem boyutunu değiştirmek, tek veritabanlarında olduğu gibi benzer bir deseni izler ve esas olarak aşağıdaki adımları gerçekleştiren hizmeti içerir:

1. Elastik havuz için yeni bilgi işlem örneği oluşturma  

    İstenen servis katmanı ve bilgi işlem boyutu yla elastik havuz için yeni bir bilgi işlem örneği oluşturulur. Bazı hizmet katmanı ve bilgi işlem boyutu değişiklikleri kombinasyonları için, her veritabanının bir kopyası, veri kopyalamayı içeren ve genel gecikmeyi güçlü bir şekilde etkileyebilen yeni bilgi işlem örneğinde oluşturulmalıdır. Ne olursa olsun, veritabanları bu adım sırasında çevrimiçi kalır ve bağlantılar özgün bilgi işlem örneğindeki veritabanlarına yönlendirilmeye devam eder.

2. Bağlantıların yönlendirmesini yeni işlem örneğine değiştirme

    Özgün işlem örneğindeki veritabanlarına varolan bağlantılar bırakılır. Yeni işlem örneğindeki veritabanlarına yeni bağlantılar kurulur. Bazı hizmet katmanı ve işlem boyutu değişiklikleri kombinasyonları için veritabanı dosyaları değiştirilir ve geçiş sırasında yeniden eklenir.  Ne olursa olsun, veritabanları genellikle 30 saniyeden kısa ve genellikle yalnızca birkaç saniye için kullanılamadığında anahtar kısa bir hizmet kesintisine neden olabilir. Bağlantılar bırakıldığında uzun süre çalışan hareketler varsa, iptal edilen hareketleri kurtarmak için bu adımın süresi daha uzun sürebilir. [Hızlandırılmış Veritabanı Kurtarma](sql-database-accelerated-database-recovery.md) uzun çalışan hareketleri iptal etkisini azaltabilir.

> [!IMPORTANT]
> İş akışındaki herhangi bir adımda veri kaybolmaz.

### <a name="latency-of-changing-service-tier-or-rescaling-compute-size"></a>Değişen hizmet katmanı veya yeniden ölçekleme hesaplama boyutunun gecikmesi

Hizmet katmanını değiştirmek veya tek bir veritabanının veya elastik havuzun işlem boyutunu yeniden ölçeklendirmek için tahmini gecikme sayısı aşağıdaki gibi parametrelendirilir:

|Hizmet katmanı|Temel tek veritabanı,</br>Standart (S0-S1)|Temel elastik havuz,</br>Standart (S2-S12), </br>Hiper ölçek, </br>Genel Amaçlı tek veritabanı veya elastik havuz|Premium veya Business Critical tek veritabanı veya esnek havuz|
|:---|:---|:---|:---|
|**Temel tek</br> veritabanı, Standart (S0-S1)**|&bull;&nbsp;Kullanılan alandan bağımsız sabit zaman gecikmesi</br>&bull;&nbsp;Genellikle, 5 dakikadan az|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|
|**Temel elastik </br>havuz, Standart (S2-S12), </br>Hiperölçek, </br>Genel Amaçlı tek veritabanı veya elastik havuz**|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|&bull;&nbsp;Kullanılan alandan bağımsız sabit zaman gecikmesi</br>&bull;&nbsp;Genellikle, 5 dakikadan az|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|
|**Premium veya Business Critical tek veritabanı veya esnek havuz**|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|&bull;&nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanıile orantılı gecikme</br>&bull;&nbsp;Genellikle, kullanılan alan GB başına 1 dakikadan az|

> [!NOTE]
>
> - Elastik bir havuz için servis katmanının değiştirilmesi veya yeniden hesaplanması durumunda, tahmini hesaplamak için havuzdaki tüm veritabanlarında kullanılan alanın toplamı kullanılmalıdır.
> - Bir veritabanının elastik bir havuza/elastik havuzdan taşınması durumunda, yalnızca veritabanı tarafından kullanılan alan, elastik havuz tarafından kullanılan alanı değil, gecikmeyi etkiler.
>
> [!TIP]
> Devam eden işlemleri izlemek için bkz: [SQL REST API'yi kullanarak işlemleri yönetin,](https://docs.microsoft.com/rest/api/sql/operations/list) [CLI kullanarak işlemleri yönetin,](/cli/azure/sql/db/op) [T-SQL'i kullanarak işlemleri izleyin, t-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) ve bu iki PowerShell komutunu kullanarak operasyonları izleyin: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) ve [Stop-AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

### <a name="additional-considerations-when-changing-service-tier-or-rescaling-compute-size"></a>Hizmet katmanını değiştirirken veya bilgi işlem boyutunu yeniden ölçekdeğiştirirken göz önünde bulundurulması gereken ek noktalar

- Elastik bir havuz için vCores veya eDTUs küçültüldüğünde, kullanılan havuz alanı hedef servis katmanı ve havuz eDTUs'larının izin verilen maksimum boyutundan daha küçük olmalıdır.
- Elastik bir havuz için eDO'ları yeniden ölçeklendirirken, (1) havuzun depolama maksimum boyutu hedef havuz tarafından desteklenirse ve (2) depolama max boyutu hedef havuzun dahil edilen depolama miktarını aşıyorsa, ek depolama maliyeti uygulanır. Örneğin, maksimum boyutu 100 GB olan 100 eDTU Standart havuz 50 eDTU Standart havuza küçültüldüyse, hedef havuz maksimum 100 GB boyutu desteklediği ve dahil edilen depolama miktarı yalnızca 50 GB olduğundan ek depolama maliyeti uygulanır. Yani, ekstra depolama miktarı 100 GB - 50 GB = 50 GB' dır. Ek depolama fiyatlandırması için [SQL Veritabanı fiyatlandırmasına](https://azure.microsoft.com/pricing/details/sql-database/)bakın. Kullanılan alan gerçek miktarı dahil depolama miktarından daha azsa, veritabanı nın maksimum boyutunu dahil edilen tutara indirgeyerek bu ekstra maliyetönlenebilir.

### <a name="billing-during-rescaling"></a>Yeniden ölçekleme sırasında faturalandırma

Kullanımdan veya veritabanının bir saatten kısa bir süre için etkin olup olmadığına bakılmaksızın, o saat içinde uygulanan en yüksek hizmet katmanı + bilgi işlem boyutu kullanılarak bir veritabanının var olduğu her saat için faturalandırılırsınız. Örneğin, tek bir veritabanı oluşturur ve beş dakika sonra silerseniz, faturanız bir veritabanı saati için bir ücret yansıtır.

## <a name="change-elastic-pool-storage-size"></a>Elastik havuz depolama boyutunu değiştirme

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri almak için bir veritabanını küçültmeniz gerekebilir. Daha fazla bilgi için Azure [SQL Veritabanı'nda dosya alanını yönet'e](sql-database-file-space-management.md)bakın.

### <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli

- Depolama, maksimum boyut sınırına kadar sağlanabilir:

  - Standart veya genel amaçlı hizmet katmanlarında depolama için, 10 GB'lık artışlarla boyutu artırın veya azaltın
  - Premium veya iş açısından kritik hizmet katmanlarında depolama için, 250 GB'lık artışlarla boyutu artırın veya azaltın
- Elastik bir havuz için depolama, maksimum boyutunu artırarak veya azaltarak sağlanabilir.
- Elastik bir havuz için depolama fiyatı, servis katmanının depolama birim fiyatıyla çarpımısonucu depolama miktarıdır. Ek depolama fiyatı yla ilgili ayrıntılar için [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/)bölümüne bakın.

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri almak için bir veritabanını küçültmeniz gerekebilir. Daha fazla bilgi için Azure [SQL Veritabanı'nda dosya alanını yönet'e](sql-database-file-space-management.md)bakın.

### <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

- Elastik bir havuz için eDTU fiyatı, ek ücret ödemeden belirli bir depolama miktarı içerir. Dahil edilen miktarın ötesinde ki ekstra depolama alanı, 250 GB'dan 1 TB'ye kadar artışlarla ve ardından 1 TB'nin ötesinde 256 GB'lık artışlarla maksimum boyut sınırına kadar ek bir maliyet için sağlanabilir. Dahil edilen depolama miktarları ve maksimum boyut sınırları için [elastik havuza bakın: depolama boyutları ve bilgi işlem boyutları.](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes)
- Elastik bir havuz için ekstra [depolama, Azure portalı](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update)veya [REST API'yi](https://docs.microsoft.com/rest/api/sql/elasticpools/update)kullanarak maksimum boyutunu artırarak sağlanabilir.
- Elastik bir havuz için ekstra depolama fiyatı, servis katmanının ekstra depolama birim fiyatıyla çarpılan ekstra depolama miktarıdır. Ek depolama fiyatı yla ilgili ayrıntılar için [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/)bölümüne bakın.

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri almak için bir veritabanını küçültmeniz gerekebilir. Daha fazla bilgi için Azure [SQL Veritabanı'nda dosya alanını yönet'e](sql-database-file-space-management.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Genel kaynak sınırları için, [SQL Database vCore tabanlı kaynak limitlerine - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md) ve SQL Veritabanı [DTU tabanlı kaynak sınırları - elastik havuzlar.](sql-database-dtu-resource-limits-elastic-pools.md)
