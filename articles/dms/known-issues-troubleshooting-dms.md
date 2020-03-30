---
title: Sık karşılaşılan sorunlar - Azure Veritabanı Geçiş Hizmeti
description: Azure Veritabanı Geçiş Hizmeti'ni kullanmayla ilişkili yaygın bilinen sorunları/hataları nasıl gidereceklerini öğrenin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: c5d2ad481124f5ae048d010cdf632ee661bbd6ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77649116"
---
# <a name="troubleshoot-common-azure-database-migration-service-issues-and-errors"></a>Sık karşılaşılan Azure Veritabanı Geçiş Hizmeti sorunlarını ve hatalarını giderme

Bu makalede, Azure Veritabanı Geçiş Hizmeti kullanıcılarının karşılaşabileceği bazı sık karşılaşılan sorunlar ve hatalar açıklanmaktadır. Makale, bu sorunların ve hataların nasıl çözüleceği hakkında da bilgiler içerir.

## <a name="migration-activity-in-queued-state"></a>Sıralı durumda geçiş etkinliği

Bir Azure Veritabanı Geçiş Hizmeti projesinde yeni etkinlikler oluşturduğunuzda, etkinlikler sıraya girerken kalır.

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Bu sorun, Azure Veritabanı Geçiş Hizmeti örneği aynı anda çalışan devam eden görevler için maksimum kapasiteye ulaştığında ortaya çıkmaktadır. Kapasite kullanılabilir hale gelene kadar herhangi bir yeni etkinlik sıraya alınır. | Veri Geçiş Hizmeti örneğini doğrulayın projeler arasında çalışan etkinlikler var. Yürütme için sıraya otomatik olarak eklenen yeni etkinlikler oluşturmaya devam edebilirsiniz. Varolan çalışan etkinliklerden herhangi biri tamamlanır tamamlanmaz, sonraki sıraya sıralanan etkinlik çalışmaya başlar ve durum otomatik olarak çalışma durumuna dönüşür. Sıralı etkinliğin geçişini başlatmak için ek bir eylem yapmanız gerekmez.<br><br> |

## <a name="max-number-of-databases-selected-for-migration"></a>Geçiş için seçilen en yüksek veritabanları sayısı

Azure SQL Veritabanı'na veya Azure SQL Veritabanı yönetilen bir örneğe geçmek için veritabanı geçiş projesi için bir etkinlik oluştururken aşağıdaki hata oluşur:

* **Hata**: Geçiş ayarları doğrulama hatası", "errorDetail":"Geçiş için 'Veritabanları'nın maksimum '4' nesnelerinden daha fazlası seçilmiştir."

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Bu hata, tek bir geçiş etkinliği için dörtten fazla veritabanı seçtiğinizde görüntülenir. Şu anda, her geçiş etkinliği dört veritabanları ile sınırlıdır. | Geçiş etkinliği başına dört veya daha az veritabanları seçin. Paralel olarak dörtten fazla veritabanını geçirmeniz gerekiyorsa, Azure Veritabanı Geçiş Hizmeti'nin başka bir örneğini sağleyin. Şu anda, her abonelik en fazla iki Azure Veritabanı Geçiş Hizmeti örneğini destekler.<br><br> |

## <a name="errors-for-mysql-migration-to-azure-mysql-with-recovery-failures"></a>MySQL'in Kurtarma hatalarıyla Azure MySQL'e geçişi nde hatalar

Azure Veritabanı Geçiş Hizmeti'ni kullanarak MySQL'den MySQL için Azure Veritabanı'na geçiş yaptığınızda, geçiş etkinliği aşağıdaki hatayla başarısız olur:

* **Hata**: Veritabanı geçiş hatası - Task 'TaskID' [n] ardışık kurtarma hataları nedeniyle askıya alındı.

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Bu hata, geçiş yapan kullanıcı çoğaltma istemcisi, REPLICATION REPLICA ve SUPER (MySQL 5.6.6'dan önceki sürümler) çoğaltma admin rolü ve/veya ayrıcalıkları eksik olduğunda oluşabilir.<br><br><br><br><br><br><br><br><br><br><br><br><br> | Kullanıcı hesabı için [ön koşul ayrıcalıklarının](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online#prerequisites) MySQL örneği için Azure Veritabanı'nda doğru şekilde yapılandırıldığından emin olun. Örneğin, gerekli ayrıcalıklara sahip 'geçirici' adlı bir kullanıcı oluşturmak için aşağıdaki adımlar izlenebilir:<br>1. 'gizli' DİlİlİlEn KULLANICI migrateuser@'% <br>2. db_name.* üzerindeki tüm ayrıcalıkları 'gizli' olarak tanımlanan 'migrateuser'@'%' için tanıyın; daha fazla veritabanlarına erişim sağlamak için bu adımı yineleme <br>3. Hibe çoğaltma köle üzerinde *.* 'gizli' ile tanımlanan 'migrateuser'@'%' için;<br>4. Hibe çoğaltma istemcisi *üzerinde .* 'gizli' ile tanımlanan 'migrateuser'@'%' için;<br>5. Flush ayrıcalıkları; |

## <a name="error-when-attempting-to-stop-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmetini durdurmaya çalışırken hata

Azure Veritabanı Geçiş Hizmeti örneğini durdururken aşağıdaki hatayı alırsınız:

* **Hata**: Hizmet Durdurulamadı. Hata: {'error':{'code':'InvalidRequest','message':'Şu anda bir veya birden fazla etkinlik çalışıyor. Hizmeti durdurmak için, etkinlikler tamamlanana kadar bekleyin veya bu etkinlikleri el ile durdurun ve yeniden deneyin.'}}

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Bu hata, durdurmaya çalıştığınız hizmet örneğinin geçiş projelerinde hala çalışan veya mevcut olan etkinlikleri içerdiğinde görüntülenir. <br><br><br><br><br><br> | Durdurmaya çalıştığınız Azure Veritabanı Geçiş Hizmeti örneğinde çalışan etkinlik olmadığından emin olun. Hizmeti durdurmaya çalışmadan önce etkinlikleri veya projeleri de silebilirsiniz. Aşağıdaki adımlar, tüm çalışan görevleri silerek geçiş hizmeti örneğini temizlemek için projelerin nasıl kaldırılacak olduğunu gösterir:<br>1. Install-Module -Adı AzureRM.DataMigration <br>2. Giriş-AzureRmAccount <br>3. Select-AzureRmSubscription -SubscriptionName "\<alt Adı>" <br> 4. Remove-AzureRmDataMigrationProject \<-Ad projeAdı> \<-ResourceGroupName \<rgName> -ServiceName serviceName> -DeleteRunningTaskTask |

## <a name="error-when-attempting-to-start-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti'ni başlatmaya çalışırken hata

Azure Veritabanı Geçiş Hizmeti örneğini başlatırken aşağıdaki hatayı alırsınız:

* **Hata**: Hizmet Başlat'ta başarısız olur. Hata: {'errorDetail':'Hizmet başlatılamamış, lütfen Microsoft desteğine başvurun'}

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Önceki örnek dahili olarak başarısız olduğunda bu hata görüntülenir. Bu hata nadiren oluşur ve mühendislik ekibi bunun farkındadır. <br> | Başlatamadığınız hizmet örneğini silin ve ardından değiştirmek için yeni bir hizmet sağlayın. |

## <a name="error-restoring-database-while-migrating-sql-to-azure-sql-db-managed-instance"></a>SQL'i Azure SQL DB yönetilen örneğe geçirerken veritabanını geri alma hatası

SQL Server'dan Azure SQL Veritabanı yönetilen bir örneğe çevrimiçi geçiş yaptığınızda, kesme aşağıdaki hatayla başarısız olur:

* **Hata**: Geri Yükleme İşlemi Id 'operationId' işlemi için başarısız oldu. Kod 'AuthorizationFailed', İleti 'clientId' nesne id 'objectId' ile 'Microsoft.Sql/locations/managedDatabaseRestoreAzureAsync/read' kapsamı üzerinde eylem gerçekleştirmek için yetkiye sahip değildir '/abonelikler/subscriptionId'.'.

| Nedeni         | Çözüm    |
| ------------- | ------------- |
| Bu hata, SQL Server'dan Azure SQL Veritabanı yönetilen örneğine çevrimiçi geçiş için kullanılan uygulama ilkesinin abonelikte katkıda bulunma izni olmadığını gösterir. Şu anda Yönetilen Örnek ile yapılan bazı API çağrıları, geri yükleme işlemi için abonelik için bu izni gerektirir. <br><br><br><br><br><br><br><br><br><br><br><br><br><br> | Kullanılan `Get-AzureADServicePrincipal` uygulama kimliğinin görüntü `-ObjectId` adını listelemek için hata iletisinden edinilebilen PowerShell cmdlet'ini kullanın.<br><br> Bu uygulamanın izinlerini doğrulayın ve abonelik düzeyinde [katılımcı rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) sahip olduğundan emin olun. <br><br> Azure Veritabanı Geçiş Hizmeti Mühendisliği Ekibi, abonelikteki geçerli katkıda bulunan rolden gerekli erişimi kısıtlamak için çalışmaktadır. Katkıda bulunan rolün kullanımına izin vermeyen bir iş gereksiniminiz varsa, ek yardım için Azure desteğine başvurun. |

## <a name="error-when-deleting-nic-associated-with-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti ile ilişkili NIC'yi serken hata

Azure Veritabanı Geçiş Hizmetiile ilişkili bir Ağ Arabirimi Kartını silmeye çalıştığınızda, silme girişimi bu hatayla başarısız olur:

* **Hata**: NIC'yi kullanan DMS hizmeti nedeniyle Azure Veritabanı Geçiş Hizmetiile ilişkili NIC'yi silemez

| Nedeni         | Çözüm    |
| ------------- | ------------- |
| Bu sorun, Azure Veritabanı Geçiş Hizmeti örneği hala mevcut olabilir ve NIC tüketen olur. <br><br><br><br><br><br><br><br> | Bu NIC'i silmek için, hizmet tarafından kullanılan NIC'yi otomatik olarak silen DMS hizmet örneğini silin.<br><br> **Önemli**: Silinen Azure Veritabanı Geçiş Hizmeti örneğinin çalışan bir etkinlik olmadığından emin olun.<br><br> Azure Veritabanı Geçiş Hizmeti örneğiyle ilişkili tüm projeler ve etkinlikler silindikten sonra hizmet örneğini silebilirsiniz. Hizmet örneği tarafından kullanılan NIC, hizmet silme işleminin bir parçası olarak otomatik olarak temizlenir. |

## <a name="connection-error-when-using-expressroute"></a>ExpressRoute kullanırken bağlantı hatası oluştu

Azure Veritabanı Geçiş Hizmeti proje sihirbazında kaynağa bağlanmayı denediğinizde, kaynak bağlantı için ExpressRoute kullanıyorsa uzun bir zaman aşımı süresinin ardından bağlantı başarısız olur.

| Nedeni         | Çözüm    |
| ------------- | ------------- |
| [ExpressRoute](https://azure.microsoft.com/services/expressroute/)kullanırken, Azure Veritabanı Geçiş Hizmeti, hizmetle ilişkili Sanal Ağ alt ağında üç hizmet bitiş noktası sağlamayı [gerektirir:](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online)<br> -- Servis Veri Günü bitiş noktası<br> -- Depolama bitiş noktası<br> -- Hedef veritabanı bitiş noktası (örneğin SQL bitiş noktası, Cosmos DB bitiş noktası)<br><br><br><br><br> | Kaynak ve Azure Veritabanı Geçiş Hizmeti arasında ExpressRoute bağlantısı için gerekli hizmet bitiş noktalarını [etkinleştirin.](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online) <br><br><br><br><br><br><br><br> |

## <a name="lock-wait-timeout-error-when-migrating-a-mysql-database-to-azure-db-for-mysql"></a>MySQL veritabanını MySQL için Azure DB'ye geçirerken bekleme zaman sonu hatasını kilitleme

Bir MySQL veritabanını Azure Veritabanı Geçiş Hizmeti aracılığıyla MySQL örneği için bir Azure Veritabanına geçirdiğinizde, geçiş, kilit bekleme zaman sonu hatasını aşağıdaki şekilde başarısız olur:

* **Hata**: Veritabanı geçiş hatası - Dosya yüklemek için başarısız - dosya 'n' RetCode için yükleme işlemi başlatmak için başarısız: SQL_ERROR SqlState: HY000 NativeError: 1205 Mesaj: [MySQL][ODBC Driver][mysqld] Kilit bekleme zaman aşımı aşıldı; hareketi yeniden başlatmayı deneyin

| Nedeni         | Çözüm    |
| ------------- | ------------- |
| Bu hata, geçiş sırasında kilit bekleme zaman ası nedeniyle geçiş başarısız olduğunda oluşur. | Sunucu parametresi **'innodb_lock_wait_timeout'** değerini artırmayı düşünün. İzin verilen en yüksek değer 1073741824'dür. |

## <a name="error-connecting-to-source-sql-server-when-using-dynamic-port-or-named-instance"></a>Dinamik bağlantı noktası veya adlandırılmış örnek kullanırken kaynak SQL Server'a bağlanma hatası

Azure Veritabanı Geçiş Hizmeti'ni adlandırılmış örnek veya dinamik bir bağlantı noktasında çalışan SQL Server kaynağına bağlamaya çalıştığınızda, bağlantı bu hatayla başarısız olur:

* **Hata**: -1 - SQL bağlantısı başarısız oldu. SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğunu ve SQL Server'ın uzak bağlantılara izin verecek şekilde yapılandırıldığından doğrulayın. (sağlayıcı: SQL Ağ Arayüzleri, hata: 26 - Hata Konumlandırma Server/Instance Specified)

| Nedeni         | Çözüm    |
| ------------- | ------------- |
| Bu sorun, Azure Veritabanı Geçiş Hizmeti'nin bağlanmaya çalıştığı kaynak SQL Server örneğinde dinamik bir bağlantı noktası olduğunda veya adlandırılmış bir örnek kullandığında ortaya çıkan bir sorundur. SQL Server Browser hizmeti, adlandırılmış bir örniğe gelen bağlantılar veya dinamik bir bağlantı noktası kullanırken UDP bağlantı noktası 1434'ü dinler. Dinamik bağlantı noktası, SQL Server hizmeti her yeniden başlatınher zaman değişebilir. SQL Server Configuration Manager'da ağ yapılandırması yoluyla bir örniğe atanan dinamik bağlantı noktasını denetleyebilirsiniz.<br><br><br> |Azure Veritabanı Geçiş Hizmeti'nin UDP bağlantı noktası 1434'teki kaynak SQL Server Browser hizmetine ve dinamik olarak atanan TCP bağlantı noktası aracılığıyla SQL Server örneğine bağlanabildiğinizi doğrulayın. |

## <a name="additional-known-issues"></a>Bilinen ek sorunlar

* [Azure SQL Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-sql-online)
* [MySQL için Azure Veritabanına çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-mysql-online)
* [PostgreSQL için Azure Veritabanına çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-postgresql-online)

## <a name="next-steps"></a>Sonraki adımlar

* Makaleyi görüntüleyin [Azure Veritabanı Geçiş Hizmeti PowerShell](https://docs.microsoft.com/powershell/module/azurerm.datamigration/?view=azurermps-6.13.0#data_migration).
* Makaleyi görüntüleyin [Azure portalını kullanarak MySQL için Azure Veritabanı'ndaki sunucu parametreleri nasıl yapılandırılabilirsiniz.](https://docs.microsoft.com/azure/mysql/howto-server-parameters)
* Makaleyi [görüntüleyin Azure Veritabanı Geçiş Hizmeti'ni kullanmak için ön koşullara genel bakış.](https://docs.microsoft.com/azure/dms/pre-reqs)
* Azure [Veritabanı Geçiş Hizmeti'ni kullanma hakkında SSS'ye](https://docs.microsoft.com/azure/dms/faq)bakın.
