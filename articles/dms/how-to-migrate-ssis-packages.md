---
title: SSIS paketlerini SQL tek veritabanına yeniden dağıtma
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti ve Veri Geçiş Yardımcısı'nı kullanarak SQL Server Integration Services paketlerini ve projelerini Azure SQL Veritabanı tek veritabanına nasıl geçirtecek veya yeniden dağıtabileceğinizi öğrenin.
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
ms.openlocfilehash: 90a39b8fe3604a05f1d35a875ae4e34491b47d72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77648538"
---
# <a name="redeploy-ssis-packages-to-azure-sql-database-with-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti ile SSIS paketlerini Azure SQL Veritabanına yeniden dağıtma

SQL Server Tümleştirme Hizmetleri 'ni (SSIS) kullanıyorsanız ve SSIS projelerinizi/paketlerinizi SQL Server tarafından barındırılan kaynak SSISDB'den Azure SQL Veritabanı tarafından barındırılan ssisdb hedefine geçirmek istiyorsanız, bunları Tümleştirme Hizmetleri Dağıtım Sihirbazı'nı kullanarak yeniden dağıtabilirsiniz. Sihirbazı SQL Server Management Studio (SSMS) içinden başlatabilirsiniz.

Kullandığınız SSIS sürümü 2012'den önceyse, SSIS projelerinizi/paketlerinizi proje dağıtım modeline yeniden dağıtmadan önce, öncelikle SSMS'ten başlatılabilen Tümleştirme Hizmetleri Proje Dönüştürme Sihirbazı'nı kullanarak bunları dönüştürmeniz gerekir. Daha fazla bilgi için, [projeleri proje dağıtım modeline dönüştüren makaleye](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert)bakın.

> [!NOTE]
> Azure Veritabanı Geçiş Hizmeti (DMS) şu anda bir kaynak SSISDB'nin Bir Azure SQL Veritabanı sunucusuna geçişini desteklemez, ancak aşağıdaki işlemi kullanarak SSIS projelerinizi/paketlerinizi yeniden dağıtabilirsiniz.

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
>
> * Kaynak SSIS projelerini/paketlerini değerlendirin.
> * SSIS projelerini/paketlerini Azure'a geçirin.

## <a name="prerequisites"></a>Ön koşullar

Bu adımları tamamlamak için şunları yapmanız gerekir:

* SSMS sürüm 17.2 veya sonrası.
* SSISDB'yi barındıracak hedef veritabanı sunucunuzun bir örneği. Zaten yoksa, SQL Server (yalnızca mantıksal sunucu) [formuna](https://ms.portal.azure.com/#create/Microsoft.SQLServer)giderek Azure portalını kullanarak bir Azure SQL Veritabanı sunucusu (veritabanı olmadan) oluşturun.
* SSIS, Azure Veri Fabrikası'nda (ADF) Azure-SSIS Tümleştirme Çalışma Süresi (IR) içeren ve Azure SQL Veritabanı sunucusu örneğinde barındırılan hedef SSISDB ile birlikte sağlanmalıdır (makalede açıklandığı gibi [Azure Veri Fabrikası'nda Azure-SSIS Tümleştirme Çalışma Süresi).](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

## <a name="assess-source-ssis-projectspackages"></a>Kaynak SSIS projelerini/paketlerini değerlendirin

Kaynak SSISDB'nin değerlendirmesi henüz Veritabanı Geçiş Yardımcısına (DMA) veya Azure Veritabanı Geçiş Hizmetine (DMS) entegre edilmese de, SSIS projeleriniz/paketleriniz, Azure SQL Veritabanı sunucusunda barındırılan hedefe yeniden dağıtıldıkça değerlendirilecektir/doğrulanır.

## <a name="migrate-ssis-projectspackages"></a>SSIS projelerini/paketlerini geçirin

SSIS projelerini/paketlerini Azure SQL Veritabanı sunucusuna geçirmek için aşağıdaki adımları gerçekleştirin.

1. SSMS'i açın ve ardından **Sunucuya Bağlan** iletişim kutusunu görüntülemek için **Seçenekler'i** seçin.

2. **Giriş** sekmesinde, hedef SSISDB'yi barındıracak Azure SQL Veritabanı sunucusuna bağlanmak için gereken bilgileri belirtin.

    ![SSIS Giriş sekmesi](media/how-to-migrate-ssis-packages/dms-ssis-login-tab.png)

3. Bağlantı **Özellikleri** sekmesinde, **veritabanımetin kutusuna bağlan,** **SSISDB'yi**seçin veya girin ve sonra **Bağlan'ı**seçin.

    ![SSIS Bağlantı Özellikleri sekmesi](media/how-to-migrate-ssis-packages/dms-ssis-conncetion-properties-tab.png)

4. SSMS Object Explorer'da, **Tümleştirme Hizmetleri Katalogları** düğümünü genişletin, **SSISDB'yi**genişletin ve varolan klasör yoksa **SSISDB'yi** sağ tıklatın ve yeni bir klasör oluşturun.

5. **SSISDB**altında, herhangi bir klasörü genişletin, **Projeler'i**sağ tıklatın ve ardından **Project'i Dağıt'ı**seçin.

    ![SSIS SSISDB düğümü genişletildi](media/how-to-migrate-ssis-packages/dms-ssis-ssisdb-node-expanded.png)

6. Tümleştirme Hizmetleri Dağıtım Sihirbazı'nda, **Giriş** sayfasında, bilgileri gözden geçirin ve sonra **İleri'yi**seçin.

    ![Dağıtım Sihirbazı Giriş sayfası](media/how-to-migrate-ssis-packages/dms-deployment-wizard-introduction-page.png)

7. Kaynak **Seç** sayfasında, dağıtmak istediğiniz varolan SSIS projesini belirtin.

    SSMS de kaynak SSISDB barındıran SQL Server bağlı ise, **Entegrasyon Hizmetleri kataloğunu**seçin ve sonra doğrudan projenizi dağıtmak için kataloğunuza sunucu adı ve proje yolu girin.

    Alternatif olarak, **Proje dağıtım dosyasını**seçin ve ardından projenizi dağıtmak için varolan bir proje dağıtım dosyasına (.ispac) giden yolu belirtin.

    ![Dağıtım Sihirbazı Kaynak sayfa seçin](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-source-page.png)
 
8. **Sonraki'ni**seçin.
9. Hedef **Seç** sayfasında, projenizin hedefini belirtin.

    a. Sunucu adı metin kutusuna, tam nitelikli Azure SQL Veritabanı sunucu adını girin (server_name>.database.windows.net <).

    b. Kimlik doğrulama bilgilerini sağlayın ve sonra **Bağlan'ı**seçin.

    ![Dağıtım Sihirbazı Hedef sayfasını seçin](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-destination-page.png)

    c. SSISDB'de hedef klasörü belirtmek için **Gözat'ı** seçin ve ardından **İleri'yi**seçin.

    > [!NOTE]
    > **Sonraki** düğmesi yalnızca **Bağlan'ı**seçtikten sonra etkinleştirilir.

10. **Doğrulama** sayfasında, hataları/uyarıları görüntüleyin ve gerekirse paketlerinizi buna göre değiştirin.

    ![Dağıtım Sihirbazı Doğrulama sayfası](media/how-to-migrate-ssis-packages/dms-deployment-wizard-validate-page.png)

11. **Sonraki'ni**seçin.

12. **İnceleme** sayfasında dağıtım ayarlarınızı gözden geçirin.

    > [!NOTE]
    > **Önceki'yi** seçerek veya sol bölmedeki adım bağlantılarından birini seçerek ayarlarınızı değiştirebilirsiniz.

13. Dağıtım işlemini başlatmak için **Dağıt'ı** seçin.

14. Dağıtım işlemi tamamlandıktan sonra, her dağıtım eyleminin başarısını veya başarısızlığını görüntüleyen Sonuçlar sayfasını görüntüleyebilirsiniz.
    a. **Sonuç** sütununda herhangi bir eylem başarısız olduysa, hatanın açıklamasını görüntülemek için **Başarısız'ı** seçin.
    b. İsteğe bağlı olarak, sonuçları bir XML dosyasına kaydetmek için **Raporu Kaydet'i** seçin.

15. Tümleştirme Hizmetleri Dağıtım Sihirbazı'ndan çıkmak için **Kapat'ı** seçin.

Projenizin dağıtımı hatasız olarak başarılı olursa, Azure-SSIS IR'nizde çalışacak paketleri seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft [Veritabanı Geçiş Kılavuzu'ndaki](https://datamigration.microsoft.com/)geçiş kılavuzunu gözden geçirin.
