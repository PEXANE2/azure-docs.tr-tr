---
title: SSIS için proxy olarak kendi kendine barındırılan tümleştirme çalışma süresini yapılandırma
description: Azure-SSIS Tümleştirme Çalışma Süresi için proxy olarak kendi kendine barındırılan bir tümleştirme çalışma süresini nasıl yapılandırıştırmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 03/27/2020
ms.openlocfilehash: 9a1923057bc318869f491791520aacb4d0d17591
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346627"
---
# <a name="configure-a-self-hosted-ir-as-a-proxy-for-an-azure-ssis-ir-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Azure-SSIS IR için proxy olarak kendi barındırılan bir IR'yi yapılandırma

Bu makalede, SQL Server Integration Services (SSIS) paketlerinin Azure Veri Fabrikası'ndaki azure-SSIS Tümleştirme Çalışma Zamanında (Azure-SSIS IR) üzerinde nasıl çalıştırılacakve proxy olarak yapılandırılan kendi kendine barındırılan tümleştirme çalışma süresi (kendi kendine barındırılan IR) açıklanmaktadır. 

Bu özellik sayesinde, [Azure-SSIS IR'nize sanal bir ağa katılmak](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)zorunda kalmadan şirket içi verilere erişebilirsiniz. Bu özellik, kurumsal ağınızın azure-SSIS IR'nizi bu yapılandırmaya enjekte edemeyecek kadar karmaşık veya çok kısıtlayıcı bir yapılandırması varsa kullanışlıdır.

Bu özellik, şirket içi veri kaynağı olan herhangi bir SSIS veri akışı görevini iki hazırlama görevine böler: 
* Kendi barındırdığı IR'nizde çalışan ilk görev, verileri ilk olarak şirket içi veri kaynağından Azure Blob depolama alanınızdaki bir hazırlama alanına taşır.
* Azure-SSIS IR'nizde çalışan ikinci görev, ardından verileri hazırlama alanından hedeflenen veri hedefine taşır.

Bu özelliğin diğer avantajları ve yetenekleri, örneğin, henüz bir Azure-SSIS IR tarafından desteklenmeyen bölgelerde kendi barındırılan IR'nizi kurmanıza ve kendi barındırdığınız IR'nizin genel statik IP adresini veri kaynaklarınızın güvenlik duvarında ayarlamanıza olanak tanır.

## <a name="prepare-the-self-hosted-ir"></a>Kendi kendine barındırılan IR'yi hazırlayın

Bu özelliği kullanmak için önce bir veri fabrikası oluşturun ve içinde bir Azure-SSIS IR ayarlayın. Bunu daha önce yapmadıysanız, Bir [Azure-SSIS IR ayarlayın](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)yönergeleri izleyin.

Daha sonra azure-SSIS IR'nizin kurulduğu aynı veri fabrikasında kendi kendine barındırılan IR'nizi ayarlarsınız. Bunu yapmak [için,](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)bkz.

Son olarak, kendi barındırılan IR'nin en son sürümünün yanı sıra ek sürücüleri ve çalışma süresini şirket içi makinenize veya Azure sanal makinenize (VM) aşağıdaki şekilde indirip yüklersiniz:
- Kendi [kendine barındırılan IR'nin](https://www.microsoft.com/download/details.aspx?id=39717)en son sürümünü indirin ve kurun.
- Paketlerinizde Nesne Bağlama ve Katıştırma Veritabanı (OLEDB) bağlayıcılarını kullanıyorsanız, ilgili OLEDB sürücülerini kendi barındırdığınız IR'nizin yüklü olduğu makineye indirin ve yükleyin.  

  SQL Server için OLEDB sürücüsünün önceki sürümünü kullanıyorsanız (SQL Server Native Client [SQLNCLI]), [64 bit sürümünü indirin.](https://www.microsoft.com/download/details.aspx?id=50402)  

  SQL Server (MSOLEDBSQL) için OLEDB sürücüsünün en son sürümünü kullanıyorsanız, [64 bit sürümünü indirin.](https://www.microsoft.com/download/details.aspx?id=56730)  
  
  PostgreSQL, MySQL, Oracle gibi diğer veritabanı sistemleri için OLEDB sürücüleri kullanıyorsanız, 64 bit sürümlerini web sitelerinden indirebilirsiniz.
- Bunu daha önce yapmadıysanız, [Visual C++ (VC) çalışma zamanının 64 bit sürümünü](https://www.microsoft.com/download/details.aspx?id=40784) kendi barındırdığınız IR'nizin yüklü olduğu makineye indirin ve yükleyin.

## <a name="prepare-the-azure-blob-storage-linked-service-for-staging"></a>Azure Blob depolama alanına bağlı hizmeti hazırlamaya hazırlayın

Bunu daha önce yapmadıysanız, Azure-SSIS IR'nizin kurulduğu aynı veri fabrikasında Azure Blob depolama alanına bağlı bir hizmet oluşturun. Bunu yapmak için [bkz.](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-linked-service) Aşağıdakileri yapmayı unutmayın:
- **Veri Deposu**için **Azure Blob Depolama'yı**seçin.  
- **Tümleştirme çalışma süresi yle**Bağlan'da, Azure Blob Depolama alanınız için erişim kimlik bilgilerini almak için varsayılan Azure IR'sini kullandığımıziçin **Otomatik ÇözümlemeRuntime'ınızı** (Azure-SSIS IR'niz veya kendi barındırılan IR'niz hariç) seçeneğini belirleyin  
- **Kimlik Doğrulama yöntemi için**Hesap **tuşu,** **SAS URI**veya Hizmet **Sorumlusu'ni**seçin.  

    >[!TIP]
    >**Hizmet Sorumlusu** yöntemini seçerseniz, hizmet yöneticinize en az bir *Depolama Blob Veri Katılımcısı* rolü verin. Daha fazla bilgi için [Azure Blob depolama konektörüne](connector-azure-blob-storage.md#linked-service-properties)bakın.

![Azure Blob depolama alanına bağlı hizmeti hazırlamaya hazırlayın](media/self-hosted-integration-runtime-proxy-ssis/shir-azure-blob-storage-linked-service.png)

## <a name="configure-an-azure-ssis-ir-with-your-self-hosted-ir-as-a-proxy"></a>Bir Azure-SSIS IR'yi proxy olarak kendi barındırılan IR'nizle yapılandırın

Kendi kendine barındırılan IR ve Azure Blob depolama alanına bağlı hizmetinizi evreleme için hazırladıktan sonra, artık yeni veya mevcut Azure-SSIS IR'nizi veri fabrikası portalınızda veya uygulamanızda proxy olarak kendi kendine barındırılan IR ile yapılandırabilirsiniz. Ancak bunu yapmadan önce, varolan Azure-SSIS IR'niz zaten çalışıyorsa, durdurun ve yeniden başlatın.

1. **Tümleştirme çalışma zamanı kurulum** bölmesinde, **İleri'yi**seçerek **Genel Ayarlar** ve **SQL Ayarları** bölümlerini atlayın. 

1. Gelişmiş **ayarlar** bölümünde aşağıdakileri yapın:

   1. **Azure-SSIS Tümleştirme Çalışma Zamanı onay kutunuz için proxy olarak Kendi Kendine Barındırılan Tümleştirme Çalışma Süresini** Ayarla'yı seçin. 

   1. Kendi **Kendine Barındırılan Tümleştirme Çalışma Zamanı** açılır listesinde, azure-SSIS IR için bir proxy olarak varolan kendi kendine barındırılan IR'nizi seçin.

   1. Hazırlama **depolamasına bağlı hizmet** açılır listesinde, mevcut Azure Blob depolama alanına bağlı hizmetinizi seçin veya evreleme için yeni bir hizmet oluşturun.

   1. Hazırlama **yolu** kutusunda, seçtiğiniz Azure Blob depolama hesabınızda bir blob kapsayıcısı belirtin veya evreleme için varsayılan bir kapsayıcı kullanmak için boş bırakın.

   1. **Devam**'ı seçin.

   ![Kendi kendine barındırılan IR ile gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

PowerShell'i kullanarak yeni veya mevcut Azure-SSIS IR'nizi kendi barındırılan IR ile proxy olarak yapılandırabilirsiniz.

```powershell
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$AzureSSISName = "[your Azure-SSIS IR name]"
# Self-hosted integration runtime info - This can be configured as a proxy for on-premises data access 
$DataProxyIntegrationRuntimeName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingLinkedServiceName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingPath = "" # OPTIONAL to configure a proxy for on-premises data access 

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
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $DataFactoryName `
    -Name $AzureSSISName `
    -Force
```

## <a name="enable-ssis-packages-to-connect-by-proxy"></a>SSIS paketlerinin proxy ile bağlanmasını etkinleştirin

Visual Studio için SSIS Projects uzantılı en son SSDT'yi veya bağımsız `ConnectByProxy` bir yükleyiciyi kullanarak OLEDB veya Flat File bağlantı yöneticilerine eklenen yeni bir özellik bulabilirsiniz.
* [Visual Studio için SSIS Projects uzantısı ile SSDT'yi indirin](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects)
* [Bağımsız yükleyiciyi indirin](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)   

Veritabanlarına veya dosyalara şirket içinde erişmenize olanak tanıyan OLEDB veya Düz Dosya kaynaklarıyla veri akışı görevleri içeren yeni paketler tasarlarken, ilgili bağlantı yöneticilerinin **Özellikler** bölmesinde *True* olarak ayarlayarak bu özelliği etkinleştirebilirsiniz.

![ConnectByProxy özelliğini etkinleştirme](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-manager-properties.png)

Varolan paketleri tek tek el ile değiştirmek zorunda kalmadan, varolan paketleri çalıştırdığınızda da bu özelliği etkinleştirebilirsiniz.  İki seçenek vardır:
- **Seçenek A**: Azure-SSIS IR'nizde çalışacak en son SSDT ile bu paketleri içeren projeyi açın, yeniden oluşturun ve yeniden dağıtın. Daha sonra, ilgili bağlantı yöneticileri için *True* olarak ayarlayarak özelliği etkinleştirebilirsiniz. SSMS'ten paketleri çalıştırırken, bu bağlantı yöneticileri **Yürüt Paketi** açılır penceresinin **Bağlantı Yöneticileri** sekmesinde görünür.

  ![ConnectByProxy özelliğini etkinleştir2](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssms.png)

  Ayrıca, Veri Fabrikası ardışık hatlarında paketleri çalıştırırken [SSIS Paketini Yürüt'ün](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) **Bağlantı Yöneticileri** sekmesinde görünen ilgili bağlantı yöneticileri için özelliği *True* olarak ayarlayarak özelliği etkinleştirebilirsiniz.
  
  ![ConnectByProxy özelliğini etkinleştir3](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssis-activity.png)

- **B Seçeneği:** SSIS IR'nizde çalışacak şekilde bu paketleri içeren projeyi yeniden dağıtın. Daha sonra özelliğini özellik `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`yolunu sağlayarak ve SSMS'ten paketleri çalıştırırken **YürütEt Paketi** açılır penceresinin **Gelişmiş** sekmesinde özellik geçersiz kılınması olarak *True* olarak ayarlayabilirsiniz.

  ![ConnectByProxy özelliğini etkinleştir4](media/self-hosted-integration-runtime-proxy-ssis/shir-advanced-tab-ssms.png)

  Ayrıca, `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`özellik yolunu sağlayarak ve Veri Fabrikası ardışık hatlarında paketleri çalıştırırken [SSIS Paketini Yürüt'ün](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) **Özellik Geçersiz Kılmalar** sekmesinde özellik geçersiz kılma olarak *True* olarak ayarlayarak özelliği etkinleştirebilirsiniz.
  
  ![ConnectByProxy özelliğini etkinleştir5](media/self-hosted-integration-runtime-proxy-ssis/shir-property-overrides-tab-ssis-activity.png)

## <a name="debug-the-first-and-second-staging-tasks"></a>Birinci ve ikinci evreleme görevlerini hata ayıklama

Kendi barındırdığınız IR'nizde, *C:\ProgramData\SSISTelemetry* klasöründe çalışma zamanı günlüklerini ve *C:\ProgramData\SSISTelemetry\ExecutionLog* klasöründe ilk evreleme görevlerinin yürütme günlüklerini bulabilirsiniz.  Paketlerinizi SSISDB'de veya dosya sisteminde, dosya paylaşımlarında veya Azure Dosyalarında saklayıp saklamadığınıza bağlı olarak, ikinci evreleme görevlerinin yürütme günlüğünü SSISDB'nizde veya belirtilen günlük yollarınızda bulabilirsiniz. Ayrıca, ikinci evreleme görevlerinin yürütme günlüklerinde ilk evreleme görevlerinin benzersiz adlarını da bulabilirsiniz. 

![İlk evreleme görevinin benzersiz kimliği](media/self-hosted-integration-runtime-proxy-ssis/shir-first-staging-task-guid.png)

## <a name="use-windows-authentication-in-staging-tasks"></a>Evreleme görevlerinde Windows kimlik doğrulamasını kullanma

Kendi barındırılan IR'nizdeki evreleme görevleri Windows kimlik doğrulaması gerektiriyorsa, [SSIS paketlerinizi aynı Windows kimlik doğrulamasını kullanacak şekilde yapılandırın.](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth?view=sql-server-ver15) 

Evreleme görevleriniz, kendi kendine barındırılan IR hizmet hesabıyla *(NT SERVICE\DIAHostService*, varsayılan olarak) çağrılacak ve veri depolarınıza Windows kimlik doğrulama hesabıyla erişilir. Her iki hesap da kendilerine atanacak belirli güvenlik ilkeleri gerektirir. Kendi kendine barındırılan IR makinesinde, **Yerel Güvenlik İlkesi** > **Yerel İlkeler** > **Kullanıcı Hakları Ataması'na**gidin ve ardından aşağıdakileri yapın:

1. *Bir işlem için bellek kotalarını ayarla* ve kendi barındırılan IR hizmet hesabına işlem düzeyi *belirteç* ilkelerini değiştirin. Bu, varsayılan hizmet hesabıyla kendi barındırılan IR'nizi yüklediğinizde otomatik olarak oluşmalıdır. Değilse, bu ilkeleri el ile atayın. Farklı bir hizmet hesabı kullanıyorsanız, aynı ilkeleri ona atayın.

1. *Oturum Açma'yı hizmet* ilkesi olarak Windows Kimlik Doğrulama hesabına atayın.

## <a name="billing-for-the-first-and-second-staging-tasks"></a>Birinci ve ikinci evreleme görevleri için faturalandırma

Kendi barındırılan IR'nizde çalışan ilk evreleme görevleri, kendi barındırılan bir IR üzerinde çalışan tüm veri hareketi etkinlikleri faturalandırılınca ayrı ayrı faturalandırılır. Bu, Azure [Veri Fabrikası veri ardışık fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) makalesinde belirtilir.

Azure-SSIS IR'nizde çalışan ikinci evreleme görevleri ayrı olarak faturalandırılmaz, ancak çalışan Azure-SSIS [IR'niz Azure-SSIS IR fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/ssis/) makalesinde belirtildiği şekilde faturalandırılır.

## <a name="enabling-tls-12"></a>TLS 1.2'yi etkinleştirme

Güçlü şifreleme/daha güvenli ağ protokolü (TLS 1.2) kullanmanız ve kendi barındırılan IR'nizde eski SSL/TLS sürümlerini devre dışı kalmanız gerekiyorsa, genel önizleme kapsayıcımızın *CustomSetupScript/UserScenarios/TLS 1.2* klasöründe bulunan *main.cmd* komut dosyasını indirebilir ve çalıştırabilirsiniz.  [Azure Depolama Gezgini'ni](https://storageexplorer.com/)kullanarak, aşağıdaki SAS URI'yi girerek genel önizleme kabımıza bağlanabilirsiniz:

`https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2020-03-25T04:00:00Z&se=2025-03-25T04:00:00Z&sv=2019-02-02&sr=c&sig=WAD3DATezJjhBCO3ezrQ7TUZ8syEUxZZtGIhhP6Pt4I%3D`

## <a name="current-limitations"></a>Geçerli sınırlamalar

- Şu anda yalnızca Açık Veritabanı Bağlantısı (ODBC)/OLEDB/Flat File kaynaklarına sahip veri akışı görevleri desteklenmektedir. 
- Yalnızca *Hesap anahtarı*, *Paylaşılan Erişim İmzası (SAS) URI*veya Hizmet *Sorumlusu* kimlik doğrulaması ile yapılandırılan Azure Blob depolamasına bağlı hizmetler şu anda desteklenir.
- OLEDB Kaynak'ta *Parametre Haritalama* henüz desteklenmedi. Geçici çözüm olarak, lütfen *AccessMode* olarak *Değişkenden SQL* Komutu'nu kullanın ve değişkenlerinizi/parametrelerinizi bir SQL komutuna eklemek için *Expression'ı* kullanın. Bir örnek olarak, genel önizleme kapsayıcımızın *SelfHostedIRProxy/Sınırlamalar* klasöründe bulunan *ParameterMappingSample.dtsx* paketine bakın. Azure Depolama Gezgini'ni kullanarak, yukarıdaki SAS URI'yi girerek genel önizleme kabımıza bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure-SSIS IR'niz için kendi barındırdığınız IR'nizi bir proxy olarak yapılandırdıktan sonra, paketlerinizi Veri Fabrikası ardışık hatlarında SSIS Paketi etkinliklerini yürütün olarak şirket içinde verilere erişmek için dağıtabilir ve çalıştırabilirsiniz. Nasıl yapılacağını öğrenmek için, [Veri Fabrikası ardışık hatlarında SSIS Paketi etkinliklerini yürütün olarak SSIS paketlerini çalıştırın.](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)
