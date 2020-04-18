---
title: Azure-SSIS Tümleştirme Çalışma Süresi için kurulumu özelleştirme
description: Bu makalede, ek bileşenler yüklemek veya ayarları değiştirmek için Azure-SSIS Tümleştirme Çalışma Zamanı için özel kurulum arabiriminin nasıl kullanılacağı açıklanmaktadır
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
ms.date: 04/15/2020
ms.openlocfilehash: ab2ba31d6b712bd3399bc8bf5b491337d462dac9
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606214"
---
# <a name="customize-the-setup-for-an-azure-ssis-integration-runtime"></a>Azure-SSIS Tümleştirme Çalışma Süresi için kurulumu özelleştirme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure-SSIS IR'nizin kurulumu veya yeniden yapılandırılması sırasında kendi adımlarınızı eklemek için bir arabirim sağlar. 

Özel kurulumu kullanarak, varsayılan işletim yapılandırmasını veya ortamını, örneğin ek Windows hizmetleri başlatmak, dosya paylaşımları için erişim kimlik bilgilerini sürdürmek veya güçlü şifreleme/daha güvenli ağ protokolü (TLS 1.2) kullanmak üzere değiştirebilirsiniz. Veya Azure-SSIS IR'nizin her düğümüne derlemeler, sürücüler veya uzantılar gibi ek bileşenler yükleyebilirsiniz.

Azure-SSIS IR'nizde özel kurulumlar yapabilirsiniz: 
* **Komut dosyası olmadan özel kurulum express**: Bazı yaygın sistem yapılandırmaları ve Windows komutları çalıştırın veya herhangi bir komut dosyası kullanmadan bazı popüler veya önerilen ek bileşenleri yükleyin.
* **Komut dosyası yla standart özel kurulum**: Bir komut dosyası ve ilişkili dosyaları hazırlayın ve bunların hepsini Azure depolama hesabınızdaki bir blob kapsayıcısına birlikte yükleyin. Azure-SSIS IR'nizi ayarladığınızda veya yeniden yapılandırdığınızda kapsayıcınız için paylaşılan erişim imzası (SAS) Tek düzen kaynak tanımlayıcısı (URI) sağlarsınız. Azure-SSIS IR'nizin her düğümü daha sonra komut dosyasını ve ilişkili dosyalarını kapsayıcınızdan indirir ve özel kurulumunuzu yüksek izinlerle çalıştırır. Özel kurulumunuz tamamlandığında, her düğüm yürütmenin standart çıktısını ve diğer günlükleri kapsayıcınıza yükler.

Hem ücretsiz, lisanssız bileşenleri hem de ücretli, lisanslı bileşenleri ekspres ve standart özel kurulumlarla yükleyebilirsiniz. Bağımsız bir yazılım satıcısıysanız (ISV), [bkz.](how-to-develop-azure-ssis-ir-licensed-components.md)

> [!IMPORTANT]
> Gelecekteki geliştirmelerden yararlanmak için Azure-SSIS IR'niz için özel kurulumla v3 veya daha sonraki düğüm serilerini kullanmanızı öneririz.

## <a name="current-limitations"></a>Geçerli sınırlamalar

Aşağıdaki sınırlamalar yalnızca standart özel kurulumlar için geçerlidir:

- Genel montaj önbelleğine (GAC) derlemeler yüklemek için komut dosyanızda *gacutil.exe* kullanmak istiyorsanız, özel kurulumunuzun bir parçası olarak *gacutil.exe* sağlamanız gerekir. Veya daha sonra "Talimatlar" bölümünde tartışılan *Genel Önizleme* kapsayıcımızda sağlanan kopyayı kullanabilirsiniz.

- Komut dosyanızda bir alt klasöre başvurmak istiyorsanız, *msiexec.exe* kök klasörüne `.\` başvuru gönderme notunu desteklemez. `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` Bunun yerine bir komut `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`kullanın.

- Windows tarafından otomatik olarak oluşturulan yönetim paylaşımları veya gizli ağ paylaşımları şu anda Azure-SSIS IR'de desteklenmez.

- IBM iSeries Access ODBC sürücüsü Azure-SSIS IR'de desteklenmez. Özel kurulumunuz sırasında yükleme hataları görebilirsiniz. Bunu yaparsanız, yardım için IBM desteğine başvurun.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure-SSIS IR'nizi özelleştirmek için aşağıdaki öğelere ihtiyacınız vardır:

- [Azure aboneliği](https://azure.microsoft.com/)

- [Azure-SSIS IR'nizi sağlama](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

- [Bir Azure depolama hesabı.](https://azure.microsoft.com/services/storage/) Ekspres özel kurulumlar için gerekli değildir. Standart özel kurulumlar için, özel kurulum komut dosyanızı ve ilişkili dosyalarını bir blob kapsayıcısına yükleyip saklarsınız. Özel kurulum işlemi, yürütme günlüklerini de aynı blob kapsayıcısına yükler.

## <a name="instructions"></a>Yönergeler

1. Azure-SSIS IR'nizi PowerShell ile kurmak veya yeniden yapılandırmak istiyorsanız, [Azure PowerShell'i](/powershell/azure/install-az-ps)indirin ve yükleyin. Ekspres özel kurulumlar için adım 4'e atlayın.

1. Özel kurulum komut dosyanızı ve ilişkili dosyalarını (örneğin, .bat, .cmd, .exe, .dll, .msi veya .ps1 dosyaları) hazırlayın.

   * *Main.cmd*adlı bir komut dosyası dosyanız olmalıdır, bu da özel kurulumunuzun giriş noktasıdır.  
   * Komut dosyasının sessizce yürütülebilmesini sağlamak için, önce yerel makinenizde test etmenizi öneririz.  
   * Başka araçlar tarafından oluşturulan ek günlüklerin (örneğin, *msiexec.exe)* kapsayıcınıza yüklenmesini istiyorsanız, komut `CUSTOM_SETUP_SCRIPT_LOG_DIR`dosyalarınızdaki günlük klasörü olarak önceden tanımlanmış ortam değişkenini belirtin (örneğin, *msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log*).

1. [Azure Depolama Gezgini'ni](https://storageexplorer.com/)indirin, yükleyin ve açın. Bunu yapmak için:

   a. Altında **(Yerel ve Bağlı)**, **Sağ tıkla Depolama Hesapları**ve sonra Azure depolama ya **bağlan'ı**seçin.

      ![Azure Depolama’ya bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

   b. **Bir depolama hesabı adı ve anahtarı kullan'ı seçin**ve sonra **İleri'yi**seçin.

      ![Depolama hesabı adını ve anahtarını kullanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

   c. Azure depolama hesabı adınızı ve anahtarınızı girin, **İleri**'yi seçin ve sonra **Bağlan'ı**seçin.

      ![Depolama hesabı adı ve anahtarı sağlama](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

   d. Bağlı Azure depolama hesabınız altında **Blob Kapsayıcıları'nı**sağ tıklatın, **Blob Kapsayıcısı Oluştur'u**seçin ve yeni kapsayıcıya ad ver.

      ![Blob kapsayıcısı oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

   e. Yeni kapsayıcıyı seçin ve özel kurulum komut dosyanızı ve ilişkili dosyalarını yükleyin. *Main.cmd'yi* herhangi bir klasöre değil, kapsayıcınızın en üst seviyesine yüklediğinizden emin olun. Ayrıca, konteynırınızın yalnızca gerekli özel kurulum dosyalarını içerdiğinden emin olun, böylece bunları Azure-SSIS IR'nize daha sonra indirmenin uzun sürmeyecektir. Özel kurulumun maksimum süresi şu anda, zaman bitmeden 45 dakika olarak ayarlanır. Buna, kapsayıcınızdaki tüm dosyaları indirme ve Azure-SSIS IR'ye yükleme zamanı da dahildir. Kurulum daha fazla zaman gerektiriyorsa, destek bileti yükseltin.

      ![Dosyaları blob kapsayıcısına yükleme](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

   f. Kapsayıcıya sağ tıklayın ve ardından **Paylaşılan Erişim İmzasını Al'ı**seçin.

      ![Kapsayıcı için Paylaşılan Erişim İmzasını Alma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

   g. Kabınız için yeterince uzun bir son kullanma süresi ne kadar uzun ve okuma/yazma/liste izni yle SAS URI'yi oluşturun. Azure-SSIS IR'nizin herhangi bir düğümü yeniden görüntülendiğinde veya yeniden başlatıldığında özel kurulum komut dosyanızı ve ilişkili dosyalarını indirmek ve çalıştırmak için SAS URI'ye ihtiyacınız vardır. Kurulum yürütme günlüklerini yüklemek için yazma iznine ihtiyacınız vardır.

      > [!IMPORTANT]
      > SAS URI'nin süresinin dolmadığından ve azure-SSIS IR'nizin tüm yaşam döngüsü boyunca özel kurulum kaynaklarının kullanılabilir olduğundan emin olun, özellikle bu dönemde Azure-SSIS IR'nizi düzenli olarak durdurup başlattığınızda, oluşturmadan silmeye kadar.

      ![Kapsayıcı için Paylaşılan Erişim İmzasını Oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

   h. Kabınızın SAS URI'sini kopyalayın ve kaydedin.

      ![Paylaşılan Erişim İmzasını kopyalayın ve kaydedin](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

1. Azure-SSIS IR'nizi bir veri fabrikası kullanıcı arabirimiyle ayarladığınızda veya yeniden yapılandırdığınızda, **Tümleştirme çalışma zamanı kurulum** bölmesinin **Gelişmiş Ayarlar** bölümünde ek sistem yapılandırmaları/bileşen yüklemeleri onay kutusunu **ekleyerek Azure-SSIS Tümleştirme Çalışma Sürenizi Özelleştir'i** seçerek özel ayarlar ekleyebilir veya kaldırabilirsiniz. 

   Standart özel kurulumlar eklemek istiyorsanız, **Özel kurulum kapsayıcısı SAS URI** kutusuna kabınızın SAS URI'sini girin. 
   
   Ekspres özel kurulumlar eklemek istiyorsanız, **Ekle Express özel kurulum** bölmesini açmak için **Yeni'yi** seçin ve ardından Express özel **kurulum türü** açılır listesinde bir tür seçin:

   * **Run cmdkey komut** türünü seçerseniz, hedeflenen bilgisayar adınızı veya etki alanı adınızı, hesap adınızı veya kullanıcı adınızı ve **/Ekle**, **/Kullanıcı**ve **/Pass** kutularına hesap anahtarı veya parolasını girerek dosya paylaşımlarınızın veya Azure Dosyaları paylaşımlarınızın veya Azure Dosyaları paylaşımlarınızın erişim kimlik bilgilerini devam ettirebilirsiniz. Bu, yerel makinenizde Windows [cmdkey](https://docs.microsoft.com/windows-server/administration/windows-commands/cmdkey) komutunu çalıştırmaya benzer.
   
   * Ortam ekle **değişken** türünü seçerseniz, Çevre değişken adınızı ve **Değişken** **değer** kutularına ortamınızı değişken adınızı ve değerigirerek Azure-SSIS IR'de çalışan paketlerinizde kullanılacak Windows ortamı değişkenleri ekleyebilirsiniz. Bu, yerel makinenizde Windows [kümesi](https://docs.microsoft.com/windows-server/administration/windows-commands/set_1) komutunu çalıştırmaya benzer.

   * Lisanslı bileşen türünü **yükle'yi** seçerseniz, **Bileşen adı** açılır listesindeki ISV iş ortaklarımızdan entegre bir bileşen seçebilirsiniz:

     * **SentryOne'un Görev Fabrikası** bileşenini seçerseniz, [Task Factory](https://www.sentryone.com/products/task-factory/high-performance-ssis-components) SentryOne'daki bileşenlerin paketini, **Lisans anahtar** kutusuna satın aldığınız ürün lisans anahtarını girerek Azure-SSIS IR'nize yükleyebilirsiniz. Geçerli entegre sürümü **2019.4.3**.

     * **Oh22's HEDDA seçerseniz. IO** bileşeni, [HEDDA yükleyebilirsiniz. ](https://hedda.io/ssis-component/)Hizmetini satın aldıktan sonra Azure-SSIS IR'nizde oh22'den IO veri kalitesi/temizleme bileşeni. Geçerli entegre sürümü **1.0.13**olduğunu.

     * **OH22'nin SQLPhonetics.NET** bileşenini seçerseniz, satın aldığınız ürün lisans anahtarını **Lisans anahtar** kutusuna girerek oh22'deki [SQLPhonetics.NET](https://sqlphonetics.oh22.is/sqlphonetics-net-for-microsoft-ssis/) veri kalitesi/eşleştirme bileşenini Azure-SSIS IR'nize yükleyebilirsiniz. Geçerli entegre sürümü **1.0.43**olduğunu.

     * **KingswaySoft'un SSIS Entegrasyon Araç Kiti** bileşenini seçerseniz, Microsoft Dynamics/SharePoint/Project Server, Oracle/Salesforce Marketing Cloud gibi CRM/ERP/pazarlama/işbirliği uygulamaları için [SSIS Entegrasyon Araç Seti](https://www.kingswaysoft.com/products/ssis-integration-toolkit-for-microsoft-dynamics-365) paketini Azure-SSIS IR'nize, Lisans **anahtarına** satın aldığınız ürün lisans anahtarını girerek yükleyebilirsiniz. Geçerli entegre sürümü **2019.2**.

     * **KingswaySoft'un SSIS Üretkenlik Paketi** bileşenini seçerseniz, [SSIS Verimlilik Paketi](https://www.kingswaysoft.com/products/ssis-productivity-pack) bileşenini, Lisans **anahtar** kutusuna satın aldığınız ürün lisans anahtarını girerek KingswaySoft bileşenleripaketini Azure-SSIS IR'nize yükleyebilirsiniz. Geçerli entegre sürümü **10.0.**

     * **Theobald Software'in Xtract IS** bileşenini seçerseniz, Satın aldığınız ürün lisans dosyasını **Lisans dosyası** kutusuna & sürükleyerek/yükleyerek, Azure-SSIS IR'nizdeki Theobald Software'den SAP sistemi (ERP, S/4HANA, BW) için [Xtract IS](https://theobald-software.com/en/xtract-is/) konektör paketini yükleyebilirsiniz. Geçerli entegre sürümü **6.1.1.3.**

   Eklenen ekspres özel kurulumlarınız **Gelişmiş Ayarlar** bölümünde görünür. Bunları kaldırmak için onay kutularını seçin ve sonra **Sil'i**seçin.

   ![Özel kurulumlarla gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-custom.png)

1. Azure-SSIS IR'nizi PowerShell ile ayarladığınızda veya yeniden yapılandırdığınızda, Azure-SSIS IR'nizi başlatmadan önce `Set-AzDataFactoryV2IntegrationRuntime` cmdlet'i çalıştırarak özel kurulumlar ekleyebilir veya kaldırabilirsiniz.
   
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
   
   Standart özel kurulumunuz bittikten ve Azure-SSIS IR'niz başladıktan sonra, depolama kabınızın *main.cmd.log* klasöründe *main.cmd* ve diğer yürütme günlüklerinin standart çıktısını bulabilirsiniz.

1. Bazı standart özel kurulum örneklerini görüntülemek için Azure Depolama Gezgini'ni kullanarak Genel Önizleme kapsayıcımıza bağlanın.

   a. Altında **(Yerel ve Ekli)**, Sağ **tıkdepolama Hesapları**, **Azure depolama ya bağlan'** ı seçin, **bağlantı dizesini veya paylaşılan erişim imzası URI'yi seçin**ve ardından **İleri'yi**seçin.

      ![Paylaşılan Erişim İmzasıyla Azure depolama alanına bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

   b. **SAS URI kullan'ı** seçin ve ardından **URI** kutusuna aşağıdaki SAS URI'yi girin:

      `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2020-03-25T04:00:00Z&se=2025-03-25T04:00:00Z&sv=2019-02-02&sr=c&sig=WAD3DATezJjhBCO3ezrQ7TUZ8syEUxZZtGIhhP6Pt4I%3D`

      ![Kapsayıcı için Paylaşılan Erişim İmzasını sağlama](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

   c. **İleri'yi**seçin ve sonra **Bağlan'ı**seçin.

   d. Sol bölmede, bağlı **publicpreview** kapsayıcısını seçin ve ardından *CustomSetupScript* klasörünü çift tıklatın. Bu klasörde aşağıdaki öğeler şunlardır:

      * Azure-SSIS IR'nizin her düğümüne temel bir görev yüklemek için özel bir kurulum içeren *örnek* klasör. Görev birkaç saniye uyumaktan başka bir şey yapmaz. Klasör de olan tüm içeriği *(gacutil.exe*, *gacutil.exe.config*, ve *1033\gacutlrc.dll*) konteyner olduğu gibi kopyalanabilir bir *gacutil* klasörü içerir.

      * Gerçek kullanıcı senaryolarından birkaç özel kurulum örneği içeren *UserScenarios* klasörü.

        ![Genel önizleme kapsayıcısının içeriği](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

   e. Aşağıdaki öğeleri bulmak için *UserScenarios* klasörünü çift tıklatın:

      * .NET *FRAMEWORK 3.5* klasörü, Azure-SSIS IR'nizin her düğümünde özel bileşenler için gerekli olabilecek .NET Framework'ün önceki bir sürümünü yüklemek için özel bir kurulum komut dosyası *(main.cmd)* içerir.

      * Sql Server komut satırı yardımcı programları *(MsSqlCmdLnUtils.msi),* toplu kopyalama programı *(bcp)* dahil olmak üzere, Azure-SSIS IR'nizin her düğümünde yüklemek için özel bir kurulum komut dosyası *(main.cmd)* içeren bir *BCP* klasörü.

      * Azure-SSIS IR'nizin her düğümünde Excel dosyalarını dinamik olarak okumak ve yazmak için komut dosyası görevlerinde kullanabileceğiniz C# derlemelerini ve kitaplıklarını yüklemek için özel bir kurulum komut dosyası *(main.cmd)* içeren bir *EXCEL* klasörü. 
      
        İlk olarak, [*ExcelDataReader.dll*](https://www.nuget.org/packages/ExcelDataReader/) ve [*DocumentFormat.OpenXml.dll*](https://www.nuget.org/packages/DocumentFormat.OpenXml/)indirin ve sonra konteyner *main.cmd* ile birlikte hepsini yükleyin. Alternatif olarak, yalnızca standart Excel Bağlantı Yöneticisi, Excel kaynağı ve Excel hedefini kullanmak istiyorsanız, gerekli erişim yeniden dağıtılabilir zaten Azure-SSIS IR'nizde önceden yüklenmiş olduğundan özel kuruluma gerek yoktur.
      
      * Azure-SSIS IR'nizin her düğümüne MySQL ODBC sürücülerini yüklemek için özel bir kurulum komut dosyası *(main.cmd)* içeren bir *MYSQL ODBC* klasörü. Bu kurulum, MySQL sunucusuna bağlanmak için ODBC Bağlantı Yöneticisi, kaynak ve hedef kullanmanızı sağlar. 
     
        İlk olarak, [MySQL ODBC sürücü yükleyicilerinin en son 64 bit ve 32 bit sürümlerini indirin](https://dev.mysql.com/downloads/connector/odbc/) (örneğin, *mysql-konektör-odbc-8.0.13-winx64.msi* ve *mysql-konektör-odbc-8.0.13-win32.msi)* ve *main.cmd* ile birlikte konteynerinize yükleyin.

      * Azure-SSIS IR Enterprise Edition'ınızın her düğümüne Oracle bağlayıcılarını ve OCI sürücüsünü yüklemek için özel kurulum komut dosyası *(main.cmd)* ve sessiz yükleme config dosyası *(client.rsp)* içeren bir *ORACLE ENTERPRISE* klasörü. Bu kurulum, Oracle Sunucusuna bağlanmak için Oracle Bağlantı Yöneticisi, kaynak ve hedef kullanmanızı sağlar. 
      
        İlk olarak, Oracle için Microsoft Connectors v5.0 indirin *(AttunitySSISOraAdaptersSetup.msi* ve *AttunitySSISOraAdaptersSetup64.msi*) [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=55179) ve en son Oracle istemcisi (örneğin, *winx64_12102_client.zip*) [Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html), ve sonra konteyner *main.cmd* ve *client.rsp* ile birlikte hepsini yükleyin. Oracle'a bağlanmak için TNS kullanıyorsanız, kurulum sırasında Oracle yükleme klasörüne kopyalanabilmesi için *tnsnames.ora,* düzenle ve kapsayıcınıza yüklemeniz gerekir.

      * Oracle ODP.NET sürücüsünü Azure-SSIS IR'nizin her düğümüne yüklemek için özel bir kurulum komut dosyası *(main.cmd)* içeren *bir ORACLE STANDARD ADO.NET* klasörü. Bu kurulum, Oracle sunucusuna bağlanmak için ADO.NET Bağlantı Yöneticisi, kaynak ve hedef kullanmanızı sağlar. 
      
        İlk olarak, [en son Oracle ODP.NET sürücüsünü indirin](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html) (örneğin, *ODP.NET_Managed_ODAC122cR1.zip),* ve *main.cmd* ile birlikte kapsayıcınıza yükleyin.
       
      * Oracle ODBC sürücüsünü yüklemek ve Azure-SSIS IR'nizin her düğümünde veri kaynak adını (DSN) yapılandırmak için özel bir kurulum komut dosyası *(main.cmd)* içeren *bir ORACLE STANDARD ODBC* klasörü. Bu kurulum, Oracle sunucusuna bağlanmak için ODBC Bağlantı Yöneticisi, kaynak ve hedef veya Güç Sorgusu Bağlantı Yöneticisi ve ODBC veri kaynağı türü ile kaynak kullanmanıza olanak sağlar. 
      
        İlk olarak, en son Oracle Instant Client (Temel Paket veya Temel Lite Paketi) ve ODBC Paketi indirin ve sonra hepsini *main.cmd* ile birlikte kabınıza yükleyin:
        * [64 bit paket indir](https://www.oracle.com/technetwork/topics/winx64soft-089540.html) (Temel Paket: *instantclient-basic-windows.x64-18.3.0.0.0dbru.zip*; Temel Lite Paketi: *instantclient-basiclite-windows.x64-18.3.0.0dbru.zip*; ODBC Paketi: *instantclient-odbc-windows.x64-18.3.0.0.0dbru.zip*) 
        * [32 bit lik paketleri indirin](https://www.oracle.com/technetwork/topics/winsoft-085727.html) (Temel Paket: *instantclient-basic-nt-18.3.0.0.0dbru.zip;* Temel Lite Paketi: *instantclient-basiclite-nt-18.3.0.0dbru.zip*; ODBC Paketi: *instantclient-odbc-nt-18.3.0.0.0dbru.zip*)

      * Azure-SSIS IR'nizin her düğümüne Oracle OLEDB sürücüsünü yüklemek için özel bir kurulum komut dosyası *(main.cmd)* içeren *bir ORACLE STANDART OLEDB* klasörü. Bu kurulum, Oracle sunucusuna bağlanmak için OLEDB Bağlantı Yöneticisi, kaynak ve hedef kullanmanızı sağlar. 
     
        İlk olarak, [en son Oracle OLEDB sürücüsünü indirin](https://www.oracle.com/partners/campaign/index-090165.html) (örneğin, *ODAC122010Xcopy_x64.zip),* ve *main.cmd* ile birlikte konteynerinize yükleyin.

      * Azure-SSIS IR'nizin her düğümüne PostgreSQL ODBC sürücülerini yüklemek için özel bir kurulum komut dosyası *(main.cmd)* içeren *bir POSTGRESQL ODBC* klasörü. Bu kurulum, PostgreSQL sunucusuna bağlanmak için ODBC Bağlantı Yöneticisi, kaynak ve hedef kullanmanızı sağlar. 
     
        İlk olarak, [PostgreSQL ODBC sürücü yükleyicilerinin en son 64 bit ve 32 bit sürümlerini (örneğin,](https://www.postgresql.org/ftp/odbc/versions/msi/) *psqlodbc_x64.msi* ve *psqlodbc_x86.msi)* indirin ve *main.cmd* ile birlikte konteynerinize yükleyin.

      * Azure-SSIS IR Enterprise Edition'ınızın her düğümüne SAP .NET bağlayıcı derlemesini *(librfc32.dll)* yüklemek için özel bir kurulum komut dosyası *(main.cmd)* içeren bir *SAP BW* klasörü. Bu kurulum, SAP BW sunucusuna bağlanmak için SAP İş Ambarı (BW) Bağlantı Yöneticisini, kaynağını ve hedefi kullanmanıza olanak tanır. 
      
        İlk olarak, SAP yükleme klasöründen 64 bit veya *librfc32.dll'nin* 32 bit sürümünü *main.cmd* ile birlikte kabınıza yükleyin. Komut dosyası daha sonra SAP derlemesini kurulum sırasında *%windir%\SysWow64* veya *%windir%\System32* klasörüne kopyalar.

      * Azure-SSIS IR'nizin her düğümüne Azure PowerShell yüklemek için özel bir kurulum komut dosyası *(main.cmd)* içeren bir *DEPOLAMA* klasörü. Bu kurulum, Azure depolama hesabınızı işlemek [için PowerShell komut dosyalarını](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell)çalıştıran SSIS paketlerini dağıtmanıza ve çalıştırmanıza olanak tanır. 
      
        *Main.cmd,* örnek bir *AzurePowerShell.msi* (veya en son sürümü kullanın) ve *storage.ps1'i* kapsayıcınıza kopyalayın. *PowerShell.dtsx'i* paketleriniz için şablon olarak kullanın. Paket şablonu, *storage.ps1'i* değiştirilebilir powershell komut dosyası olarak indiren bir [Azure Blob indirme görevi](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task)ve her düğümdeki komut dosyasını çalıştıran bir Yürütme işlemi [görevini](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/)birleştirir.

      * Özel bir kurulum komut dosyası *(main.cmd),* ilişkili dosya *(install.cmd)* ve yükleyici paketleri (*.msi)* içeren *teradata* klasörü. Bu dosyalar, Azure-SSIS IR Enterprise Edition'ınızın her düğümüne Teradata bağlayıcılarını, Teradata Paralel Taşıyıcı (TPT) API'sini ve ODBC sürücüsünü yükler. Bu kurulum, Teradata sunucusuna bağlanmak için Teradata Bağlantı Yöneticisi, kaynak ve hedef kullanmanızı sağlar. 
      
        İlk olarak, [Teradata Tools and Utilities 15.x zip dosyasını (örneğin,](http://partnerintelligence.teradata.com) *TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip)* indirin ve daha sonra daha önce bahsedilen *.cmd* ve *.msi* dosyalarıyla birlikte kapsayıcınıza yükleyin.

      * Güçlü *TLS 1.2* şifreleme/daha güvenli ağ protokolü (TLS*main.cmd*1.2) kullanmak ve Azure-SSIS IR'nizin her düğümünde eski SSL/TLS sürümlerini devre dışı kullanabilirsiniz.

      * Azure-SSIS IR'nizin her düğümüne Zulu OpenJDK'yı yüklemek için özel bir kurulum komut dosyası *(main.cmd*) ve PowerShell dosyası *(install_openjdk.ps1)* içeren bir *ZULU OPENJDK* klasörü. Bu kurulum, ORC ve Parke dosyalarını işlemek için Azure Veri Gölü Deposu ve Esnek Dosya bağlayıcılarını kullanmanıza olanak tanır. Daha fazla bilgi [için, Tümleştirme Hizmetleri için Azure Özellik Paketi'ne](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-ver15#dependency-on-java)bakın. 
      
        İlk olarak, [en son Zulu OpenJDK 'yı indirin](https://www.azul.com/downloads/zulu/zulu-windows/) (örneğin, *zulu8.33.0.1-jdk8.0.192-win_x64.zip),* ve *main.cmd* ve *install_openjdk.ps1* ile birlikte konteynerinize yükleyin.

        ![Kullanıcı senaryoları klasöründeki klasörler](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

   f. Bu özel kurulum örneklerini denemek için, içeriği seçili klasörden kapsayıcınıza kopyalayın.
   
      Veri Fabrikası Kullanıcı Arabirimi'ni kullanarak Azure-SSIS IR'nizi ayarladığınızda veya yeniden yapılandırdığınızda, **Gelişmiş Ayarlar** **bölümündeki ek sistem yapılandırmaları/bileşen yüklemeleri onay kutusunu kullanarak Azure-SSIS Tümleştirme Çalışma Sürenizi Özelleştir'i** seçin ve ardından **Özel kurulum kapsayıcısı SAS URI** kutusuna kabınızın SAS URI'sini girin.
   
      Azure-SSIS IR'nizi PowerShell ile ayarladığınızda veya yeniden `Set-AzDataFactoryV2IntegrationRuntime` yapılandırdığınızda, `SetupScriptContainerSasUri` parametre değeri olarak kabınızın SAS URI'si ile cmdlet çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure-SSIS Tümleştirme Çalışma Zamanı'nın Kurumsal Sürümünü Ayarlama](how-to-configure-azure-ssis-ir-enterprise-edition.md)
- [Azure-SSIS Tümleştirme Çalışma Süresi için ücretli veya lisanslı özel bileşenler geliştirme](how-to-develop-azure-ssis-ir-licensed-components.md)
