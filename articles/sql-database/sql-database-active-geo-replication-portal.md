---
title: 'Öğretici: Geo-çoğaltma portalda failover &'
description: Azure portalını kullanarak Azure SQL Veritabanı'nda tek veya havuza toplanan bir veritabanı için coğrafi çoğaltma yı yapılandırın ve başarısız olun.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 02/13/2019
ms.openlocfilehash: 59616fb217b28a8c47d9a5d13e2f4c1b9a8f6bb3
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605219"
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a>Azure portalında Azure SQL Veritabanı için etkin coğrafi çoğaltmayı yapılandırın ve başarısız

Bu makalede, [Azure portalını](https://portal.azure.com) kullanarak Azure SQL Veritabanı'ndaki [tek ve havuzlu veritabanları için etkin coğrafi çoğaltmayı](sql-database-active-geo-replication.md#active-geo-replication-terminology-and-capabilities) nasıl yapılandırabileceğinizi ve başarısız olmayı nasıl başlatabileceğinizi gösterir.

Tek ve havuzlu veritabanlarına sahip otomatik hata grupları hakkında bilgi için, [tek ve havuzlu veritabanlarına sahip failover gruplarını kullanmanın en iyi uygulamalarını](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools)görün. Yönetilen Örneklerle otomatik hata grupları hakkında bilgi için, [yönetilen örneklerle başarısız grupları kullanmanın en iyi uygulamaları](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-managed-instances)bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

Azure portalını kullanarak etkin coğrafi çoğaltmayı yapılandırmak için aşağıdaki kaynağa ihtiyacınız var:

* Azure SQL veritabanı: Farklı bir coğrafi bölgeye çoğaltmak istediğiniz birincil veritabanı.

> [!Note]
> Azure portalını kullanırken, yalnızca birincil abonelikle aynı abonelik içinde ikincil bir veritabanı oluşturabilirsiniz. İkincil veritabanının farklı bir abonelikte olması gerekiyorsa, [Veritabanı REST API'sını](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) veya [ALTER DATABASE Transact-SQL API'yi](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql)kullanın.

## <a name="add-a-secondary-database"></a>İkincil veritabanı ekleme

Aşağıdaki adımlar, bir coğrafi çoğaltma ortaklığında yeni bir ikincil veritabanı oluşturur.  

İkincil bir veritabanı eklemek için abonelik sahibi veya ortak sahibi olmalısınız.

İkincil veritabanı birincil veritabanıyla aynı ada sahiptir ve varsayılan olarak aynı hizmet katmanı ve işlem boyutuna sahiptir. İkincil veritabanı tek bir veritabanı veya havuzlu bir veritabanı olabilir. Daha fazla bilgi için [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [vCore tabanlı satın alma modeline](sql-database-service-tiers-vcore.md)bakın.
İkincil oluşturulduktan ve tohumlandıktan sonra, veriler birincil veritabanından yeni ikincil veritabanına çoğalmaya başlar.

> [!NOTE]
> Ortak veritabanı zaten varsa (örneğin, önceki bir coğrafi çoğaltma ilişkisini sonlandırma sonucu) komut başarısız olur.

1. Azure [portalında,](https://portal.azure.com)coğrafi çoğaltma için ayarlamak istediğiniz veritabanına göz atın.
2. SQL veritabanı **sayfasında, coğrafi çoğaltma'yı**seçin ve ardından ikincil veritabanını oluşturmak için bölgeyi seçin. Birincil veritabanını barındıran bölge dışında herhangi bir bölge seçebilirsiniz, ancak [eşleştirilmiş bölgeyi](../best-practices-availability-paired-regions.md)öneririz.

    ![Coğrafi çoğaltmayı yapılandırma](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. İkincil veritabanı için sunucu ve fiyatlandırma katmanını seçin veya yapılandırın.

    ![İkincil yapı](./media/sql-database-geo-replication-portal/create-secondary.png)
4. İsteğe bağlı olarak, elastik bir havuza ikincil bir veritabanı ekleyebilirsiniz. Havuzda ikincil veritabanı oluşturmak için **elastik havuzu** tıklatın ve hedef sunucuda bir havuz seçin. Hedef sunucuda zaten bir havuz bulunması gerekir. Bu iş akışı bir havuz oluşturmaz.
5. İkincil eklemek için **Oluştur'u** tıklatın.
6. İkincil veritabanı oluşturulur ve tohumlama işlemi başlar.

    ![İkincil yapı](./media/sql-database-geo-replication-portal/seeding0.png)
7. Tohumlama işlemi tamamlandığında, ikincil veritabanı durumunu görüntüler.

    ![Tohumlama tamamlandı](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Bir başarısızbaşlatma

İkincil veritabanı birincil olmak için değiştirilebilir.  

1. Azure [portalında,](https://portal.azure.com)coğrafi çoğaltma ortaklığındaki birincil veritabanına göz atın.
2. SQL Database bıçak üzerinde, **Tüm ayarları** > **coğrafi çoğaltma**seçin.
3. **SECONDARIES** listesinde, yeni birincil olmak istediğiniz veritabanını seçin ve **Zorla Failover'ı**tıklatın.

    ![Başarısızlık](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Başarısız olmaya başlamak için **Evet'i** tıklatın.

Komut hemen ikincil veritabanını birincil role dönüştürür. Bu işlem normalde 30 sn veya daha az içinde tamamlanmalıdır.

Roller değiştirilirken her iki veritabanının da kullanılamadığı (0 ile 25 saniye arasında) kısa bir süre vardır. Birincil veritabanında birden çok ikincil veritabanı varsa, komut diğer ikincil verileri yeni birincil bağlanmak için otomatik olarak yeniden yapılandırır. Tüm operasyonun normal şartlar altında tamamlanması bir dakikadan az sürer.

> [!NOTE]
> Bu komut, bir kesinti durumunda veritabanının hızlı bir şekilde kurtarılması için tasarlanmıştır. Veri senkronizasyonu (zorla başarısız) olmadan başarısızolmasını tetikler.  Birincil çevrimiçi yse ve komut verildiğinde işlem gerçekleştiriyorsa bazı veri kaybı oluşabilir.

## <a name="remove-secondary-database"></a>İkincil veritabanını kaldırma

Bu işlem, ikincil veritabanına çoğaltmayı kalıcı olarak sonlandırır ve ikincil veritabanının rolünü normal bir okuma-yazma veritabanına değiştirir. İkincil veritabanına bağlantı bozulursa, komut başarılı olur, ancak bağlantı geri yüklenene kadar ikincil okuma yazma olmaz.  

1. Azure [portalında,](https://portal.azure.com)coğrafi çoğaltma ortaklığındaki birincil veritabanına göz atın.
2. SQL veritabanı **sayfasında, coğrafi çoğaltma'yı**seçin.
3. **SECONDARIES** listesinde, coğrafi çoğaltma ortaklığından kaldırmak istediğiniz veritabanını seçin.
4. **Çoğaltmayı Durdur'u**tıklatın.

    ![İkincil kaldırma](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Onay penceresi açılır. Veritabanını coğrafi çoğaltma ortaklığından kaldırmak için **Evet'i** tıklatın. (Herhangi bir çoğaltma parçası olmayan bir okuma-yazma veritabanına ayarlayın.)

## <a name="next-steps"></a>Sonraki adımlar

* Etkin coğrafi çoğaltma hakkında daha fazla bilgi edinmek için [etkin coğrafi çoğaltma'ya](sql-database-active-geo-replication.md)bakın.
* Otomatik hata grupları hakkında bilgi edinmek için Bkz. [Otomatik hata grupları](sql-database-auto-failover-group.md)
* İş sürekliliğine genel bakış ve senaryolar için [İş sürekliliğine genel bakış](sql-database-business-continuity.md)alabakın.
