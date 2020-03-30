---
title: "Öğretici: SQL Server'ı SQL yönetilen örneğine geçirin"
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak SQL Server'dan şirket içinde Azure SQL Veritabanı yönetilen bir veritabanına geçiş yapmayı öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 01/08/2020
ms.openlocfilehash: 416be7de4b3cef4fb6e1bcfd09d934937f8c96d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297723"
---
# <a name="tutorial-migrate-sql-server-to-an-azure-sql-database-managed-instance-offline-using-dms"></a>Öğretici: SQL Server'ı DMS kullanarak Sql Sql Veritabanı yönetilen bir örneğini çevrimdışı olarak geçirin

Veritabanlarını şirket içi BIR SQL Server [örneğinden Azure SQL Veritabanı yönetilen](../sql-database/sql-database-managed-instance.md)bir örneğe geçirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz. Bazı el ile çaba gerektiren ek yöntemler için, Sql Server örnek Azure [SQL Veritabanı yönetilen örneğinmakalesine](../sql-database/sql-database-managed-instance-migrate.md)bakın.

Bu öğreticide, **Adventureworks2012** veritabanını SQL Server'ın şirket içi bir örneğinden Azure Veritabanı Geçiş Hizmeti'ni kullanarak yönetilen bir SQL Veritabanı veritabanına geçirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> - Azure Veritabanı Geçiş Hizmeti örneği oluşturun.
> - Azure Veritabanı Geçiş Hizmeti'ni kullanarak bir geçiş projesi oluşturun.
> - Geçişi çalıştırma.
> - Geçişi izleme.
> - Geçiş raporu indirme.

> [!IMPORTANT]
> SQL Server'dan SQL Veritabanı yönetilen örneğinçevrimdışı geçişleri için Azure Veritabanı Geçiş Hizmeti sizin için yedek dosyalar oluşturabilir. Alternatif olarak, hizmetin veritabanlarınızı geçirmek için kullanacağı Kobİ ağ paylaşımındaki en son tam veritabanı yedeklemesini sağlayabilirsiniz. Birden çok yedeklemeyi tek bir yedekleme ortamına eklemayın; her yedeklemeyi ayrı bir yedekleme dosyasına alın. Büyük yedeklemelerin geçişiyle ilgili olası sorunlarla karşılaşma olasılığını azaltmak için sıkıştırılmış yedeklemeleri de kullanabileceğinizi unutmayın.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, SQL Server'dan SQL Veritabanı yönetilen bir örneğe çevrimdışı geçiş açıklanmaktadır. Çevrimiçi geçiş için bkz: SQL Server'ı DMS kullanarak çevrimiçi olarak [yönetilen bir Azure SQL Veritabanına Geçirin.](tutorial-sql-server-managed-instance-online.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

- [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için bir Microsoft Azure Sanal Ağı oluşturun. Azure Veritabanı Geçiş [Hizmeti'ni kullanarak örnek geçişleri yöneten Azure SQL Veritabanı için ağ topolojilerini öğrenin.](https://aka.ms/dmsnetworkformi) Sanal ağ oluşturma hakkında daha fazla bilgi için [Sanal Ağ Belgeleri'ne](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarıiçeren hızlı başlangıç makalelerini görün.

    > [!NOTE]
    > Sanal ağ kurulumu sırasında, Microsoft'a ağ la bakan ExpressRoute kullanıyorsanız, hizmetin sağlandığı alt ağa aşağıdaki hizmet [bitiş noktalarını](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) ekleyin:
    > - Hedef veritabanı bitiş noktası (örneğin, SQL bitiş noktası, Cosmos DB bitiş noktası vb.)
    > - Depolama bitiş noktası
    > - Servis veri günü bitiş noktası
    >
    > Azure Veritabanı Geçiş Hizmeti internet bağlantısından yoksun olduğundan bu yapılandırma gereklidir.

- Sanal ağ Ağ Güvenlik Grubu kurallarınızın Azure Veritabanı Geçiş Hizmeti'ne aşağıdaki gelen iletişim bağlantı noktalarını engellemediğinden emin olun: 443, 53, 9354, 445, 12000. Sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için, [ağ güvenlik grupları ile](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)makale Filtre ağ trafiği bakın.
- [Windows Güvenlik Duvarınızı kaynak veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
- Azure Veritabanı Geçiş Hizmeti'nin varsayılan olarak TCP bağlantı noktası 1433 olan kaynak SQL Server'a erişmesine izin vermek için Windows Güvenlik Duvarınızı açın.
- Dinamik bağlantı noktalarını kullanarak birden çok adlandırılmış SQL Server örneği çalıştırıyorsanız, SQL Tarayıcı Hizmeti'ni etkinleştirmek ve Azure Veritabanı Geçiş Hizmeti'nin kaynağınızdaki adlandırılmış bir örne bağlanabilmesi için güvenlik duvarlarınız aracılığıyla UDP bağlantı noktası 1434'e erişime izin vermek isteyebilirsiniz Sunucu.
- Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin geçiş için kaynak veritabanına(lar) ve SMB port 445 üzerinden dosyalara erişmesine izin vermek için güvenlik duvarı kuralları eklemeniz gerekebilir.
- Makaledeki ayrıntıları izleyerek yönetilen bir Azure SQL [Veritabanı oluşturma, Azure portalında bir Azure SQL Veritabanı yönetilen örnek oluşturun.](https://aka.ms/sqldbmi)
- Kaynak SQL Server ve hedef yönetilen örnek bağlamak için kullanılan girişleri sysadmin sunucu rolü üyeleri olduğundan emin olun.

    >[!NOTE]
    >Varsayılan olarak, Azure Veritabanı Geçiş Hizmeti yalnızca geçiş halindeki SQL oturumlarını destekler. Ancak, Windows oturum açmalarını aşağıdaki yollarla geçirme olanağını etkinleştirebilirsiniz:
    >
    >- Hedef SQL Veritabanı yönetilen örnek Şirket **Yöneticisi**veya **Global Administrator**" rolü ile bir kullanıcı tarafından Azure portalı üzerinden yapılandırılabilir AAD okuma erişimi ne olduğundan emin olun.
    >- Azure portalı üzerinden ayarlanan Windows kullanıcı/grup oturum açma geçişlerini Yapılandırma sayfasında etkinleştirmek için Azure Veritabanı Geçiş Hizmeti örneğinizi yapılandırma. Bu ayarı etkinleştirdikten sonra, değişikliklerin etkili olması için hizmeti yeniden başlatın.
    >
    > Hizmeti yeniden kullanıma açtıktan sonra, Windows kullanıcı/grup girişleri geçiş için kullanılabilir oturum açmalistesinde görünür. Geçiş yaptığınız herhangi bir Windows kullanıcı/grup girişi için, ilişkili etki alanı adını sağlamanız istenir. Hizmet kullanıcı hesapları (etki alanı adı NT AUTHORITY olan hesap) ve sanal kullanıcı hesapları (alan adı NT SERVICE olan hesap adı) desteklenmez.

- Azure Veritabanı Geçiş Hizmeti'nin kaynak veritabanını yedeklemek için kullanabileceği bir ağ paylaşımı oluşturun.
- Kaynak SQL Server örneğini çalıştıran hizmet hesabının oluşturduğunuz ağ paylaşımında yazma ayrıcalıklarına sahip olduğundan ve kaynak sunucunun bilgisayar hesabının aynı paylaşımda okuma/yazma erişimine sahip olduğundan emin olun.
- Önceden oluşturduğunuz ağ paylaşımında tam denetim ayrıcalığına sahip olan Windows kullanıcısını (ve parolasını) not edin. Azure Veritabanı Geçiş Hizmeti, yedekleme dosyalarını geri yükleme işlemi için Azure Depolama kapsayıcısına yüklemek için kullanıcı kimlik bilgilerini taklit eder.
- Bir blob kapsayıcısı oluşturun ve [Depolama Gezgini ile Azure Blob Depolama kaynaklarını yönetme](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) makalesindeki adımları kullanarak SAS URI'sini alın. SAS URI değerini oluştururken ilke penceresinde tüm izinleri (Okuma, Yazma, Silme, Listeleme) seçtiğinizden emin olun. Bu ayrıntı, veritabanlarını Azure SQL Veritabanı yönetilen örneğe geçirmek için kullanılan yedekleme dosyalarını yüklemek için depolama hesabı kapsayıcınıza erişim sağlayan Azure Veritabanı Geçiş Hizmeti sağlar.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

    ![Portal aboneliklerini gösterme](media/tutorial-sql-server-to-managed-instance/portal-select-subscriptions.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **Kaynak sağlayıcılarını**seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-sql-server-to-managed-instance/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-sql-server-to-managed-instance/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Azure Veritabanı Geçiş Hizmeti örneğini oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, **Azure Veritabanı Geçiş Hizmeti** araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Market](media/tutorial-sql-server-to-managed-instance/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-sql-server-to-managed-instance/dms-create1.png)

3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. DMS örneğini oluşturmak istediğiniz konumu seçin.

5. Varolan bir sanal ağ seçin veya bir ağ oluşturun.

    Sanal ağ, Kaynak SQL Server'a erişim sağlayan Azure Veritabanı Geçiş Hizmeti sağlar ve hedeflenen Azure SQL Veritabanı yönetilen örnek.

    Azure portalında sanal ağ oluşturma hakkında daha fazla bilgi için Azure [portalını kullanarak sanal ağ oluşturma makalesine](https://aka.ms/DMSVnet)bakın.

    Ek ayrıntı için, [Azure Veritabanı Geçiş Hizmeti'ni kullanarak Azure SQL DB yönetilen örnek geçişleri için Ağ topolojileri](https://aka.ms/dmsnetworkformi)makalesine bakın.

6. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![DMS hizmetini başlatma](media/tutorial-sql-server-to-managed-instance/dms-create-service2.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmetin bir örneği oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Veritabanı Geçiş Hizmeti'nin tüm örneklerini bulma](media/tutorial-sql-server-to-managed-instance/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti ekranında** oluşturduğunuz örneğin adını arayın ve sonuçlardan bu örneği seçin.

3. +**Yeni Geçiş Projesi**'ni seçin.

4. **Yeni geçiş projesi** ekranında proje için bir ad belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda **Azure SQL Veritabanı Yönetilen Örneği** ve **Etkinlik türünü seçin** alanında **Çevrimdışı veri geçişi** seçimini yapın.

   ![DMS projesi oluşturma](media/tutorial-sql-server-to-managed-instance/dms-create-project2.png)

5. Projeyi oluşturmak için **Oluştur**'u seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server bağlantı ayrıntılarını belirtin.

2. Sunucunuza bir güvenilir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Kendi imzalı bir sertifika kullanılarak şifrelenen TLS bağlantıları güçlü güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda kendi imzalı sertifikaları kullanarak TLS'ye güvenmemelisiniz.

   ![Kaynak Ayrıntıları](media/tutorial-sql-server-to-managed-instance/dms-source-details1.png)

3. **Kaydet'i**seçin.

4. **Kaynak veritabanlarını seçin** ekranında geçiş için **Adventureworks2012** veritabanını seçin.

   ![Kaynak veritabanlarını seçme](media/tutorial-sql-server-to-managed-instance/dms-source-database1.png)

    > [!IMPORTANT]
    > SQL Server Integration Services (SSIS) kullanıyorsanız, DMS şu anda SSIS projeleriniz/paketleriniz (SSISDB) için katalog veritabanını SQL Server'dan Azure SQL Veritabanı yönetilen örneğe geçirmeyi desteklemez. Ancak, Azure Veri Fabrikası'nda (ADF) SSIS sağlayabilir ve SSIS projelerinizi/paketlerinizi Azure SQL Veritabanı yönetilen örneği tarafından barındırılan hedef SSISDB'ye yeniden dağıtabilirsiniz. SSIS paketlerini geçirme hakkında daha fazla bilgi için SQL [Server Tümleştirme Hizmetleri paketlerini Azure'a geçir'e](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages)bakın.

5. **Kaydet'i**seçin.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. Geçiş **hedef ayrıntıları** ekranında, **AdventureWorks2012** veritabanını geçiriyor olduğunuz önceden sağlanmış Azure SQL Veritabanı yönetilen örneği olan hedefin bağlantı ayrıntılarını belirtin.

    SQL Veritabanı yönetilen örneğini zaten sağlamadıysanız, örneği sağlamanıza yardımcı olacak [bağlantıyı](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started) seçin. Proje oluşturmaya devam edebilir ve ardından Azure SQL Veritabanı yönetilen örnek hazır olduğunda, geçişi yürütmek için bu özel projeye geri dönebilirsiniz.

    ![Hedef seçme](media/tutorial-sql-server-to-managed-instance/dms-target-details2.png)

2. **Kaydet'i**seçin.

## <a name="select-source-databases"></a>Kaynak veritabanlarını seçme

1. **Kaynak veritabanlarını seçin** ekranında geçirmek istediğiniz kaynak veritabanını seçin.

    ![Kaynak veritabanlarını seçme](media/tutorial-sql-server-to-managed-instance/select-source-databases.png)

2. **Kaydet'i**seçin.

## <a name="select-logins"></a>Oturum açmaları seçme

1. **Oturum açmaları seçin** ekranında geçirmek istediğiniz oturum açmaları seçin.

    >[!NOTE]
    >Varsayılan olarak, Azure Veritabanı Geçiş Hizmeti yalnızca geçiş halindeki SQL oturumlarını destekler. Windows oturum açmaları için destek etkinleştirmek için bu öğreticinin **Önkoşullar** bölümüne bakın.

    ![Oturum açmaları seçme](media/tutorial-sql-server-to-managed-instance/select-logins.png)

2. **Kaydet'i**seçin.

## <a name="configure-migration-settings"></a>Geçiş ayarlarını yapılandırma

1. **Geçiş ayarlarını yapılandırma** ekranında şu bilgileri girin:

    | | |
    |--------|---------|
    |**Kaynak yedekleme seçeneği seçin** | Veritabanı geçişinde kullanılacak DMS için tam yedekleme dosyaları varsa **En son yedekleme dosyalarını ben sağlayacağım** seçeneğini belirleyin. DMS'nin önce kaynak veritabanının tam yedeğini alıp geçiş için bu yedeği kullanmasını istiyorsanız **Azure Veritabanı Geçiş Hizmeti'nin yedek dosyalar oluşturmasına izin veriyorum** seçeneğini belirleyin. |
    |**Ağ konumu paylaşımı** | Azure Veritabanı Geçiş Hizmeti'nin kaynak veritabanı yedeklemelerini götürebileceği yerel Kobİ ağı paylaşır. Kaynak SQL Server örneğini çalıştıran hizmet hesabının ağ paylaşımında yazma ayrıcalıkları olmalıdır. Ağ paylaşımındaki bir sunucunun FQDN veya IP adresi değerini girin, örneğin: '\\\sunucuadi.etkialaniadi.com\yedeklemeklasoru' veya '\\\IP adresi\yedeklemeklasoru'.|
    |**Kullanıcı adı** | Windows kullanıcısının yukarıda belirttiğiniz ağ paylaşımında tam denetim ayrıcalığına sahip olduğundan emin olun. Azure Veritabanı Geçiş Hizmeti, yedekleme dosyalarını geri yükleme işlemi için Azure Depolama kapsayıcısına yüklemek için kullanıcı kimlik bilgilerini taklit edecektir. Geçiş için TDE özelliğinin etkin olduğu veritabanlarının seçilmesi durumunda yukarıdaki Windows kullanıcısının yerleşik yönetici hesabı olması ve Azure Veritabanı Geçiş Hizmeti'nin sertifika dosyalarını yükleyip silmesi için [Kullanıcı Hesabı Denetimi](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/user-account-control-overview) özelliğinin devre dışı bırakılmış olması gerekir. |
    |**Parola** | Kullanıcının parolası. |
    |**Depolama hesabı ayarları** | Azure Veritabanı Geçiş Hizmeti'ne, hizmetin yedekleme dosyalarını yüklediği depolama hesabı kapsayıcınıza erişim sağlayan ve veritabanlarını Azure SQL Veritabanı yönetilen örneğine geçirmek için kullanılan SAS URI. [Blob kapsayıcısı için SAS URI değerini almayı öğrenin](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    |**TDE Ayarları** | Şeffaf Veri Şifreleme (TDE) etkin leştirilmiş kaynak veritabanlarını geçirtiyorsanız, hedef Azure SQL Veritabanı yönetilen örneğinde yazma ayrıcalıklarına sahip olmanız gerekir.  Açılan menüden Azure SQL Veritabanı'nın örnek olarak yönetildiği aboneliği seçin.  Açılan menüden **Azure SQL Veritabanı Yönetilen Örneği** hedefini seçin. |

    ![Geçiş Ayarlarını Yapılandırma](media/tutorial-sql-server-to-managed-instance/dms-configure-migration-settings3.png)

2. **Kaydet'i**seçin.

## <a name="review-the-migration-summary"></a>Geçiş özetini gözden geçirme

1. **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

2. **Doğrulama seçeneği** bölümünü genişleterek **Doğrulama seçeneğini belirleyin** ekranını açın ve geçirilen veritabanlarında sorgu doğrulaması yapma tercihini belirtin ve **Kaydet**'i seçin.

3. Geçiş projesiyle ilgili ayrıntıları gözden geçirin ve doğrulayın.

    ![Geçiş projesi özeti](media/tutorial-sql-server-to-managed-instance/dms-project-summary2.png)

4. **Kaydet'i**seçin.

## <a name="run-the-migration"></a>Geçişi çalıştırma

- **Geçişi çalıştır**'ı seçin.

  Geçiş etkinliği penceresi açılır ve etkinliğin durum bilgisi **Beklemede** olarak değişir.

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranını güncelleştirmek için **Yenile**'yi seçin.

   ![Geçiş etkinliği sürüyor](media/tutorial-sql-server-to-managed-instance/dms-monitor-migration1.png)

    İlgili sunucu nesnelerinin geçiş durumunu izlemek için veritabanları ve oturum açma işlemleri kategorilerini genişletebilirsiniz.

   ![Geçiş etkinliği sürüyor](media/tutorial-sql-server-to-managed-instance/dms-monitor-migration-extend.png)

2. Geçiş tamamlandıktan sonra **Raporu indir**'i seçerek geçiş işleminin ayrıntılarını içeren raporu indirebilirsiniz.

3. Hedef Azure SQL Veritabanı'ndaki hedef veritabanının örnek ortamını yönettiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- T-SQL RESTORE komutunu kullanarak veritabanını yönetilen bir örneğe nasıl geçirtileceğinizi gösteren bir öğretici için, [geri yükleme komutunu kullanarak yönetilen bir örneğe yedekleme geri yükleme'ye](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md)bakın.
- Yönetilen örnek hakkında bilgi için yönetilen [örnek nedir'e](../sql-database/sql-database-managed-instance.md)bakın.
- Uygulamaları yönetilen bir örne bağlama hakkında bilgi [için](../sql-database/sql-database-managed-instance-connect-app.md)bkz.
