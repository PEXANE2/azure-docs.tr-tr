---
title: "Öğretici: SQL Server'ı çevrimiçi olarak SQL yönetilen bir örneğe geçirin"
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak SQL Server'dan şirket içi bir Azure SQL Veritabanı'na çevrimiçi geçiş gerçekleştirmeyi öğrenin.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 01/10/2020
ms.openlocfilehash: 236c68b3c26049073d3e6e942ce2a6be8b7f4fde
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298896"
---
# <a name="tutorial-migrate-sql-server-to-an-azure-sql-database-managed-instance-online-using-dms"></a>Öğretici: SQL Server'ı DMS kullanarak çevrimiçi olarak yönetilen bir Azure SQL Veritabanına geçirin

Veritabanlarını şirket içi BIR SQL Server örneğinden en az kapalı kalma süresiyle [Azure SQL Veritabanı yönetilen](../sql-database/sql-database-managed-instance.md) bir örneğine geçirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz. Bazı el ile çaba gerektiren ek yöntemler için, Sql Server örnek Azure [SQL Veritabanı yönetilen örneğinmakalesine](../sql-database/sql-database-managed-instance-migrate.md)bakın.

Bu öğreticide, **Adventureworks2012** veritabanını SQL Server'ın şirket içi bir örneğinden, Azure Veritabanı Geçiş Hizmeti'ni kullanarak en az kapalı kalma süresine sahip, SQL Veritabanı yönetilen bir örneğine geçirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturun.
> * Azure Veritabanı Geçiş Hizmeti'ni kullanarak bir geçiş projesi oluşturun ve çevrimiçi geçiş başlatmayı başlatın.
> * Geçişi izleme.
> * Hazır olduğunuzda geçiş kesme gerçekleştirin.

> [!IMPORTANT]
> Azure Veritabanı Geçiş Hizmeti'ni kullanarak SQL Server'dan SQL Veritabanı'na yönetilen örnek için, hizmetin veritabanlarınızı geçirmek için kullanabileceği SMB ağ paylaşımında tam veritabanı yedeklemesi ve sonraki günlük yedeklemelerini sağlamanız gerekir. Azure Veritabanı Geçiş Hizmeti herhangi bir yedekleme başlatmaz ve bunun yerine geçiş için olağanüstü durum kurtarma planınızın bir parçası olarak zaten sahip olabileceğiniz varolan yedeklemeleri kullanır.
> [WITH CHECKSUM seçeneğini kullanarak yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server?view=sql-server-2017)aldığınızdan emin olun. Ayrıca, birden çok yedeklemeyi (yani tam ve t-log) tek bir yedekleme ortamına eklemeyeceğinizden emin olun; her yedeklemeyi ayrı bir yedekleme dosyasına alın. Son olarak, büyük yedeklemelerin geçişiyle ilişkili olası sorunlarla karşılaşma olasılığını azaltmak için sıkıştırılmış yedeklemeler kullanabilirsiniz.

> [!NOTE]
> Çevrimiçi geçiş gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanmak, Premium fiyatlandırma katmanını temel alan bir örnek oluşturmayı gerektirir.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, hedef veritabanıyla aynı Azure bölgesinde Azure Veritabanı Geçiş Hizmeti örneği oluşturmanızı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

> [!IMPORTANT]
> Örnek yeniden yapılandırma veya planlı bakım dan kaynaklanan kesinti riskini en aza indirmek için çevrimiçi geçiş işleminin süresini mümkün olduğunca azaltın. Böyle bir durumda geçiş işlemi en başından itibaren başlar. Planlı bakım durumunda, geçiş işleminin yeniden başlatılmasından önce 36 saatlik bir yetkisiz kullanım süresi vardır.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, SQL Server'dan SQL Veritabanı yönetilen bir örneğe çevrimiçi geçiş açıklanmaktadır. Çevrimdışı geçiş için bkz: [SQL Server'ı DMS kullanarak çevrimdışı](tutorial-sql-server-to-managed-instance.md)yönetilen bir SQL Veritabanı örneği için geçirin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için bir Microsoft Azure Sanal Ağı oluşturun. Azure Veritabanı Geçiş [Hizmeti'ni kullanarak örnek geçişleri yöneten Azure SQL Veritabanı için ağ topolojilerini öğrenin.](https://aka.ms/dmsnetworkformi) Sanal ağ oluşturma hakkında daha fazla bilgi için [Sanal Ağ Belgeleri'ne](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarıiçeren hızlı başlangıç makalelerini görün.

    > [!NOTE]
    > Sanal ağ kurulumu sırasında, Microsoft'a ağ la bakan ExpressRoute kullanıyorsanız, hizmetin sağlandığı alt ağa aşağıdaki hizmet [bitiş noktalarını](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) ekleyin:
    >
    > * Hedef veritabanı bitiş noktası (örneğin, SQL bitiş noktası, Cosmos DB bitiş noktası vb.)
    > * Depolama bitiş noktası
    > * Servis veri günü bitiş noktası
    >
    > Azure Veritabanı Geçiş Hizmeti internet bağlantısından yoksun olduğundan bu yapılandırma gereklidir.
    >
    >Şirket içi ağ ile Azure arasında siteden siteye bağlantınız yoksa veya siteden siteye bağlantı bant genişliği sınırlıysa, karma modda Azure Veritabanı Geçiş Hizmeti 'ni (Önizleme) kullanmayı düşünün. Karma mod, bulutta çalışan Azure Veritabanı Geçiş Hizmeti örneğiyle birlikte şirket içi geçiş işçisinden yararlanır. Karma modda Azure Veritabanı Geçiş Hizmeti örneği oluşturmak [Create an instance of Azure Database Migration Service in hybrid mode using the Azure portal](https://aka.ms/dms-hybrid-create)için bkz.

    > [!IMPORTANT]
    > Geçişin bir parçası olarak kullanılan depolama hesabıyla ilgili olarak şunları yapmalısınız:
    > * Tüm ağın depolama hesabına erişmesine izin vermeyi seçin.
    > * MI alt netinde [alt ağ delegasyonu](https://docs.microsoft.com/azure/virtual-network/manage-subnet-delegation) açın ve bu alt ağa izin vermek için Depolama Hesabı güvenlik duvarı kurallarını güncelleştirin.

* Sanal ağ Ağ Güvenlik Grubu kurallarınızın Azure Veritabanı Geçiş Hizmeti'ne aşağıdaki gelen iletişim bağlantı noktalarını engellemediğinden emin olun: 443, 53, 9354, 445, 12000. Sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için, [ağ güvenlik grupları ile](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)makale Filtre ağ trafiği bakın.
* [Windows Güvenlik Duvarınızı kaynak veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* Azure Veritabanı Geçiş Hizmeti'nin varsayılan olarak TCP bağlantı noktası 1433 olan kaynak SQL Server'a erişmesine izin vermek için Windows Güvenlik Duvarınızı açın.
* Dinamik bağlantı noktalarını kullanarak birden çok adlandırılmış SQL Server örneği çalıştırıyorsanız, SQL Tarayıcı Hizmeti'ni etkinleştirmek ve Azure Veritabanı Geçiş Hizmeti'nin kaynağınızdaki adlandırılmış bir örne bağlanabilmesi için güvenlik duvarlarınız aracılığıyla UDP bağlantı noktası 1434'e erişime izin vermek isteyebilirsiniz Sunucu.
* Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin geçiş için kaynak veritabanına(lar) ve SMB port 445 üzerinden dosyalara erişmesine izin vermek için güvenlik duvarı kuralları eklemeniz gerekebilir.
* Makaledeki ayrıntıları izleyerek yönetilen bir SQL Veritabanı örneği oluşturun [Azure portalında bir Azure SQL Veritabanı yönetilen örnek oluşturun.](https://aka.ms/sqldbmi)
* Kaynak SQL Server ve hedef yönetilen örnek bağlantısı kurmak için kullanılan oturum açma bilgilerinin sysadmin sunucu rolüne üye olduğundan emin olun.
* Tüm veritabanı tam veritabanı yedekleme dosyalarınızı ve Azure Veritabanı Geçiş Hizmeti'nin veritabanı geçişi için kullanabileceği sonraki işlem günlüğü yedekleme dosyalarını içeren bir Kobİ ağ paylaşımı sağlayın.
* Kaynak SQL Server örneğini çalıştıran hizmet hesabının oluşturduğunuz ağ paylaşımında yazma ayrıcalıklarına sahip olduğundan ve kaynak sunucunun bilgisayar hesabının aynı paylaşımda okuma/yazma erişimine sahip olduğundan emin olun.
* Önceden oluşturduğunuz ağ paylaşımında tam denetim ayrıcalığına sahip olan Windows kullanıcısını (ve parolasını) not edin. Azure Veritabanı Geçiş Hizmeti, yedekleme dosyalarını geri yükleme işlemi için Azure Depolama kapsayıcısına yüklemek için kullanıcı kimlik bilgilerini taklit eder.
* Azure Veritabanı Geçiş Hizmeti'nin hedeflenen Azure Veritabanı yönetilen örneği ve Azure Depolama Kapsayıcısı'na bağlanmak için kullanabileceği Uygulama Kimliği anahtarını oluşturan bir Azure Etkin Dizin Uygulama Kimliği oluşturun. Daha fazla bilgi için bkz. [Kaynaklara erişebilen bir Azure Active Directory uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

  > [!NOTE]
  > Azure Veritabanı Geçiş Hizmeti, belirtilen Uygulama Kimliği için abonelikte Katılımcı izni gerektirir. Alternatif olarak, Azure Veritabanı Geçiş Hizmeti'nin gerektirdiği belirli izinleri veren özel roller oluşturabilirsiniz. Özel rolleri kullanma yla ilgili adım adım kılavuz için, SQL Server için SQL Veritabanı için Özel rollere bakın [örnek çevrimiçi geçişler yönetildi.](https://docs.microsoft.com/azure/dms/resource-custom-roles-sql-db-managed-instance)

* DMS hizmetinin veritabanı yedekleme dosyalarını yükleyebileceği ve veritabanlarını geçirmek için kullanabileceği bir **Standart Performans katmanı** Azure Depolama Hesabı oluşturun veya belirtin.  Azure Veritabanı Geçiş Hizmeti örneği yle aynı bölgede Azure Depolama Hesabı oluşturduğunuzdan emin olun.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

    ![Portal aboneliklerini gösterme](media/tutorial-sql-server-to-managed-instance-online/portal-select-subscriptions.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **Kaynak sağlayıcılarını**seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-sql-server-to-managed-instance-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-sql-server-to-managed-instance-online/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Azure Veritabanı Geçiş Hizmeti örneğini oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, **Azure Veritabanı Geçiş Hizmeti** araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

     ![Azure Market](media/tutorial-sql-server-to-managed-instance-online/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-sql-server-to-managed-instance-online/dms-create1.png)

3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. DMS örneğini oluşturmak istediğiniz konumu seçin.

5. Varolan bir sanal ağ seçin veya bir ağ oluşturun.

    Sanal ağ, Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server'a erişim sağlar ve hedef SQL Veritabanı yönetilen örnek.

    Azure portalında sanal ağ oluşturma hakkında daha fazla bilgi için Azure [portalını kullanarak sanal ağ oluşturma makalesine](https://aka.ms/DMSVnet)bakın.

    Ek ayrıntı için, Azure Veritabanı Geçiş [Hizmeti'ni kullanarak Azure SQL Veritabanı için Ağ topolojileri örnek geçişleri yönetilen](https://aka.ms/dmsnetworkformi)makaleye bakın.

6. Premium fiyatlandırma katmanından bir SKU seçin.

    > [!NOTE]
    > Çevrimiçi geçişler yalnızca Premium katmanı kullanırken desteklenir.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![DMS hizmetini başlatma](media/tutorial-sql-server-to-managed-instance-online/dms-create-service3.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmetin bir örneği oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Veritabanı Geçiş Hizmeti'nin tüm örneklerini bulma](media/tutorial-sql-server-to-managed-instance-online/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti ekranında** oluşturduğunuz örneğin adını arayın ve sonuçlardan bu örneği seçin.

3. +**Yeni Geçiş Projesi**'ni seçin.

4. Yeni **geçiş projesi** ekranında, **Kaynak sunucu türü** metin kutusunda proje için bir ad belirtin, Hedef sunucu **türü** metin kutusunda **SQL Server'ı**seçin, Azure SQL Veritabanı **Yönetilen Örnek'i**seçin ve ardından etkinlik **türünü seçin**, Çevrimiçi **veri geçişi'ni**seçin.

   ![Azure Veritabanı Geçiş Hizmeti Projesi Oluştur](media/tutorial-sql-server-to-managed-instance-online/dms-create-project3.png)

5. Projeyi oluşturmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server bağlantı ayrıntılarını belirtin.

2. Sunucunuza bir güvenilir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Kendi imzalı bir sertifika kullanılarak şifrelenen TLS bağlantıları güçlü güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda kendi imzalı sertifikaları kullanarak TLS'ye güvenmemelisiniz.

   ![Kaynak Ayrıntıları](media/tutorial-sql-server-to-managed-instance-online/dms-source-details2.png)

3. **Kaydet'i**seçin.

4. **Kaynak veritabanlarını seçin** ekranında geçiş için **Adventureworks2012** veritabanını seçin.

   ![Kaynak veritabanlarını seçme](media/tutorial-sql-server-to-managed-instance-online/dms-source-database1.png)

    > [!IMPORTANT]
    > SQL Server Integration Services (SSIS) kullanıyorsanız, DMS şu anda SSIS projeleriniz/paketleriniz (SSISDB) için katalog veritabanını SQL Server'dan Azure SQL Veritabanı yönetilen örneğe geçirmeyi desteklemez. Ancak, Azure Veri Fabrikası'nda (ADF) SSIS sağlayabilir ve SSIS projelerinizi/paketlerinizi Azure SQL Veritabanı yönetilen örneği tarafından barındırılan hedef SSISDB'ye yeniden dağıtabilirsiniz. SSIS paketlerini geçirme hakkında daha fazla bilgi için SQL [Server Tümleştirme Hizmetleri paketlerini Azure'a geçir'e](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages)bakın.

5. **Kaydet'i**seçin.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. Geçiş **hedef ayrıntıları** ekranında, DMS örneğinin Azure SQL Veritabanı yönetilen örneğinin hedef örneğine ve Azure Depolama Hesabı'na bağlanmak için kullanabileceği **Uygulama Kimliği** ve **Anahtarını** belirtin.

    Daha fazla bilgi için bkz. [Kaynaklara erişebilen bir Azure Active Directory uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

2. Azure SQL Veritabanı yönetilen örneğinin hedef örneğini içeren **Abonelik'i** seçin ve ardından hedef örneği seçin.

    Azure SQL Veritabanı yönetilen örneğini henüz sağlamadıysanız, örneği sağlamanıza yardımcı olacak [bağlantıyı](https://aka.ms/SQLDBMI) seçin. Azure SQL Veritabanı yönetilen örneği hazır olduğunda, geçiş işlemini yürütmek için bu özel projeye dönün.

3. Azure SQL Veritabanı yönetilen örneğe bağlanmak için **SQL Kullanıcı** ve **Parola** sağlayın.

    ![Hedef seçme](media/tutorial-sql-server-to-managed-instance-online/dms-target-details3.png)

4. **Kaydet'i**seçin.

## <a name="select-source-databases"></a>Kaynak veritabanlarını seçme

1. **Kaynak veritabanlarını seçin** ekranında geçirmek istediğiniz kaynak veritabanını seçin.

    ![Kaynak veritabanlarını seçme](media/tutorial-sql-server-to-managed-instance-online/dms-select-source-databases2.png)

2. **Kaydet'i**seçin.

## <a name="configure-migration-settings"></a>Geçiş ayarlarını yapılandırma

1. **Geçiş ayarlarını yapılandırma** ekranında şu bilgileri girin:

    | | |
    |--------|---------|
    |**SMB Ağ konumu paylaşımı** | Azure Veritabanı Geçiş Hizmeti'nin geçiş için kullanabileceği Tam veritabanı yedekleme dosyaları ve işlem günlüğü yedekleme dosyalarını içeren yerel Kobİ ağ paylaşımı veya Azure dosya paylaşımı. Kaynak SQL Server örneğini çalıştıran hizmet hesabının ağ paylaşımında okuma/yazma ayrıcalıkları olmalıdır. Ağ paylaşımındaki bir sunucunun FQDN veya IP adresi değerini girin, örneğin: '\\\sunucuadi.etkialaniadi.com\yedeklemeklasoru' veya '\\\IP adresi\yedeklemeklasoru'. Daha iyi performans için, geçirilecek her veritabanı için ayrı bir klasör kullanılması önerilir. **Gelişmiş Ayarlar** seçeneğini kullanarak veritabanı düzeyinde dosya paylaşım yolunu sağlayabilirsiniz. |
    |**Kullanıcı adı** | Windows kullanıcısının yukarıda belirttiğiniz ağ paylaşımında tam denetim ayrıcalığına sahip olduğundan emin olun. Azure Veritabanı Geçiş Hizmeti, yedekleme dosyalarını geri yükleme işlemi için Azure Depolama kapsayıcısına yüklemek için kullanıcı kimlik bilgilerini taklit edecektir. Azure Dosya paylaşımını kullanıyorsanız, azure\ ile önceden phazırolan depolama hesabı adını kullanıcı adı olarak kullanın. |
    |**Parola** | Kullanıcının parolası. Azure dosya paylaşımını kullanıyorsanız, parola olarak bir depolama hesabı anahtarı kullanın. |
    |**Azure Depolama Hesabının aboneliği** | Azure Depolama Hesabını içeren aboneliği seçin. |
    |**Azure Depolama Hesabı** | DMS'nin SMB ağ paylaşımındaki yedekleme dosyalarını yükleyebileceği ve veritabanı geçişi için kullanabileceği Azure Depolama Hesabını seçin.  En iyi dosya yükleme performansı için DMS hizmetiyle aynı bölgede bir Depolama Hesabı seçmenizi öneririz. |

    ![Geçiş Ayarlarını Yapılandırma](media/tutorial-sql-server-to-managed-instance-online/dms-configure-migration-settings4.png)

    > [!NOTE]
    > Azure Veritabanı Geçiş Hizmeti 'Sistem Hatası 53' veya 'Sistem Hatası 57' hatası gösterirse, bunun nedeni Azure Veritabanı Geçiş Hizmeti'nin Azure dosya paylaşımına erişememesinden kaynaklanabilir. Bu hatalardan biriyle karşılaşırsanız, lütfen [buradaki](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network)yönergeleri kullanarak sanal ağdan depolama hesabına erişim izni verin.

    > [!IMPORTANT]
    > Geri dönüş denetimi işlevi etkinse ve kaynak SQL Server ve dosya paylaşımı aynı bilgisayardaysa, kaynak FQDN kullanarak dosyalara erişemez. Bu sorunu gidermek için, [burada](https://support.microsoft.com/help/926642/error-message-when-you-try-to-access-a-server-locally-by-using-its-fqd)yönergeleri kullanarak geri alma denetimini devre dışı kaldırın.

2. **Kaydet'i**seçin.

## <a name="review-the-migration-summary"></a>Geçiş özetini gözden geçirme

1. **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

2. Geçiş projesiyle ilgili ayrıntıları gözden geçirin ve doğrulayın.

    ![Geçiş projesi özeti](media/tutorial-sql-server-to-managed-instance-online/dms-project-summary3.png)

## <a name="run-and-monitor-the-migration"></a>Geçişi çalıştırma ve izleme

1. **Geçişi çalıştır**'ı seçin.

2. Geçiş etkinliği ekranını güncelleştirmek için **Yenile**'yi seçin.

   ![Geçiş etkinliği sürüyor](media/tutorial-sql-server-to-managed-instance-online/dms-monitor-migration2.png)

    İlgili sunucu nesnelerinin geçiş durumunu izlemek için veritabanları ve oturum açma işlemleri kategorilerini genişletebilirsiniz.

   ![Geçiş etkinliği sürüyor](media/tutorial-sql-server-to-managed-instance-online/dms-monitor-migration-extend2.png)

## <a name="performing-migration-cutover"></a>Tam geçişi gerçekleştirme

Tam veritabanı yedekleme sql veritabanı yönetilen örnek hedef örnek geri yüklendikten sonra, veritabanı bir geçiş kesme gerçekleştirmek için kullanılabilir.

1. Çevrimiçi veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat**'ı seçin.

2. Kaynak veritabanlarına gelen tüm trafiği durdurun.

3. [Sonradan alınan günlük yedeğini] alın, yedekleme dosyasını SMB ağ paylaşımına yerleştirin ve bu son işlem günlüğü yedeği geri yüklenene kadar bekleyin.

    Bu noktada **Bekleyen değişiklikler** 0 olmalıdır.

4. **Onayla**'yı ve ardından, **Uygula**'yı seçin.

    ![Tam geçişi tamamlamaya hazırlanma](media/tutorial-sql-server-to-managed-instance-online/dms-complete-cutover.png)

5. Veritabanı geçiş durumu **Tamamlandı'yı**gösterdiğinde, uygulamalarınızı Azure SQL Veritabanı yönetilen yeni hedef örneğine bağlayın.

    ![Tam geçiş tamamlandı](media/tutorial-sql-server-to-managed-instance-online/dms-cutover-complete.png)

## <a name="next-steps"></a>Sonraki adımlar

* T-SQL RESTORE komutunu kullanarak veritabanını yönetilen bir örneğe nasıl geçirtileceğinizi gösteren bir öğretici için, [geri yükleme komutunu kullanarak yönetilen bir örneğe yedekleme geri yükleme'ye](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md)bakın.
* Yönetilen örnek hakkında bilgi için yönetilen [örnek nedir'e](../sql-database/sql-database-managed-instance.md)bakın.
* Uygulamaları yönetilen bir örne bağlama hakkında bilgi [için](../sql-database/sql-database-managed-instance-connect-app.md)bkz.
