---
title: Azure-SSIS Tümleştirme Çalışma Zamanı Sağlama | Microsoft Docs
description: Azure'da SSIS paketleri dağıtıp çalıştırabilmek için Azure Data Factory'de Azure-SSIS tümleştirme çalışma zamanı sağlamayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: tutorial
ms.date: 09/15/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: b3b180fe4b465f3e0c7de888043326fd1a43cf23
ms.sourcegitcommit: a819209a7c293078ff5377dee266fa76fd20902c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71009647"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Data Factory'de Azure-SSIS Tümleştirme Çalışma Zamanı Sağlama

Bu öğreticide, Azure Data Factory (ADF) içinde bir Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR) sağlamak için Azure portal kullanma adımları sunulmaktadır. Azure-SSIS IR, Azure SQL veritabanı sunucusu/yönetilen örnek (proje dağıtım modeli) tarafından barındırılan ve dosya sistemlerine/dosya paylaşımlarına/Azure dosyalarına (paket dağıtım modeli) dağıtılan SSIS kataloğuna (SSSıSDB) dağıtılmış paketlerin çalıştırılmasını destekler. Azure-SSIS IR sağlandıktan sonra, SQL Server veri araçları (SSDT)/SQL Server Management Studio (SSMS) gibi tanıdık araçları ve gibi komut satırı yardımcı programlarını `dtinstall` / `dtutil` / `dtexec`kullanabilirsiniz. Azure 'da paketlerinizi dağıtın ve çalıştırın. Azure-SSIS IR’ler hakkında kavramsal bilgiler için bkz. [Azure SSIS tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md#azure-ssis-integration-runtime).

Bu öğreticide, aşağıdaki adımları tamamlayacaksınız:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure-SSIS tümleştirme çalışma zamanı sağlama.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- **Azure SQL veritabanı sunucusu (isteğe bağlı)** . Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portal bir tane oluşturun. ADF, bu veritabanı sunucusunda SSıSDB oluşturacak. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının yürütme günlüklerini Azure bölgeleriyle çıkmadan SSSıSDB 'ye yazmasını sağlar. 
    - Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSıSDB barındıracak veritabanı sunucusu türünü seçme konusunda rehberlik için bkz. [Azure SQL veritabanı tek veritabanı/elastik havuz/yönetilen örnek karşılaştırması](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). SSıSDB barındırmak için özel bir uç nokta ile sanal ağ hizmet uç noktaları/yönetilen örnek içeren Azure SQL veritabanı sunucusunu veya şirket içinde barındırılan IR 'yi yapılandırmadan şirket içi verilere erişim gerektir ' i kullanıyorsanız, Azure-SSIS IR bir sanal ağa katılmanız gerekir, bkz. [bir sanal ağda Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Veritabanı sunucusunda **Azure hizmetlerine erişime izin ver** ayarının etkin olduğundan emin olun. Bu, SSıSDB 'yi barındırmak için özel bir uç nokta olan sanal ağ hizmet uç noktaları/yönetilen örnek ile Azure SQL veritabanı sunucusu kullandığınızda geçerli değildir. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Bu ayarı PowerShell kullanarak etkinleştirmek için, bkz. [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - Veritabanı sunucusunun güvenlik duvarı ayarlarındaki istemci IP adresi listesine istemci makinenin IP adresini veya istemci makinenin IP adresini içeren bir IP adresi aralığını ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md).
    - Sunucu Yöneticisi kimlik bilgileriniz veya Azure Active Directory (AAD) kimlik doğrulamasıyla SQL kimlik doğrulaması kullanarak ADF 'niz için yönetilen kimlikle veritabanı sunucusuna bağlanabilirsiniz. İkinci seçenekte ADF için yönetilen kimliğinizi veritabanı sunucusuna erişim izni olan bir AAD grubuna eklemeniz gerekir, bkz. [AAD kimlik doğrulaması ile Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Veritabanı sunucunuzun zaten bir SSSıSDB içermediğinden emin olun. Azure-SSIS IR sağlanması, var olan bir SSıSDB kullanımını desteklemez.

> [!NOTE]
> - ADF ve Azure-SSIS IR Şu anda kullanılabildiği Azure bölgelerinin listesi için, [bölgeye göre ADF + SSIS IR kullanılabilirliği](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all)bölümüne bakın. 

## <a name="create-a-data-factory"></a>Data factory oluştur

ADF 'yi Azure portal aracılığıyla oluşturmak için, [Kullanıcı arabirimi aracılığıyla ADF oluşturma](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory) makalesindeki adım adım yönergeleri izleyin ve oluşturulduktan sonra hızlı erişime izin vermek Için **panoya sabitle** ' yi seçin. 

ADF kurulduktan sonra, Azure portal ' nin genel **bakış sayfasını açın** ve **Yazar & izleyici** kutucuğuna tıklayarak Azure-SSIS IR oluşturmaya devam edebilirsiniz.   

## <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma

### <a name="from-the-data-factory-overview"></a>Data Factory genel bakış

1. **Başlarken** SAYFASıNDA, **SSIS Integration Runtime Yapılandır** kutucuğuna tıklayın. 

   !["SSIS Tümleştirme Çalışma Zamanı Yapılandır" kutucuğu](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. Azure SSIS IR kurulumunun diğer adımları için [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime) bölümüne bakın. 

### <a name="from-the-authoring-ui"></a>Yazma kullanıcı arabiriminden

1. Veri fabrikanızdaki mevcut tümleştirme çalışma zamanlarını görüntülemek için Azure Data Factory kullanıcı arabiriminde **Düzenle** sekmesine geçin, **Bağlantılar**’ı seçin ve sonra **Tümleştirme Çalışma Zamanları** sekmesine geçin. 

   ![Var olan IR’leri görüntüleme seçimleri](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Bir Azure-SSIS IR oluşturmak için **Yeni**'yi seçin. 

   ![Menü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde **Mevcut SSIS paketlerini Azure’da yürütmek üzere lift-and-shift ile taşıma**’yı seçip **İleri**’yi seçin. 

   ![Tümleştirme çalışma zamanının türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Azure SSIS IR kurulumunun diğer adımları için [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime) bölümüne bakın. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı sağlama

1. **Tümleştirme Çalışma Zamanı Kurulumu**’nun **Genel Ayarlar** sayfasında aşağıdaki adımları tamamlayın: 

   ![Genel ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. **Name** (Ad) alanına tümleştirme çalışma zamanınızın adını girin. 

   b. **Description** (Açıklama) alanına tümleştirme çalışma zamanınız için bir açıklama girin. 

   c. **Location** (Konum)alanında tümleştirme çalışma zamanınızın konumunu seçin. Yalnızca desteklenen konumlar görüntülenir. SSISDB'yi barındırmak için veritabanı sunucunuz ile aynı konumu seçmenizi öneririz. 

   d. **Node Size** (Düğüm Boyutu) alanında tümleştirme çalışma zamanınızdaki düğümün boyutunu seçin. Yalnızca desteklenen düğüm boyutları görüntülenir. Yoğun işlemci/bellek kullanan çok sayıda paket çalıştırmak istiyorsanız büyük bir düğüm boyutu seçin. 

   e. **Node Number** (Düğüm Sayısı) alanında tümleştirme çalışma zamanınızdaki düğüm sayısını seçin. Yalnızca desteklenen düğüm sayıları görüntülenir. Aynı anda birden fazla paket çalıştırmak istiyorsanız çok sayıda düğüme sahip bir küme seçin (ölçeklendirme için). 

   f. **Sürüm/lisans**için, tümleştirme çalışma zamanı için SQL Server Edition/Lisans ' ı seçin: Standart veya kurumsal. Tümleştirme çalışma zamanınızda gelişmiş/premium özellikleri kullanmak istiyorsanız Enterprise sürümünü seçin. 

   g. Tasarruf **için, tümleştirme**çalışma zamanı için Azure hibrit avantajı (AHB) seçeneğini belirleyin: Evet veya Hayır. Karma kullanımla maliyet tasarrufu yapmak üzere Yazılım Güvencesi ile kendi SQL Server lisansınızı getirmek istiyorsanız Evet'i seçin. 

   h. **İleri**'ye tıklayın. 

1. **SQL Ayarları** sayfasında aşağıdaki adımları tamamlayın: 

   ![SQL ayarları](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. **SSIS Kataloğu oluştur...** onay kutusunda, Azure-SSIS IR paketlerin çalıştırılacağı dağıtım modelini seçin: Paketlerin, dosya sistemlerinizde/dosya paylaşımınıza/Azure dosyalarınıza dağıtıldığı, veritabanı sunucunuz veya paket dağıtım modeliniz tarafından barındırılan SSSıSDB 'ye dağıtıldığı proje dağıtım modeli. Bu dosyayı denetlemeniz durumunda, sizin adınıza oluşturacağınız ve yönetebilmemiz için kendi veritabanı sunucunuzu SSıSDB barındırmanıza gerek duyarsınız.
   
   b. **Subscription** (Abonelik) alanında SSISDB'yi barındıran Azure aboneliğini seçin. 

   c. **Location** (Konum) alanında SSISDB'yi barındıran veritabanı sunucunuzun konumunu seçin. Tümleştirme çalışma zamanınızla aynı konumu seçmenizi öneririz.

   d. **Catalog Database Server Endpoint** (Katalog Veritabanı Sunucusu Uç Noktası) alanında SSISDB'yi barındıracak veritabanı sunucunuzun uç noktasını seçin. Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSıSDB barındıracak veritabanı sunucusu türünü seçme konusunda rehberlik için bkz. [Azure SQL veritabanı tek veritabanı/elastik havuz/yönetilen örnek karşılaştırması](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). SSıSDB barındırmak için özel bir uç nokta ile sanal ağ hizmet uç noktaları/yönetilen örnek içeren Azure SQL veritabanı sunucusu ' nu veya şirket içinde barındırılan IR 'yi yapılandırmadan şirket içi verilere erişim gerektir ' i seçerseniz, Azure-SSIS IR bir sanal ağa katılmanız gerekir bkz. [bir sanal ağda Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   e. **AAD kimlik doğrulamasını kullan...** onay kutusunda, veritabanı sunucunuzun SSISDB barındıracak kimlik doğrulama yöntemini seçin: ADF 'niz için yönetilen kimlikle SQL kimlik doğrulaması veya AAD kimlik doğrulaması. Bunu yaparsanız, ADF 'niz için yönetilen kimliği, veritabanı sunucunuza erişim izinleri olan bir AAD grubuna eklemeniz gerekir, bkz. [AAD kimlik doğrulamasıyla Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   f. **Admin Username** (Yönetici Kullanıcı Adı) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması kullanıcı adını girin. 

   g. **Admin Password** (Yönetici Parolası) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması parolasını girin. 

   h. **Katalog veritabanı hizmet katmanı**IÇIN, SSISDB barındırmak üzere veritabanı sunucunuzun hizmet katmanını seçin: Temel/Standart/Premium katmanı veya elastik havuz adı.

   i. **Test Connection** (Bağlantıyı Sına) öğesine tıklayın ve başarılı olursa **Next** (İleri) öğesine tıklayın. 

1. **Advanced Settings** (Gelişmiş Ayarlar) sayfasında aşağıdaki adımları tamamlayın: 

   ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. **Maximum Parallel Executions Per Node** (Düğüm Başına Maksimum Paralel Yürütme) alanında tümleştirme çalışma zamanınızda her düğümde eş zamanlı olarak yürütülecek maksimum paket sayısını seçin. Yalnızca desteklenen paket sayıları görüntülenir. Yoğun işlemci/bellek kullanan tek bir büyük/ağır paket çalıştırmak için birden fazla çekirdek kullanmak istiyorsanız düşük bir sayı seçin. Tek bir çekirdekte bir veya daha fazla küçük/hafif paket çalıştırmak istiyorsanız daha yüksek bir sayı seçin. 

   b. **Custom Setup Container SAS URI** (Özel Kurulum Kapsayıcısı SAS URI'si) alanında isteğe bağlı olarak kurulum betiğinizin ve ilgili dosyalarının depolandığı Azure Depolama Blobu kapsayıcısının Paylaşılan Erişim İmzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) değerini girebilirsiniz, bkz. [Azure-SSIS IR için özel kurulum](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

   c. **Select a VNet...** (VNet seçin...) onay kutusunda tümleştirme çalışma zamanınızı sanal ağa eklemek isteyip istemediğinizi seçin. SSıSDB barındırmak için özel bir uç nokta içeren sanal ağ hizmet uç noktaları/yönetilen örnek ile Azure SQL veritabanı sunucusu kullanıyorsanız veya şirket içinde barındırılan IR 'yi yapılandırmadan şirket içi verilere erişim gerektirmek için bunu denetlemeniz gerekir, bkz. [Azure-SSIS IR oluşturma sanal ağ](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   d. Şirket içinde **barındırılan...** onay kutusunu, Azure-SSIS IR için şirket IÇINDE barındırılan IR 'yi yapılandırmak isteyip istemediğinizi seçin, bkz. [bir proxy olarak ŞIRKET Içinde barındırılan IR ayarlama](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). 

1. **İleri**'ye tıklayın. 

1. **Özet** sayfasında, tüm sağlama ayarlarını gözden geçirin, önerilen belge bağlantılarına yer işareti ekleyin ve **son** ' a tıklayarak tümleştirme çalışma zamanının oluşturulmasını başlatın. 

   > [!NOTE]
   > Özel kurulum süresini dışlayarak, bu işlem 5 dakika içinde tamamlanmalıdır.
   >
   > SSıSDB kullanırsanız, ADF hizmeti SSıSDB 'yi hazırlamak için veritabanı sunucunuza bağlanır. 
   > 
   > Bir Azure-SSIS IR sağladığınızda, SSIS için yeniden dağıtılabilir ve Azure Özellik paketi de yüklenir. Bu bileşenler, yerleşik bileşenler tarafından zaten desteklenen veri kaynaklarına ek olarak Excel/Access dosyalarına ve çeşitli Azure veri kaynaklarına bağlantı sağlar. Ayrıca, ek bileşenleri de yükleyebilirsiniz, bkz. [Azure-SSIS IR Için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

1. **Bağlantılar** sekmesinde, gerekirse **Tümleştirme Çalışma Zamanları**’na geçin. Durumu yenilemek için **Yenile**’yi seçin. 

   !["Yenile" düğmesi ile oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Tümleştirme çalışma zamanını durdurmak/başlatmak, düzenlemek veya silmek için **Eylemler** sütunundaki bağlantıları kullanın. Tümleştirme çalışma zamanının JSON kodunu görüntülemek için son bağlantıyı kullanın. Düzenle ve sil düğmeleri yalnızca IR durdurulmuş durumdayken etkin olur. 

   !["Eylemler" sütunundaki bağlantılar](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png) 

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma

SSSıSDB kullanıyorsanız, paketlerinizi bu sunucuya dağıtabilir ve sunucu uç noktası aracılığıyla veritabanı sunucunuza bağlanan SSDT/SSMS araçlarını kullanarak Azure-SSIS IR çalıştırabilirsiniz. Genel bitiş noktası olan Azure SQL veritabanı sunucusu/yönetilen örneği için sunucu uç noktası biçimi sırasıyla olur `<server name>.database.windows.net`. / `<server name>.public.<dns prefix>.database.windows.net,3342` SSISDB kullanmıyorsanız, paketlerinizi dosya sistemlerine/dosya paylaşımlarına/Azure dosyalarına dağıtabilir ve komut satırı yardımcı programlarını `dtinstall` kullanarak / `dtutil` / `dtexec` Azure-SSIS IR çalıştırabilirsiniz. Daha fazla bilgi için bkz. [SSIS paketlerini dağıtma](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). Her iki durumda da, ADF işlem hatlarında SSIS paketi yürütme etkinliğini kullanarak Azure-SSIS IR dağıtılan paketlerinizi çalıştırabilirsiniz, bkz. [birinci sınıf ADF etkinliği olarak SSIS paketi yürütmeyi çağırma](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity). 

Ayrıca, SSIS belgelerindeki aşağıdaki makalelere bakın: 

- [Azure 'da SSIS paketlerini dağıtma, çalıştırma ve izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) 
- [Azure 'da SSıSDB 'ye bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Azure 'da paket yürütmelerini zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) 
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure-SSIS tümleştirme çalışma zamanı sağlama.
> * SSIS paketlerini dağıtma

Azure-SSIS tümleştirme çalışma zamanınızı özelleştirme hakkında bilgi edinmek için şu makaleye bakın: 

> [!div class="nextstepaction"]
> [Azure-SSIS IR hizmetini özelleştirme](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)