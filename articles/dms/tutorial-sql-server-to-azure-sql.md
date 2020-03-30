---
title: "Öğretici: SQL Server'ı çevrimdışı olarak SQL tek veritabanına geçirin"
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak SQL Server'dan şirket içinde tek bir veritabanına veya Azure SQL Veritabanı'nda kapalı çevrimiçi havuzlu veritabanına geçiş yapmayı öğrenin.
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
ms.openlocfilehash: ff47246482bd0712ea4e741d44b12f2c6767380b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298923"
---
# <a name="tutorial-migrate-sql-server-to-a-single-database-or-pooled-database-in-azure-sql-database-offline-using-dms"></a>Öğretici: SQL Server'ı DMS kullanarak Azure SQL Veritabanı'nda tek bir veritabanına veya havuza veritabanına çevrimdışı olarak geçirin

Veritabanlarını şirket içi BIR SQL Server örneğinden [Azure SQL Veritabanı'na](https://docs.microsoft.com/azure/sql-database/)geçirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz. Bu öğreticide, Azure Veritabanı Geçiş Hizmeti'ni kullanarak Sql Server 2016'nın (veya sonraki) şirket içi bir örneğine yüklenen **Adventureworks2012** veritabanını Azure Veritabanı Geçiş Hizmeti'ni kullanarak Azure SQL Veritabanı'nda tek bir veritabanına veya havuza alınan veritabanına geçirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> - Data Migration Yardımcısı'nı kullanarak şirket içi veritabanınızı değerlendirme.
> - Data Migration Yardımcısı'nı kullanarak örnek şemayı geçirme.
> - Azure Veritabanı Geçiş Hizmeti örneği oluşturun.
> - Azure Veritabanı Geçiş Hizmeti'ni kullanarak bir geçiş projesi oluşturun.
> - Geçişi çalıştırma.
> - Geçişi izleme.
> - Geçiş raporu indirme.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, SQL Server'dan tek bir veritabanına veya Azure SQL Veritabanı'nda havuza alınan veritabanına çevrimdışı geçiş açıklanmaktadır. Çevrimiçi geçiş için bkz. [DMS kullanarak çevrimiçi biçimde SQL Server'ı Azure SQL Veritabanı’na geçirme](tutorial-sql-server-azure-sql-online.md).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

- [SQL Server 2016 veya sonrası](https://www.microsoft.com/sql-server/sql-server-downloads)indirin ve yükleyin.
- [Sunucu Ağ Protokolünü Etkinleştirme veya Devre Dışı Bırakma](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) makalesindeki yönergeleri izleyerek SQL Server Express yüklemesi sırasında varsayılan olarak devre dışı bırakılan TCP/IP protokolünü etkinleştirin.
- Azure SQL Veritabanı'nda tek bir (veya havuza) veritabanı oluşturun ve makaledeki ayrıntıları izleyerek [Azure portalını kullanarak Azure SQL Veritabanı'nda tek bir veritabanı oluşturun.](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-get-started)

    > [!NOTE]
    > SQL Server Integration Services (SSIS) kullanıyorsanız ve SSIS projeleriniz/paketleriniz (SSISDB) için katalog veritabanını SQL Server'dan Azure SQL Veritabanı'na geçirmek istiyorsanız, Azure Veri Fabrikası'nda (ADF) SSIS'i sizlerle birlikte ssis'i sizin adınıza otomatik olarak yönetecektir. SSIS paketlerini geçirme hakkında daha fazla bilgi için SQL [Server Tümleştirme Hizmetleri paketlerini Azure'a geçir'e](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages)bakın.
  
- [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) 3.3 veya üzeri sürümünü indirip yükleyin.
- [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için bir Microsoft Azure Sanal Ağı oluşturun. Sanal ağ oluşturma hakkında daha fazla bilgi için [Sanal Ağ Belgeleri'ne](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarıiçeren hızlı başlangıç makalelerini görün.

    > [!NOTE]
    > Sanal ağ kurulumu sırasında, Microsoft'a ağ la bakan ExpressRoute kullanıyorsanız, hizmetin sağlandığı alt ağa aşağıdaki hizmet [bitiş noktalarını](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) ekleyin:
    >
    > - Hedef veritabanı bitiş noktası (örneğin, SQL bitiş noktası, Cosmos DB bitiş noktası vb.)
    > - Depolama bitiş noktası
    > - Servis veri günü bitiş noktası
    >
    > Azure Veritabanı Geçiş Hizmeti internet bağlantısından yoksun olduğundan bu yapılandırma gereklidir.
    >
    >Şirket içi ağ ile Azure arasında siteden siteye bağlantınız yoksa veya siteden siteye bağlantı bant genişliği sınırlıysa, karma modda Azure Veritabanı Geçiş Hizmeti 'ni (Önizleme) kullanmayı düşünün. Karma mod, bulutta çalışan Azure Veritabanı Geçiş Hizmeti örneğiyle birlikte şirket içi geçiş işçisinden yararlanır. Karma modda Azure Veritabanı Geçiş Hizmeti örneği oluşturmak [Create an instance of Azure Database Migration Service in hybrid mode using the Azure portal](https://aka.ms/dms-hybrid-create)için bkz.

- Sanal ağ Ağ Güvenlik Grubu kurallarınızın Azure Veritabanı Geçiş Hizmeti'ne aşağıdaki gelen iletişim bağlantı noktalarını engellemediğinden emin olun: 443, 53, 9354, 445, 12000. Azure sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için ağ [güvenlik gruplarıyla ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)makalesine bakın.
- [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
- Azure Veritabanı Geçiş Hizmeti'nin varsayılan olarak TCP bağlantı noktası 1433 olan kaynak SQL Server'a erişmesine izin vermek için Windows güvenlik duvarınızı açın.
- Dinamik bağlantı noktalarını kullanarak birden çok adlandırılmış SQL Server örneği çalıştırıyorsanız, SQL Tarayıcı Hizmeti'ni etkinleştirmek ve Azure Veritabanı Geçiş Hizmeti'nin kaynağınızdaki adlandırılmış bir örne bağlanabilmesi için güvenlik duvarlarınız aracılığıyla UDP bağlantı noktası 1434'e erişime izin vermek isteyebilirsiniz Sunucu.
- Kaynak veritabanınızın (ler) önünde bir güvenlik duvarı cihazı kullanırken, Azure Veritabanı Geçiş Hizmeti'nin geçiş için kaynak veritabanına(ler) erişmesine izin vermek için güvenlik duvarı kuralları eklemeniz gerekebilir.
- Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanlarına erişmesine izin vermek için Azure SQL Veritabanı sunucusu için sunucu düzeyinde bir IP [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağın alt ağ aralığını sağlayın.
- SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerinin [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izinlerine sahip olduğundan emin olun.
- Hedef Azure SQL Veritabanı örneğine bağlanmak için kullanılan kimlik bilgilerinin hedef Azure SQL veritabanlarında CONTROL DATABASE iznine sahip olduğundan emin olun.

## <a name="assess-your-on-premises-database"></a>Şirket içi veritabanınızı değerlendirme

Verileri şirket içi BIR SQL Server örneğinden Azure SQL Veritabanı'nda tek bir veritabanına veya havuza alınan veritabanına geçirmeden önce, geçişi engelleyebilecek engelleme sorunları için SQL Server veritabanını değerlendirmeniz gerekir. Data Migration Yardımcısı 3.3 veya üzeri bir sürümü kullanarak [SQL Server geçiş değerlendirmesi yapma](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) makalesindeki adımları tamamlayın ve şirket içi veritabanı değerlendirmesi yapın. Yapılması gereken adımların özeti aşağıda verilmiştir:

1. Data Migration Yardımcısı'nda Yeni (+) simgesini ve **Değerlendirme** proje türünü seçin.
2. Bir proje adı belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda **Azure SQL Veritabanı** seçimini yapın ve ardından **Oluştur**'a tıklayarak projeyi oluşturun.

    Azure SQL Veritabanı'nda tek bir veritabanına veya havuza alınan veritabanına geçiş sağlayan kaynak SQL Server veritabanını değerlendirirken, aşağıdaki değerlendirme raporu türlerinden birini veya her ikisini seçebilirsiniz:

   - Veritabanı uyumluluğunu denetle
   - Özellik eşliğini denetle

    İki rapor türü de varsayılan olarak seçilidir.

3. Data Migration Yardımcısı'nın **Seçenekler** ekranında **İleri**'yi seçin.
4. **Kaynak seçin** ekranının **Sunucuya bağlan** iletişim kutusunda SQL Server bağlantı bilgilerini girin ve **Bağlan**'ı seçin.
5. **Kaynak ekle** iletişim kutusunda **AdventureWorks2012**'yi, **Ekle**'yi ve ardından **Değerlendirmeyi Başlat**'ı seçin.

    > [!NOTE]
    > SSIS kullanıyorsanız, DMA şu anda kaynak SSISDB'nin değerlendirilmesini desteklemez. Ancak, SSIS projeleri/paketleri, Azure SQL Veritabanı tarafından barındırılan hedef SSISDB'ye yeniden dağıtıldıkça değerlendirilecektir/doğrulanacaktır. SSIS paketlerini geçirme hakkında daha fazla bilgi için SQL [Server Tümleştirme Hizmetleri paketlerini Azure'a geçir'e](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages)bakın.

    Değerlendirme tamamlandığında aşağıdaki grafiğe benzer sonuçlar görüntülenir:

    ![Veri geçişi değerlendirmesi](media/tutorial-sql-server-to-azure-sql/dma-assessments.png)

    Azure SQL Veritabanı'ndaki tek veritabanları veya havuza alınan veritabanları için değerlendirmeler, özellik eşitliği sorunlarını ve tek bir veritabanına veya havuza alınan veritabanına dağıtmak için geçiş engelleme sorunlarını tanımlar.

    - **SQL Server özellik eşliği** kategorisi kapsamlı öneriler, Azure'daki alternatif yaklaşımlar ve geçiş projelerini planlama konusunda yardımcı olacak çıkarılabilecek adımlar sunar.
    - **Uyumluluk sorunları** kategorisi, şirket içi SQL Server veritabanlarının Azure SQL Veritabanına geçirilmesini engelleyebilecek uyumluluk sorunlarını yansıtan kısmen desteklenen ve desteklenmeyen özellikleri tanımlar. Bu sorunları gidermenize yardımcı olan öneriler de sağlanır.

6. İlgili seçenekleri belirleyerek değerlendirme sonuçlarındaki geçiş engelleyici sorunları ve özellik eşliği sorunlarını gözden geçirin.

## <a name="migrate-the-sample-schema"></a>Örnek şemayı geçirme

Değerlendirmeden memnun olduktan ve seçili veritabanının Azure SQL Veritabanı'nda tek bir veritabanına veya havuza alınan veritabanına geçiş için uygun bir aday olduğundan memnun olduktan sonra, şemayı Azure SQL Veritabanı'na geçirmek için DMA'yı kullanın.

> [!NOTE]
> Data Migration Yardımcısı'nda bir geçiş projesi oluşturmadan önce önkoşullarda belirtilen şekilde bir Azure SQL veritabanı sağladığınızdan emin olun. Bu öğreticide Azure SQL Veritabanı’nın adının **AdventureWorksAzure** olduğu kabul edilmiştir, ancak istediğiniz adı kullanabilirsiniz.

> [!IMPORTANT]
> SSIS kullanıyorsanız, DMA şu anda kaynak SSISDB geçişini desteklemez, ancak SSIS projelerinizi/paketlerinizi Azure SQL Veritabanı tarafından barındırılan hedef SSISDB'ye yeniden dağıtabilirsiniz. SSIS paketlerini geçirme hakkında daha fazla bilgi için SQL [Server Tümleştirme Hizmetleri paketlerini Azure'a geçir'e](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages)bakın.

**AdventureWorks2012** şemasını tek bir veritabanına veya havuza veritabanı Azure SQL Veritabanına geçirmek için aşağıdaki adımları gerçekleştirin:

1. Data Migration Yardımcısı'nda Yeni (+) simgesini ve ardından **Proje türü** bölümünde **Geçiş**'i seçin.
2. Bir proje adı belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda da **Azure SQL Veritabanı**'nı seçin.
3. **Geçiş Kapsamı** bölümünde **Yalnızca şema**'yı seçin.

    Yukarıdaki adımları gerçekleştirdikten sonra aşağıdaki görüntüde gösterildiği gibi Data Migration Yardımcısı arabiriminin görünmesi gerekir:

    ![Data Migration Yardımcısı Projesi Oluşturma](media/tutorial-sql-server-to-azure-sql/dma-create-project.png)

4. Projeyi oluşturmak için **Oluştur**'u seçin.
5. Data Migration Yardımcısı'nda kaynak SQL Server için bağlantı bilgilerini belirtin, **Bağlan**'ı ve ardından **AdventureWorks2012** veritabanını seçin.

    ![Data Migration Yardımcısı Kaynak Bağlantı Ayrıntıları](media/tutorial-sql-server-to-azure-sql/dma-source-connect.png)

6. **Hedef sunucuya Bağlan'ın**altında **İleri'yi**seçin, Azure SQL Veritabanı için hedef bağlantı ayrıntılarını belirtin, **Bağlan'ı**seçin ve ardından Azure SQL Veritabanı'nda önceden sağladığınız **AdventureWorksAzure** veritabanını seçin.

    ![Data Migration Yardımcısı Hedef Bağlantı Ayrıntıları](media/tutorial-sql-server-to-azure-sql/dma-target-connect.png)

7. **İleri**'yi seçerek **Nesneleri seçin** ekranına ilerleyin ve bu ekranda Azure SQL Veritabanına dağıtılacak **AdventureWorks2012** veritabanındaki şema nesnelerini belirtin.

    Varsayılan olarak, tüm nesneler seçilir.

    ![SQL Betiği Oluşturma](media/tutorial-sql-server-to-azure-sql/dma-assessment-source.png)

8. **SQL betiği oluştur**'u seçerek SQL betiklerini oluşturun ve hata olup olmadığını inceleyin.

    ![Şema Betiği](media/tutorial-sql-server-to-azure-sql/dma-schema-script.png)

9. **Şemayı dağıtma**'yı seçerek şemayı Azure SQL Veritabanına dağıtın. Şema dağıtıldıktan sonra hedef sunucuda anomali olup olmadığını kontrol edin.

    ![Şemayı Dağıtma](media/tutorial-sql-server-to-azure-sql/dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure Portal’da oturum açın. **Abonelikleri**arayın ve seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-sql-server-to-azure-sql/portal-select-subscription1.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **Kaynak sağlayıcılarını**seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-sql-server-to-azure-sql/portal-select-resource-provider.png)

3. Geçiş arama ve ardından **Microsoft.DataMigration**için **Kaydol'u** seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-sql-server-to-azure-sql/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Örnek oluşturma

1. Azure portalı menüsünde veya **Ana** sayfada **kaynak oluştur'u**seçin. Azure Veritabanı **Geçiş Hizmeti'ni**arayın ve seçin.

    ![Azure Market](media/tutorial-sql-server-to-azure-sql/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-sql-server-to-azure-sql/dms-create1.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz konumu seçin.

5. Varolan bir sanal ağ seçin veya yeni bir ağ oluşturun.

    Sanal ağ, kaynak SQL Server'a ve hedef Azure SQL Veritabanı örneğine erişim sağlayan Azure Veritabanı Geçiş Hizmeti sağlar.

    Azure portalında sanal ağ oluşturma hakkında daha fazla bilgi için Azure [portalını kullanarak sanal ağ oluşturma makalesine](https://aka.ms/DMSVnet)bakın.

6. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-sql-server-to-azure-sql/dms-settings2.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalı menüsünde **Tüm hizmetler'i**seçin. Azure Veritabanı **Geçiş Hizmetlerini**arayın ve seçin.

     ![Azure Veritabanı Geçiş Hizmeti'nin tüm örneklerini bulma](media/tutorial-sql-server-to-azure-sql/dms-search.png)

2. Azure **Veritabanı Geçiş Hizmetleri** ekranında, oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğini seçin.

3. **Yeni Geçiş Projesi'ni**seçin.

     ![Azure Veritabanı Geçiş Hizmeti örneğinizi bulma](media/tutorial-sql-server-to-azure-sql/dms-instance-search.png)

4. **Yeni geçiş projesi** ekranında proje için bir ad belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda **Azure SQL Veritabanı** ve **Etkinlik türünü seçin** alanında **Çevrimdışı veri geçişi** seçimini yapın.

    ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-sql-server-to-azure-sql/dms-create-project2.png)

5. Projeyi oluşturmak ve geçiş etkinliğini çalıştırmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server örneğinin bağlantı ayrıntılarını belirtin.

    Kaynak SQL Server örneği adı için Tam Etki Alanı Adı (FQDN) kullandığınızdan emin olun. DNS ad çözümlemenin mümkün olmadığı durumlarda IP Adresini de kullanabilirsiniz.

2. Kaynak sunucunuza bir güvenilir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Kendi imzalı bir sertifika kullanılarak şifrelenen TLS bağlantıları güçlü güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda kendi imzalı sertifikaları kullanarak TLS'ye güvenmemelisiniz.

   ![Kaynak Ayrıntıları](media/tutorial-sql-server-to-azure-sql/dms-source-details2.png)

    > [!IMPORTANT]
    > SSIS kullanıyorsanız, DMS şu anda kaynak SSISDB geçişini desteklemez, ancak SSIS projelerinizi/paketlerinizi Azure SQL Veritabanı tarafından barındırılan hedef SSISDB'ye yeniden dağıtabilirsiniz. SSIS paketlerini geçirme hakkında daha fazla bilgi için SQL [Server Tümleştirme Hizmetleri paketlerini Azure'a geçir'e](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages)bakın.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. **Kaydet**'i seçin ve **Geçiş hedef ayrıntıları** ekranında Data Migration Yardımcısı kullanılarak **AdventureWorks2012** şemasının dağıtıldığı önceden sağlanmış Azure SQL Veritabanı olan hedef Azure SQL Veritabanı sunucusunun bağlantı ayrıntılarını girin.

    ![Hedef seçme](media/tutorial-sql-server-to-azure-sql/dms-select-target2.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanı kaynak veritabanıyla aynı veritabanı adını içeriyorsa, Azure Veritabanı Geçiş Hizmeti varsayılan olarak hedef veritabanını seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-sql-server-to-azure-sql/dms-map-targets-activity2.png)

3. **Kaydet**'i seçin ve **Tablo seçme** ekranında tablo listesini genişletip etkilenen alan listesini inceleyin.

    Azure Veritabanı Geçiş Hizmeti otomatik olarak hedef Azure SQL Veritabanı örneğinde bulunan tüm boş kaynak tabloları seçer. Veri içeren tabloları yeniden geçirmek isterseniz bu dikey pencerede tabloları seçmeniz gerekir.

    ![Tabloları seçme](media/tutorial-sql-server-to-azure-sql/dms-configure-setting-activity2.png)

4. **Kaydet**'i seçin ve **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

5. **Doğrulama seçeneği** bölümünü genişleterek **Doğrulama seçeneğini belirleyin** ekranını açın ve geçirilen veritabanlarında **Şema karşılaştırması**, **Veri tutarlılığı** ve **Sorgu doğruluğu** doğrulaması yapma tercihlerini belirtin.

    ![Doğrulama seçeneğini belirleme](media/tutorial-sql-server-to-azure-sql/dms-configuration2.png)

6. **Kaydet**'i seçin ve özeti gözden geçirerek kaynak ve hedef ayrıntılarının belirttiğiniz verilere uyduğundan emin olun.

    ![Geçiş Özeti](media/tutorial-sql-server-to-azure-sql/dms-run-migration2.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

- **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi görüntülenir ve etkinliğin **Durumu** **Beklemededir.**

    ![Etkinlik Durumu](media/tutorial-sql-server-to-azure-sql/dms-activity-status1.png)

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek geçişin **Durum** bilgisi **Tamamlandı** olana kadar gösterilen verileri güncelleştirebilirsiniz.

    ![Etkinlik Durumu Tamamlandı](media/tutorial-sql-server-to-azure-sql/dms-completed-activity1.png)

2. Geçiş tamamlandıktan sonra **Raporu indir**'i seçerek geçiş işleminin ayrıntılarını içeren raporu indirebilirsiniz.

3. Azure SQL Veritabanı sunucusundaki hedef veritabanlarını doğrulayın.

### <a name="additional-resources"></a>Ek kaynaklar

- Azure Veri Geçiş Hizmeti uygulamalı [laboratuarını kullanarak SQL geçişi.](https://www.microsoft.com/handsonlabs/SelfPacedLabs/?storyGuid=3b671509-c3cd-4495-8e8f-354acfa09587)
- Azure SQL Veritabanı'na çevrimiçi geçişler gerçekleştirirken bilinen sorunlar ve sınırlamalar hakkında bilgi için, Bilinen makaleye bakın [Azure SQL Veritabanı çevrimiçi geçişleri ile](known-issues-azure-sql-online.md)ilgili sorunlar ve geçici geçici işler.
- Azure Veritabanı Geçiş Hizmeti hakkında bilgi [What is Azure Database Migration Service?](https://docs.microsoft.com/azure/dms/dms-overview)için bkz.
- Azure SQL Veritabanı hakkında bilgi için makaleye bakın [Azure SQL Veritabanı hizmeti nedir?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
