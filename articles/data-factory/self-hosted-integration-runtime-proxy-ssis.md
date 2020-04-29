---
title: Kendini barındırılan tümleştirme çalışma zamanını SSIS için proxy olarak yapılandırma
description: Şirket içinde barındırılan tümleştirme çalışma zamanının bir Azure-SSIS Integration Runtime proxy olarak nasıl yapılandırılacağını öğrenin.
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
ms.date: 04/15/2020
ms.openlocfilehash: 4cb5b84f3889dcf4e0f28d525afb42cfeac5b54c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81605505"
---
# <a name="configure-a-self-hosted-ir-as-a-proxy-for-an-azure-ssis-ir-in-azure-data-factory"></a>Azure Data Factory içinde Azure-SSIS IR için otomatik olarak barındırılan bir IR ara sunucu olarak yapılandırma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, proxy olarak yapılandırılan şirket içinde barındırılan tümleştirme çalışma zamanı (Şirket içinde barındırılan IR) ile Azure Data Factory SQL Server Integration Services (SSIS) paketlerinin bir Azure-SSIS Integration Runtime (Azure-SSIS IR) üzerinde nasıl çalıştırılacağı açıklanmaktadır. 

Bu özellikle, [Azure-SSIS IR bir sanal ağa katmak](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)zorunda kalmadan şirket içi verilere erişebilirsiniz. Bu özellik, kurumsal ağınızın bir yapılandırması çok karmaşık olduğunda veya bir ilke Azure-SSIS IR eklemek için çok fazla kısıtlayıcı olduğunda faydalıdır.

Bu özellik, şirket içi veri kaynağı olan tüm SSIS veri akışı görevlerini iki hazırlama görevine ayırır: 
* Şirket içinde barındırılan IR üzerinde çalışan ilk görev, önce verileri şirket içi veri kaynağından Azure Blob depolama alanındaki bir hazırlama alanına taşımalıdır.
* Azure-SSIS IR çalışan ikinci görev, hazırlama alanındaki verileri amaçlanan veri hedefine taşıır.

Bu özelliğin diğer avantajları ve özellikleri, örneğin, henüz bir Azure-SSIS IR tarafından desteklenmeyen bölgelerde şirket içinde barındırılan IR ayarlamanıza ve veri kaynaklarınızın güvenlik duvarında şirket içinde barındırılan IR 'nizin genel statik IP adresine izin verir.

## <a name="prepare-the-self-hosted-ir"></a>Şirket içinde barındırılan IR 'yi hazırlama

Bu özelliği kullanmak için, önce bir veri fabrikası oluşturun ve içinde bir Azure-SSIS IR ayarlayın. Daha önce yapmadıysanız, [Azure-SSIS IR ayarlama](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)bölümündeki yönergeleri izleyin.

Daha sonra, Azure-SSIS IR ayarlandığı veri fabrikasında kendi kendine barındırılan IR 'yi ayarlarsınız. Bunu yapmak için bkz. [Şirket içinde BARıNDıRıLAN IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime).

Son olarak, şirket içi makinenize veya Azure sanal makinenize (VM) ek sürücü ve çalışma zamanının yanı sıra şirket içinde barındırılan IR 'nin en son sürümünü indirip yüklersiniz:
- [Şirket içinde BARıNDıRıLAN IR](https://www.microsoft.com/download/details.aspx?id=39717)'nin en son sürümünü indirip yükleyin.
- Paketlerinize nesne bağlama ve katıştırma veritabanı (OLEDB) bağlayıcıları kullanıyorsanız, henüz yapmadıysanız, ilgili OLEDB sürücülerini şirket içinde barındırılan IR 'nin yüklü olduğu makinede indirin ve yükleyin.  

  SQL Server (SQL Server Native Client [SQLNCLI]) için OLEDB sürücüsünün önceki sürümünü kullanıyorsanız, [64 bitlik sürümü indirin](https://www.microsoft.com/download/details.aspx?id=50402).  

  SQL Server (MSOLEDBSQL) için OLEDB sürücüsünün en son sürümünü kullanıyorsanız, [64 bitlik sürümü indirin](https://www.microsoft.com/download/details.aspx?id=56730).  
  
  PostgreSQL, MySQL, Oracle vb. gibi diğer veritabanı sistemleri için OLEDB sürücüleri kullanıyorsanız, 64 bitlik sürümleri Web sitelerinden indirebilirsiniz.
- Daha önce yapmadıysanız, [Visual C++ (VC) çalışma zamanının 64 bitlik sürümünü](https://www.microsoft.com/download/details.aspx?id=40784) , şirket IÇINDE barındırılan IR 'nin yüklü olduğu makinede indirin ve yükleyin.

## <a name="prepare-the-azure-blob-storage-linked-service-for-staging"></a>Azure Blob depolama ile bağlantılı hizmeti hazırlama için hazırlama

Daha önce yapmadıysanız, Azure-SSIS IR ayarlandığı veri fabrikasında bir Azure Blob depolama alanına bağlı hizmet oluşturun. Bunu yapmak için bkz. [Azure Data Factory 'ye bağlı hizmet oluşturma](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-linked-service). Aşağıdakileri yaptığınızdan emin olun:
- **Veri depolama**Için **Azure Blob depolama**' yı seçin.  
- **Tümleştirme çalışma zamanı aracılığıyla Bağlan**Için, Azure Blob depolama alanınızı erişim kimlik bilgilerini getirmek üzere varsayılan Azure IR kullandığımızda **Otomatik resolveıntegrationruntime** (Azure-SSIS IR ve kendi içinde barındırılan IR) seçeneğini belirleyin.
- **Kimlik doğrulama yöntemi**için **hesap anahtarı**, **SAS URI 'si**veya **hizmet sorumlusu**' nı seçin.  

    >[!TIP]
    >**Hizmet sorumlusu** yöntemini seçerseniz, hizmet sorumlusuna en az bir *Depolama Blobu veri katılımcısı* rolü verin. Daha fazla bilgi için [Azure Blob depolama Bağlayıcısı](connector-azure-blob-storage.md#linked-service-properties)' na bakın.

![Azure Blob depolama ile bağlantılı hizmeti hazırlama için hazırlama](media/self-hosted-integration-runtime-proxy-ssis/shir-azure-blob-storage-linked-service.png)

## <a name="configure-an-azure-ssis-ir-with-your-self-hosted-ir-as-a-proxy"></a>Ara sunucu olarak şirket içinde barındırılan IR ile bir Azure-SSIS IR yapılandırma

Şirket içinde barındırılan IR ve Azure Blob depolama ile bağlantılı hizmetinizi hazırlama için hazırladığınıza göre, artık yeni veya mevcut Azure-SSIS IR, Veri Fabrikası portalınızdaki veya uygulamanızda ara sunucu olarak şirket içinde barındırılan IR ile yapılandırabilirsiniz. Bunu yapmadan önce, mevcut Azure-SSIS IR zaten çalışıyorsa, durdurun ve yeniden başlatın.

1. **Tümleştirme çalışma zamanı kurulumu** bölmesinde, **İleri**' yi seçerek **Genel ayarlar** ve **SQL ayarları** bölümlerinin ötesine atlayın. 

1. **Gelişmiş ayarlar** bölümünde şunları yapın:

   1. Azure-SSIS Integration Runtime onay kutusu **Için şirket Içinde barındırılan Integration Runtime ara sunucu olarak ayarla** ' yı seçin. 

   1. Şirket içinde **barındırılan Integration Runtime** açılır listesinde, Azure-SSIS IR için mevcut şirket IÇINDE barındırılan IR 'yi bir ara sunucu olarak seçin.

   1. **Hazırlama depolama bağlı hizmeti** aşağı açılan listesinde, mevcut Azure Blob depolama ile bağlantılı hizmetinizi seçin veya hazırlama için yeni bir tane oluşturun.

   1. **Hazırlama yolu** kutusunda, seçili Azure Blob depolama hesabınızda bir blob kapsayıcısı belirtin veya hazırlama için varsayılan bir tane kullanmak üzere boş bırakın.

   1. **Devam**'ı seçin.

   ![Şirket içinde barındırılan IR ile gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

Ayrıca, PowerShell kullanarak, yeni veya mevcut Azure-SSIS IR şirket içinde barındırılan IR ile proxy olarak yapılandırabilirsiniz.

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

## <a name="enable-ssis-packages-to-connect-by-proxy"></a>SSIS paketlerini proxy ile bağlanacak şekilde etkinleştir

Visual Studio veya tek başına yükleyici için SSIS projeleri Uzantısı ile en son SSDT 'yi kullanarak, OLEDB veya düz dosya bağlantı yöneticilerine `ConnectByProxy` eklenen yeni bir özellik bulabilirsiniz.
* [Visual Studio için SSIS projeleri Uzantısı ile SSDT 'yi indirin](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects)
* [Tek başına yükleyiciyi indirme](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)   

OLEDB veya düz dosya kaynaklarıyla veri akışı görevleri içeren yeni paketleri tasarlarken, şirket içi veritabanlarına veya dosyalara erişmenize olanak tanıyan bu özelliği, ilgili bağlantı yöneticilerinin **Özellikler** bölmesinde *true* olarak ayarlayarak etkinleştirebilirsiniz.

![ConnectByProxy özelliğini etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-manager-properties.png)

Bu özelliği, var olan paketleri çalıştırdığınızda, bunları bir tane ile el ile değiştirmek zorunda kalmadan da etkinleştirebilirsiniz.  İki seçenek vardır:
- **Seçenek A**: Azure-SSIS IR için en son SSDT ile bu paketleri içeren projeyi açın, yeniden oluşturun ve yeniden dağıtın. Daha sonra ilgili bağlantı yöneticileri için özelliği *true* olarak ayarlayarak özelliği etkinleştirebilirsiniz. Paketleri SSMS 'den çalıştırırken, bu bağlantı yöneticileri **paket Çalıştır** açılır penceresinin **bağlantı yöneticileri** sekmesinde görüntülenir.

  ![ConnectByProxy Property2 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssms.png)

  Ayrıca, Data Factory işlem hatları 'nda paket çalıştırırken [SSIS paketi etkinliğini Çalıştır etkinliğinin](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) **bağlantı yöneticileri** sekmesinde görünen ilgili bağlantı yöneticileri için özelliği *true* olarak ayarlayarak de etkinleştirebilirsiniz.
  
  ![ConnectByProxy property3 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssis-activity.png)

- **Seçenek B:** SSIS IR 'niz üzerinde çalışmak üzere bu paketlerin bulunduğu projeyi yeniden dağıtın. Daha sonra özellik `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`yolunu sağlayarak özelliği etkinleştirebilir ve paketleri SSMS 'Den çalıştırırken **paket Çalıştır** açılır penceresinin **Gelişmiş** sekmesinde özellik geçersiz kılma olarak *true* olarak ayarlayabilirsiniz.

  ![ConnectByProxy property4 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-advanced-tab-ssms.png)

  Ayrıca, `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`özellik yolunu sağlayarak özelliği etkinleştirebilir ve Data Factory işlem hatlarında paket çalıştırırken [SSIS paketi yürütme etkinliğinin](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) **geçersiz** kılınması sekmesinde özellik geçersiz kılma olarak true olarak *ayarlanır* .
  
  ![ConnectByProxy Property5 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-property-overrides-tab-ssis-activity.png)

## <a name="debug-the-first-and-second-staging-tasks"></a>İlk ve ikinci hazırlama görevlerinde hata ayıkla

Şirket içinde barındırılan IR 'de, çalışma zamanı günlüklerini *C:\ProgramData\SSISTelemetry* klasöründe ve *C:\ProgramData\SSISTelemetry\ExecutionLog* klasöründeki ilk hazırlama görevlerinin yürütme günlüklerinde bulabilirsiniz.  Paketlerinizi SSSıSDB veya dosya sistemi, dosya paylaşımları veya Azure dosyalarında depoladığınıza bağlı olarak, SSıSDB veya belirtilen günlük yollarındaki ikinci hazırlama görevlerinin yürütme günlüklerini bulabilirsiniz. İkinci hazırlama görevlerinin yürütme günlüklerinde ilk hazırlama görevlerinin benzersiz kimliklerini de bulabilirsiniz. 

![İlk hazırlama görevinin benzersiz KIMLIĞI](media/self-hosted-integration-runtime-proxy-ssis/shir-first-staging-task-guid.png)

## <a name="use-windows-authentication-in-staging-tasks"></a>Hazırlama görevlerinde Windows kimlik doğrulamasını kullanma

Şirket içinde barındırılan IR 'de hazırlama görevleri Windows kimlik doğrulaması gerektiriyorsa, [SSIS paketlerinizi aynı Windows kimlik doğrulamasını kullanacak şekilde yapılandırın](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth?view=sql-server-ver15). 

Hazırlama görevleriniz, şirket içinde barındırılan IR Hizmeti hesabıyla (varsayılan olarak*NT SERVICE\DIAHostService*) çağrılacaktır ve veri depolarınız Windows kimlik doğrulama hesabıyla erişilir. Her iki hesap de belirli güvenlik ilkelerinin atanmasını gerektirir. Şirket içinde barındırılan IR makinesinde **yerel güvenlik ilkesi** > **Yerel ilkeler** > **Kullanıcı hakları ataması**' na gidin ve ardından aşağıdakileri yapın:

1. *Bir işlem için bellek kotalarını ayarlama* ve *bir işlem düzeyi belirteç* ILKELERINI şirket içinde barındırılan IR Hizmeti hesabıyla değiştirme. Bu, kendi kendine barındırılan IR 'yi varsayılan hizmet hesabıyla yüklediğinizde otomatik olarak gerçekleşir. Değilse, bu ilkeleri el ile atayın. Farklı bir hizmet hesabı kullanıyorsanız, buna aynı ilkeleri atayın.

1. Windows kimlik doğrulama hesabına *hizmet olarak oturum* açma ilkesi atayın.

## <a name="billing-for-the-first-and-second-staging-tasks"></a>İlk ve ikinci hazırlama görevleri için faturalandırma

Şirket içinde barındırılan IR üzerinde çalışan ilk hazırlama görevleri, tıpkı şirket içinde barındırılan bir IR üzerinde çalışan tüm veri taşıma etkinlikleri faturalandırılır, ayrı olarak faturalandırılır. Bu, [Azure Data Factory veri işlem hattı fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) makalesinde belirtilmiştir.

Azure-SSIS IR çalışan ikinci hazırlama görevleri ayrı olarak faturalandırılmaz, ancak çalıştırdığınız Azure-SSIS IR [Azure-SSIS IR fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/ssis/) makalesinde belirtilen şekilde faturalandırılır.

## <a name="enabling-tls-12"></a>TLS 1,2 etkinleştiriliyor

Güçlü şifreleme/daha güvenli ağ protokolü (TLS 1,2) kullanmanız ve şirket içinde barındırılan IR 'de eski SSL/TLS sürümlerini devre dışı bırakmanız gerekirse, genel önizleme kapsayıcımız için *Customsetupscript/Usersenaryolar/tls 1,2* klasöründe bulunan *Main. cmd* betiğini indirebilir ve çalıştırabilirsiniz.  [Azure Depolama Gezgini](https://storageexplorer.com/)kullanarak, aşağıdaki SAS URI 'sini girerek genel önizleme kapsayıcımıza bağlanabilirsiniz:

`https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2020-03-25T04:00:00Z&se=2025-03-25T04:00:00Z&sv=2019-02-02&sr=c&sig=WAD3DATezJjhBCO3ezrQ7TUZ8syEUxZZtGIhhP6Pt4I%3D`

## <a name="current-limitations"></a>Geçerli sınırlamalar

- Yalnızca açık veritabanı bağlantısı (ODBC)/OLEDB/düz dosya kaynakları veya OLEDB hedefi olan veri akışı görevleri şu anda destekleniyor. 
- Yalnızca *hesap anahtarı*, *paylaşılan ERIŞIM imzası (SAS) URI 'Si*veya *hizmet sorumlusu* kimlik doğrulaması ile yapılandırılmış Azure Blob depolama ile bağlantılı hizmetler şu anda desteklenmektedir.
- OLEDB kaynağında *Parametermapping* henüz desteklenmiyor. Geçici bir çözüm olarak, lütfen bir SQL komutuna değişkenlerinizi/parametrelerini eklemek için ' den *AccessMode* olarak *değişkenden SQL komutunu* kullanın ve *ifadesini* kullanın. Bir çizim olarak, genel önizleme kapsayımızın *Selfhostedirproxy/sınırlamalar* klasöründe bulunan *parametermappingsample. dtsx* paketine bakın. Azure Depolama Gezgini kullanarak, yukarıdaki SAS URI 'sini girerek genel önizleme kapsayıcımıza bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Şirket içinde barındırılan IR 'yi, Azure-SSIS IR için bir proxy olarak yapılandırdıktan sonra, Data Factory işlem hatlarında SSIS paketi etkinliklerini yürütmek üzere paketlerinizi dağıtıp çalıştırabilirsiniz. Nasıl yapılacağını öğrenmek için bkz. [Data Factory işlem hatları 'nda SSIS paketlerini yürütme SSIS paketi etkinlikleri](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).
