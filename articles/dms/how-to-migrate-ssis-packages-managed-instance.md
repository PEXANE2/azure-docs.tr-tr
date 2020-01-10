---
title: SSIS paketlerini SQL yönetilen örneğine geçirme
titleSuffix: Azure Database Migration Service
description: Azure veritabanı geçiş hizmeti 'ni veya Data Migration Yardımcısı kullanarak Azure SQL veritabanı yönetilen örneği 'ne SQL Server Integration Services (SSIS) paketlerini ve projelerini geçirmeyi öğrenin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 01/08/2020
ms.openlocfilehash: 22f3e6a0e4c041024e826a7ed724d788ce77da62
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75751209"
---
# <a name="migrate-sql-server-integration-services-packages-to-an-azure-sql-database-managed-instance"></a>SQL Server Integration Services paketlerini Azure SQL veritabanı yönetilen örneği 'ne geçirme
SQL Server Integration Services (SSIS) kullanıyorsanız ve SSIS projelerinizi/SQL Server paketlerinizi Azure SQL veritabanı yönetilen örneği tarafından barındırılan hedef SSSıSDB 'ye geçirmek istiyorsanız, Azure veritabanı geçiş hizmeti ' ni kullanabilirsiniz.

SSIS 'nin kullandığınız sürümü 2012 'den daha eski veya SSIS projelerini/paketlerinizi geçirmeden önce, SSIS projelerinizi/paketlerinizi geçirmeden önce bunları, SSMS 'den başlatılabilen Tümleştirme Hizmetleri proje Dönüştürme Sihirbazı 'Nı kullanarak dönüştürmeniz gerekir. Daha fazla bilgi için [projeleri proje dağıtım modeline dönüştürme](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert)makalesine bakın.

> [!NOTE]
> Azure veritabanı geçiş hizmeti (DMS) Şu anda Azure SQL veritabanını hedef geçiş hedefi olarak desteklemiyor. SSIS projelerini/paketlerini Azure SQL veritabanı 'na yeniden dağıtmak için [SQL Server Integration Services paketlerini Azure SQL veritabanı 'na yeniden dağıtma](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages)makalesine bakın.

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
>
> * Kaynak SSIS projelerini/paketlerini değerlendirin.
> * SSIS projelerini/paketlerini Azure 'a geçirin.

## <a name="prerequisites"></a>Ön koşullar

Bu adımları tamamlayabilmeniz için şunlar gerekir:

* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlayan Azure Resource Manager dağıtım modelini kullanarak Azure veritabanı geçiş hizmeti için Microsoft Azure sanal ağ oluşturmak için. Daha fazla bilgi için [Azure veritabanı geçiş hizmeti 'ni kullanarak Azure SQL veritabanı yönetilen örnek geçişleri Için ağ topolojileri]( https://aka.ms/dmsnetworkformi)makalesine bakın. Sanal ağ oluşturma hakkında daha fazla bilgi için [sanal ağ belgelerine](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarla birlikte hızlı başlangıç makalelerine bakın.
* Sanal ağ ağ güvenlik grubu kurallarınızın, Azure veritabanı geçiş hizmeti 'ne yönelik aşağıdaki gelen iletişim bağlantı noktalarını engellemediğinden emin olmak için: 443, 53, 9354, 445, 12000. Sanal ağ NSG trafik filtrelemesi hakkında daha fazla bilgi için ağ [güvenlik grupları ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)makalesine bakın.
* [Kaynak veritabanı altyapısı erişimi Için Windows Güvenlik duvarınızı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access?view=sql-server-2017)yapılandırmak için.
* Azure veritabanı geçiş hizmeti 'nin kaynak SQL Server erişmesine izin vermek üzere Windows Güvenlik duvarınızı açmak için, varsayılan olarak TCP bağlantı noktası 1433 ' dir.
* Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
* Kaynak veritabanlarınızın önünde bir güvenlik duvarı cihazı kullanıyorsanız Azure Veritabanı Geçiş Hizmeti'nin geçiş amacıyla kaynak veritabanlarına ve 445 numaralı SMB bağlantı noktası aracılığıyla dosyalara erişmesi için güvenlik duvarı kuralları eklemeniz gerekebilir.
* SSıSDB barındırmak için bir Azure SQL veritabanı yönetilen örneği. Bir tane oluşturmanız gerekiyorsa, [Azure SQL veritabanı yönetilen örneği oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started)makalesindeki ayrıntıları izleyin.
* Kaynak SQL Server ve hedef yönetilen örneği bağlamak için kullanılan oturum açma bilgilerinin sysadmin sunucu rolünün üyeleri olduğundan emin olmak için.
* SSIS 'nin bir Azure SQL veritabanı yönetilen örneği tarafından barındırılan hedef SSSıSDB ile Azure-SSIS Integration Runtime (IR) içeren Azure Data Factory (ADF) olduğunu doğrulamak için ( [Azure Data Factory 'de Azure-SSIS tümleştirme çalışma zamanını oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)makalesinde açıklandığı gibi).

## <a name="assess-source-ssis-projectspackages"></a>Kaynak SSIS projelerini/paketlerini değerlendir

Kaynak SSSıSDB değerlendirmesi henüz veritabanı Geçiş Yardımcısı (DMA) ile tümleştirmemişse, SSIS projeleriniz/paketleriniz, Azure SQL veritabanı yönetilen örneği üzerinde barındırılan hedef SSıSDB 'ye yeniden dağıtıldıklarında değerlendirilecek/onaylanır.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler** seçeneğini belirleyin ve ardından **Abonelikler**'i seçin.

    ![Portal aboneliklerini gösterme](media/how-to-migrate-ssis-packages-mi/portal-select-subscriptions.png)

2. Azure veritabanı geçiş hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **kaynak sağlayıcıları**' nı seçin.

    ![Kaynak sağlayıcılarını gösterme](media/how-to-migrate-ssis-packages-mi/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/how-to-migrate-ssis-packages-mi/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Azure Veritabanı Geçiş Hizmeti örneğini oluşturma

1. Azure portalda +**Kaynak oluştur**'u seçin, **Azure Veritabanı Geçiş Hizmeti** araması yapın ve açılan listeden **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

     ![Azure Marketi](media/how-to-migrate-ssis-packages-mi/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/how-to-migrate-ssis-packages-mi/dms-create1.png)

3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. DMS örneğini oluşturmak istediğiniz konumu seçin.

5. Var olan bir sanal ağı seçin veya bir tane oluşturun.

    Sanal ağ, Azure veritabanı geçiş hizmeti 'ne kaynak SQL Server erişimi sağlar ve Azure SQL veritabanı yönetilen örneği ' ni hedefleyin.

    Azure portal bir sanal ağ oluşturma hakkında daha fazla bilgi için [Azure Portal kullanarak sanal ağ oluşturma](https://aka.ms/DMSVnet)makalesine bakın.

    Daha fazla ayrıntı için [Azure veritabanı geçiş hizmeti 'ni kullanarak Azure SQL DB yönetilen örnek geçişleri Için ağ topolojileri](https://aka.ms/dmsnetworkformi)makalesine bakın.

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

4. **Yeni geçiş projesi** ekranında, proje için bir ad belirtin, **kaynak sunucu türü** metin kutusunda **SQL Server**' i seçin, **hedef sunucu türü** metin kutusunda, **Azure SQL veritabanı yönetilen örneği**' ni seçin ve ardından **etkinlik türü seç**' i seçin, **SSIS paketi geçişi**' ni seçin.

   ![DMS projesi oluşturma](media/how-to-migrate-ssis-packages-mi/dms-create-project2.png)

5. Projeyi oluşturmak için **Oluştur**'u seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server bağlantı ayrıntılarını belirtin.

2. Sunucunuza bir güvenilir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Otomatik olarak imzalanan sertifika kullanarak şifrelenmiş SSL bağlantıları yüksek güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda otomatik olarak imzalanan sertifika ile SSL kullanımına güvenmemeniz gerekir.

   ![Kaynak Ayrıntıları](media/how-to-migrate-ssis-packages-mi/dms-source-details1.png)

3. **Kaydet**’i seçin.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. **Geçiş hedefi ayrıntıları** ekranında, hedefin bağlantı ayrıntılarını belirtin.

     ![Hedef Ayrıntıları](media/how-to-migrate-ssis-packages-mi/dms-target-details2.png)

2. **Kaydet**’i seçin.

## <a name="review-the-migration-summary"></a>Geçiş özetini gözden geçirme

1. **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

2. **SSIS projesi ve ortam üzerine yazma seçeneği**için, var olan SSIS projelerinin ve ortamlarının üzerine yazılıp yazılmayacağını belirtin.

    ![Geçiş projesi özeti](media/how-to-migrate-ssis-packages-mi/dms-project-summary2.png)

3. Geçiş projesiyle ilgili ayrıntıları gözden geçirin ve doğrulayın.

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft [veritabanı geçiş kılavuzu](https://datamigration.microsoft.com/)' nda geçiş kılavuzunu gözden geçirin.
