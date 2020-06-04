---
title: Azure-SSIS Integration Runtime için kurulumu özelleştirme
description: Bu makalede, ek bileşenleri yüklemek veya ayarları değiştirmek için bir Azure-SSIS Integration Runtime için özel kurulum arabiriminin nasıl kullanılacağı açıklanır.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
manager: mflasko
ms.reviewer: douglasl
ms.custom: seo-lt-2019
ms.date: 06/03/2020
ms.openlocfilehash: 576861265771977f7e13140dd595f47bf556e585
ms.sourcegitcommit: 79508e58c1f5c58554378497150ffd757d183f30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84331908"
---
# <a name="customize-the-setup-for-an-azure-ssis-integration-runtime"></a>Azure-SSIS Integration Runtime için kurulumu özelleştirme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Factory (ADF) içindeki bir Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR) için özel kurulum, Azure-SSIS IR sağlama veya yeniden yapılandırma sırasında kendi adımlarınızı eklemek için bir arabirim sağlar. 

Özel kurulumu kullanarak, varsayılan işletim yapılandırması veya ortamını, örneğin, ek Windows Hizmetleri başlatmak, dosya paylaşımları için erişim kimlik bilgilerini kalıcı hale getirmek veya güçlü şifreleme/daha güvenli ağ protokolü (TLS 1,2) kullanmak üzere değiştirebilirsiniz. İsterseniz, Azure-SSIS IR her bir düğümüne derlemeler, sürücüler veya uzantılar gibi ek özel/üçüncü taraf bileşenleri de yükleyebilirsiniz. Yerleşik/önceden yüklenmiş bileşenler hakkında daha fazla bilgi için, bkz. [Azure-SSIS IR yerleşik/önceden yüklenmiş bileşenler](https://docs.microsoft.com/azure/data-factory/built-in-preinstalled-components-ssis-integration-runtime).

Azure-SSIS IR özel kurulumları iki şekilde yapabilirsiniz: 
* **Komut dosyası Ile standart özel kurulum**: bir betiği ve ilişkili dosyalarını hazırlayın ve hepsini Azure Depolama hesabınızdaki bir blob kapsayıcısına birlikte karşıya yükleyin. Daha sonra, Azure-SSIS IR ayarladığınızda veya yeniden yapılandırdığınızda, kapsayıcınıza yönelik bir paylaşılan erişim Imzası (SAS) Tekdüzen Kaynak tanımlayıcısı (URI) sağlarsınız. Azure-SSIS IR her bir düğüm, komut dosyasını ve ilişkili dosyalarını kapsayıcınızdan indirir ve özel kurulumunuzu yükseltilmiş izinlerle çalıştırır. Özel Kurulum tamamlandığında her düğüm, çalışma ve diğer günlüklerin standart çıkışını kapsayıcınıza yükler.
* **Komut dosyası olmadan hızlı özel kurulum**: bazı ortak sistem yapılandırma ve Windows komutlarını çalıştırın ya da herhangi bir betiği kullanmadan popüler veya önerilen ek bileşenleri kurun.

Standart ve hızlı özel kurulumlarla, ücretsiz, lisanssız bileşenleri ve ücretli, lisanslı bileşenleri yükleyebilirsiniz. Bağımsız bir yazılım satıcısı (ISV) iseniz, bkz. [Azure-SSIS IR için ücretli veya lisanslı bileşenler geliştirme](how-to-develop-azure-ssis-ir-licensed-components.md).

> [!IMPORTANT]
> Gelecekteki geliştirmelerden faydalanmak için, özel kurulumla Azure-SSIS IR için v3 veya üzeri bir düğüm serisi kullanmanızı öneririz.

## <a name="current-limitations"></a>Geçerli sınırlamalar

Aşağıdaki sınırlamalar yalnızca standart özel kurulumların için geçerlidir:

- Derlemeleri genel derleme önbelleği 'ne (GAC) yüklemek için betiğinizdeki *Gacutil. exe* ' yi kullanmak istiyorsanız, özel kurulumlarınızın bir parçası olarak *Gacutil. exe* ' yi sağlamanız gerekir. Ya da, daha sonra "yönergeler" bölümünde ele alınan *genel önizleme* kapsayıcımız içinde sunulan kopyayı kullanabilirsiniz.

- Betiğinizdeki bir alt klasöre başvurmak istiyorsanız, *msiexec. exe* `.\` kök klasöre başvurmak için gösterimi desteklemez. Yerine bir komut kullanın `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...` .

- Windows tarafından otomatik olarak oluşturulan yönetimsel paylaşımlar veya gizli ağ paylaşımları Şu anda Azure-SSIS IR desteklenmemektedir.

- IBM ıferies erişimi ODBC sürücüsü Azure-SSIS IR desteklenmez. Özel kurulumunuz sırasında yükleme hataları görebilirsiniz. Bunu yaparsanız yardım için IBM desteği ile iletişime geçin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure-SSIS IR özelleştirmek için aşağıdaki öğeler gereklidir:

- [Bir Azure aboneliği](https://azure.microsoft.com/)

- [Azure-SSIS IR sağlama](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

- [Bir Azure depolama hesabı](https://azure.microsoft.com/services/storage/). Express özel kurulumları için gerekli değildir. Standart özel kurulumlarda, özel kurulum komut dosyanızı ve ilişkili dosyalarını bir blob kapsayıcısına yükler ve depolar. Özel Kurulum işlemi aynı zamanda yürütme günlüklerini aynı blob kapsayıcısına yükler.

## <a name="instructions"></a>Yönergeler

ADF Kullanıcı arabirimindeki özel kurulumlarla Azure-SSIS IR sağlayabilir veya yeniden yapılandırabilirsiniz. PowerShell kullanarak aynı yapmak istiyorsanız, [Azure PowerShell](/powershell/azure/install-az-ps)indirin ve yükleyin.

### <a name="standard-custom-setup"></a>Standart özel kurulum

Azure-SSIS IR standart özel kurulumlarla sağlamak veya yeniden yapılandırmak için aşağıdaki adımları izleyin.

1. Özel kurulum komut dosyanızı ve ilişkili dosyalarını (örneğin,. bat,. cmd,. exe,. dll,. msi veya. ps1 dosyaları) hazırlayın.

   * Özel kurulumlarınızın giriş noktası olan *Main. cmd*adlı bir betik dosyanız olmalıdır.  
   * Betiğin sessizce yürütülebileceğini sağlamak için, bunu önce yerel makinenizde test etmenizi öneririz.  
   * Diğer araçların (örneğin, *msiexec. exe*), kapsayıcınıza yüklenmek üzere oluşturulmasını istiyorsanız, önceden tanımlanmış ortam değişkenini `CUSTOM_SETUP_SCRIPT_LOG_DIR` betiklerinizde günlük klasörü olarak belirtin (örneğin, *msiexec/i xxx. msi/quiet/LV% CUSTOM_SETUP_SCRIPT_LOG_DIR% \ install. log*).

1. [Azure Depolama Gezgini](https://storageexplorer.com/)indirin, yükleyin ve açın.

   a. **(Yerel ve bağlı)** altında, **depolama hesapları**' na sağ tıklayın ve ardından **Azure depolama 'ya Bağlan**' ı seçin.

      ![Azure Depolama’ya bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

   b. **Depolama hesabı adı ve anahtarı kullan**' ı seçin ve ardından **İleri**' yi seçin.

      ![Depolama hesabı adını ve anahtarını kullanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

   c. Azure depolama hesabınızın adını ve anahtarınızı girip, **İleri**' yi seçin ve ardından **Bağlan**' ı seçin.

      ![Depolama hesabı adını ve anahtarını belirtin](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

   d. Bağlı Azure depolama hesabınız altında, **BLOB kapsayıcıları**' na sağ tıklayın, **BLOB kapsayıcısı oluştur**' u seçin ve yeni kapsayıcıyı adlandırın.

      ![Blob kapsayıcısı oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

   e. Yeni kapsayıcıyı seçin ve özel kurulum komut dosyanızı ve ilişkili dosyalarını karşıya yükleyin. *Main. cmd* ' yi, kapsayıcının en üst düzeyinde (herhangi bir klasörde değil) karşıya yüklediğinizden emin olun. Ayrıca, kapsayıcınıza yalnızca gerekli özel kurulum dosyalarını içerdiğinden emin olun, böylece daha sonra Azure-SSIS IR indirmek uzun zaman almaz. Özel kurulumun en uzun süresi şu anda zaman aşımına uğramadan önce 45 dakika içinde ayarlanmıştır. Bu, kapsayıcıınızdan tüm dosyaları indirme ve Azure-SSIS IR yükleme saatini içerir. Kurulum daha fazla zaman gerektiriyorsa, bir destek bileti yükseltin.

      ![Blob kapsayıcısına dosya yükleme](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

   f. Kapsayıcıya sağ tıklayın ve **paylaşılan erişim Imzası al**' ı seçin.

      ![Kapsayıcı için paylaşılan erişim Imzasını al](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

   örneğin: Kapsayıcınız için SAS URI 'sini, yeterince uzun bir süre sonu ve okuma/yazma/listeleme izni ile oluşturun. Azure-SSIS IR herhangi bir düğüm yeniden oluşturulduğunda veya yeniden başlatıldığında özel kurulum komut dosyanızı ve ilişkili dosyalarını indirmek ve çalıştırmak için SAS URI 'sine ihtiyacınız vardır. Kurulum yürütme günlüklerini karşıya yüklemek için yazma izninizin olması gerekir.

      > [!IMPORTANT]
      > SAS URI 'sinin süresi dolana ve özel kurulum kaynaklarının Azure-SSIS IR, özellikle de bu süre boyunca düzenli olarak durdurup Azure-SSIS IR başlatırsanız, oluşturma işleminin silinmesine göre her zaman kullanılabilir olduğundan emin olun.

      ![Kapsayıcı için paylaşılan erişim Imzasını oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

   h. Kapsayıcının SAS URI 'sini kopyalayın ve kaydedin.

      ![Paylaşılan erişim Imzasını Kopyala ve Kaydet](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

1. ADF Kullanıcı arabirimindeki Azure-SSIS IR sağladığınızda veya yeniden yapılandırdığınızda, **tümleştirme çalışma zamanı kurulum** bölmesinin **gelişmiş ayarlar** sayfasında **Azure-SSIS Integration Runtime ek sistem yapılandırması/bileşen yüklemeleri ile Özelleştir** onay kutusunu seçin ve **özel kurulum KAPSAYıCıSı SAS URI 'si** kutusuna kapsayıcının SAS URI 'sini girin.

   ![Özel kurulumlarla Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-custom.png)

### <a name="express-custom-setup"></a>Hızlı özel kurulum

Azure-SSIS IR Express özel kurulumlarla sağlamak veya yeniden yapılandırmak için aşağıdaki adımları izleyin.

1. ADF Kullanıcı arabirimindeki Azure-SSIS IR sağladığınızda veya yeniden yapılandırdığınızda, **tümleştirme çalışma zamanı kurulum** bölmesinin **gelişmiş ayarlar** sayfasında **Azure-SSIS Integration Runtime ek sistem yapılandırması/bileşen yüklemeleri ile Özelleştir** onay kutusunu seçin. 

1. **Yeni** ' yi seçerek **hızlı özel kurulum ekle** bölmesini açın ve ardından **hızlı özel kurulum türü** açılan listesinde bir tür seçin:

   * **Cmdkey komut türünü Çalıştır** ' ı seçerseniz, hedeflenen bilgisayar adı veya etki alanı adı, hesap adı veya Kullanıcı adı ve **/Add**, **/User**ve **/pass** kutularında hesap anahtarınızı veya parolayı girerek Azure-SSIS IR dosya paylaşımlarınız veya Azure dosya paylaşımlarınız için erişim kimlik bilgilerini kalıcı hale getirebilirsiniz. Bu, yerel makinenizde Windows [cmdkey](https://docs.microsoft.com/windows-server/administration/windows-commands/cmdkey) komutunu çalıştırmaya benzer.
   
   * **Ortam değişkeni Ekle** türünü seçerseniz, **değişken adı** ve **değişken değeri** kutularına ortam değişkeni adı ve değeri girerek Azure-SSIS IR çalışan paketleriniz için kullanılacak Windows ortam değişkenlerini ekleyebilirsiniz. Bu, yerel makinenizde Windows [set](https://docs.microsoft.com/windows-server/administration/windows-commands/set_1) komutunu çalıştırmaya benzer.

   * **Lisanslı bileşen türünü yüklemeyi** seçerseniz, **bileşen adı** açılır listesinde ISV iş ortaklarımızın tümleşik bir bileşenini seçebilirsiniz:

     * **Sentryone 'ın 'ın görev fabrikası** bileşenini seçerseniz, **Lisans anahtarı** kutusuna onlardan satın aldığınız ürün lisans anahtarını girerek, sentryone 'ın ' den bir bileşen [görev fabrikası](https://www.sentryone.com/products/task-factory/high-performance-ssis-components) paketini Azure-SSIS IR yükleyebilirsiniz. Geçerli tümleşik sürüm **2019.4.3**.

     * **Oh22's HEDDA ' yi seçerseniz. IO** bileşeni, Hedda 'yi yükleyebilirsiniz [. ](https://hedda.io/ssis-component/)Hizmet satın aldıktan sonra Azure-SSIS IR, GÇ veri kalitesi/oh22 'tan bileşeni Temizleme. Geçerli tümleşik sürüm **1.0.13**.

     * **Oh22's SQLPhonetics.net** bileşenini seçerseniz, **Lisans anahtarı** kutusuna onlardan satın aldığınız ürün lisans anahtarını girerek Azure-SSIS IR [SQLPhonetics.net](https://appsource.microsoft.com/product/web-apps/oh22.sqlphonetics-ssis) veri kalitesini/eşleşen bileşeni yükleyebilirsiniz. Geçerli tümleşik sürüm **1.0.43**.

     * **Kingswaysoft 'ıN SSIS Tümleştirme Araç Seti** bileşenini seçerseniz, Microsoft Dynamics/SharePoint/Project Server, Oracle/Salesforce Marketing Cloud gibi CRM/ERP/pazarlama/işbirliği uygulamalarına yönelik [SSIS Integration Toolkit](https://www.kingswaysoft.com/products/ssis-integration-toolkit-for-microsoft-dynamics-365) Bağlayıcısı paketini, **Lisans anahtarı** kutusuna onlardan satın aldığınız ürün lisans anahtarını girerek Azure-SSIS IR yükleyebilirsiniz. Geçerli tümleşik sürüm **2019,2**' dir.

     * **Kingswaysoft 'ıN SSIS üretkenlik paketi** bileşenini seçerseniz, **Lisans anahtarı** kutusuna onlardan satın aldığınız ürün lisans anahtarını girerek Azure-SSIS IR ssmsoft ' den alınan [SSIS üretkenlik paketi](https://www.kingswaysoft.com/products/ssis-productivity-pack) paketini yükleyebilirsiniz. Geçerli tümleşik sürüm **10,0**' dir.

     * **The Obald yazılımının Xdikkatini** alma bileşenini seçerseniz, bu bilgisayardan satın aldığınız ürün lisans dosyasını **Lisans dosyası** kutusuna bırakarak/karşıya yükleyerek & Azure-SSIS IR, SAP sistemi (ERP, s/4HANA, siyah beyaz) Için, Şirket içindeki Obald yazılımlarından [xplanıg](https://theobald-software.com/en/xtract-is/) Bağlayıcısı paketini yükleyebilirsiniz. Geçerli tümleşik sürüm **6.1.1.3**.

Eklediğiniz Express özel kurulumları **Gelişmiş ayarlar** sayfasında görünür. Bunları kaldırmak için onay kutularını işaretleyin ve ardından **Sil**' i seçin.

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell kullanarak özel kurulumlarla Azure-SSIS IR sağlamak veya yeniden yapılandırmak için aşağıdaki adımları izleyin.

1. Azure-SSIS IR zaten başlatılmış/çalışıyor ise, önce bunu durdurun.

1. Daha sonra, `Set-AzDataFactoryV2IntegrationRuntime` Azure-SSIS IR başlamadan önce cmdlet 'ini çalıştırarak özel kurulumları ekleyebilir veya kaldırabilirsiniz.

   ```powershell
   $ResourceGroupName = "[your Azure resource group name]"
   $DataFactoryName = "[your data factory name]"
   $AzureSSISName = "[your Azure-SSIS IR name]"
   # Custom setup info: Standard/express custom setups
   $SetupScriptContainerSasUri = "" # OPTIONAL to provide a SAS URI of blob container for standard custom setup where your script and its associated files are stored
   $ExpressCustomSetup = "[RunCmdkey|SetEnvironmentVariable|SentryOne.TaskFactory|oh22is.SQLPhonetics.NET|oh22is.HEDDA.IO|KingswaySoft.IntegrationToolkit|KingswaySoft.ProductivityPack|Theobald.XtractIS or leave it empty]" # OPTIONAL to configure an express custom setup without script

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
   Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
       -DataFactoryName $DataFactoryName `
       -Name $AzureSSISName `
       -Force
   ```

1. Standart özel kurulum bittikten ve Azure-SSIS IR başladıktan sonra, *Main. cmd* ve diğer yürütme günlüklerinin standart çıkışını kapsayıcının *Main. cmd. log* klasöründe bulabilirsiniz.

### <a name="standard-custom-setup-samples"></a>Standart özel kurulum örnekleri

Standart özel kurulumların bazı örneklerini görüntülemek ve yeniden kullanmak için aşağıdaki adımları izleyin.

1. Azure Depolama Gezgini kullanarak genel önizleme kapsayımuza bağlanın.

   a. **(Yerel ve bağlı)** altında **depolama hesapları**' na sağ tıklayın, **Azure depolama 'ya Bağlan**' ı seçin, **bir bağlantı dizesi veya paylaşılan erişim imzası URI 'Si kullan**' ı seçin ve ardından **İleri**' yi seçin.

      ![Paylaşılan erişim Imzasıyla Azure depolama 'ya bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

   b. **SAS URI 'Si kullan** ' ı seçin ve ardından **URI** kutusuna aşağıdaki SAS URI 'sini girin:

      `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2020-03-25T04:00:00Z&se=2025-03-25T04:00:00Z&sv=2019-02-02&sr=c&sig=WAD3DATezJjhBCO3ezrQ7TUZ8syEUxZZtGIhhP6Pt4I%3D`

      ![Kapsayıcı için paylaşılan erişim Imzasını sağlama](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

   c. **İleri**' yi seçin ve ardından **Bağlan**' ı seçin.

   d. Sol bölmede bağlı **publicpreview** kapsayıcısını seçin ve ardından *customsetupscript* klasörüne çift tıklayın. Bu klasörde aşağıdaki öğeler vardır:

      * Azure-SSIS IR her bir düğümüne temel bir görev yüklemeye yönelik özel bir kurulum içeren *örnek* bir klasör. Görev hiçbir şey yapmaz, ancak birkaç saniye uyku moduna geçmez. Klasör Ayrıca, tüm içeriklerin (*Gacutil. exe*, *Gacutil. exe. config*ve *1033 \ gacutlrc. dll*), kapsayıcınıza olduğu gibi kopyalanabilmesi için bir *Gacutil* klasörü içerir.

      * Gerçek Kullanıcı senaryolarından birkaç özel kurulum örneği içeren bir *Usersenaryolar* klasörü.

        ![Genel Önizleme kapsayıcısının içeriği](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

   e. Aşağıdaki öğeleri bulmak için *Usersenaryolar* klasörünü çift tıklatın:

      * Azure-SSIS IR her bir düğümdeki özel bileşenler için gerekebilecek .NET Framework önceki bir sürümünü yüklemek için özel bir kurulum betiği (*Main. cmd*) içeren BIR *.NET Framework 3,5* klasörü.

      * Azure-SSIS IR her bir düğümüne toplu kopyalama programı (*bcp*) dahil olmak üzere SQL Server komut satırı yardımcı programlarını (*Mssqlcmdlnutils. msi*) yüklemek için özel bir kurulum betiği (*Main. cmd*) içeren bir *bcp* klasörü.

      * Komut dosyası görevlerinde kullanabileceğiniz ve Azure-SSIS IR her bir düğümdeki Excel dosyalarını dinamik olarak okumak ve yazmak için kullanabileceğiniz C# derlemelerini ve kitaplıklarını yüklemek üzere özel bir kurulum betiği (*Main. cmd*) Içeren bir *Excel* klasörü. 
      
        İlk olarak, [*Exceldatareader. dll*](https://www.nuget.org/packages/ExcelDataReader/) ve [*documentformat. openxml. dll*](https://www.nuget.org/packages/DocumentFormat.OpenXml/)dosyasını indirin ve ardından Tümünü *Main. cmd* ile birlikte kapsayıcınıza yükleyin. Alternatif olarak, yalnızca standart Excel Bağlantı Yöneticisi, Excel kaynağı ve Excel hedefini kullanmak istiyorsanız, gerekli erişim yeniden dağıtılabilir Azure-SSIS IR önceden yüklenmiş olduğundan, özel kuruluma gerek kalmaz.
      
      * Azure-SSIS IR her bir düğümüne MySQL ODBC sürücülerini yüklemek için özel bir kurulum betiği (*Main. cmd*) Içeren BIR *MySQL ODBC* klasörü. Bu kurulum, MySQL sunucusuna bağlanmak için ODBC bağlantı yöneticisini, kaynağını ve hedefini kullanmanıza olanak sağlar. 
     
        İlk olarak, [MySQL ODBC sürücü yükleyicilerinin en son 64-bit ve 32 bit sürümlerini indirin](https://dev.mysql.com/downloads/connector/odbc/) (örneğin, *MySQL-Connector-ODBC-8.0.13-Winx64. msi* ve *MySQL-Connector-ODBC-8.0.13-Win32. msi*) ve ardından bunları *ana. cmd* ile birlikte kapsayıcınıza yükleyin.

      * Azure-SSIS IR Enterprise Edition 'ın her bir düğümüne Oracle bağlayıcıları ve OCı sürücüsünü yüklemek için özel bir kurulum betiği (*Main. cmd*) ve sessiz yükleme yapılandırma dosyası (*Client. rsp*) içeren bir *Oracle ENTERPRISE* klasörü. Bu kurulum, Oracle sunucusuna bağlanmak için Oracle bağlantı yöneticisini, kaynağını ve hedefini kullanmanıza olanak sağlar. 
      
        İlk olarak, [Microsoft Indirme merkezi](https://www.microsoft.com/en-us/download/details.aspx?id=55179) 'nden Oracle (*Attunityssisorauyarlanterssetup. msi* ve *AttunitySSISOraAdaptersSetup64. msi*) için Microsoft bağlayıcıları v 5.0 ' ı ve [Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html)'dan en son Oracle istemcisini (örneğin, *winx64_12102_client. zip*) indirin ve ardından bunları kendi *ana. cmd* ve *Client. rsp* ile birlikte kapsayıcınıza yükleyin. Oracle 'a bağlanmak için TNS kullanırsanız, *tnsnames. ora*'yı indirmeniz, düzenlemeniz ve bu dosyayı kapsayıcınıza yüklemeniz gerekir, bu sayede kurulum sırasında Oracle yükleme klasörüne kopyalanabilirler.

      * Azure-SSIS IR her bir düğümüne Oracle ODP.NET sürücüsünü yüklemek için özel bir kurulum betiği (*Main. cmd*) Içeren BIR *Oracle standart ADO.net* klasörü. Bu kurulum, Oracle sunucusuna bağlanmak için ADO.NET bağlantı yöneticisini, kaynağını ve hedefini kullanmanıza olanak sağlar. 
      
        İlk olarak, [en son Oracle odp.net sürücüsünü indirin](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html) (örneğin, *ODP. NET_Managed_ODAC122cR1. zip*) ve ardından dosyayı *Main. cmd* ile birlikte kapsayıcınıza yükleyin.
       
      * Oracle ODBC sürücüsünü yüklemek için özel bir kurulum betiği (*Main. cmd*) içeren ve Azure-SSIS IR her bir düğümde veri kaynağı adı 'Nı (DSN) yapılandıran BIR *Oracle standart ODBC* klasörü. Bu kurulum, Oracle sunucusuna bağlanmak için ODBC bağlantı Yöneticisi, kaynak ve hedef ya da Power Query bağlantı yöneticisini ve kaynak ODBC veri kaynağı türünü kullanmanıza olanak sağlar. 
      
        İlk olarak, en son Oracle Instant Client 'ı (temel paket veya temel Lite paketi) ve ODBC paketini indirin ve ardından bunları kendi *ana. cmd* ile birlikte kapsayıcınıza yükleyin:
        * [64 bitlik paketleri indirin](https://www.oracle.com/technetwork/topics/winx64soft-089540.html) (temel paket: *instantclient-Basic-Windows. x64-18.3.0.0.0 dbru. zip*; Temel Lite paketi: *instantclient-basiclite-Windows. x64-18.3.0.0.0 dbru. zip*; ODBC paketi: *instantclient-ODBC-Windows. x64-18.3.0.0.0 dbru. zip*) 
        * [32 bitlik paketleri indirin](https://www.oracle.com/technetwork/topics/winsoft-085727.html) (temel paket: *instantclient-Basic-NT-18.3.0.0.0 dbru. zip*; Temel Lite paketi: *instantclient-basiclite-NT-18.3.0.0.0 dbru. zip*; ODBC paketi: *instantclient-ODBC-NT-18.3.0.0.0 dbru. zip*)

      * Azure-SSIS IR her bir düğümüne Oracle OLEDB sürücüsünü yüklemek için özel bir kurulum betiği (*Main. cmd*) Içeren BIR *Oracle Standard OLEDB* klasörü. Bu kurulum, Oracle sunucusuna bağlanmak için OLEDB bağlantı yöneticisini, kaynağını ve hedefini kullanmanıza olanak sağlar. 
     
        İlk olarak, [en son Oracle oledb sürücüsünü indirin](https://www.oracle.com/partners/campaign/index-090165.html) (örneğin, *ODAC122010Xcopy_x64. zip*) ve ardından bunu *ana. cmd* ile birlikte kapsayıcınıza yükleyin.

      * Azure-SSIS IR her bir düğümüne PostgreSQL ODBC sürücülerini yüklemek için özel bir kurulum betiği (*Main. cmd*) Içeren bir *PostgreSQL ODBC* klasörü. Bu kurulum, PostgreSQL sunucusuna bağlanmak için ODBC bağlantı yöneticisini, kaynağını ve hedefini kullanmanıza olanak sağlar. 
     
        İlk olarak, [PostgreSQL ODBC sürücü yükleyicilerinin en son 64-bit ve 32-bit sürümlerini indirin](https://www.postgresql.org/ftp/odbc/versions/msi/) (örneğin, *psqlodbc_x64. msi* ve *psqlodbc_x86. msi*) ve ardından bunları kendi *ana. cmd* ile birlikte kapsayıcınıza yükleyin.

      * Azure-SSIS IR Enterprise Edition 'ın her bir düğümüne SAP .NET bağlayıcı derlemesini (*librfc32. dll*) yüklemek için özel bir kurulum betiği (*Main. cmd*) içeren *SAP BW* klasörü. Bu kurulum, SAP BW sunucusuna bağlanmak için SAP Business Warehouse (siyah beyaz) bağlantı yöneticisini, kaynağını ve hedefini kullanmanıza olanak sağlar. 
      
        İlk olarak, *librfc32. dll* ' nin 64 bit veya 32 BIT sürümünü SAP yükleme klasöründen. dll ' nin, kapsayıcınıza *Main. cmd* ile birlikte yükleyin. Daha sonra betik, kurulum sırasında SAP derlemesini *%Windir%\SysWOW64* veya *%Windir%\System32* klasörüne kopyalar.

      * Azure-SSIS IR her bir düğümüne Azure PowerShell yüklemek için özel bir kurulum betiği (*Main. cmd*) Içeren bir *depolama* klasörü. Bu kurulum, [Azure depolama hesabınızı işlemek Için PowerShell betikleri](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell)çalıştıran SSIS paketlerini dağıtmanıza ve çalıştırmanıza olanak tanır. 
      
        Bir örnek *AzurePowerShell. msi* (veya en son sürümü kullanın) ve *depolama. ps1* *öğesini kapsayıcınıza kopyalayın.* Paketleriniz için bir şablon olarak *PowerShell. dtsx* kullanın. Paket şablonu, *Storage. ps1* öğesini değiştirilebilir bir PowerShell betiği olarak yükleyen bir [Azure Blob indirme görevini](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task)ve komut dosyasını her düğümde yürüten bir [işlem Çalıştır görevi](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/)birleştirir.

      * Özel bir kurulum betiği (*Main. cmd*), ilişkili dosya (*Install. cmd*) ve yükleyici paketleri (*. msi*) içeren bir *Teradata* klasörü. Bu dosyalar, Azure-SSIS IR Enterprise Edition 'ın her bir düğümüne Teradata bağlayıcıları, Teradata Parallel Transporter (TPT) API 'sini ve ODBC sürücüsünü yükler. Bu kurulum, Teradata sunucusuna bağlanmak için Teradata bağlantı yöneticisini, kaynağını ve hedefini kullanmanıza olanak sağlar. 
      
        İlk olarak, [Teradata araçları ve yardımcı programlar 15. x ZIP dosyasını](http://partnerintelligence.teradata.com) (örneğin, *TeradataToolsAndUtilitiesBase__windows_indep. 15.10.22.00. zip*) indirip daha önce bahsedilen *. cmd* ve *. msi* dosyalarıyla birlikte kapsayıcınıza yükleyin.

      * Güçlü şifreleme/daha güvenli ağ protokolü (TLS 1,2) kullanmak için özel bir kurulum betiği (*Main. cmd*) Içeren bir *TLS 1,2* klasörü ve Azure-SSIS IR her bir düğümünde eski SSL/TLS sürümlerini devre dışı bırakın.

      * Azure-SSIS IR her bir düğümüne Zulu OpenJDK yüklemek için özel bir kurulum betiği (*Main. cmd*) ve PowerShell dosyası (*install_openjdk. ps1*) IÇEREN BIR *Zulu OpenJDK* klasörü. Bu kurulum, ORC ve Parquet dosyalarını işlemek için Azure Data Lake Store ve esnek dosya bağlayıcıları kullanmanıza olanak sağlar. Daha fazla bilgi için bkz. [Tümleştirme Hizmetleri Için Azure Feature Pack](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-ver15#dependency-on-java). 
      
        İlk olarak, [en son Zulu OpenJDK](https://www.azul.com/downloads/zulu/zulu-windows/) 'yi (örneğin, *Zulu 8.33.0.1-jdk 8.0.192-win_x64. zip*) indirin ve ardından bunu *Main. cmd* ve *install_openjdk. ps1* ile birlikte, kapsayıcınıza yükleyin.

        ![Kullanıcı senaryoları klasöründeki klasörler](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

   f. Bu standart özel kurulum örneklerini yeniden kullanmak için, seçili klasörün içeriğini kapsayıcınıza kopyalayın.

1. ADF Kullanıcı arabirimindeki Azure-SSIS IR sağladığınızda veya yeniden yapılandırdığınızda, **tümleştirme çalışma zamanı kurulum** bölmesinin **gelişmiş ayarlar** sayfasında **Azure-SSIS Integration Runtime ek sistem yapılandırması/bileşen yüklemeleri ile Özelleştir** onay kutusunu seçin ve **özel kurulum KAPSAYıCıSı SAS URI 'si** kutusuna kapsayıcının SAS URI 'sini girin.
   
1. Azure PowerShell kullanarak Azure-SSIS IR sağladığınızda veya yeniden yapılandırdığınızda, zaten başlatılmış/çalışıyor durumunda uygulamayı durdurun, `Set-AzDataFactoryV2IntegrationRuntime` cmdlet 'ini parametre değeri olarak KAPSAYıCıNıN SAS URI 'siyle çalıştırın `SetupScriptContainerSasUri` ve ardından Azure-SSIS IR başlatın.

1. Standart özel kurulum bittikten ve Azure-SSIS IR başladıktan sonra, *Main. cmd* ve diğer yürütme günlüklerinin standart çıkışını kapsayıcının *Main. cmd. log* klasöründe bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure-SSIS IR Enterprise sürümünü ayarlama](how-to-configure-azure-ssis-ir-enterprise-edition.md)
- [Azure-SSIS IR için ücretli veya lisanslı bileşenler geliştirin](how-to-develop-azure-ssis-ir-licensed-components.md)
