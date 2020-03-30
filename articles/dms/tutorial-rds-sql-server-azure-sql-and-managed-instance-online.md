---
title: "Öğretici: RDS SQL Server'ı çevrimiçi olarak SQL Veritabanına geçirin"
titleSuffix: Azure Database Migration Service
description: RDS SQL Server'dan Azure SQL Veritabanı tek veritabanına veya Yönetilen örneke Azure Veritabanı Geçiş Hizmeti'ni kullanarak çevrimiçi geçiş gerçekleştirmeyi öğrenin.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 01/08/2020
ms.openlocfilehash: 8d538deca610fd9981d401d28b6bea1c31c6d4c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298863"
---
# <a name="tutorial-migrate-rds-sql-server-to-azure-sql-database-or-an-azure-sql-database-managed-instance-online-using-dms"></a>Öğretici: RDS SQL Server'ı Azure SQL Veritabanına veya DMS kullanarak yönetilen bir Azure SQL Veritabanı'na geçirin
Veritabanlarını RDS SQL Server örneğinden [Azure SQL Veritabanı'na](https://docs.microsoft.com/azure/sql-database/) veya en az kapalı kalma süresiyle yönetilen bir Azure SQL [Veritabanı örneğine](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index) geçirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz. Bu öğreticide, SQL Server 2012'nin (veya sonraki) RDS SQL Server örneğine yüklenen **Adventureworks2012** veritabanını Azure Veritabanı Veritabanı'na veya Azure Veritabanı Geçiş Hizmeti'ni kullanarak yönetilen bir Azure SQL Veritabanı'na geçirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure SQL Veritabanı veya Azure SQL Veritabanı yönetilen bir örnek oluşturun. 
> * Data Migration Yardımcısı'nı kullanarak örnek şemayı geçirme.
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
> * Azure Veritabanı Geçiş Hizmeti'ni kullanarak geçiş projesi oluşturma.
> * Geçişi çalıştırma.
> * Geçişi izleme.
> * Geçiş raporu indirme.

> [!NOTE]
> Çevrimiçi geçiş gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanmak, Premium fiyatlandırma katmanını temel alan bir örnek oluşturmayı gerektirir. Daha fazla bilgi için Azure Veritabanı Geçiş Hizmeti [fiyatlandırma](https://azure.microsoft.com/pricing/details/database-migration/) sayfasına bakın.

> [!IMPORTANT]
> En iyi geçiş deneyimi için Microsoft, Azure Veritabanı Geçiş Hizmeti’nin bir örneğini hedef veritabanıyla aynı Azure bölgesinde oluşturmayı önerir. Verileri bölgeler veya coğrafyalar arasında taşımak, geçiş sürecini yavaşlatabilir ve hatalara neden olabilir.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, RDS SQL Server'dan Azure SQL Veritabanı'na veya Azure SQL Veritabanı yönetilen bir örneğe çevrimiçi geçiş açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* Bir [RDS SQL Server veritabanı](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.SQLServer.html)oluşturun.
* Azure portalında bir Azure SQL veritabanı oluştur makaledeki ayrıntıları izleyerek yaptığınız Azure SQL [Veritabanı'nın bir örneğini](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)oluşturun.

    > [!NOTE]
    > Azure SQL Veritabanı yönetilen bir örneğe geçiş yapıyorsunuz, makaledeki ayrıntıları izleyin [Azure SQL Veritabanı yönetilen örneği oluşturun](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started)ve ardından **AdventureWorks2012**adlı boş bir veritabanı oluşturun. 
 
* [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) 3.3 veya sonraki bir sürümünü indirip yükleyin.
* Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için bir Microsoft Azure Sanal Ağı oluşturun. Azure SQL Veritabanı yönetilen bir örneğe geçiş yapıyorsunuz, Azure SQL Veritabanı yönetilen örneği için kullanılan aynı sanal ağda, ancak farklı bir alt ağda DMS örneğini oluşturduğunuzdan emin olun.  Alternatif olarak, DMS için farklı bir sanal ağ kullanıyorsanız, iki sanal ağ arasında sanal ağ oluşturmanız gerekir. Sanal ağ oluşturma hakkında daha fazla bilgi için [Sanal Ağ Belgeleri'ne](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarıiçeren hızlı başlangıç makalelerini görün.

    > [!NOTE]
    > Sanal ağ kurulumu sırasında, Microsoft'a ağ la bakan ExpressRoute kullanıyorsanız, hizmetin sağlandığı alt ağa aşağıdaki hizmet [bitiş noktalarını](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) ekleyin:
    >
    > * Hedef veritabanı bitiş noktası (örneğin, SQL bitiş noktası, Cosmos DB bitiş noktası vb.)
    > * Depolama bitiş noktası
    > * Servis veri günü bitiş noktası
    >
    > Azure Veritabanı Geçiş Hizmeti internet bağlantısından yoksun olduğundan bu yapılandırma gereklidir. 

* Sanal ağ Ağ Güvenlik Grubu kurallarınızın Azure Veritabanı Geçiş Hizmeti'ne aşağıdaki gelen iletişim bağlantı noktalarını engellemediğinden emin olun: 443, 53, 9354, 445, 12000. Sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için, [ağ güvenlik grupları ile](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)makale Filtre ağ trafiği bakın.
* [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server erişimi sağlamak için Windows güvenlik duvarınızı açın. Varsayılan ayarlarda 1433 numaralı TCP bağlantı noktası kullanılır.
* Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanlarına erişmesini sağlama amacıyla Azure SQL Veritabanı için sunucu düzeyinde [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağın alt ağ aralığını sağlayın.
* Kaynak RDS SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerinin "Processadmin" sunucu rolünün bir üyesi olan bir hesapla ve geçirilecek tüm veritabanlarındaki "db_owner" veritabanı rollerinin bir üyesiyle ilişkili olduğundan emin olun.
* Hedeflenen Azure SQL Veritabanı örneğine bağlanmak için kullanılan kimlik bilgilerinin hedef Azure SQL veritabanlarında CONTROL DATABASE iznine ve Azure SQL Veritabanı yönetilen bir örneğe geçiş yaptığında sysadmin rolünün bir üyesine sahip olduğundan emin olun.
* Kaynak RDS SQL Server sürümü SQL Server 2012 ve üzeri olmalıdır. SQL Server örneğinizin sürümünü belirlemek için [SQL Server ve bileşenlerinin sürümünü ve güncelleştirme düzeyini belirleme](https://support.microsoft.com/help/321185/how-to-determine-the-version-edition-and-update-level-of-sql-server-an) başlıklı makaleye bakın.
* RDS SQL Server veritabanında ve geçiş için seçilen tüm kullanıcı tablosu(lar) üzerinde Veri Yakalamayı Değiştir'i (CDC) etkinleştirin.
    > [!NOTE]
    > Bir RDS SQL Server veritabanında CDC etkinleştirmek için aşağıdaki komut dosyasını kullanabilirsiniz.
    ```
    exec msdb.dbo.rds_cdc_enable_db 'AdventureWorks2012'
    ```
    > Tüm tablolarda CDC'yi etkinleştirmek için aşağıdaki komut dosyasını kullanabilirsiniz.
    ```
    use <Database name>
    go
    exec sys.sp_cdc_enable_table 
    @source_schema = N'Schema name', 
    @source_name = N'table name', 
    @role_name = NULL, 
    @supports_net_changes = 1 --for PK table 1, non PK tables 0
    GO
    ```
* Hedef Azure SQL Veritabanı'nda veritabanı tetikleyicilerini devre dışı bırakın.
    > [!NOTE]
    > Şu sorguyu kullanarak hedef Azure SQL Veritabanı'ndaki veritabanı tetikleyicilerini bulabilirsiniz:
    ```
    Use <Database name>
    select * from sys.triggers
    DISABLE TRIGGER (Transact-SQL)
    ```
    Daha fazla bilgi için [DISABLE TRIGGER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/disable-trigger-transact-sql?view=sql-server-2017) makalesine bakın.

## <a name="migrate-the-sample-schema"></a>Örnek şemayı geçirme
Şemayı Azure SQL Veritabanına geçirmek için DMA'yı kullanın.

> [!NOTE]
> DMA'da bir geçiş projesi oluşturmadan önce önkoşullarda belirtilen şekilde bir Azure SQL veritabanı sağladığınızdan emin olun. Bu öğretici nin amaçları doğrultusunda, Azure SQL Veritabanı'nın adının **AdventureWorks2012**olduğu varsayılır, ancak istediğiniz adı sağlayabilirsiniz.

**AdventureWorks2012** şemasını Azure SQL Veritabanına geçirmek için aşağıdaki adımları gerçekleştirin:

1. Data Migration Yardımcısı'nda Yeni (+) simgesini ve ardından **Proje türü** bölümünde **Geçiş**'i seçin.
2. Bir proje adı belirtin, **Kaynak sunucu türü** metin kutusunda **SQL Server**, **Hedef sunucu türü** metin kutusunda da **Azure SQL Veritabanı**'nı seçin.
3. **Geçiş Kapsamı** bölümünde **Yalnızca şema**'yı seçin.

    Yukarıdaki adımların gerçekleştirilmesinin ardından DMA arabiriminin aşağıda yer alan grafikteki gibi görünmesi gerekir:

    ![Data Migration Yardımcısı Projesi Oluşturma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-create-project.png)

4. Projeyi oluşturmak için **Oluştur**'u seçin.
5. DMA'da SQL Server'ınız için kaynak bağlantı ayrıntılarını belirtin, **Bağlan**'ı ve ardından **AdventureWorks2012** veritabanını seçin.

    ![Data Migration Yardımcısı Kaynak Bağlantı Ayrıntıları](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-source-connect.png)

6. Hedef **sunucuya Bağlan'ın**altında **İleri'yi**seçin, Azure SQL veritabanı için hedef bağlantı ayrıntılarını belirtin, **Bağlan'ı**seçin ve ardından Azure SQL Veritabanı'nda önceden sağlanmış **olduğunuz AdventureWorksAzure** veritabanını seçin.

    ![Data Migration Yardımcısı Hedef Bağlantı Ayrıntıları](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-target-connect.png)

7. **İleri**'yi seçerek **Nesneleri seçin** ekranına ilerleyin ve bu ekranda Azure SQL Veritabanına dağıtılacak **AdventureWorks2012** veritabanındaki şema nesnelerini belirtin.

    Varsayılan olarak, tüm nesneler seçilir.

    ![SQL Betiği Oluşturma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-assessment-source.png)

8. **SQL betiği oluştur**'u seçerek SQL betiklerini oluşturun ve hata olup olmadığını inceleyin.

    ![Şema Betiği](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-schema-script.png)

9. **Şemayı dağıtma**'yı seçerek şemayı Azure SQL Veritabanına dağıtın. Şema dağıtıldıktan sonra hedef sunucuda anomali olup olmadığını kontrol edin.

    ![Şemayı Dağıtma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/portal-select-subscription1.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-sql-server-to-azure-sql-online/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Örnek oluşturma

1. Azure portalında + **Kaynak Oluştur'** seçeneğini belirleyin, Azure Veritabanı Geçiş Hizmeti'ni arayın ve ardından açılan listeden **Azure Veritabanı Geçiş Hizmeti'ni** seçin.

    ![Azure Market](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create1.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz konumu seçin. 

5. Varolan bir sanal ağ seçin veya yeni bir ağ oluşturun.

    Sanal ağ, kaynak SQL Server'a ve hedef Azure SQL Veritabanı örneğine erişim sağlayan Azure Veritabanı Geçiş Hizmeti sağlar.

    Azure portalında sanal ağ oluşturma hakkında daha fazla bilgi için Azure [portalını kullanarak sanal ağ oluşturma makalesine](https://aka.ms/DMSVnet)bakın.

6. Bir fiyatlandırma katmanı seçin; Bu çevrimiçi geçiş için Premium fiyatlandırma katmanını seçtiğinizden emin olun.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

     ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-settings3.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

      ![Azure Veritabanı Geçiş Hizmeti’nin tüm örneklerini bulma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-search.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın ve sonuçlardan bu örneği seçin.

     ![Azure Veritabanı Geçiş Hizmeti örneğinizi bulma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-instance-search.png)

3. +**Yeni Geçiş Projesi**'ni seçin.
4. Yeni **geçiş projesi** ekranında, **Kaynak sunucu türü** metin kutusunda proje için bir ad belirtin, SQL Server için **AWS RDS'yi**seçin, **Hedef sunucu türü** metin kutusunda, Azure **SQL Veritabanı'nı**seçin.

    > [!NOTE]
    > Hedef sunucu türü için, hem Azure SQL Veritabanı singleton veritabanına hem de Azure SQL Veritabanı yönetilen örneğine geçiş yapmak için **Azure SQL** Veritabanı'nı seçin.

5. Etkinlik **türünü seç** **bölümünde, Çevrimiçi veri geçişi'ni**seçin.

    > [!IMPORTANT]
    > Çevrimiçi veri **geçişi**seçtiğinizden emin olun; çevrimdışı geçişler bu senaryo için desteklenmez.

    ![Veritabanı Geçiş Hizmeti Projesi Oluşturma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create-project4.png)

    > [!NOTE]
    > Alternatif olarak, yalnızca geçiş projesini şimdi oluşturmak ve geçişi daha sonra yürütmek için **proje oluştur'u** seçebilirsiniz.

6. **Kaydet'i**seçin.

7. Projeyi oluşturmak ve geçiş etkinliğini çalıştırmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

    ![Veritabanı Geçiş Hizmeti Etkinliği Oluşturma ve Çalıştırma](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create-and-run-activity1.png)

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. **Geçiş kaynağı ayrıntıları** ekranında SQL Server örneğinin bağlantı ayrıntılarını belirtin.

    Kaynak SQL Server örneği adı için Tam Etki Alanı Adı (FQDN) kullandığınızdan emin olun.

2. Kaynak sunucunuza güvenilir bir sertifika yüklemediyseniz **Sunucu sertifikasına güven** onay kutusunu işaretleyin.

    Güvenilir sertifika yüklü değilse SQL Server, örnek başlatıldığında otomatik olarak imzalanan bir sertifika oluşturur. Bu sertifika, istemci bağlantılarında kimlik bilgilerini şifrelemek için kullanılır.

    > [!CAUTION]
    > Kendi imzalı bir sertifika kullanılarak şifrelenen TLS bağlantıları güçlü güvenlik sağlamaz. Ortadaki adam saldırılarına maruz kalabilirler. Üretim ortamında veya internete bağlı sunucularda kendi imzalı sertifikaları kullanarak TLS'ye güvenmemelisiniz.

   ![Kaynak Ayrıntıları](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-source-details3.png)

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. **Kaydet**'i seçin ve **Geçiş hedef ayrıntıları** ekranında DMA kullanılarak **AdventureWorks2012** şemasının dağıtıldığı önceden sağlanmış Azure SQL Veritabanı olan hedef Azure SQL Veritabanı sunucusunun bağlantı ayrıntılarını belirtin.

    ![Hedef seçme](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-select-target3.png)

2. **Kaydet**'i seçin ve **Hedef veritabanlarıyla eşleyin** ekranında geçiş yapılacak kaynak ve hedef veritabanlarını eşleyin.

    Hedef veritabanı, kaynak veritabanıyla aynı veritabanı adına sahipse Azure Veritabanı Geçiş Hizmeti varsayılan olarak hedef veritabanını seçer.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-map-targets-activity4.png)

3. **Kaydet**'i seçin ve **Tablo seçme** ekranında tablo listesini genişletip etkilenen alan listesini inceleyin.

    Azure Veritabanı Geçiş Hizmeti hedef Azure SQL Veritabanı örneğinde bulunan tüm boş kaynak tablolarını seçer. Zaten veri içeren tabloları yeniden geçirmek istiyorsanız, bu ekranda tabloları açıkça seçmeniz gerekir.

    ![Tabloları seçme](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-configure-setting-activity4.png)

4. Aşağıdaki **Gelişmiş çevrimiçi geçiş ayarlarını**ayarladıktan sonra **Kaydet'i**seçin.

    | Ayar | Açıklama |
    | ------------- | ------------- |
    | **Paralel olarak yüklenmesi gereken maksimum tablo sayısı** | Geçiş sırasında DMS'nin paralel olarak yürüttüğü tablo sayısını belirtir. Varsayılan değer 5'tir, ancak poc geçişlerini temel alan belirli geçiş gereksinimlerini karşılamak için en uygun değere ayarlanabilir. |
    | **Kaynak tablo kesildi** | DMS'nin geçiş sırasında hedef tabloyu tonup bağlamadığını belirtir. Geçiş işleminin bir parçası olarak bir veya daha fazla tablo kesilirse, bu ayar yararlı olabilir. |
    | **Büyük nesneler (LOB) verileri için ayarları yapılandırma** | DMS'nin sınırsız LOB verisi geçirip geçirmediğini veya geçirilen LOB verilerini belirli bir boyuta geçirip sınırlamadığını belirtir.  Geçirilen LOB verilerinde bir sınır olduğunda, bu sınırın ötesindeki tüm LOB verileri kesilir. Üretim geçişleri için, veri kaybını önlemek için **sınırsız LOB boyutuna izin ver'i** seçmeniz önerilir. Sınırsız LOB boyutuna izin vermek için belirtirken, **lob boyutu (KB) belirtilen** onay kutusundan daha az olduğunda, performansı artırmak için lob boyutu tek bir blokta Geçir LOB verilerini seçin. |

    ![Gelişmiş çevrimiçi geçiş ayarlarını ayarlama](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-advanced-online-migration-settings.png)

5. **Kaydet**'i seçin, **Geçiş özeti** ekranındaki **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin ve ardından, kaynak ve hedef ayrıntılarının önceden belirttiğiniz ayrıntılarla eşleştiğinden emin olmak üzere özeti gözden geçirin.

    ![Geçiş Özeti](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-migration-summary.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi görüntülenir ve etkinliğin **Durumu** **Başharftir.**

    ![Etkinlik Durumu - başlatılıyor](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-activity-status2.png)

## <a name="monitor-the-migration"></a>Geçişi izleme

1. Geçiş etkinliği ekranında **Yenile**'yi seçerek, gösterilen verileri, geçişin **Durum** bilgisi **Çalıştırılıyor** olana kadar güncelleştirebilirsiniz.

2. **Tam veri yüklemesi** ve **Artımlı veri eşitleme** işlemleri için geçiş durumunu almak üzere belirli bir veritabanına tıklayın.

    ![Etkinlik Durumu - devam ediyor](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-activity-in-progress.png)

## <a name="perform-migration-cutover"></a>Tam geçiş gerçekleştirme

İlk Tam yük tamamlandıktan sonra, veritabanları **Geçiş için hazır** olarak işaretlenir.

1. Veritabanı geçişini tamamlamaya hazır olduğunuzda **Tam Geçişi Başlat** seçeneğini belirleyin.

    ![Tam geçişi başlat](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-start-cutover.png)

2. **Bekleyen değişiklikler** sayacı **0** değerini gösterene kadar bekleyerek kaynak veritabanına gelen tüm işlemleri durdurduğunuzdan emin olun.
3. **Onayla**'yı ve ardından, **Uygula**'yı seçin.
4. Veritabanı geçişi durumu **Tamamlandı** olarak gösterildiğinde, uygulamalarınızı yeni hedef Azure SQL Veritabanı'na bağlayın.

    ![Etkinlik Durumu - tamamlandı](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-activity-completed.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure SQL DatabaseL'e çevrimiçi geçişler gerçekleştirirken bilinen sorunlar ve sınırlamalar hakkında bilgi için, Bilinen makaleye bakın [Azure SQL Veritabanı çevrimiçi geçişleri ile](known-issues-azure-sql-online.md)ilgili sorunlar ve geçici geçici işler.
* Azure Veritabanı Geçiş Hizmeti hakkında bilgi için [What is the Azure Database Migration Service? (Azure Veritabanı Geçiş Hizmeti nedir?)](https://docs.microsoft.com/azure/dms/dms-overview) başlıklı makaleye bakın.
* Azure SQL Veritabanı hakkında bilgi için makaleye bakın [Azure SQL Veritabanı hizmeti nedir?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
* Azure SQL Veritabanı yönetilen örnekleri hakkında bilgi için [Azure SQL Veritabanı Yönetilen Örnek](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index)sayfasına bakın.
