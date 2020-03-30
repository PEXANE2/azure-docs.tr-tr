---
title: SSIS paketlerini SQL yönetilen örneğine geçirin
titleSuffix: Azure Database Migration Service
description: SQL Server Integration Services (SSIS) paketlerini ve projelerini Azure Veritabanı Geçiş Hizmeti veya Veri Geçiş Yardımcısı'nı kullanarak yönetilen bir Azure SQL Veritabanı veritabanına nasıl geçirebilirsiniz öğrenin.
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
ms.openlocfilehash: 97a466ab033a42016c0d82465d1f98e2dcae8080
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297175"
---
# <a name="migrate-sql-server-integration-services-packages-to-an-azure-sql-database-managed-instance"></a>SQL Server Integration Services paketlerini Azure SQL Veritabanı yönetilen örneğine geçirme
SQL Server Tümleştirme Hizmetleri 'ni (SSIS) kullanıyorsanız ve SSIS projelerinizi/paketlerinizi SQL Server tarafından barındırılan kaynak SSISDB'den Azure SQL Veritabanı yönetilen bir örnek tarafından barındırılan hedef SSISDB'ye geçirmek istiyorsanız, Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz.

Kullandığınız SSIS sürümü 2012'den önceyse veya SSIS projelerinizi/paketlerinizi geçirmeden önce SSISDB olmayan paket mağaza türlerini kullanıyorsanız, Bunları SSMS'ten de başlatılabilen Entegrasyon Hizmetleri Proje Dönüştürme Sihirbazı'nı kullanarak dönüştürmeniz gerekir. Daha fazla bilgi için, [projeleri proje dağıtım modeline dönüştüren makaleye](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert)bakın.

> [!NOTE]
> Azure Veritabanı Geçiş Hizmeti (DMS), şu anda hedef geçiş hedefi olarak Azure SQL Veritabanı'nı desteklememektedir. SSIS projelerini/paketlerini Azure SQL Veritabanına yeniden dağıtmak [için, SQL Server Tümleştirme Hizmetleri paketlerini Azure SQL Veritabanına yeniden dağıt'a](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages)bakın.

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
>
> * Kaynak SSIS projelerini/paketlerini değerlendirin.
> * SSIS projelerini/paketlerini Azure'a geçirin.

## <a name="prerequisites"></a>Ön koşullar

Bu adımları tamamlamak için şunları yapmanız gerekir:

* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için Bir Microsoft Azure Sanal Ağı oluşturmak. Daha fazla bilgi için, [Azure Veritabanı Geçiş Hizmeti'ni kullanarak Azure SQL Veritabanı için Ağ topolojileri örnek geçişleri yönetilen]( https://aka.ms/dmsnetworkformi)makaleye bakın. Sanal ağ oluşturma hakkında daha fazla bilgi için [Sanal Ağ Belgeleri'ne](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarıiçeren hızlı başlangıç makalelerini görün.
* Sanal ağ Ağ Güvenliği Grubu kurallarınızın Azure Veritabanı Geçiş Hizmeti'ne aşağıdaki gelen iletişim bağlantı noktalarını engellemediğinden emin olmak için: 443, 53, 9354, 445, 12000. Sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için, [ağ güvenlik grupları ile](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)makale Filtre ağ trafiği bakın.
* Kaynak veritabanı [altyapısına erişim için Windows Güvenlik Duvarınızı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access?view=sql-server-2017)yapılandırmak için.
* Azure Veritabanı Geçiş Hizmeti'nin varsayılan olarak TCP bağlantı noktası 1433 olan kaynak SQL Server'a erişmesine izin vermek için Windows Güvenlik Duvarınızı açmak için.
* Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
* Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına ve 445 numaralı SMB bağlantı noktası aracılığıyla dosyalara erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
* SSISDB'yi barındırmak için yönetilen bir Azure SQL Veritabanı örneği. Bir tane oluşturmanız gerekiyorsa, bir [Azure SQL Veritabanı Yönetilen Örnek Oluştur](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started)makaledeki ayrıntıları izleyin.
* Kaynak SQL Server ve hedef yönetilen örnek bağlamak için kullanılan girişleri sysadmin sunucu rolü üyeleri olduğundan emin olmak için.
* SSIS'in Azure Veri Fabrikası'nda (ADF) Azure-SSIS Tümleştirme Çalışma Zamanı (IR) içeren ve Azure SQL Veritabanı yönetilen bir örnek tarafından barındırılan hedef SSISDB ile sağlandığını doğrulamak için (makalede açıklandığı gibi [Azure Veri Fabrikası'nda Azure-SSIS tümleştirme çalışma süresini oluşturun).](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)

## <a name="assess-source-ssis-projectspackages"></a>Kaynak SSIS projelerini/paketlerini değerlendirin

Kaynak SSISDB'nin değerlendirmesi henüz Veritabanı Geçiş Yardımcısı'na (DMA) entegre edilmese de, SSIS projeleriniz/paketleriniz, Azure SQL Veritabanı yönetilen bir örnekte barındırılan hedefe yeniden dağıtıldıkça değerlendirilecektir/doğrulanır.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

    ![Portal aboneliklerini gösterme](media/how-to-migrate-ssis-packages-mi/portal-select-subscriptions.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **Kaynak sağlayıcılarını**seçin.

    ![Kaynak sağlayıcılarını gösterme](media/how-to-migrate-ssis-packages-mi/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/how-to-migrate-ssis-packages-mi/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Azure Veritabanı Geçiş Hizmeti örneğini oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, **Azure Veritabanı Geçiş Hizmeti** araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

     ![Azure Market](media/how-to-migrate-ssis-packages-mi/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/how-to-migrate-ssis-packages-mi/dms-create1.png)

3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. DMS örneğini oluşturmak istediğiniz konumu seçin.

5. Varolan bir sanal ağ seçin veya bir ağ oluşturun.

    Sanal ağ, Kaynak SQL Server'a erişim sağlayan Azure Veritabanı Geçiş Hizmeti sağlar ve hedeflenen Azure SQL Veritabanı yönetilen örnek.

    Azure portalında sanal ağ oluşturma hakkında daha fazla bilgi için Azure [portalını kullanarak sanal ağ oluşturma makalesine](https://aka.ms/DMSVnet)bakın.

    Ek ayrıntı için, [Azure Veritabanı Geçiş Hizmeti'ni kullanarak Azure SQL DB yönetilen örnek geçişleri için Ağ topolojileri makalesine](https://aka.ms/dmsnetworkformi)bakın.

6. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![DMS hizmetini başlatma](media/how-to-migrate-ssis-packages-mi/dms-create-service2.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmetin bir örneği oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

    ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/how-to-migrate-ssis-packages-mi/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti ekranında** oluşturduğunuz örneğin adını arayın ve sonuçlardan bu örneği seçin.

3. +**Yeni Geçiş Projesi**'ni seçin.

4. Yeni **geçiş projesi** ekranında, **Kaynak sunucu türü** metin kutusunda proje için bir ad belirtin, Hedef sunucu **türü** metin kutusunda **SQL Server'ı**seçin, Azure SQL Veritabanı **Yönetilen Örnek'i**seçin ve ardından etkinlik **türünü seçin**, **SSIS paket geçişi**seçin.

   ![DMS projesi oluşturma](media/how-to-migrate-ssis-packages-mi/dms-create-project2.png)

5. Projeyi oluşturmak için **Oluştur**'u seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server bağlantı ayrıntılarını belirtin.

2. Sunucunuza bir güvenilir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Kendi imzalı bir sertifika kullanılarak şifrelenen TLS bağlantıları güçlü güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda kendi imzalı sertifikaları kullanarak TLS'ye güvenmemelisiniz.

   ![Kaynak Ayrıntıları](media/how-to-migrate-ssis-packages-mi/dms-source-details1.png)

3. **Kaydet'i**seçin.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. Geçiş **hedef ayrıntıları** ekranında, hedefin bağlantı ayrıntılarını belirtin.

     ![Hedef ayrıntılar](media/how-to-migrate-ssis-packages-mi/dms-target-details2.png)

2. **Kaydet'i**seçin.

## <a name="review-the-migration-summary"></a>Geçiş özetini gözden geçirme

1. **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

2. **SSIS proje(ler) ve çevre(ler) üzerine yazma seçeneği**için, varolan SSIS projelerinin ve ortamlarının üzerine mi yazacak veya yok saymak gerektiğini belirtin.

    ![Geçiş projesi özeti](media/how-to-migrate-ssis-packages-mi/dms-project-summary2.png)

3. Geçiş projesiyle ilgili ayrıntıları gözden geçirin ve doğrulayın.

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft [Veritabanı Geçiş Kılavuzu'ndaki](https://datamigration.microsoft.com/)geçiş kılavuzunu gözden geçirin.
