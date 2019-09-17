---
title: Azure Data Factory Azure-SSIS Integration Runtime oluştur | Microsoft Docs
description: Azure 'da SSIS paketlerini dağıtabilmeniz ve çalıştırabilmek için Azure Data Factory Azure-SSIS Integration Runtime oluşturma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/15/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 36cb4d306cd74dcde09fa55fc582a043bc324f65
ms.sourcegitcommit: a819209a7c293078ff5377dee266fa76fd20902c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71010017"
---
# <a name="create-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Data Factory Azure-SSIS Integration Runtime oluşturma

Bu öğretici, Azure Data Factory (ADF) içinde bir Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR) sağlamaya yönelik adımlar sağlar. Azure-SSIS IR, Azure SQL veritabanı sunucusu/yönetilen örnek (proje dağıtım modeli) tarafından barındırılan ve dosya sistemlerine/dosya paylaşımlarına/Azure dosyalarına (paket dağıtım modeli) dağıtılan SSIS kataloğuna (SSSıSDB) dağıtılmış paketlerin çalıştırılmasını destekler. Azure-SSIS IR sağlandıktan sonra, SQL Server veri araçları (SSDT)/SQL Server Management Studio (SSMS) gibi tanıdık araçları ve gibi komut satırı yardımcı programlarını `dtinstall` / `dtutil` / `dtexec`kullanabilirsiniz. Azure 'da paketlerinizi dağıtın ve çalıştırın.

[Öğretici: Sağlama Azure-SSIS IR](tutorial-create-azure-ssis-runtime-portal.md) , Azure Portal/ADF uygulaması aracılığıyla bir Azure-SSIS IR oluşturmayı ve isteğe bağlı olarak sssısdb barındırmak için Azure SQL veritabanı sunucusu/yönetilen örneğini kullanmayı gösterir. Bu makale öğreticiye genişleterek aşağıdaki şeyleri nasıl yapabileceğinize ilişkin bir işlem gösterir:

- Sanal ağ hizmet uç noktaları/yönetilen örnek ile Azure SQL veritabanı sunucusu, SSıSDB barındırmak için özel bir uç nokta ile birlikte kullanılır. Bir önkoşul olarak, bir sanal ağa katılması için Azure-SSIS IR sanal ağ izinlerini/ayarlarını yapılandırmanız gerekir.

- Azure SQL veritabanı sunucusu/yönetilen örneğine bağlanmak için, ADF 'nizin yönetilen kimliğiyle birlikte Azure Active Directory (AAD) kimlik doğrulaması kullanın. Önkoşul olarak, ADF 'niz için yönetilen kimliği, SSıSDB oluşturma yeteneğine sahip bir veritabanı kullanıcısı olarak eklemeniz gerekir.

- İsteğe bağlı olarak Azure-SSIS IR bir sanal ağa katın/Şirket içindeki verilere erişmek için Azure-SSIS IR bir ara sunucu olarak şirket içinde barındırılan IR yapılandırın.

## <a name="overview"></a>Genel Bakış

Bu makalede Azure-SSIS IR sağlamanın farklı yolları gösterilmektedir:

- [Azure portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure Resource Manager şablonu](#azure-resource-manager-template)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği**. Aboneliğiniz yoksa [ücretsiz bir deneme](https://azure.microsoft.com/pricing/free-trial/) hesabı oluşturabilirsiniz.
- **Azure SQL veritabanı sunucusu/yönetilen örneği (isteğe bağlı)** . Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portal bir tane oluşturun. ADF, bu veritabanı sunucusunda SSıSDB oluşturacak. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının yürütme günlüklerini Azure bölgeleriyle çıkmadan SSSıSDB 'ye yazmasını sağlar. 
    - Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSıSDB barındıracak veritabanı sunucusu türünü seçme konusunda rehberlik için bkz. [Azure SQL veritabanı tek veritabanı/elastik havuz/yönetilen örnek karşılaştırması](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). SSıSDB barındırmak için özel bir uç nokta ile sanal ağ hizmet uç noktaları/yönetilen örnek içeren Azure SQL veritabanı sunucusunu veya şirket içinde barındırılan IR 'yi yapılandırmadan şirket içi verilere erişim gerektir ' i kullanıyorsanız, Azure-SSIS IR bir sanal ağa katılmanız gerekir, bkz. [Azure-SSIS IR bir sanal ağa ekleme](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).
    - Veritabanı sunucusunda **Azure hizmetlerine erişime izin ver** ayarının etkin olduğundan emin olun. Bu, SSıSDB 'yi barındırmak için özel bir uç nokta olan sanal ağ hizmet uç noktaları/yönetilen örnek ile Azure SQL veritabanı sunucusu kullandığınızda geçerli değildir. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Bu ayarı PowerShell kullanarak etkinleştirmek için, bkz. [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - Veritabanı sunucusunun güvenlik duvarı ayarlarındaki istemci IP adresi listesine istemci makinenin IP adresini veya istemci makinenin IP adresini içeren bir IP adresi aralığını ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md).
    - Sunucu Yöneticisi kimlik bilgileriniz veya Azure Active Directory (AAD) kimlik doğrulamasıyla SQL kimlik doğrulaması kullanarak ADF 'niz için yönetilen kimlikle veritabanı sunucusuna bağlanabilirsiniz.  İkincisi için, ADF 'niz için yönetilen kimliği, veritabanı sunucusuna erişim izinleri olan bir AAD grubuna eklemeniz gerekir, bkz. [Azure-SSIS IR IÇIN AAD kimlik doğrulamasını etkinleştirme](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir).
    - Veritabanı sunucunuzun zaten bir SSSıSDB içermediğinden emin olun. Azure-SSIS IR sağlanması, var olan bir SSıSDB kullanımını desteklemez.
- **Sanal ağ Azure Resource Manager (isteğe bağlı)** . Aşağıdaki koşullardan en az biri doğru ise bir Azure Resource Manager sanal ağınız olmalıdır:
    - Sanal ağ hizmeti uç noktaları/yönetilen örnek ile Azure SQL veritabanı sunucusunda SSıSDB 'yi özel bir uç nokta ile barındırıyorsanız.
    - Şirket içinde barındırılan IR 'yi yapılandırmadan Azure-SSIS IR çalışan SSIS paketlerindeki şirket içi veri depolarına bağlanmak istiyorsunuz.
- **Azure PowerShell (isteğe bağlı)** . Azure-SSIS IR sağlamak için bir PowerShell Betiği çalıştırmak istiyorsanız, [Azure PowerShell yükleyip yapılandırma](/powershell/azure/install-az-ps)yönergelerini izleyin.

### <a name="region-support"></a>Bölge desteği

ADF ve Azure-SSIS IR Şu anda kullanılabildiği Azure bölgelerinin listesi için, [bölgeye göre ADF + SSIS IR kullanılabilirliği](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all)bölümüne bakın.

### <a name="compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance"></a>SQL Database tek veritabanı/elastik havuzunu ve SQL veritabanı yönetilen örneğini karşılaştırın

Aşağıdaki tabloda, Azure-SSıR IR ile bağlantılı olarak Azure SQL veritabanı sunucusu ve yönetilen örnek 'nin belirli özellikleri karşılaştırılmaktadır:

| Özellik | tek veritabanı/elastik havuz| Yönetilen Örnek |
|---------|--------------|------------------|
| **Planlama** | SQL Server Agent kullanılamıyor.<br/><br/>Bkz. [ADF ardışık düzeninde paket yürütme zamanlama](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages?view=sql-server-2017#activity).| Yönetilen örnek Aracısı kullanılabilir. |
| **Kimlik Doğrulaması** | **Db_owner** rolünde bir üye olarak, ADF 'nizin yönetilen kimliği ile HERHANGI bir AAD grubunu temsil eden bir kapsanan veritabanı kullanıcısı ile sssısdb oluşturabilirsiniz.<br/><br/>Bkz. Azure [SQL veritabanı sunucusunda SSISDB oluşturmak Için Azure AD kimlik doğrulamasını etkinleştirme](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | ADF 'nizin yönetilen kimliğini temsil eden kapsanan bir veritabanı kullanıcısı ile SSıSDB oluşturabilirsiniz. <br/><br/>Bkz. Azure [SQL veritabanı yönetilen örneği 'NDE SSıSDB oluşturmak Için Azure AD kimlik doğrulamasını etkinleştirme](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Hizmet katmanı** | Azure SQL veritabanı sunucunuz ile Azure-SSIS IR oluşturduğunuzda SSSıSDB için hizmet katmanını seçebilirsiniz. Birden çok hizmet katmanı vardır. | Yönetilen örneğiniz ile Azure-SSIS IR oluşturduğunuzda SSSıSDB için hizmet katmanını seçemezsiniz. Yönetilen örnekteki tüm veritabanları, bu örneğe ayrılan kaynağı paylaşır. |
| **Sanal ağ** | , Sanal ağ hizmet uç noktaları ile Azure SQL veritabanı sunucusu kullanıyorsanız veya şirket içinde barındırılan IR yapılandırması olmadan şirket içi veri depolarına erişim gerektirmek için Azure-SSIS IR yalnızca Azure Resource Manager sanal ağları destekler. | , Azure-SSIS IR katılması için yalnızca Azure Resource Manager sanal ağları destekler. Yönetilen örneğiniz için genel bir uç nokta etkinleştirmezseniz sanal ağ gereklidir.<br/><br/>Azure-SSIS IR yönetilen örneğiniz ile aynı sanal ağa katılırsanız, Azure-SSIS IR yönetilen örneğinizin farklı bir alt ağda olduğundan emin olun. Azure-SSIS IR, yönetilen örneğinizdeki farklı bir sanal ağa katılırsanız, sanal ağ bağlantısı için sanal ağ eşlemesi veya sanal ağ kullanmanızı öneririz. Bkz. [uygulamanızı Azure SQL veritabanı yönetilen örneğine bağlama](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Dağıtılmış işlemler** | Esnek Işlemler aracılığıyla desteklenir. Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) işlemleri desteklenmez. SSIS paketleriniz dağıtılmış işlemleri koordine etmek için MSDTC kullanıyorsa, Azure SQL veritabanı için elastik Işlemlere geçiş yapmayı göz önünde bulundurun. Daha fazla bilgi için bkz. [bulut veritabanları arasında dağıtılmış işlemler](../sql-database/sql-database-elastic-transactions-overview.md). | Desteklenmiyor. |
| | | |

## <a name="azure-portal"></a>Azure portal

Bu bölümde, Azure-SSIS IR oluşturmak için özellikle ADF Kullanıcı arabirimi (UI)/App Azure portal kullanırsınız.

### <a name="create-a-data-factory"></a>Data factory oluştur

ADF 'yi Azure portal aracılığıyla oluşturmak için, [Kullanıcı arabirimi aracılığıyla ADF oluşturma](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory) makalesindeki adım adım yönergeleri izleyin ve oluşturulduktan sonra hızlı erişime izin vermek Için **panoya sabitle** ' yi seçin. 

ADF kurulduktan sonra, Azure portal ' nin genel **bakış sayfasını açın** ve **Yazar & izleyici** kutucuğuna tıklayarak Azure-SSIS IR oluşturmaya devam edebilirsiniz.   

### <a name="provision-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı sağlama

1. **Başlarken** SAYFASıNDA, **SSIS Integration Runtime Yapılandır** kutucuğuna tıklayın.

   ![SSIS Tümleştirme Çalışma Zamanı Yapılandır kutucuğu](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

2. **Tümleştirme Çalışma Zamanı Kurulumu**’nun **Genel Ayarlar** sayfasında aşağıdaki adımları tamamlayın:

   ![Genel ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. **Name** (Ad) alanına tümleştirme çalışma zamanınızın adını girin.

    b. **Description** (Açıklama) alanına tümleştirme çalışma zamanınız için bir açıklama girin.

    c. **Location** (Konum)alanında tümleştirme çalışma zamanınızın konumunu seçin. Yalnızca desteklenen konumlar görüntülenir. SSISDB'yi barındırmak için veritabanı sunucunuz ile aynı konumu seçmenizi öneririz.

    d. **Node Size** (Düğüm Boyutu) alanında tümleştirme çalışma zamanınızdaki düğümün boyutunu seçin. Yalnızca desteklenen düğüm boyutları görüntülenir. Yoğun işlemci/bellek kullanan çok sayıda paket çalıştırmak istiyorsanız büyük bir düğüm boyutu seçin.

    e. **Node Number** (Düğüm Sayısı) alanında tümleştirme çalışma zamanınızdaki düğüm sayısını seçin. Yalnızca desteklenen düğüm sayıları görüntülenir. Aynı anda birden fazla paket çalıştırmak istiyorsanız çok sayıda düğüme sahip bir küme seçin (ölçeklendirme için).

    f. **Sürüm/lisans**için, tümleştirme çalışma zamanı için SQL Server Edition/Lisans ' ı seçin: Standart veya kurumsal. Tümleştirme çalışma zamanınızda gelişmiş/premium özellikleri kullanmak istiyorsanız Enterprise sürümünü seçin.

    g. Tasarruf **için, tümleştirme**çalışma zamanı için Azure hibrit avantajı (AHB) seçeneğini belirleyin: Evet veya Hayır. Karma kullanımla maliyet tasarrufu yapmak üzere Yazılım Güvencesi ile kendi SQL Server lisansınızı getirmek istiyorsanız Evet'i seçin.

    h. **İleri**'ye tıklayın.

3. **SQL Ayarları** sayfasında aşağıdaki adımları tamamlayın:

   ![SQL ayarları](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. **SSIS Kataloğu oluştur...** onay kutusunda, Azure-SSIS IR paketlerin çalıştırılacağı dağıtım modelini seçin: Paketlerin, dosya sistemlerinizde/dosya paylaşımınıza/Azure dosyalarınıza dağıtıldığı, veritabanı sunucunuz veya paket dağıtım modeliniz tarafından barındırılan SSSıSDB 'ye dağıtıldığı proje dağıtım modeli. Bu dosyayı denetlemeniz durumunda, sizin adınıza oluşturacağınız ve yönetebilmemiz için kendi veritabanı sunucunuzu SSıSDB barındırmanıza gerek duyarsınız.
   
    b. **Subscription** (Abonelik) alanında SSISDB'yi barındıran Azure aboneliğini seçin. 

    c. **Location** (Konum) alanında SSISDB'yi barındıran veritabanı sunucunuzun konumunu seçin. Tümleştirme çalışma zamanınızla aynı konumu seçmenizi öneririz. 

    d. **Catalog Database Server Endpoint** (Katalog Veritabanı Sunucusu Uç Noktası) alanında SSISDB'yi barındıracak veritabanı sunucunuzun uç noktasını seçin. Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSıSDB barındıracak veritabanı sunucusu türünü seçme konusunda rehberlik için bkz. [Azure SQL veritabanı tek veritabanı/elastik havuz/yönetilen örnek karşılaştırması](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). SSıSDB barındırmak için özel bir uç nokta ile sanal ağ hizmet uç noktaları/yönetilen örnek içeren Azure SQL veritabanı sunucusu ' nu veya şirket içinde barındırılan IR 'yi yapılandırmadan şirket içi verilere erişim gerektir ' i seçerseniz, Azure-SSIS IR bir sanal ağa katılmanız gerekir bkz. [Azure-SSIS IR bir sanal ağa ekleme](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

    e. **AAD kimlik doğrulamasını kullan...** onay kutusunda, veritabanı sunucunuzun SSISDB barındıracak kimlik doğrulama yöntemini seçin: ADF 'niz için yönetilen kimlikle SQL kimlik doğrulaması veya AAD kimlik doğrulaması. İade ederseniz, ADF 'niz için yönetilen kimliği, veritabanı sunucunuza erişim izinleri olan bir AAD grubuna eklemeniz gerekir. [Azure-SSIS IR IÇIN AAD kimlik doğrulamasını etkinleştirme](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir)konusuna bakın. 

    f. **Admin Username** (Yönetici Kullanıcı Adı) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması kullanıcı adını girin. 

    g. **Admin Password** (Yönetici Parolası) alanına SSISDB'yi barındıracak veritabanı sunucunuzun SQL kimlik doğrulaması parolasını girin. 

    h. **Katalog veritabanı hizmet katmanı**IÇIN, SSISDB barındırmak üzere veritabanı sunucunuzun hizmet katmanını seçin: Temel/Standart/Premium katmanı veya elastik havuz adı. 

    i. **Test Connection** (Bağlantıyı Sına) öğesine tıklayın ve başarılı olursa **Next** (İleri) öğesine tıklayın. 

4. **Advanced Settings** (Gelişmiş Ayarlar) sayfasında aşağıdaki adımları tamamlayın:

    ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. **Maximum Parallel Executions Per Node** (Düğüm Başına Maksimum Paralel Yürütme) alanında tümleştirme çalışma zamanınızda her düğümde eş zamanlı olarak yürütülecek maksimum paket sayısını seçin. Yalnızca desteklenen paket sayıları görüntülenir. İşlem/bellek kullanımı yoğun tek bir büyük/ağır paket çalıştırmak için birden fazla çekirdek kullanmak istiyorsanız düşük bir sayı seçin. Tek bir çekirdekte bir veya daha fazla küçük/hafif paket çalıştırmak istiyorsanız daha yüksek bir sayı seçin.

    b. **Custom Setup Container SAS URI** (Özel Kurulum Kapsayıcısı SAS URI'si) alanında isteğe bağlı olarak kurulum betiğinizin ve ilgili dosyalarının depolandığı Azure Depolama Blobu kapsayıcısının Paylaşılan Erişim İmzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) değerini girebilirsiniz, bkz. [Azure-SSIS IR için özel kurulum](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

5. **Bir sanal ağ seçin...** onay kutusunda, tümleştirme çalışma zamanını bir sanal ağa katmak isteyip istemediğinizi seçin. SSıSDB 'yi barındırmak veya şirket içi verilere erişim istemek için özel bir uç nokta ile Azure SQL veritabanı sunucusu uç noktaları/yönetilen örnek ile birlikte kullanarak bu dosyayı denetleyin. Örneğin, SSIS paketinizdeki şirket içi veri kaynaklarınız/hedefleri, gerek olmadan Şirket içinde barındırılan IR 'yi yapılandırma, bkz. [Azure-SSIS IR bir sanal ağa ekleme](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Bu denetimi yaparsanız, aşağıdaki adımları izleyin:

   ![Sanal ağ ile gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. **Abonelik**için sanal ağınıza sahip Azure aboneliğini seçin.

    b. **Konum**için, tümleştirme çalışma zamanından aynı konum seçilidir.

    c. **Tür**için, sanal ağınızın türünü seçin: Klasik veya Azure Resource Manager. Klasik sanal ağ yakında kullanım dışı bırakılacak Azure Resource Manager sanal ağ ' ı seçmenizi öneririz.

    d. **VNET adı**için sanal ağınızın adını seçin. Bu sanal ağ, SSıSDB veya şirket içi ağınıza bağlı bir sanal ağ hizmet uç noktaları/yönetilen örneği olan Azure SQL veritabanı sunucusu için kullanılan aynı olmalıdır.

    e. **Alt ağ adı**için, sanal ağınız için alt ağ adını seçin. Bu, SSıSDB 'yi barındırmak için bir sanal ağdaki yönetilen örnek için kullanılandan farklı bir alt ağ olmalıdır.

6. Şirket içinde **barındırılan...** onay kutusunu, Azure-SSIS IR için şirket IÇINDE barındırılan IR 'yi yapılandırmak isteyip istemediğinizi seçin, bkz. [bir proxy olarak ŞIRKET Içinde barındırılan IR ayarlama](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). Bu denetimi yaparsanız, aşağıdaki adımları izleyin:

   ![Şirket içinde barındırılan IR ile gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

    a. **Şirket Içinde barındırılan Integration Runtime**, Azure-SSIS IR Için mevcut şirket IÇINDE barındırılan IR 'yi bir ara sunucu olarak seçin.

    b. **Hazırlama depolama bağlı hizmeti**Için mevcut Azure Blob depolama bağlı hizmetinizi seçin veya hazırlama için yeni bir tane oluşturun.

    c. **Hazırlama yolu**için, seçtiğiniz Azure Blob depolama alanında bir blob kapsayıcısı belirtin veya hazırlama için varsayılan bir tane kullanmak üzere boş bırakın.

7. **VNET doğrulaması** ' na ve **İleri**' ye tıklayın. 

8. **Özet** sayfasında, tüm sağlama ayarlarını gözden geçirin, önerilen belge bağlantılarına yer işareti ekleyin ve **son** ' a tıklayarak tümleştirme çalışma zamanının oluşturulmasını başlatın.

    > [!NOTE]
    > Özel kurulum süresini dışlayarak, bu işlem 5 dakika içinde tamamlanmalıdır, ancak sanal bir ağa katılmak Azure-SSIS IR için yaklaşık 20-30 dakika sürebilir.
    >
    > SSıSDB kullanırsanız, ADF hizmeti SSıSDB 'yi hazırlamak için veritabanı sunucunuza bağlanır. Ayrıca, belirtilmişse sanal ağınız için izinleri ve ayarları yapılandırır ve Azure-SSIS IR sanal ağa birleştirir.
    > 
    > Bir Azure-SSIS IR sağladığınızda, SSIS için yeniden dağıtılabilir ve Azure Özellik paketi de yüklenir. Bu bileşenler, yerleşik bileşenler tarafından zaten desteklenen veri kaynaklarına ek olarak Excel/Access dosyalarına ve çeşitli Azure veri kaynaklarına bağlantı sağlar. Ayrıca, ek bileşenleri de yükleyebilirsiniz, bkz. [Azure-SSIS IR Için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

7. **Bağlantılar** penceresinde, gerekirse **Tümleştirme Çalışma Zamanları**’na geçin. Durumu yenilemek için **Yenile**’ye tıklayın.

   ![Oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

8. Tümleştirme çalışma zamanını durdurmak/başlatmak, düzenlemek veya silmek için **Eylemler** sütunundaki bağlantılar ' ı kullanın. Tümleştirme çalışma zamanının JSON kodunu görüntülemek için son bağlantıyı kullanın. Düzenle ve sil düğmeleri yalnızca IR durdurulmuş durumdayken etkin olur.

   ![Azure SSIS IR - eylemler](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Portalda Azure SSIS tümleştirmesi çalışma zamanları

1. Veri fabrikanızdaki mevcut tümleştirme çalışma zamanlarını görüntülemek için Azure Data Factory kullanıcı arabiriminde **Düzenle** sekmesine geçin, **Bağlantılar**’a tıklayın ve sonra **Tümleştirme Çalışma Zamanları** sekmesine geçin.

   ![Mevcut IRS 'yi görüntüle](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

2. Yeni bir Azure-SSIS IR oluşturmak için **Yeni**'ye tıklayın.

   ![Menü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

3. Azure-SSIS tümleştirme çalışma zamanı oluşturmak için resimde gösterildiği gibi **Yeni**’ye tıklayın.

4. Tümleştirme Çalışma Zamanı Kurulumu penceresinde **Mevcut SSIS paketlerini Azure’da yürütmek üzere lift-and-shift ile taşıma**’yı seçip **İleri**’ye tıklayın.

   ![Tümleştirme çalışma zamanının türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

5. Geriye kalan Azure SSIS IR kurulumu adımları için bkz. [Azure SSIS tümleştirme çalışma zamanı sağlama](#provision-an-azure-ssis-integration-runtime).

## <a name="azure-powershell"></a>Azure PowerShell

Bu bölümde, bir Azure-SSIS IR oluşturmak için Azure PowerShell kullanırsınız.

### <a name="create-variables"></a>Değişken oluşturma

Aşağıdaki betiği kopyalayıp yapıştırın-değişkenler için değerler belirtin. 

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance with a private endpoint/on-premises data without configuring Self-Hosted IR, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used for your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance with a private endpoint

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"
```

### <a name="sign-in-and-select-subscription"></a>Oturum açın ve abonelik ' ı seçin

Aşağıdaki kodu betiğe ekleyerek oturum açın ve Azure aboneliğinizi seçin:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database-server"></a>Veritabanı sunucusuyla bağlantıyı doğrulama

Azure SQL veritabanı sunucunuzu/yönetilen örneğinizi doğrulamak için aşağıdaki betiği ekleyin.

```powershell
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}
```

### <a name="configure-virtual-network"></a>Sanal ağ yapılandırma

Azure-SSIS tümleştirme çalışma zamanının katılacağı sanal ağın izinlerini/ayarlarını otomatik olarak yapılandırmak için aşağıdaki betiği ekleyin.

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutunu kullanarak bir [Azure Kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Kaynak grubunuz zaten varsa, bu kodu betiğe kopyalamayın. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Data factory oluştur

Veri fabrikası oluşturmak için aşağıdaki komutu çalıştırın.

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturma

Azure 'da SSIS paketlerini çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutları çalıştırın.

SSıSDB kullanmıyorsanız, CatalogServerEndpoint, CatalogPricingTier ve CatalogAdminCredential parametrelerini atlayabilirsiniz.

SSıSDB barındırmak veya şirket içi verilere erişim istemek için özel bir uç nokta ile Azure SQL veritabanı sunucusu uç noktaları/yönetilen örnek ile birlikte kullanmıyorsanız, Vnetıd ve alt ağ parametrelerini atlayabilir veya boş değerleri geçirebilirsiniz. Şirket içinde verilere erişmek üzere Azure-SSIS IR için kendi kendine barındırılan IR 'yi bir ara sunucu olarak yapılandırırsanız bunları da atlayabilirsiniz. Aksi takdirde, bunları yok sayamaz ve sanal ağ yapılandırmasından geçerli değerler geçirmeniz gerekir, bkz. [Azure-SSIS IR bir sanal ağa ekleme](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

SSıSDB 'yi barındırmak için yönetilen örnek kullanırsanız, CatalogPricingTier parametresini atlayabilir veya boş bir değer geçirebilirsiniz. Aksi takdirde, bunu atlayamazsınız ve Azure SQL veritabanı için desteklenen fiyatlandırma katmanları listesinden geçerli bir değer geçirmeniz gerekir, bkz. [SQL veritabanı kaynak sınırları](../sql-database/sql-database-resource-limits.md).

Veritabanı sunucusuna bağlanmak için ADF 'nizin yönetilen kimliğiyle birlikte Azure Active Directory (AAD) kimlik doğrulaması kullanırsanız, CatalogAdminCredential parametresini atlayabilirsiniz, ancak ADF 'niz için yönetilen kimliği erişimi olan bir AAD grubuna eklemeniz gerekir veritabanı sunucusu izinleri, bkz. [Azure-SSIS IR IÇIN AAD kimlik doğrulamasını etkinleştirme](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). Aksi takdirde, yok sayamaz ve SQL kimlik doğrulaması için Sunucu Yöneticisi Kullanıcı adınızla ve parolanınızdan oluşturulmuş geçerli bir nesne geçirmeniz gerekir.

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

### <a name="start-integration-runtime"></a>Tümleştirme çalışma zamanını başlatma

Azure-SSIS tümleştirme çalışma zamanını başlatmak için aşağıdaki komutları çalıştırın.

```powershell
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

> [!NOTE]
> Özel kurulum süresini dışlayarak, bu işlem 5 dakika içinde tamamlanmalıdır, ancak sanal bir ağa katılmak Azure-SSIS IR için yaklaşık 20-30 dakika sürebilir.
>
> SSıSDB kullanırsanız, ADF hizmeti SSıSDB 'yi hazırlamak için veritabanı sunucunuza bağlanır. Ayrıca, belirtilmişse sanal ağınız için izinleri ve ayarları yapılandırır ve Azure-SSIS IR sanal ağa birleştirir.
> 
> Bir Azure-SSIS IR sağladığınızda, SSIS için yeniden dağıtılabilir ve Azure Özellik paketi de yüklenir. Bu bileşenler, yerleşik bileşenler tarafından zaten desteklenen veri kaynaklarına ek olarak Excel/Access dosyalarına ve çeşitli Azure veri kaynaklarına bağlantı sağlar. Ayrıca, ek bileşenleri de yükleyebilirsiniz, bkz. [Azure-SSIS IR Için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="full-script"></a>Tam betik

Azure-SSIS tümleştirme çalışma zamanı oluşturan tam betik aşağıda verilmiştir.

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance with a private endpoint/on-premises data without configuring Self-Hosted IR, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance with a private endpoint

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"

### Sign in and select subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database server
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}

### Configure virtual network
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

### Create a data factory
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter when you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

### Start integration runtime
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Bu bölümde, Azure-SSIS tümleştirme çalışma zamanı oluşturmak için Azure Resource Manager şablonunu kullanırsınız. Örnek bir anlatım aşağıda verilmiştir:

1. Aşağıdaki Azure Resource Manager şablonuyla bir JSON dosyası oluşturun. Açılı parantezdeki (yer tutucuları) değerleri kendi değerlerinizle değiştirin.

    ```json
    {
      "contentVersion": "1.0.0.0",
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2018-06-01",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D8_v3",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 8
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL Database server name>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL Database server admin username>",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Database server admin password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```

2. Azure Resource Manager şablonunu dağıtmak için, aşağıdaki örnekte gösterildiği gibi New-AzResourceGroupDeployment komutunu çalıştırın; burada ADFTutorialResourceGroup, kaynak grubunuzun adı ve ADFTutorialARM. JSON, için JSON tanımı içeren dosyadır veri Fabrikanızın ve Azure-SSIS IR.

    ```powershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Bu komut, veri fabrikanızı oluşturur ve içinde Azure-SSIS IR, ancak IR 'yi başlatmaz.

3. Azure-SSIS IR başlatmak için Start-AzDataFactoryV2IntegrationRuntime komutunu çalıştırın:

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ```

> [!NOTE]
> Özel kurulum süresini dışlayarak, bu işlem 5 dakika içinde tamamlanmalıdır, ancak sanal bir ağa katılmak Azure-SSIS IR için yaklaşık 20-30 dakika sürebilir.
>
> SSıSDB kullanırsanız, ADF hizmeti SSıSDB 'yi hazırlamak için veritabanı sunucunuza bağlanır. Ayrıca, belirtilmişse sanal ağınız için izinleri ve ayarları yapılandırır ve Azure-SSIS IR sanal ağa birleştirir.
> 
> Bir Azure-SSIS IR sağladığınızda, SSIS için yeniden dağıtılabilir ve Azure Özellik paketi de yüklenir. Bu bileşenler, yerleşik bileşenler tarafından zaten desteklenen veri kaynaklarına ek olarak Excel/Access dosyalarına ve çeşitli Azure veri kaynaklarına bağlantı sağlar. Ayrıca, ek bileşenleri de yükleyebilirsiniz, bkz. [Azure-SSIS IR Için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma

SSSıSDB kullanıyorsanız, paketlerinizi bu sunucuya dağıtabilir ve sunucu uç noktası aracılığıyla veritabanı sunucunuza bağlanan SSDT/SSMS araçlarını kullanarak Azure-SSIS IR çalıştırabilirsiniz.  Ortak uç nokta ile özel bir uç nokta/yönetilen örnek içeren Azure SQL veritabanı sunucusu/yönetilen örneği için sunucu uç noktası biçimi sırasıyla olur `<server name>.database.windows.net`. / `<server name>.<dns prefix>.database.windows.net` / `<server name>.public.<dns prefix>.database.windows.net,3342` SSISDB kullanmıyorsanız, paketlerinizi dosya sistemlerine/dosya paylaşımlarına/Azure dosyalarına dağıtabilir ve komut satırı yardımcı programlarını `dtinstall` kullanarak / `dtutil` / `dtexec` Azure-SSIS IR çalıştırabilirsiniz. Daha fazla bilgi için bkz. [SSIS paketlerini dağıtma](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). Her iki durumda da, ADF işlem hatlarında SSIS paketi yürütme etkinliğini kullanarak Azure-SSIS IR dağıtılan paketlerinizi çalıştırabilirsiniz, bkz. [birinci sınıf ADF etkinliği olarak SSIS paketi yürütmeyi çağırma](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca bkz. bu belgelerde diğer Azure-SSIS IR konuları:

- [Azure-SSIS Integration Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede, Azure-SSIS IR dahil olmak üzere genel olarak tümleştirme çalışma zamanları hakkında bilgi sağlanır.
- [İzleyici Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede, Azure-SSIS IR hakkındaki bilgileri nasıl alacağınızı ve anlayacağınızı görebilirsiniz.
- [Azure-SSIS IR yönetin](manage-azure-ssis-integration-runtime.md). Bu makalede Azure-SSIS IR durdurma, başlatma veya silme işlemlerinin nasıl yapılacağı gösterilir. Ayrıca, daha fazla düğüm ekleyerek Azure-SSIS IR nasıl ölçeklendirilebilen gösterilmektedir.
- [Azure-SSIS IR bir sanal ağa katın](join-azure-ssis-integration-runtime-virtual-network.md). Bu makalede, Azure-SSIS IR bir sanal ağa katılma hakkında bilgi sağlanır.