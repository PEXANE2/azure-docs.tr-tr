---
title: Kaynakları başka bir bölgeye taşıma
description: Azure SQL Veritabanınızı, Azure SQL elastik havuzunuzu veya Azure SQL yönetilen örneğini başka bir bölgeye nasıl taşıyabilirsiniz öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 06/25/2019
ms.openlocfilehash: 851ef49a5c066f12a95baa54daf5e267cb4278c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73821425"
---
# <a name="how-to-move-azure-sql-resources-to-another-region"></a>Azure SQL kaynaklarını başka bir bölgeye taşıma

Bu makalede, Azure SQL Veritabanı tek veritabanı, elastik havuz ve yönetilen örnek yeni bir bölgeye taşımak için nasıl genel bir iş akışı öğretir. 

## <a name="overview"></a>Genel Bakış

Varolan Azure SQL kaynaklarınızı bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, işletmenizi yeni bir bölgeye genişletir siniz ve yeni müşteri tabanı için optimize etmek istersiniz. Veya uyumluluk nedenleriyle işlemleri farklı bir bölgeye taşımanız gerekir. Veya Azure, daha iyi bir yakınlık sağlayan ve müşteri deneyimini geliştiren yepyeni bir bölge yayımladı.  

Bu makalede, kaynakları farklı bir bölgeye taşımak için genel bir iş akışı sağlar. İş akışı aşağıdaki adımlardan oluşur: 

- Taşıma için ön koşulları doğrulayın 
- Kaynakları kapsamda taşımaya hazırlanın
- Hazırlık sürecini izleme
- Taşıma işlemini test edin
- Gerçek hareketi başlatma 
- Kaynakları kaynak bölgeden kaldırma 


> [!NOTE]
> Bu makale, Azure genel bulutu içindeki veya aynı egemen bulut içindeki geçişler için geçerlidir. 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="move-single-database"></a>Tek bir veritabanını taşıma

### <a name="verify-prerequisites"></a>Önkoşulları doğrulama 

1. Her kaynak sunucu için bir hedef mantıksal sunucu oluşturun. 
1. [PowerShell'i](scripts/sql-database-create-and-configure-database-powershell.md)kullanarak güvenlik duvarını doğru özel durumlar dışında yapılandırın.  
1. Mantıksal sunucuları doğru oturum açmalarla yapılandırın. Abonelik yöneticisi veya SQL sunucu yöneticisi değilseniz, ihtiyacınız olan izinleri atamak için yöneticiyle birlikte çalışın. Daha fazla bilgi için, [olağanüstü durum kurtarma sonrasında Azure SQL veritabanı güvenliğini nasıl yönetebilirsiniz'e](sql-database-geo-replication-security-config.md)bakın. 
1. Veritabanlarınız TDE ile şifrelenmişse ve Azure anahtar kasasında kendi şifreleme anahtarınızı kullanıyorsa, doğru şifreleme materyalinin hedef bölgelerde sağlanmıştır ğından emin olun. Daha fazla bilgi için Azure [Anahtar Kasası'nda müşteri tarafından yönetilen anahtarlarla Azure SQL Saydam Veri Şifrelemesi'ne](transparent-data-encryption-byok-azure-sql.md) bakın
1. Veritabanı düzeyinde denetim etkinleştirilmişse, denetimi devre dışı kaldırın ve bunun yerine sunucu düzeyinde denetimi etkinleştirin. Başarısız olduktan sonra, veritabanı düzeyinde denetim, hareketten sonra istenmeyen veya mümkün olacak bölgeler arası trafiği gerektirir. 
1. Sunucu düzeyinde denetimler için şunları
   - Depolama kapsayıcısı, Log Analytics veya etkinlik hub'ı varolan denetim günlüklerine sahip hedef bölgeye taşınır. 
   - Denetim hedef sunucuda yapılandırılır. Daha fazla bilgi için [bkz.](sql-database-auditing.md) 
1. Örneğinizin uzun vadeli bir bekletme ilkesi (LTR) varsa, varolan LTR yedeklemeleri geçerli sunucuyla ilişkili kalır. Hedef sunucu farklı olduğundan, sunucu silinse bile kaynak sunucuyu kullanarak kaynak bölgedeki eski LTR yedeklemelerine erişebilirsiniz. 

  > [!NOTE]
  > Bu, egemen bulut ile halka açık bir bölge arasında hareket etmek için yeterli olmayacaktır. Böyle bir geçiş, LTR yedeklemelerinin şu anda desteklenmeyen hedef sunucuya taşınmasını gerektirir. 

### <a name="prepare-resources"></a>Kaynakları hazırlama

1. Kaynağın mantıksal sunucusu ile hedefin mantıksal sunucusu arasında bir [başarısızlık grubu](sql-database-single-database-failover-group-tutorial.md#2---create-the-failover-group) oluşturun.  
1. Failover grubuna taşımak istediğiniz veritabanlarını ekleyin. 
    - Eklenen tüm veritabanlarının çoğaltılması otomatik olarak başlatılır. Daha fazla bilgi için, [tek veritabanları ile failover grupları kullanmak için en iyi uygulamalar](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools)bakın. 
 
### <a name="monitor-the-preparation-process"></a>Hazırlık sürecini izleme

Kaynaktan hedefe veritabanlarınızın çoğaltılmasını izlemek için düzenli olarak [Get-AzSqlDatabaseFailoverGroup'u](/powershell/module/az.sql/get-azsqldatabasefailovergroup) arayabilirsiniz. Çoğaltma Durumu `Get-AzSqlDatabaseFailoverGroup` için bir özellik içeren çıktı **nesnesi:** 
   - **ReplicationState = 2** (CATCH_UP) veritabanının eşitlenmiş olduğunu ve güvenli bir şekilde başarısız olabileceğini gösterir. 
   - **ReplicationState = 0** (SEEDING) veritabanının henüz tohumlanmadığını ve başarısız bir girişimin başarısız olacağını gösterir. 

### <a name="test-synchronization"></a>Test eşitleme

Bir kez `2` **ReplicationState,** her veritabanına bağlanmak veya ikincil bitiş `<fog-name>.secondary.database.windows.net` noktası kullanarak veritabanları alt kümesi ve bağlantı, uygun güvenlik yapılandırması ve veri çoğaltma sağlamak için veritabanlarına karşı herhangi bir sorgu gerçekleştirin. 

### <a name="initiate-the-move"></a>Hareketi başlatma

1. İkincil bitiş noktasını `<fog-name>.secondary.database.windows.net`kullanarak hedef sunucuya bağlanın.
1. [Switch-AzSqlDatabaseFailoverGroup'u](/powershell/module/az.sql/switch-azsqldatabasefailovergroup) kullanarak ikincil yönetilen örneği tam senkronizasyona sahip birincil örnek olarak değiştirin. Bu işlem ya başarılı olur ya da geri döner. 
1. DNS CNAME giriş noktalarının hedef bölge IP adresine geldiğini tespit etmek için komutun `nslook up <fog-name>.secondary.database.windows.net` başarıyla tamamlandığını doğrulayın. Anahtar komutu başarısız olursa, CNAME güncelleştirilmeyecek. 

### <a name="remove-the-source-databases"></a>Kaynak veritabanlarını kaldırma

Taşıma tamamlandıktan sonra, gereksiz ücretleri önlemek için kaynak bölgedeki kaynakları kaldırın. 

1. [Remove-AzSqlDatabaseFailoverGroup'u](/powershell/module/az.sql/remove-azsqldatabasefailovergroup)kullanarak failover grubunu silin. 
1. Kaynak sunucudaki veritabanlarının her biri için [Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase) kullanarak her kaynak veritabanını silin. Bu, coğrafi çoğaltma bağlantılarını otomatik olarak sonlandırır. 
1. [Remove-AzSqlServer](/powershell/module/az.sql/remove-azsqlserver)kullanarak kaynak sunucuyu silin. 
1. Anahtar kasasını, denetim depolama kapsayıcılarını, olay merkezini, AAD örneğini ve diğer bağımlı kaynakları kaldırın ve bunlar için faturalandırılmayı durdurun. 

## <a name="move-elastic-pools"></a>Elastik havuzları hareket ettirin

### <a name="verify-prerequisites"></a>Önkoşulları doğrulama 

1. Her kaynak sunucu için bir hedef mantıksal sunucu oluşturun. 
1. [PowerShell'i](scripts/sql-database-create-and-configure-database-powershell.md)kullanarak güvenlik duvarını doğru özel durumlar dışında yapılandırın. 
1. Mantıksal sunucuları doğru oturum açmalarla yapılandırın. Abonelik yöneticisi veya SQL sunucu yöneticisi değilseniz, ihtiyacınız olan izinleri atamak için yöneticiyle birlikte çalışın. Daha fazla bilgi için, [olağanüstü durum kurtarma sonrasında Azure SQL veritabanı güvenliğini nasıl yönetebilirsiniz'e](sql-database-geo-replication-security-config.md)bakın. 
1. Veritabanlarınız TDE ile şifrelenmişse ve Azure anahtar kasasında kendi şifreleme anahtarınızı kullanıyorsa, doğru şifreleme materyalinin hedef bölgede sağlanmıştırğından emin olun.
1. Her kaynak elastik havuz için bir hedef elastik havuz oluşturun, havuzun aynı hizmet katmanında, aynı ada ve aynı boyutta oluşturulduğundan emin olun. 
1. Veritabanı düzeyinde bir denetim etkinleştirilmişse, denetimi devre dışı kaldırın ve bunun yerine sunucu düzeyinde denetimi etkinleştirin. Başarısız olduktan sonra, veritabanı düzeyinde denetim, istenmeyen veya taşımadan sonra mümkün olan bölgeler arası trafik gerektirir. 
1. Sunucu düzeyinde denetimler için şunları
    - Depolama kapsayıcısı, Log Analytics veya etkinlik hub'ı varolan denetim günlüklerine sahip hedef bölgeye taşınır.
    - Denetim yapılandırması hedef sunucuda yapılandırılır. Daha fazla bilgi için [SQL veritabanı denetimine](sql-database-auditing.md)bakın.
1. Örneğinizin uzun vadeli bir bekletme ilkesi (LTR) varsa, varolan LTR yedeklemeleri geçerli sunucuyla ilişkili kalır. Hedef sunucu farklı olduğundan, sunucu silinse bile kaynak sunucuyu kullanarak kaynak bölgedeki eski LTR yedeklemelerine erişebilirsiniz. 

  > [!NOTE]
  > Bu, egemen bulut ile halka açık bir bölge arasında hareket etmek için yeterli olmayacaktır. Böyle bir geçiş, LTR yedeklemelerinin şu anda desteklenmeyen hedef sunucuya taşınmasını gerektirir. 

### <a name="prepare-to-move"></a>Hareket etmeye hazırlanın
 
1.  Kaynak mantıksal sunucudaki her elastik havuz ile hedef sunucudaki karşıt elastik havuzu arasında ayrı bir [hata grubu](sql-database-elastic-pool-failover-group-tutorial.md#3---create-the-failover-group) oluşturun. 
1.  Havuzdaki tüm veritabanlarını failover grubuna ekleyin. 
    - Eklenen veritabanlarının çoğaltılması otomatik olarak başlatılır. Daha fazla bilgi [için, elastik havuzları olan başarısız gruplar için en iyi uygulamalara](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools)bakın. 

  > [!NOTE]
  > Birden çok elastik havuz içeren bir başarısız grup oluşturmak mümkün olsa da, her havuz için ayrı bir başarısızlık grubu oluşturmanızı şiddetle öneririz. Taşımanız gereken birden çok elastik havuzda çok sayıda veritabanınız varsa, hazırlama adımlarını paralel olarak çalıştırabilir ve ardından taşıma adımını paralel olarak başlatabilirsiniz. Bu süreç daha iyi ölçeklendirilecek ve aynı failover grubunda birden fazla elastik havuza sahip olmak la karşılaştırıldığında daha az zaman alacaktır. 

### <a name="monitor-the-preparation-process"></a>Hazırlık sürecini izleme

Kaynaktan hedefe veritabanlarınızın çoğaltılmasını izlemek için düzenli olarak [Get-AzSqlDatabaseFailoverGroup'u](/powershell/module/az.sql/get-azsqldatabasefailovergroup) arayabilirsiniz. Çoğaltma Durumu `Get-AzSqlDatabaseFailoverGroup` için bir özellik içeren çıktı **nesnesi:** 
   - **ReplicationState = 2** (CATCH_UP) veritabanının eşitlenmiş olduğunu ve güvenli bir şekilde başarısız olabileceğini gösterir. 
   - **ReplicationState = 0** (SEEDING) veritabanının henüz tohumlanmadığını ve başarısız bir girişimin başarısız olacağını gösterir. 

### <a name="test-synchronization"></a>Test eşitleme
 
Bir kez `2` **ReplicationState,** her veritabanına bağlanmak veya ikincil bitiş `<fog-name>.secondary.database.windows.net` noktası kullanarak veritabanları alt kümesi ve bağlantı, uygun güvenlik yapılandırması ve veri çoğaltma sağlamak için veritabanlarına karşı herhangi bir sorgu gerçekleştirin. 

### <a name="initiate-the-move"></a>Hareketi başlatma
 
1. İkincil bitiş noktasını `<fog-name>.secondary.database.windows.net`kullanarak hedef sunucuya bağlanın.
1. [Switch-AzSqlDatabaseFailoverGroup'u](/powershell/module/az.sql/switch-azsqldatabasefailovergroup) kullanarak ikincil yönetilen örneği tam senkronizasyona sahip birincil örnek olarak değiştirin. Bu işlem ya başarılı olur ya da geri döner. 
1. DNS CNAME giriş noktalarının hedef bölge IP adresine geldiğini tespit etmek için komutun `nslook up <fog-name>.secondary.database.windows.net` başarıyla tamamlandığını doğrulayın. Anahtar komutu başarısız olursa, CNAME güncelleştirilmeyecek. 

### <a name="remove-the-source-elastic-pools"></a>Kaynak elastik havuzları kaldırın
 
Taşıma tamamlandıktan sonra, gereksiz ücretleri önlemek için kaynak bölgedeki kaynakları kaldırın. 

1. [Remove-AzSqlDatabaseFailoverGroup'u](/powershell/module/az.sql/remove-azsqldatabasefailovergroup)kullanarak failover grubunu silin.
1. [Remove-AzSqlElasticPool](/powershell/module/az.sql/remove-azsqlelasticpool)kullanarak kaynak sunucudaki her kaynak elastik havuzu silin. 
1. [Remove-AzSqlServer](/powershell/module/az.sql/remove-azsqlserver)kullanarak kaynak sunucuyu silin. 
1. Anahtar kasasını, denetim depolama kapsayıcılarını, olay merkezini, AAD örneğini ve diğer bağımlı kaynakları kaldırın ve bunlar için faturalandırılmayı durdurun. 

## <a name="move-managed-instance"></a>Yönetilen örneği taşıma

### <a name="verify-prerequisites"></a>Önkoşulları doğrulama
 
1. Yönetilen her kaynak örneği için hedef bölgede aynı boyutta bir hedef yönetilen örnek oluşturun.  
1. Yönetilen bir örnek için ağı yapılandırın. Daha fazla bilgi için [ağ yapılandırması'na](sql-database-howto-managed-instance.md#network-configuration)bakın.
1. Hedef ana veritabanını doğru oturum açmalarla yapılandırın. Abonelik yöneticisi veya SQL sunucu yöneticisi değilseniz, ihtiyacınız olan izinleri atamak için yöneticiyle birlikte çalışın. 
1. Veritabanlarınız TDE ile şifrelenmişse ve Azure anahtar kasasında kendi şifreleme anahtarınızı kullanıyorsa, aynı şifreleme anahtarlarına sahip AKV'nin hem kaynak hem de hedef bölgelerde bulunduğundan emin olun. Daha fazla bilgi için [Azure Key Vault'ta müşteri tarafından yönetilen anahtarlarla TDE'ye](transparent-data-encryption-byok-azure-sql.md)bakın.
1. Örnekte denetim etkinse, aşağıdakileri
    - Varolan günlükleri içeren depolama kapsayıcısı veya olay hub'ı hedef bölgeye taşınır. 
    - Denetim hedef örnekte yapılandırılır. Daha fazla bilgi için, [yönetilen örnekle denetim](sql-database-managed-instance-auditing.md)etüt bakın.
1. Örneğinizin uzun vadeli bir bekletme ilkesi (LTR) varsa, varolan LTR yedeklemeleri geçerli sunucuyla ilişkili kalır. Hedef sunucu farklı olduğundan, sunucu silinse bile kaynak sunucuyu kullanarak kaynak bölgedeki eski LTR yedeklemelerine erişebilirsiniz. 

  > [!NOTE]
  > Bu, egemen bulut ile halka açık bir bölge arasında hareket etmek için yeterli olmayacaktır. Böyle bir geçiş, LTR yedeklemelerinin şu anda desteklenmeyen hedef sunucuya taşınmasını gerektirir. 

### <a name="prepare-resources"></a>Kaynakları hazırlama

Her kaynak örneği ile karşılık gelen hedef örnek arasında bir başarısız lık grubu oluşturun.
    - Her örnekteki tüm veritabanlarının çoğaltılması otomatik olarak başlatılır. Daha fazla bilgi için [Otomatik başarısız gruplarına](sql-database-auto-failover-group.md) bakın.

 
### <a name="monitor-the-preparation-process"></a>Hazırlık sürecini izleme

Kaynaktan hedefe veritabanlarınızın çoğaltılmasını izlemek için düzenli olarak [Get-AzSqlDatabaseFailoverGroup'u](/powershell/module/az.sql/get-azsqldatabasefailovergroup?view=azps-2.3.2) arayabilirsiniz. Çoğaltma Durumu `Get-AzSqlDatabaseFailoverGroup` için bir özellik içeren çıktı **nesnesi:** 
   - **ReplicationState = 2** (CATCH_UP) veritabanının eşitlenmiş olduğunu ve güvenli bir şekilde başarısız olabileceğini gösterir. 
   - **ReplicationState = 0** (SEEDING) veritabanının henüz tohumlanmadığını ve başarısız bir girişimin başarısız olacağını gösterir. 

### <a name="test-synchronization"></a>Test eşitleme

Bir kez `2` **ReplicationState,** her veritabanına bağlanmak veya ikincil bitiş `<fog-name>.secondary.database.windows.net` noktası kullanarak veritabanları alt kümesi ve bağlantı, uygun güvenlik yapılandırması ve veri çoğaltma sağlamak için veritabanlarına karşı herhangi bir sorgu gerçekleştirin. 

### <a name="initiate-the-move"></a>Hareketi başlatma 

1. İkincil bitiş noktasını `<fog-name>.secondary.database.windows.net`kullanarak hedef sunucuya bağlanın.
1. [Switch-AzSqlDatabaseFailoverGroup'u](/powershell/module/az.sql/switch-azsqldatabasefailovergroup?view=azps-2.3.2) kullanarak ikincil yönetilen örneği tam senkronizasyona sahip birincil örnek olarak değiştirin. Bu işlem ya başarılı olur ya da geri döner. 
1. DNS CNAME giriş noktalarının hedef bölge IP adresine geldiğini tespit etmek için komutun `nslook up <fog-name>.secondary.database.windows.net` başarıyla tamamlandığını doğrulayın. Anahtar komutu başarısız olursa, CNAME güncelleştirilmeyecek. 

### <a name="remove-the-source-managed-instances"></a>Kaynak yönetilen örnekleri kaldırma
Taşıma tamamlandıktan sonra, gereksiz ücretleri önlemek için kaynak bölgedeki kaynakları kaldırın. 

1. [Remove-AzSqlDatabaseFailoverGroup'u](/powershell/module/az.sql/remove-azsqldatabasefailovergroup)kullanarak failover grubunu silin. Bu, failover grubu yapılandırmasını düşürür ve iki örnek arasındaki coğrafi çoğaltma bağlantılarını sonlandırır. 
1. [Remove-AzSqlInstance](/powershell/module/az.sql/remove-azsqlinstance)kullanarak kaynak yönetilen örneği silin. 
1. Sanal küme, sanal ağ ve güvenlik grubu gibi kaynak grubundaki ek kaynakları kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar 

Azure SQL Veritabanınızı geçirildikten sonra [yönetin.](sql-database-manage-after-migration.md) 

