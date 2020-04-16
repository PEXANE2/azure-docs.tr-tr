---
title: Azure-SSIS tümleştirme çalışma süresini sağlama
description: Azure'da SSIS paketleri dağıtıp çalıştırabilmek için Azure Data Factory'de Azure-SSIS tümleştirme çalışma zamanı sağlamayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 12/23/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.openlocfilehash: 91962b578f9620384af86d25a33ba35830abb285
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418635"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Azure-SSIS tümleştirme çalışma süresini sağlama

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu öğretici, Azure Veri Fabrikası'nda bir Azure-SQL Server Tümleştirme Hizmetleri (SSIS) tümleştirme çalışma zamanı (IR) sağlamak için Azure portalını kullanmak için adımlar sağlar. Azure-SSIS IR şunları destekler:

- Azure SQL Veritabanı sunucusu veya yönetilen bir örnek (Project Deployment Model) tarafından barındırılan SSIS kataloğuna (SSISDB) dağıtılan paketleri çalıştırma.
- Dosya sistemlerine, dosya paylaşımlarına veya Azure Dosyalarına (Paket Dağıtım Modeli) dağıtılan paketleri çalıştırma. 

Azure-SSIS IR sağlandıktan sonra, paketlerinizi Azure'da dağıtmak ve çalıştırmak için tanıdık araçları kullanabilirsiniz. Bu araçlar arasında SQL Server Data Tools (SSDT), SQL Server Management `dtinstall`Studio `dtutil`(SSMS) ve , , ve `dtexec`.

Azure-SSIS IR’ler hakkında kavramsal bilgiler için bkz. [Azure SSIS tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md#azure-ssis-integration-runtime).

Bu öğreticide, aşağıdaki adımları tamamlayacaksınız:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure-SSIS tümleştirme çalışma zamanı sağlama.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği.** Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
- **Azure SQL Veritabanı sunucusu (isteğe bağlı)**. Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portalında bir tane oluşturun. Veri Fabrikası da bu veritabanı sunucusunda bir SSISDB örneği oluşturur. 

  Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, Azure bölgelerini geçmeden ssisdb'ye yürütme günlüklerini yazma işlemlerinin tümleştirmesini sağlar.

  Bu noktaları aklınızda bulundurun:

  - Seçili veritabanı sunucusuna bağlı olarak, SSISDB örneği sizin adınıza tek bir veritabanı olarak, elastik bir havuzun parçası olarak veya yönetilen bir örnekte oluşturulabilir. Ortak bir ağda veya sanal ağa katılarak erişilebilir. SSISDB'yi barındıracak veritabanı sunucusu türünü seçmede kılavuz için [bkz.](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance) 
  
    IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanı sunucusu veya SSISDB'yi barındırmak için özel bitiş noktası olan yönetilen bir örnek kullanıyorsanız veya kendi kendine barındırılan bir IR yapılandırmadan şirket içi verilere erişmeniz gerekiyorsa, Azure-SSIS IR'nizi sanal bir ağa katılmanız gerekir. Daha fazla bilgi için bkz. [sanal ağda Azure-SSIS IR oluştur.](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)

  - Veritabanı sunucusu **için Azure hizmetlerine Erişime İzin Ver** ayarının etkinleştirildiğinden onaylayın. Bu ayar, IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanı sunucusu veya SSISDB'yi barındıracak özel bitiş noktası olan yönetilen bir örnek kullandığınızda geçerli değildir. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). PowerShell kullanarak bu ayarı etkinleştirmek için [Bkz. Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).

  - İstemci makinesinin IP adresini veya istemci makinesinin IP adresini içeren bir dizi IP adresini veritabanı sunucusunun güvenlik duvarı ayarlarındaki istemci IP adres listesine ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md).

  - Sunucu yönetici kimlik bilgilerinizle SQL kimlik doğrulaması kullanarak veya veri fabrikanızın yönetilen kimliğiyle Azure AD kimlik doğrulamasını kullanarak veritabanı sunucusuna bağlanabilirsiniz. İkincisi için, veritabanı sunucusuna erişim izinleri olan bir Azure REKLAM grubuna veri fabrikanız için yönetilen kimliği eklemeniz gerekir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)

  - Veritabanı sunucunuzun zaten bir SSISDB örneği olmadığını doğrulayın. Bir Azure-SSIS IR'nin sağlanması, varolan bir SSISDB örneğini kullanmayı desteklemez.


> [!NOTE]
> Veri Fabrikası ve Azure-SSIS IR'nin şu anda kullanılabildiği Azure bölgelerinin listesi için, [bölgeye göre Veri Fabrikası ve SSIS IR kullanılabilirliğine](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all)bakın. 

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

Azure portalı üzerinden veri fabrikanızı oluşturmak için, [UI üzerinden veri fabrikası oluştur'daki](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory)adım adım yönergeleri izleyin. Oluşturulduktan sonra hızlı erişim sağlamak için **panoya Pin'i** seçin. 

Veri fabrikanız oluşturulduktan sonra, Azure portalında genel bakış sayfasını açın. Ayrı bir sekmede **başlatalım** sayfasını açmak için **Yazar & İzle** döşemesini seçin. Burada, Azure-SSIS IR'nizi oluşturmaya devam edebilirsiniz.

## <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma

### <a name="from-the-data-factory-overview"></a>Veri Fabrikası'na genel bakış

1. **Başlarken** sayfasında **SSIS Tümleştirme Çalışma Zamanı Yapılandır** kutucuğunu seçin. 

   !["SSIS Tümleştirme Çalışma Zamanı Yapılandır" kutucuğu](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. Azure SSIS IR kurulumunun diğer adımları için [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime) bölümüne bakın. 

### <a name="from-the-authoring-ui"></a>Yazarlı UI gönderen

1. Azure Veri Fabrikası UI'sinde, **Değiştir** sekmesine geçin ve **Bağlantılar'ı**seçin. Ardından, veri fabrikanızdaki varolan tümleştirme çalışma sürelerini görüntülemek için **Tümleştirme Çalışma Saatleri** sekmesine geçin. 

   ![Var olan IR’leri görüntüleme seçimleri](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Bir Azure-SSIS IR oluşturmak için **Yeni**'yi seçin. 

   ![Menü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. **Tümleştirme Çalışma Zamanı Kurulumu** **panelinde, Azure döşemesinde yürütmek için varolan SSIS paketlerini kaldır ve kaydır** ve sonra **İleri'yi**seçin. 

   ![Tümleştirme çalışma zamanının türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Azure SSIS IR kurulumunun diğer adımları için [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime) bölümüne bakın. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı sağlama

1. **Tümleştirme Çalışma Zamanı Kurulum** panelinin **Genel Ayarlar** bölümünde aşağıdaki adımları tamamlayın. 

   ![Genel ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   1. **Name** (Ad) alanına tümleştirme çalışma zamanınızın adını girin. 

   1. **Description** (Açıklama) alanına tümleştirme çalışma zamanınız için bir açıklama girin. 

   1. **Location** (Konum)alanında tümleştirme çalışma zamanınızın konumunu seçin. Yalnızca desteklenen konumlar görüntülenir. SSISDB'yi barındırmak için veritabanı sunucunuz ile aynı konumu seçmenizi öneririz. 

   1. **Node Size** (Düğüm Boyutu) alanında tümleştirme çalışma zamanınızdaki düğümün boyutunu seçin. Yalnızca desteklenen düğüm boyutları görüntülenir. Çok sayıda işlem yoğun veya bellek yoğun paketleri çalıştırmak istiyorsanız büyük bir düğüm boyutu (ölçeklendirmek) seçin. 

   1. **Node Number** (Düğüm Sayısı) alanında tümleştirme çalışma zamanınızdaki düğüm sayısını seçin. Yalnızca desteklenen düğüm sayıları görüntülenir. Birçok paketi paralel olarak çalıştırmak istiyorsanız, çok sayıda düğümiçeren büyük bir küme seçin (ölçeklendirin). 

   1. **Edition/License**için, entegrasyon çalışma süreniz için SQL Server sürümünü seçin: Standart veya Kurumsal. Tümleştirme çalışma zamanınızda gelişmiş özellikler kullanmak istiyorsanız Enterprise'ı seçin. 

   1. **Paradan Tasarruf**etmek için, tümleştirme çalışma süreniz için Azure Karma Avantajı seçeneğini seçin: **Evet** veya **Hayır**. Hibrit kullanımla maliyet tasarrufundan yararlanmak için Yazılım Güvencesi ile kendi SQL Server lisansınızı getirmek istiyorsanız **Evet'i** seçin. 

   1. **İleri**’yi seçin. 

1. SQL **Ayarları** bölümünde aşağıdaki adımları tamamlayın. 

   ![SQL ayarları](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   1. Azure-SSIS IR'nizde çalışacak paketler için dağıtım modelini seçmek için **projelerinizi/paketlerinizi/ortamlarınızı/yürütme günlüklerinizi depolamak için Azure SQL Veritabanı sunucusu/Yönetilen Örnek tarafından barındırılan SSIS Kataloğu Oluştur'u (SSISDB)** seçin. Paketlerin veritabanı sunucunuz tarafından barındırılan SSISDB'ye dağıtıldığı Proje Dağıtım Modeli'ni veya paketlerin dosya sistemlerine, dosya paylaşımlarına veya Azure Dosyalarına dağıtıldığı Paket Dağıtım Modelini seçersiniz.
   
      Onay kutusunu seçerseniz, sizin adınıza oluşturup yöneteceğimiz SSISDB'yi barındırmak için kendi veritabanı sunucunuzu getirmeniz gerekir.
   
      1. **Subscription** (Abonelik) alanında SSISDB'yi barındıran Azure aboneliğini seçin. 

      1. **Location** (Konum) alanında SSISDB'yi barındıran veritabanı sunucunuzun konumunu seçin. Tümleştirme çalışma zamanınızla aynı konumu seçmenizi öneririz.

      1. **Catalog Database Server Endpoint** (Katalog Veritabanı Sunucusu Uç Noktası) alanında SSISDB'yi barındıracak veritabanı sunucunuzun uç noktasını seçin. 
   
         Seçili veritabanı sunucusuna bağlı olarak, SSISDB örneği sizin adınıza tek bir veritabanı olarak, elastik bir havuzun parçası olarak veya yönetilen bir örnekte oluşturulabilir. Ortak bir ağda veya sanal ağa katılarak erişilebilir. SSISDB'yi barındıracak veritabanı sunucusu türünü seçmede kılavuz için [bkz.](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance)   

         IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanı sunucusu veya SSISDB'yi barındırmak için özel bitiş noktası olan yönetilen bir örnek seçerseniz veya kendi kendine barındırılan bir IR yapılandırmadan şirket içi verilere erişmeniz gerekiyorsa, Azure-SSIS IR'nizi sanal bir ağa katılmanız gerekir. Daha fazla bilgi için bkz. [sanal ağda Azure-SSIS IR oluştur.](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)

      1. Veritabanı sunucunuzun SSISDB'yi barındıracak kimlik doğrulama yöntemini seçmek için ADF onay **kutunuzun yönetilen kimliğiyle AAD kimlik bilgilerini kullanın.** Veri fabrikanızın yönetilen kimliğiyle SQL kimlik doğrulaması veya Azure AD kimlik doğrulaması seçeneğini seçersiniz.

         Onay kutusunu seçerseniz, veri fabrikanızın yönetilen kimliğini veritabanı sunucunuza erişim izinleri içeren bir Azure AD grubuna eklemeniz gerekir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)
   
      1. **Yönetici Kullanıcı Adı**için, SSISDB'yi barındırmak için veritabanı sunucunuzun SQL kimlik doğrulama kullanıcı adını girin. 

      1. **Yönetici Parlebesi**için, SSISDB'yi barındırmak için veritabanı sunucunuzun SQL kimlik doğrulama parolasını girin. 

      1. **Katalog Veritabanı Hizmet Katmanı**için, SSISDB'yi barındıracak veritabanı sunucunuzun hizmet katmanını seçin. Temel, Standart veya Premium katmanı seçin veya esnek bir havuz adı seçin.

      1. **Test Bağlantısı'nı**seçin. Test başarılı olursa, **İleri'yi**seçin. 

1. Gelişmiş **Ayarlar** bölümünde aşağıdaki adımları tamamlayın. 

   ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   1. **Düğüm Başına Maksimum Paralel Yürütmeler**için, tümleştirme çalışma zamanı kümenizde düğüm başına aynı anda çalışacak en fazla paket sayısını seçin. Yalnızca desteklenen paket sayıları görüntülenir. İşlem veya bellek yoğun olan tek bir büyük paketi çalıştırmak için birden fazla çekirdek kullanmak istiyorsanız düşük bir sayı seçin. Tek bir çekirdekte bir veya daha fazla küçük paket çalıştırmak istiyorsanız yüksek bir sayı seçin. 

   1. Azure-SSIS IR'nize standart/ifade özel kurulumları eklemek isteyip istemediğinizi seçmek için **ek sistem yapılandırmaları/bileşen yüklemeleri onay kutusunu ile Azure-SSIS Tümleştirme Çalışma sürenizi özelleştir'i** seçin. Daha fazla bilgi için [Azure-SSIS IR için Özel kuruluma](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)bakın.
   
   1. Katılmak **için Azure-SSIS Tümleştirme Çalışma süreniz için Bir VNet Seç'i seçin, ADF'nin belirli ağ kaynakları oluşturmasına izin verin ve isteğe bağlı olarak Azure-SSIS IR'nize** sanal bir ağa katılmak isteyip istemediğinizi seçmek için kendi statik genel IP adresleri onay kutunuzu getirin.

      IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanı sunucusu veya SSISDB'yi barındıracak özel bitiş noktası olan yönetilen bir örnek kullanıyorsanız veya kendi barındırılan bir IR yapılandırmadan şirket içi verilere erişmeye ihtiyaç duyuyorsanız seçin. Daha fazla bilgi için bkz. [sanal ağda Azure-SSIS IR oluştur.](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime) 
   
   1. **Azure-SSIS IR'niz için** proxy olarak kendi kendine barındırılan Bir IR'yi yapılandırmak isteyip istemediğinizi seçmek için Azure-SSIS Tümleştirme Çalışma zamanı onay kutunuz için proxy olarak Kendi Kendine Barındırılan Tümleştirme Çalışma Süresini Ayarlayın'ı seçin. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis)   

   1. **Devam**'ı seçin. 

1. **Özet** bölümünde, tüm sağlama ayarlarını gözden geçirin, önerilen belge bağlantılarını yer imi yapın ve tümleştirme çalışma sürenizi oluşturmaya başlamak için **Finish'i** seçin. 

   > [!NOTE]
   > Herhangi bir özel kurulum süresi hariç, bu işlem 5 dakika içinde bitirilmelidir.
   >
   > SSISDB kullanıyorsanız, Veri Fabrikası hizmeti SSISDB hazırlamak için veritabanı sunucunuza bağlanır. 
   > 
   > Bir Azure-SSIS IR'i sağınızda, SSIS için Access Redistributable ve Azure Özellik Paketi de yüklenir. Bu bileşenler, yerleşik bileşenlerin zaten desteklediği veri kaynaklarına ek olarak Excel dosyalarına, Access dosyalarına ve çeşitli Azure veri kaynaklarına bağlantı sağlar. Yükleyebileceğiniz diğer bileşenler hakkında daha fazla bilgi için [Azure-SSIS IR için Özel kuruluma](how-to-configure-azure-ssis-ir-custom-setup.md)bakın.

1. **Bağlantılar** sekmesinde, gerekirse **Tümleştirme Çalışma Zamanları**’na geçin. Durumu yenilemek için **Yenile**’yi seçin. 

   !["Yenile" düğmesi ile oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Tümleştirme çalışma zamanını durdurmak/başlatmak, düzenlemek veya silmek için **Eylemler** sütunundaki bağlantıları kullanın. Tümleştirme çalışma zamanının JSON kodunu görüntülemek için son bağlantıyı kullanın. Düzenle ve sil düğmeleri yalnızca IR durdurulmuş durumdayken etkin olur. 

   !["Eylemler" sütunundaki bağlantılar](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png) 

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma

SSISDB kullanıyorsanız, paketlerinizi bu pakete dağıtabilir ve SQL Server Data Tools (SSDT) veya SQL Server Management Studio (SSMS) araçlarını kullanarak Azure-SSIS IR'de çalıştırabilirsiniz. Bu araçlar, sunucu bitiş noktası üzerinden veritabanı sunucunuza bağlanır: 

- Bir Azure SQL Veritabanı sunucusu için sunucu `<server name>.database.windows.net`bitiş noktası biçimi.
- Özel bitiş noktası olan yönetilen bir örnek için `<server name>.<dns prefix>.database.windows.net`sunucu bitiş noktası biçimi.
- Genel bitiş noktası olan yönetilen bir örnek için `<server name>.public.<dns prefix>.database.windows.net,3342`sunucu bitiş noktası biçimi. 

SSISDB kullanmıyorsanız, paketlerinizi dosya sistemlerine, dosya paylaşımlarına veya Azure Dosyalarına dağıtabilir ve `dtinstall`"ve `dtutil` `dtexec` komut satırı" araçlarını kullanarak Azure-SSIS IR'de çalıştırabilirsiniz. Daha fazla bilgi için [bkz.](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) 

Her iki durumda da, Dağıtılan paketlerinizi Veri Fabrikası ardışık hatlarındaki SSIS Paketi etkinliğini kullanarak Azure-SSIS IR'de çalıştırabilirsiniz. Daha fazla bilgi için, [birinci sınıf bir Veri Fabrikası etkinliği olarak Invoke SSIS paket yürütme](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)bakın.

Ayrıca aşağıdaki SSIS belgelerine bakın: 

- [Azure'da SSIS paketlerini dağıtma, çalıştırma ve izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) 
- [Azure'da SSISDB'ye bağlanın](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Azure'da paket yürütmelerini zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) 
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="next-steps"></a>Sonraki adımlar

Azure-SSIS tümleştirme çalışma zamanınızı özelleştirme hakkında bilgi edinmek için şu makaleye bakın: 

> [!div class="nextstepaction"]
> [Azure-SSIS IR'yi özelleştirme](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)