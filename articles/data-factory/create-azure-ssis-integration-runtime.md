---
title: Azure Veri Fabrikası'nda Azure-SSIS tümleştirme çalışma zamanı oluşturma
description: Azure Veri Fabrikası'nda SSIS paketlerini dağıtıp çalıştırmak için Azure Veri Fabrikası'nda Azure-SSIS tümleştirme çalışma zamanı nasıl oluşturabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/27/2020
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.openlocfilehash: 1c3f58d42b6f311e4e238dcffe7da42afd8a5306
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416722"
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Azure-SSIS tümleştirme çalışma zamanı oluşturma

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, Azure Veri Fabrikası'nda bir Azure-SQL Server Integration Services (SSIS) tümleştirme çalışma süresi (IR) sağlama adımları sağlanmaktadır. Azure-SSIS IR şunları destekler:

- Azure SQL Veritabanı sunucusu veya yönetilen bir örnek (Project Deployment Model) tarafından barındırılan SSIS kataloğuna (SSISDB) dağıtılan paketleri çalıştırma.
- Dosya sistemlerine, dosya paylaşımlarına veya Azure Dosyalarına (Paket Dağıtım Modeli) dağıtılan paketleri çalıştırma. 

Azure-SSIS IR sağlandıktan sonra, paketlerinizi Azure'da dağıtmak ve çalıştırmak için tanıdık araçları kullanabilirsiniz. Bu araçlar arasında SQL Server Data Tools (SSDT), SQL Server Management `dtinstall`Studio `dtutil`(SSMS) ve , , ve `dtexec`.

[Azure-SSIS IR sağlama](tutorial-create-azure-ssis-runtime-portal.md) öğreticisi, Azure portalı veya Veri Fabrikası uygulaması üzerinden azure-SSIS IR'nin nasıl oluşturulacağımı gösterir. Öğretici ayrıca, bir Azure SQL Veritabanı sunucusunun isteğe bağlı olarak nasıl kullanılacağını veya SSISDB'yi barındırmak için yönetilen örneği de gösterir. Bu makalede öğretici genişletir ve bu isteğe bağlı görevleri nasıl yapacağını açıklar:

- IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanı sunucusu veya SSISDB'yi barındırmak için özel bitiş noktası olan yönetilen bir örnek kullanın. Ön koşul olarak, sanal ağa katılmak için Azure-SSIS IR'niz için sanal ağ izinlerini ve ayarlarını yapılandırmanız gerekir.

- Bir Azure SQL Veritabanı sunucusuna veya yönetilen örneğe bağlanmak için veri fabrikanızın yönetilen kimliğiyle Azure Active Directory (Azure AD) kimlik doğrulamasını kullanın. Ön koşul olarak, bir SSISDB örneği oluşturabilen bir veritabanı kullanıcısı olarak veri fabrikanız için yönetilen kimliği eklemeniz gerekir.

- Azure-SSIS IR'nize sanal bir ağa katılın veya azure-SSIS IR'nizin şirket içinde verilere erişmek için proxy olarak barındırılan bir IR'sini yapılandırın.

Bu makalede, Azure portalı, Azure PowerShell ve Azure Kaynak Yöneticisi şablonu kullanılarak azure-ssis IR nasıl sağlanır.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği.** Zaten aboneliğiniz yoksa, ücretsiz bir [deneme](https://azure.microsoft.com/pricing/free-trial/) hesabı oluşturabilirsiniz.

- **Azure SQL Veritabanı sunucusu veya yönetilen örnek (isteğe bağlı)**. Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portalında bir tane oluşturun. Veri Fabrikası da bu veritabanı sunucusunda bir SSISDB örneği oluşturur. 

  Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, Azure bölgelerini geçmeden ssisdb'ye yürütme günlüklerini yazma işlemlerinin tümleştirmesini sağlar.

  Bu noktaları aklınızda bulundurun:

  - Seçili veritabanı sunucusuna bağlı olarak, SSISDB örneği sizin adınıza tek bir veritabanı olarak, elastik bir havuzun parçası olarak veya yönetilen bir örnekte oluşturulabilir. Ortak bir ağda veya sanal ağa katılarak erişilebilir. SSISDB'yi barındıracak veritabanı sunucusu türünü seçmede kılavuz için, bu makalede [bir Azure SQL Veritabanını Karşılaştır tek veritabanı, elastik havuz ve yönetilen örnek](#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance) bölümüne bakın. 
  
    IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanı sunucusu veya SSISDB'yi barındırmak için özel bitiş noktası olan yönetilen bir örnek kullanıyorsanız veya kendi kendine barındırılan bir IR yapılandırmadan şirket içi verilere erişmeniz gerekiyorsa, Azure-SSIS IR'nizi sanal bir ağa katılmanız gerekir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)

  - Veritabanı sunucusu **için Azure hizmetlerine Erişime İzin Ver** ayarının etkinleştirildiğinden onaylayın. Bu ayar, IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanı sunucusu veya SSISDB'yi barındıracak özel bitiş noktası olan yönetilen bir örnek kullandığınızda geçerli değildir. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). PowerShell kullanarak bu ayarı etkinleştirmek için [Bkz. Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).

  - İstemci makinesinin IP adresini veya istemci makinesinin IP adresini içeren bir dizi IP adresini veritabanı sunucusunun güvenlik duvarı ayarlarındaki istemci IP adres listesine ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md).

  - Sunucu yönetici kimlik bilgilerinizle SQL kimlik doğrulaması kullanarak veya veri fabrikanızın yönetilen kimliğiyle Azure AD kimlik doğrulamasını kullanarak veritabanı sunucusuna bağlanabilirsiniz. İkincisi için, veritabanı sunucusuna erişim izinleri olan bir Azure REKLAM grubuna veri fabrikanız için yönetilen kimliği eklemeniz gerekir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir)

  - Veritabanı sunucunuzun zaten bir SSISDB örneği olmadığını doğrulayın. Bir Azure-SSIS IR'nin sağlanması, varolan bir SSISDB örneğini kullanmayı desteklemez.

- **Azure Kaynak Yöneticisi sanal ağ (isteğe bağlı)**. Aşağıdaki koşullardan en az biri doğruysa, bir Azure Kaynak Yöneticisi sanal ağına sahip olmalısınız:
  - SSISDB'yi, IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktaları veya özel bitiş noktası olan yönetilen bir örneğe sahip bir Azure SQL Veritabanı sunucusunda barındırıyorsınız.
  - Azure-SSIS IR'nizde çalışan SSIS paketlerinden şirket içi veri depolarına, kendi barındırılan bir IR yapılandırmadan bağlanmak istiyorsunuz.

- **Azure PowerShell (isteğe bağlı)**. Azure-SSIS IR'nizi sağlamak için bir PowerShell komut dosyası çalıştırmak istiyorsanız Azure [PowerShell'i nasıl yükleyip yapılandıracağınız](/powershell/azure/install-az-ps)yönergeleri izleyin.

### <a name="regional-support"></a>Bölgesel destek

Veri Fabrikası ve Azure-SSIS IR'nin kullanılabildiği Azure bölgelerinin listesi için, [bölgeye göre Veri Fabrikası ve SSIS IR kullanılabilirliğine](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all)bakın.

### <a name="comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance"></a>SQL Veritabanı tek veritabanı, elastik havuz ve yönetilen örneğin karşılaştırılması

Aşağıdaki tablo, Azure-SSIR IR ile ilgili olarak bir Azure SQL Veritabanı sunucusunun belirli özelliklerini ve yönetilen örneği karşılaştırır:

| Özellik | Tek veritabanı/elastik havuz| Yönetilen örnek |
|---------|--------------|------------------|
| **Zamanlama** | SQL Server Agent kullanılamıyor.<br/><br/>Bkz. [Veri Fabrikası ardışık hattında bir paket yürütme zamanlama.](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages?view=sql-server-2017#activity)| Yönetilen Örnek Aracı kullanılabilir. |
| **Kimlik Doğrulaması** | Veri fabrikanızın yönetilen kimliğine sahip herhangi bir Azure REKLAM grubunu temsil eden ve **db_owner** rolüne üye olarak bulunan bir Veritabanı kullanıcısıyla birlikte bir SSISDB örneği oluşturabilirsiniz.<br/><br/>Bkz. [Azure SQL Veritabanı sunucusunda bir SSISDB örneği oluşturmak için Azure AD kimlik doğrulamasını etkinleştir.](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database) | Veri fabrikanızın yönetilen kimliğini temsil eden, içerdiği bir veritabanı kullanıcısıyla bir SSISDB örneği oluşturabilirsiniz. <br/><br/>Bkz. [Azure SQL Veritabanı yönetilen bir örnekte Bir SSISDB örneği oluşturmak için Azure AD kimlik doğrulamasını etkinleştir.](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance) |
| **Hizmet katmanı** | Azure SQL Veritabanı sunucunuzla bir Azure-SSIS IR oluşturduğunuzda, SSISDB için hizmet katmanını seçebilirsiniz. Birden çok hizmet katmanı vardır. | Yönetilen örneğinizle bir Azure-SSIS IR oluşturduğunuzda, SSISDB için hizmet katmanını seçemezsiniz. Yönetilen örneğinizdeki tüm veritabanları, bu örne ayrılan aynı kaynağı paylaşır. |
| **Sanal ağ** | Azure-SSIS IR'niz, IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanı sunucusu kullanıyorsanız, Azure Kaynak Yöneticisi sanal ağına katılabilir. | Azure-SSIS IR'niz, özel bitiş noktası olan yönetilen bir örnek kullanırsanız, Azure Kaynak Yöneticisi sanal ağına katılabilir. Yönetilen örneğiniz için ortak bir bitiş noktasını etkinleştirmediğinizde sanal ağ gereklidir.<br/><br/>Azure-SSIS IR'nize yönetilen örneğinizle aynı sanal ağa katılırsanız, Azure-SSIS IR'nizin yönetilen örneğinizden farklı bir alt ağda olduğundan emin olun. Azure-SSIS IR'nize yönetilen örneğinizden farklı bir sanal ağa katılırsanız, sanal ağ da bir bakış veya ağdan ağa bağlantı öneririz. Bkz. [Uygulamanızı Azure SQL Veritabanı yönetilen bir örneğe bağlayın.](../sql-database/sql-database-managed-instance-connect-app.md) |
| **Dağıtılmış işlemler** | Bu özellik elastik işlemler yoluyla desteklenir. Microsoft Dağıtılmış Hareket Koordinatörü (MSDTC) hareketleri desteklenmez. SSIS paketleriniz dağıtılmış hareketleri koordine etmek için MSDTC kullanıyorsa, Azure SQL Veritabanı için esnek hareketlere geçiş yapmayı düşünün. Daha fazla bilgi için [bkz.](../sql-database/sql-database-elastic-transactions-overview.md) | Desteklenmiyor. |
| | | |

## <a name="use-the-azure-portal-to-create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturmak için Azure portalını kullanın

Bu bölümde, bir Azure-SSIS IR oluşturmak için Azure portalını, özellikle Veri Fabrikası kullanıcı arabirimini (UI) veya uygulamayı kullanırsınız.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

Azure portalı üzerinden veri fabrikanızı oluşturmak için, [UI üzerinden veri fabrikası oluştur'daki](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory)adım adım yönergeleri izleyin. Oluşturulduktan sonra hızlı erişim sağlamak için **panoya Pin'i** seçin. 

Veri fabrikanız oluşturulduktan sonra, Azure portalında genel bakış sayfasını açın. Ayrı bir sekmede **başlatalım** sayfasını açmak için **Yazar & Monitör** döşemesini seçin. Burada, Azure-SSIS IR'nizi oluşturmaya devam edebilirsiniz.   

### <a name="provision-an-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı sağlama

1. **Başlarken** sayfasında **SSIS Tümleştirme Çalışma Zamanı Yapılandır** kutucuğunu seçin.

   ![SSIS Tümleştirme Çalışma Zamanı Yapılandır kutucuğu](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. **Tümleştirme Çalışma Zamanı Kurulum** panelinin **Genel Ayarlar** bölümünde aşağıdaki adımları tamamlayın.

   ![Genel ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   1. **Name** (Ad) alanına tümleştirme çalışma zamanınızın adını girin.

   1. **Description** (Açıklama) alanına tümleştirme çalışma zamanınız için bir açıklama girin.

   1. **Location** (Konum)alanında tümleştirme çalışma zamanınızın konumunu seçin. Yalnızca desteklenen konumlar görüntülenir. SSISDB'yi barındırmak için veritabanı sunucunuz ile aynı konumu seçmenizi öneririz.

   1. **Düğüm Boyutu**için, tümleştirme çalışma zamanı kümenizdeki düğümün boyutunu seçin. Yalnızca desteklenen düğüm boyutları görüntülenir. Çok sayıda işlem yoğun veya bellek yoğun paketleri çalıştırmak istiyorsanız büyük bir düğüm boyutu (ölçeklendirmek) seçin.

   1. **Node Number** (Düğüm Sayısı) alanında tümleştirme çalışma zamanınızdaki düğüm sayısını seçin. Yalnızca desteklenen düğüm sayıları görüntülenir. Birçok paketi paralel olarak çalıştırmak istiyorsanız, çok sayıda düğümiçeren büyük bir küme seçin (ölçeklendirin).

   1. **Edition/License**için, entegrasyon çalışma süreniz için SQL Server sürümünü seçin: Standart veya Kurumsal. Tümleştirme çalışma zamanınızda gelişmiş özellikler kullanmak istiyorsanız Enterprise'ı seçin.

   1. **Paradan Tasarruf**etmek için, tümleştirme çalışma süreniz için Azure Karma Avantajı seçeneğini seçin: **Evet** veya **Hayır**. Hibrit kullanımla maliyet tasarrufundan yararlanmak için Yazılım Güvencesi ile kendi SQL Server lisansınızı getirmek istiyorsanız **Evet'i** seçin.

   1. **İleri**’yi seçin.

1. SQL **Ayarları** bölümünde aşağıdaki adımları tamamlayın.

   ![SQL ayarları](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   1. Azure-SSIS IR'nizde çalışacak paketler için dağıtım modelini seçmek için **projelerinizi/paketlerinizi/ortamlarınızı/yürütme günlüklerinizi depolamak için Azure SQL Veritabanı sunucusu/Yönetilen Örnek tarafından barındırılan SSIS Kataloğu Oluştur'u (SSISDB)** seçin. Paketlerin veritabanı sunucunuz tarafından barındırılan SSISDB'ye dağıtıldığı Proje Dağıtım Modeli'ni veya paketlerin dosya sistemlerine, dosya paylaşımlarına veya Azure Dosyalarına dağıtıldığı Paket Dağıtım Modelini seçersiniz. 
    
      Onay kutusunu seçerseniz, sizin adınıza oluşturup yöneteceğimiz SSISDB örneğini barındırmak için kendi veritabanı sunucunuzu getirmeniz gerekir.
   
      1. **Subscription** (Abonelik) alanında SSISDB'yi barındıran Azure aboneliğini seçin. 

      1. **Location** (Konum) alanında SSISDB'yi barındıran veritabanı sunucunuzun konumunu seçin. Tümleştirme çalışma zamanınızla aynı konumu seçmenizi öneririz. 

      1. **Catalog Database Server Endpoint** (Katalog Veritabanı Sunucusu Uç Noktası) alanında SSISDB'yi barındıracak veritabanı sunucunuzun uç noktasını seçin. 
    
         Seçili veritabanı sunucusuna bağlı olarak, SSISDB örneği sizin adınıza tek bir veritabanı olarak, elastik bir havuzun parçası olarak veya yönetilen bir örnekte oluşturulabilir. Ortak bir ağda veya sanal ağa katılarak erişilebilir. SSISDB'yi barındıracak veritabanı sunucusu türünü seçmede kılavuz için, bu makalede [bir Azure SQL Veritabanını Karşılaştır tek veritabanı, elastik havuz ve yönetilen örnek](#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance) bölümüne bakın. 
    
         IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanı sunucusu veya SSISDB'yi barındırmak için özel bitiş noktası olan yönetilen bir örnek seçerseniz veya kendi kendine barındırılan bir IR yapılandırmadan şirket içi verilere erişmeniz gerekiyorsa, Azure-SSIS IR'nizi sanal bir ağa katılmanız gerekir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) 

      1. Veritabanı sunucunuzun SSISDB'yi barındıracak kimlik doğrulama yöntemini seçmek için ADF onay **kutunuzun yönetilen kimliğiyle AAD kimlik bilgilerini kullanın.** Veri fabrikanızın yönetilen kimliğiyle SQL kimlik doğrulaması veya Azure AD kimlik doğrulaması seçeneğini seçersiniz. 
    
         Onay kutusunu seçerseniz, veri fabrikanızın yönetilen kimliğini veritabanı sunucunuza erişim izinleri içeren bir Azure AD grubuna eklemeniz gerekir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir) 

      1. **Yönetici Kullanıcı Adı**için, SSISDB'yi barındırmak için veritabanı sunucunuzun SQL kimlik doğrulama kullanıcı adını girin. 

      1. **Yönetici Parlebesi**için, SSISDB'yi barındırmak için veritabanı sunucunuzun SQL kimlik doğrulama parolasını girin. 

      1. **Katalog Veritabanı Hizmet Katmanı**için, SSISDB'yi barındıracak veritabanı sunucunuzun hizmet katmanını seçin. Temel, Standart veya Premium katmanı seçin veya esnek bir havuz adı seçin. 

      1. **Test Bağlantısı'nı**seçin. Test başarılı olursa, **İleri'yi**seçin. 

1. Gelişmiş **Ayarlar** bölümünde aşağıdaki adımları tamamlayın.

   ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   1. **Düğüm Başına Maksimum Paralel Yürütmeler**için, tümleştirme çalışma zamanı kümenizde düğüm başına aynı anda çalışacak en fazla paket sayısını seçin. Yalnızca desteklenen paket sayıları görüntülenir. İşlem veya bellek yoğun olan tek bir büyük paketi çalıştırmak için birden fazla çekirdek kullanmak istiyorsanız düşük bir sayı seçin. Tek bir çekirdekte bir veya daha fazla küçük paket çalıştırmak istiyorsanız yüksek bir sayı seçin.

   1. Azure-SSIS IR'nize standart/ifade özel kurulumları eklemek isteyip istemediğinizi seçmek için **ek sistem yapılandırmaları/bileşen yüklemeleri onay kutusunu ile Azure-SSIS Tümleştirme Çalışma sürenizi özelleştir'i** seçin. Daha fazla bilgi için [Azure-SSIS IR için Özel kuruluma](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)bakın.

      Onay kutusunu seçerseniz, aşağıdaki adımları tamamlayın.

      ![Özel kurulumlarla gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-custom.png)
   
      1. **Özel kurulum kapsayıcısı SAS URI**için, standart özel kurulumlar için komut dosyalarını ve ilişkili dosyaları depoladığınız kapsayıcınızın SAS URI'sini girin.

      1. **Express özel kurulumu için,** Ekle express özel **kurulum** panelini açmak için **Yeni'yi** seçin ve ardından Express özel **kurulum türü** açılır menüsü altında herhangi bir tür seçin, örneğin **cmdkey komutunu çalıştır**, ortam **değişkeni ekle**, lisanslı bileşen **yükle,** vb.

         Lisanslı bileşen türünü **yükle'yi** seçerseniz, **bileşen adı** açılır menüsü altında ISV ortaklarımızdan tümleşik bileşenleri seçebilir ve gerekirse Lisans **anahtarı** alanına onlardan satın aldığınız ürün lisans anahtarını girebilirsiniz.
  
         Eklenen ekspres özel kurulumlarınız **Gelişmiş Ayarlar** bölümünde görünür. Bunları kaldırmak için onay kutularını seçebilir ve sonra **Sil'i**seçebilirsiniz.

   1. Katılmak **için Azure-SSIS Tümleştirme Çalışma süreniz için Bir VNet Seçin' i seçin, ADF' nin belirli ağ kaynakları oluşturmasına izin verin ve isteğe bağlı olarak,** tümleştirme çalışma sürenizi sanal bir ağa katmak isteyip istemediğinizi seçmek için kendi statik genel IP adreslerinizi onay kutlayın. 

      IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanı sunucusu veya SSISDB'yi barındırmak için özel bitiş noktası olan yönetilen bir örnek kullanıyorsanız veya kendi barındırılan bir IR yapılandırmadan şirket içi verilere (diğer bir deyişle, SSIS paketlerinizdeki şirket içi veri kaynaklarına veya varış noktalarına) erişime ihtiyacınız varsa bunu seçin. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) 

      Onay kutusunu seçerseniz, aşağıdaki adımları tamamlayın.

      ![Bir sanal ağda gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

      1. **Abonelik**için, sanal ağınıza sahip Azure aboneliğini seçin.

      1. **Konum**için, tümleştirme çalışma sürenizin aynı konumu seçilir.

      1. **Türü**için sanal ağınızın türünü seçin: klasik veya Azure Kaynak Yöneticisi. Klasik sanal ağlar yakında küçümseneceği için bir Azure Kaynak Yöneticisi sanal ağı seçmenizi öneririz.

      1. **VNet Adı**için sanal ağınızın adını seçin. Sanal ağ hizmeti uç noktaları veya yönetilen örneği ile Azure SQL Veritabanı sunucunuz için kullanılan ın aynısı olmalıdır. Ya da şirket içi ağınıza bağlı olanın aynısı olmalıdır. Aksi takdirde, Azure-SSIS IR için kendi statik genel IP adreslerinizi getirmek herhangi bir sanal ağ olabilir.

      1. **Subnet Adı**için, sanal ağınız için alt ağ adını seçin. SSISDB'yi barındırmak için sanal ağ hizmeti uç noktalarına sahip Azure SQL Veritabanı sunucunuz için kullanılanın aynısı olmalıdır. Ya da SSISDB'yi barındırmak için özel bitiş noktası olan yönetilen örneğiniz için kullanılandan farklı bir alt ağ olmalıdır. Aksi takdirde, Azure-SSIS IR için kendi statik genel IP adreslerinizi getirmek herhangi bir alt ağ olabilir.

      1. Azure-SSIS IR için kendi statik genel IP adreslerinizi getirmek isteyip istemediğinizi seçmek için **Azure-SSIS Tümleştirme Çalışma zamanı onay kutunuz için statik genel IP adreslerini getir'i** seçin, böylece veri kaynaklarınız için güvenlik duvarında bunlara izin verebilirsiniz.

         Onay kutusunu seçerseniz, aşağıdaki adımları tamamlayın.

         1. **İlk statik genel IP adresi**için, Azure-SSIS IR'nizin gereksinimlerini karşılayan ilk statik genel IP adresini seçin. Yoksa, Azure portalında statik genel IP adresleri oluşturmak için yeni bağlantı **oluştur'u** tıklatın ve ardından bunları seçebilmeniz için burada yenile düğmesini tıklatın.
      
         1. **İkinci statik genel IP adresi**için, Azure-SSIS IR'nizin gereksinimlerini karşılayan ikinci statik genel IP adresini seçin. Yoksa, Azure portalında statik genel IP adresleri oluşturmak için yeni bağlantı **oluştur'u** tıklatın ve ardından bunları seçebilmeniz için burada yenile düğmesini tıklatın.

   1. **Azure-SSIS IR'niz için** proxy olarak kendi kendine barındırılan Bir IR'yi yapılandırmak isteyip istemediğinizi seçmek için Azure-SSIS Tümleştirme Çalışma zamanı onay kutunuz için proxy olarak Kendi Kendine Barındırılan Tümleştirme Çalışma Süresini Ayarlayın'ı seçin. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) 

      Onay kutusunu seçerseniz, aşağıdaki adımları tamamlayın.

      ![Kendi kendine barındırılan IR ile gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

      1. **Kendi Kendine Barındırılan Tümleştirme Çalışma Süresi**için, azure-SSIS IR için proxy olarak varolan kendi kendine barındırılan IR'nizi seçin.

      1. **Depolama Bağlantılı Hizmet Evreleme**için, mevcut Azure Blob depolama bağlantılı hizmetinizi seçin veya evreleme için yeni bir hizmet oluşturun.

      1. **Hazırlama Yolu**için, seçtiğiniz Azure Blob depolama hesabında bir blob kapsayıcıbelirtin veya evreleme için varsayılan bir kapsayıcı kullanmak için boş bırakın.

   1. **VNet Doğrulama** > **devam'ı**seçin. 

1. **Özet** bölümünde, tüm sağlama ayarlarını gözden geçirin, önerilen belge bağlantılarını yer imi yapın ve tümleştirme çalışma sürenizi oluşturmaya başlamak için **Finish'i** seçin.

   > [!NOTE]
   > Herhangi bir özel kurulum süresi hariç, bu işlem 5 dakika içinde bitirilmelidir. Ancak Azure-SSIS IR'nin sanal ağa katılması 20-30 dakika sürebilir.
   >
   > SSISDB kullanıyorsanız, Veri Fabrikası hizmeti SSISDB hazırlamak için veritabanı sunucunuza bağlanır. Ayrıca, belirtildiği takdirde sanal ağınızın izinlerini ve ayarlarını yapılandırır ve Azure-SSIS IR'nizi sanal ağa katılır.
   > 
   > Bir Azure-SSIS IR'i sağınızda, SSIS için Access Redistributable ve Azure Özellik Paketi de yüklenir. Bu bileşenler, yerleşik bileşenlerin zaten desteklediği veri kaynaklarına ek olarak Excel dosyalarına, Access dosyalarına ve çeşitli Azure veri kaynaklarına bağlantı sağlar. Yükleyebileceğiniz diğer bileşenler hakkında daha fazla bilgi için [Azure-SSIS IR için Özel kuruluma](how-to-configure-azure-ssis-ir-custom-setup.md)bakın.

1. **Bağlantılar** sekmesinde, gerekirse **Tümleştirme Çalışma Zamanları**’na geçin. Durumu yenilemek için **Yenile**’yi seçin.

   ![Oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Tümleştirme çalışma zamanını durdurmak/başlatmak, düzenlemek veya silmek için **Eylemler** sütunundaki bağlantıları kullanın. Tümleştirme çalışma zamanının JSON kodunu görüntülemek için son bağlantıyı kullanın. Düzenle ve sil düğmeleri yalnızca IR durdurulmuş durumdayken etkin olur.

   ![Azure SSIS IR eylemleri](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Portalda Azure SSIS tümleştirmesi çalışma zamanları

1. Azure Veri Fabrikası UI'sinde, **Değiştir** sekmesine geçin ve **Bağlantılar'ı**seçin. Ardından, veri fabrikanızdaki varolan tümleştirme çalışma sürelerini görüntülemek için **Tümleştirme Çalışma Saatleri** sekmesine geçin.

   ![Mevcut IR’leri görüntüle](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Yeni bir Azure-SSIS IR oluşturmak için **Yeni'yi** seçin.

   ![Menü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. **Tümleştirme Çalışma Zamanı Kurulumu** **panelinde, Azure döşemesinde yürütmek için varolan SSIS paketlerini kaldır ve kaydır** ve sonra **İleri'yi**seçin.

   ![Tümleştirme çalışma zamanının türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Bir Azure-SSIS IR ayarlamak için kalan adımlar için, [Bir Azure SSIS tümleştirmesi](#provision-an-azure-ssis-integration-runtime) sağlama bölümüne bakın.

## <a name="use-azure-powershell-to-create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturmak için Azure PowerShell'i kullanın

Bu bölümde, bir Azure-SSIS IR oluşturmak için Azure PowerShell'i kullanırsınız.

### <a name="create-variables"></a>Değişken oluşturma

Aşağıdaki komut dosyasını kopyalayın ve yapıştırın. Değişkenler için değerleri belirtin. 

```powershell
### Azure Data Factory info
# If your input contains a PSH special character like "$", precede it with the escape character "`" - for example, "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime info - This is a Data Factory compute resource for running SSIS packages.
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, whereas Enterprise lets you use advanced features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, whereas BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported. For other nodes, up to (2 x number of cores) are currently supported.
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info: Standard/express custom setups
$SetupScriptContainerSasUri = "" # OPTIONAL to provide a SAS URI of blob container for standard custom setup where your script and its associated files are stored
$ExpressCustomSetup = "[RunCmdkey|SetEnvironmentVariable|SentryOne.TaskFactory|oh22is.SQLPhonetics.NET|oh22is.HEDDA.IO|KingswaySoft.IntegrationToolkit|KingswaySoft.ProductivityPack|Theobald.XtractIS or leave it empty]" # OPTIONAL to configure an express custom setup without script
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use an Azure SQL Database server with IP firewall rules/virtual network service endpoints or a managed instance with private endpoint to host SSISDB, or if you require access to on-premises data without configuring a self-hosted IR. We recommend an Azure Resource Manager virtual network, because classic virtual networks will be deprecated soon.
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Use the same subnet as the one used for your Azure SQL Database server with virtual network service endpoints, or a different subnet from the one used for your managed instance with a private endpoint
# Public IP address info: OPTIONAL to provide two standard static public IP addresses with DNS name under the same subscription and in the same region as your virtual network
$FirstPublicIP = "[your first public IP address resource ID or leave it empty]"
$SecondPublicIP = "[your second public IP address resource ID or leave it empty]"

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or managed instance name.DNS prefix.database.windows.net or managed instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, ensure that there's no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure AD
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for Azure AD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for Azure AD authentication]"
# For the basic pricing tier, specify "Basic," not "B." For standard, premium, and elastic pool tiers, specify "S0," "S1," "S2," "S3," etc. See https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for managed instance]"

### Self-hosted integration runtime info - This can be configured as a proxy for on-premises data access 
$DataProxyIntegrationRuntimeName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingLinkedServiceName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingPath = "" # OPTIONAL to configure a proxy for on-premises data access 
```

### <a name="sign-in-and-select-a-subscription"></a>Oturum açın ve bir abonelik seçin

Oturum açmak ve Azure aboneliğinizi seçmek için aşağıdaki komut dosyasını ekleyin.

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database-server"></a>Veritabanı sunucusuna bağlantıyı doğrulama

Azure SQL Veritabanı sunucunuzu veya yönetilen örneğinizi doğrulamak için aşağıdaki komut dosyasını ekleyin.

```powershell
# Validate only if you use SSISDB and you don't use virtual network or Azure AD authentication
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

### <a name="configure-the-virtual-network"></a>Sanal ağı yapılandırma

Azure-SSIS tümleştirme çalışma sürenizin katılmak üzere sanal ağ izinlerini ve ayarlarını otomatik olarak yapılandırmak için aşağıdaki komut dosyasını ekleyin.

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

[Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutunu kullanarak bir [Azure kaynak grubu](../azure-resource-manager/management/overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Kaynak grubunuz zaten varsa, bu kodu betiğinize kopyalamayın. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

Veri fabrikası oluşturmak için aşağıdaki komutu çalıştırın.

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
    -Location $DataFactoryLocation `
    -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturma

Azure'da SSIS paketlerini çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutları çalıştırın.

SSISDB kullanmıyorsanız, parametreleri ve `CatalogServerEndpoint` `CatalogPricingTier` `CatalogAdminCredential` parametreleri atlayabilirsiniz.

IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanı sunucusu veya SSISDB'yi barındırmak için özel bitiş noktası olan `VNetId` `Subnet` yönetilen bir örnek kullanmıyorsanız veya şirket içi verilere erişim istemiyorsanız, parametreleri atlayabilir veya bunlar için boş değerleri aktarabilirsiniz. Ayrıca, azure-SSIS IR'nizin şirket içinde verilere erişmek için proxy olarak barındırılan bir IR'yi yapılandırırsanız bunları atlayabilirsiniz. Aksi takdirde, bunları atlayamaz ve geçerli değerleri sanal ağ yapılandırmanızdan geçirmeniz gerekir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)

Yönetilen örneği SSISDB'yi barındırmak için kullanıyorsanız, parametreyi `CatalogPricingTier` atlayabilir veya bunun için boş bir değer geçirebilirsiniz. Aksi takdirde, bunu atlayamaz ve Azure SQL Veritabanı için desteklenen fiyatlandırma katmanları listesinden geçerli bir değer geçmesi gerekir. Daha fazla bilgi için [SQL Veritabanı kaynak sınırlarına](../sql-database/sql-database-resource-limits.md)bakın.

Veritabanı sunucusuna bağlanmak için veri fabrikanızın yönetilen kimliğiyle Azure AD kimlik doğrulamasını `CatalogAdminCredential` kullanırsanız, parametreyi atlayabilirsiniz. Ancak, veri fabrikanızın yönetilen kimliğini veritabanı sunucusuna erişim izinleri olan bir Azure REKLAM grubuna eklemeniz gerekir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir) Aksi takdirde, bunu atlayamaz ve SQL kimlik doğrulaması için sunucu yönetici kullanıcı adı ve parolanızdan oluşan geçerli bir nesneyi geçirmeniz gerekir.

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
       
# Add the CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -CatalogServerEndpoint $SSISDBServerEndpoint `
        -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add the CatalogAdminCredential parameter if you don't use Azure AD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
            -DataFactoryName $DataFactoryName `
            -Name $AzureSSISName `
            -CatalogAdminCredential $serverCreds
    }
}

# Add custom setup parameters if you use standard/express custom setups
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
if(![string]::IsNullOrEmpty($ExpressCustomSetup))
{
    if($ExpressCustomSetup -eq "RunCmdkey")
    {
        $addCmdkeyArgument = "YourFileShareServerName or YourAzureStorageAccountName.file.core.windows.net"
        $userCmdkeyArgument = "YourDomainName\YourUsername or azure\YourAzureStorageAccountName"
        $passCmdkeyArgument = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourPassword or YourAccessKey")
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.CmdkeySetup($addCmdkeyArgument, $userCmdkeyArgument, $passCmdkeyArgument)
    }
    if($ExpressCustomSetup -eq "SetEnvironmentVariable")
    {
        $variableName = "YourVariableName"
        $variableValue = "YourVariableValue"
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.EnvironmentVariableSetup($variableName, $variableValue)
    }
    if($ExpressCustomSetup -eq "SentryOne.TaskFactory")
    {
        $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
    }
    if($ExpressCustomSetup -eq "oh22is.SQLPhonetics.NET")
    {
        $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
    }
    if($ExpressCustomSetup -eq "oh22is.HEDDA.IO")
    {
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup)
    }
    if($ExpressCustomSetup -eq "KingswaySoft.IntegrationToolkit")
    {
        $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
    }
    if($ExpressCustomSetup -eq "KingswaySoft.ProductivityPack")
    {
        $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
    }    
    if($ExpressCustomSetup -eq "Theobald.XtractIS")
    {
        $jsonData = Get-Content -Raw -Path YourLicenseFile.json
        $jsonData = $jsonData -replace '\s',''
        $jsonData = $jsonData.replace('"','\"')
        $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString($jsonData)
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
    }
    # Create an array of one or more express custom setups
    $setups = New-Object System.Collections.ArrayList
    $setups.Add($setup)

    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -ExpressCustomSetup $setups
}

# Add self-hosted integration runtime parameters if you configure a proxy for on-premises data accesss
if(![string]::IsNullOrEmpty($DataProxyIntegrationRuntimeName) -and ![string]::IsNullOrEmpty($DataProxyStagingLinkedServiceName))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -DataProxyIntegrationRuntimeName $DataProxyIntegrationRuntimeName `
        -DataProxyStagingLinkedServiceName $DataProxyStagingLinkedServiceName

    if(![string]::IsNullOrEmpty($DataProxyStagingPath))
    {
        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
            -DataFactoryName $DataFactoryName `
            -Name $AzureSSISName `
            -DataProxyStagingPath $DataProxyStagingPath
    }
}

# Add public IP address parameters if you bring your own static public IP addresses
if(![string]::IsNullOrEmpty($FirstPublicIP) -and ![string]::IsNullOrEmpty($SecondPublicIP))
{
    $publicIPs = @($FirstPublicIP, $SecondPublicIP)
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -PublicIPs $publicIPs
}
```

### <a name="start-the-integration-runtime"></a>Tümleştirme çalışma süresini başlatma

Azure-SSIS tümleştirme çalışma süresini başlatmak için aşağıdaki komutları çalıştırın.

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
> Herhangi bir özel kurulum süresi hariç, bu işlem 5 dakika içinde bitirilmelidir. Ancak Azure-SSIS IR'nin sanal ağa katılması 20-30 dakika sürebilir.
>
> SSISDB kullanıyorsanız, Veri Fabrikası hizmeti SSISDB hazırlamak için veritabanı sunucunuza bağlanır. Ayrıca, belirtildiği takdirde sanal ağınızın izinlerini ve ayarlarını yapılandırır ve Azure-SSIS IR'nizi sanal ağa katılır.
> 
> Bir Azure-SSIS IR'i sağınızda, SSIS için Access Redistributable ve Azure Özellik Paketi de yüklenir. Bu bileşenler, yerleşik bileşenlerin zaten desteklediği veri kaynaklarına ek olarak Excel dosyalarına, Access dosyalarına ve çeşitli Azure veri kaynaklarına bağlantı sağlar. Yükleyebileceğiniz diğer bileşenler hakkında daha fazla bilgi için [Azure-SSIS IR için Özel kuruluma](how-to-configure-azure-ssis-ir-custom-setup.md)bakın.

### <a name="full-script"></a>Tam betik

Azure-SSIS tümleştirme çalışma zamanı oluşturan tam komut dosyası aşağıda veda edinebilirsiniz.

```powershell
### Azure Data Factory info
# If your input contains a PSH special character like "$", precede it with the escape character "`" - for example, "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime info - This is a Data Factory compute resource for running SSIS packages.
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, whereas Enterprise lets you use advanced features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, whereas BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from the Azure Hybrid Benefit option
# For a Standard_D1_v2 node, up to four parallel executions per node are supported. For other nodes, up to (2 x number of cores) are currently supported.
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info: Standard/express custom setups
$SetupScriptContainerSasUri = "" # OPTIONAL to provide a SAS URI of blob container for standard custom setup where your script and its associated files are stored
$ExpressCustomSetup = "[RunCmdkey|SetEnvironmentVariable|SentryOne.TaskFactory|oh22is.SQLPhonetics.NET|oh22is.HEDDA.IO|KingswaySoft.IntegrationToolkit|KingswaySoft.ProductivityPack|Theobald.XtractIS or leave it empty]" # OPTIONAL to configure an express custom setup without script
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use an Azure SQL Database server with IP firewall rules/virtual network service endpoints or a managed instance with private endpoint to host SSISDB, or if you require access to on-premises data without configuring a self-hosted IR. We recommend an Azure Resource Manager virtual network, because classic virtual networks will be deprecated soon.
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Use the same subnet as the one used for your Azure SQL Database server with virtual network service endpoints, or a different subnet from the one used for your managed instance with a private endpoint
# Public IP address info: OPTIONAL to provide two standard static public IP addresses with DNS name under the same subscription and in the same region as your virtual network
$FirstPublicIP = "[your first public IP address resource ID or leave it empty]"
$SecondPublicIP = "[your second public IP address resource ID or leave it empty]"

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or managed instance name.DNS prefix.database.windows.net or managed instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, ensure that there's no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure AD
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for Azure AD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for Azure AD authentication]"
# For the basic pricing tier, specify "Basic," not "B." For standard, premium, and elastic pool tiers, specify "S0," "S1," "S2," "S3," etc. See https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for managed instance]"

### Self-hosted integration runtime info - This can be configured as a proxy for on-premises data access 
$DataProxyIntegrationRuntimeName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingLinkedServiceName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingPath = "" # OPTIONAL to configure a proxy for on-premises data access 

### Sign in and select a subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to the database server
# Validate only if you use SSISDB and don't use a virtual network or Azure AD authentication
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

### Configure a virtual network
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

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add the CatalogAdminCredential parameter if you don't use Azure AD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
            -DataFactoryName $DataFactoryName `
            -Name $AzureSSISName `
            -CatalogAdminCredential $serverCreds
    }
}

# Add custom setup parameters if you use standard/express custom setups
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
if(![string]::IsNullOrEmpty($ExpressCustomSetup))
{
    if($ExpressCustomSetup -eq "RunCmdkey")
    {
        $addCmdkeyArgument = "YourFileShareServerName or YourAzureStorageAccountName.file.core.windows.net"
        $userCmdkeyArgument = "YourDomainName\YourUsername or azure\YourAzureStorageAccountName"
        $passCmdkeyArgument = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourPassword or YourAccessKey")
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.CmdkeySetup($addCmdkeyArgument, $userCmdkeyArgument, $passCmdkeyArgument)
    }
    if($ExpressCustomSetup -eq "SetEnvironmentVariable")
    {
        $variableName = "YourVariableName"
        $variableValue = "YourVariableValue"
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.EnvironmentVariableSetup($variableName, $variableValue)
    }
    if($ExpressCustomSetup -eq "SentryOne.TaskFactory")
    {
        $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
    }
    if($ExpressCustomSetup -eq "oh22is.SQLPhonetics.NET")
    {
        $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
    }
    if($ExpressCustomSetup -eq "oh22is.HEDDA.IO")
    {
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup)
    }
    if($ExpressCustomSetup -eq "KingswaySoft.IntegrationToolkit")
    {
        $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
    }
    if($ExpressCustomSetup -eq "KingswaySoft.ProductivityPack")
    {
        $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
    }    
    if($ExpressCustomSetup -eq "Theobald.XtractIS")
    {
        $jsonData = Get-Content -Raw -Path YourLicenseFile.json
        $jsonData = $jsonData -replace '\s',''
        $jsonData = $jsonData.replace('"','\"')
        $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString($jsonData)
        $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
    }
    # Create an array of one or more express custom setups
    $setups = New-Object System.Collections.ArrayList
    $setups.Add($setup)

    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -ExpressCustomSetup $setups
}

# Add self-hosted integration runtime parameters if you configure a proxy for on-premises data accesss
if(![string]::IsNullOrEmpty($DataProxyIntegrationRuntimeName) -and ![string]::IsNullOrEmpty($DataProxyStagingLinkedServiceName))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -DataProxyIntegrationRuntimeName $DataProxyIntegrationRuntimeName `
        -DataProxyStagingLinkedServiceName $DataProxyStagingLinkedServiceName

    if(![string]::IsNullOrEmpty($DataProxyStagingPath))
    {
        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
            -DataFactoryName $DataFactoryName `
            -Name $AzureSSISName `
            -DataProxyStagingPath $DataProxyStagingPath
    }
}

# Add public IP address parameters if you bring your own static public IP addresses
if(![string]::IsNullOrEmpty($FirstPublicIP) -and ![string]::IsNullOrEmpty($SecondPublicIP))
{
    $publicIPs = @($FirstPublicIP, $SecondPublicIP)
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -PublicIPs $publicIPs
}

### Start the integration runtime
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $DataFactoryName `
    -Name $AzureSSISName `
    -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="use-an-azure-resource-manager-template-to-create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturmak için Azure Kaynak Yöneticisi şablonu kullanma

Bu bölümde, Azure-SSIS tümleştirme çalışma süresini oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullanırsınız. Aşağıda örnek bir gözden geçirme:

1. Aşağıdaki Azure Kaynak Yöneticisi şablonuna sahip bir JSON dosyası oluşturun. Açı braketleri (yer tutucular) değerleri kendi değerlerinizle değiştirin.

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

2. Azure Kaynak Yöneticisi şablonunu dağıtmak `New-AzResourceGroupDeployment` için aşağıdaki örnekte gösterildiği gibi komutu çalıştırın. Örnekte, `ADFTutorialResourceGroup` kaynak grubunuzun adı dır. `ADFTutorialARM.json`veri fabrikanız ve Azure-SSIS IR için JSON tanımını içeren dosyadır.

    ```powershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Bu komut, veri fabrikanızı ve Azure-SSIS IR'nizi oluşturur, ancak IR'yi başlatmaz.

3. Azure-SSIS IR'nizi başlatmak `Start-AzDataFactoryV2IntegrationRuntime` için aşağıdaki komutu çalıştırın:

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
        -DataFactoryName "<Data Factory Name>" `
        -Name "<Azure SSIS IR Name>" `
        -Force
    ```

> [!NOTE]
> Herhangi bir özel kurulum süresi hariç, bu işlem 5 dakika içinde bitirilmelidir. Ancak Azure-SSIS IR'nin sanal ağa katılması 20-30 dakika sürebilir.
>
> SSISDB kullanıyorsanız, Veri Fabrikası hizmeti SSISDB hazırlamak için veritabanı sunucunuza bağlanır. Ayrıca, belirtildiği takdirde sanal ağınızın izinlerini ve ayarlarını yapılandırır ve Azure-SSIS IR'nizi sanal ağa katılır.
> 
> Bir Azure-SSIS IR'i sağınızda, SSIS için Access Redistributable ve Azure Özellik Paketi de yüklenir. Bu bileşenler, yerleşik bileşenlerin zaten desteklediği veri kaynaklarına ek olarak Excel dosyalarına, Access dosyalarına ve çeşitli Azure veri kaynaklarına bağlantı sağlar. Yükleyebileceğiniz diğer bileşenler hakkında daha fazla bilgi için [Azure-SSIS IR için Özel kuruluma](how-to-configure-azure-ssis-ir-custom-setup.md)bakın.

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma

SSISDB kullanıyorsanız, paketlerinizi bu pakete dağıtabilir ve SQL Server Data Tools (SSDT) veya SQL Server Management Studio (SSMS) araçlarını kullanarak Azure-SSIS IR'nizde çalıştırabilirsiniz. Bu araçlar, sunucu bitiş noktası üzerinden veritabanı sunucunuza bağlanır: 

- Bir Azure SQL Veritabanı sunucusu için sunucu `<server name>.database.windows.net`bitiş noktası biçimi.
- Özel bitiş noktası olan yönetilen bir örnek için `<server name>.<dns prefix>.database.windows.net`sunucu bitiş noktası biçimi.
- Genel bitiş noktası olan yönetilen bir örnek için `<server name>.public.<dns prefix>.database.windows.net,3342`sunucu bitiş noktası biçimi. 

SSISDB kullanmıyorsanız, paketlerinizi dosya sistemlerine, dosya paylaşımlarına veya Azure Dosyalarına dağıtabilir ve `dtinstall`"ve `dtutil` `dtexec` komut satırı" araçlarını kullanarak Azure-SSIS IR'nizde çalıştırabilirsiniz. Daha fazla bilgi için [bkz.](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) 

Her iki durumda da, Dağıtılan paketlerinizi Veri Fabrikası ardışık hatlarındaki SSIS Paketi etkinliğini kullanarak Azure-SSIS IR'de çalıştırabilirsiniz. Daha fazla bilgi için, [birinci sınıf bir Veri Fabrikası etkinliği olarak Invoke SSIS paket yürütme](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgelerdeki diğer Azure-SSIS IR konularına bakın:

- [Azure-SSIS tümleştirme çalışma zamanı.](concepts-integration-runtime.md#azure-ssis-integration-runtime) Bu makalede, Azure-SSIS IR de dahil olmak üzere genel olarak tümleştirme çalışma saatleri hakkında bilgi sağlar.
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede, Azure-SSIS IR'niz hakkındaki bilgileri nasıl alıp anlayacağınızı gösterilmektedir.
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makalede, Azure-SSIS IR'nizi nasıl durdurabileceğiniz, başlatabileceğiniz veya silebilirsiniz. Ayrıca, daha fazla düğüm ekleyerek Azure-SSIS IR'nizi nasıl ölçeklendirdiğinizi de gösterir.
- [Azure'da SSIS paketlerini dağıtma, çalıştırma ve izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Azure'da SSISDB'ye bağlanın](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 
- [Azure'da paket yürütmelerini zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)