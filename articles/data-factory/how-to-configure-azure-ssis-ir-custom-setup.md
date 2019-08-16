---
title: Azure-SSIS tümleştirme çalışma zamanı için kurulumu özelleştirme | Microsoft Docs
description: Bu makalede, ek bileşenleri yüklemek veya ayarları değiştirmek için Azure-SSIS tümleştirme çalışma zamanı için özel kurulum arabirimi 'nin nasıl kullanılacağı açıklanır.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 1/25/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 4962070d69af98d0c7b10dc6f931612766529dce
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69515711"
---
# <a name="customize-setup-for-the-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı için kurulumu özelleştirme

Azure-SSIS Integration Runtime için özel kurulum arabirimi, Azure-SSIS IR 'nizin sağlanması veya yeniden yapılandırılması sırasında kendi kurulum adımlarınızı eklemek için bir arabirim sağlar. Özel Kurulum, varsayılan işletim yapılandırması veya ortamını değiştirmenize (örneğin, ek Windows Hizmetleri başlatmak veya dosya paylaşımları için erişim kimlik bilgilerini kalıcı hale getirmek) veya ek bileşenleri (örneğin, derlemeler, sürücüler veya Uzantılar) yüklemenizi sağlar Azure-SSIS IR 'nizin her bir düğümünde.

Özel kurulumunuzu, bir betiği ve ilişkili dosyalarını hazırlarken ve bunları Azure Storage hesabınızdaki bir blob kapsayıcısına yükleyerek yapılandırırsınız. Azure-SSIS IR 'nizi sağladığınızda veya yeniden yapılandırdığınızda, kapsayıcınıza yönelik bir paylaşılan erişim Imzası (SAS) Tekdüzen Kaynak tanımlayıcısı (URI) sağlarsınız. Azure-SSIS IR 'nizin her bir düğümü bundan sonra, komut dosyasını ve ilişkili dosyalarını kapsayıcınızdan indirir ve özel kurulumunuzu yükseltilmiş ayrıcalıklarla çalıştırır. Özel Kurulum tamamlandığında her düğüm, çalışma ve diğer günlüklerin standart çıkışını kapsayıcınıza yükler.

Hem ücretsiz hem de lisanssız bileşenleri, ücretli veya lisanslı bileşenleri yükleyebilirsiniz. ISV iseniz bkz. [Azure-SSIS IR için ücretli veya lisanslı bileşenleri geliştirme](how-to-develop-azure-ssis-ir-licensed-components.md).

> [!IMPORTANT]
> Azure-SSIS IR 'nin v2 serisi düğümleri özel kurulum için uygun değildir, bu nedenle lütfen bunun yerine v3 serisi düğümlerini kullanın.  V2 serisi düğümlerini zaten kullanıyorsanız, lütfen v3 serisi düğümleri en kısa sürede kullanmaya geçin.

## <a name="current-limitations"></a>Geçerli sınırlamalar

-   Derlemeleri genel derleme önbelleğinde ( `gacutil.exe` GAC) yüklemek için kullanmak istiyorsanız, özel kurulumlarınızın bir parçası olarak sağlamanız `gacutil.exe` veya genel önizleme kapsayıcısında sunulan kopyayı kullanmanız gerekir.

-   Betiğinizdeki `msiexec.exe` bir alt klasöre başvurmak istiyorsanız kök klasöre başvurmak için `.\` gösterimi desteklemez. `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` Yerine bir komut kullanın. `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`

-   Azure-SSIS IR 'yi özel kurulumla bir sanal ağa eklemeniz gerekiyorsa yalnızca sanal ağ Azure Resource Manager desteklenir. Klasik sanal ağ desteklenmiyor.

-   Yönetim paylaşma Şu anda Azure-SSIS IR üzerinde desteklenmiyor.

-   Azure-SSIS IR 'de IBM ıferies erişimi ODBC sürücüsü desteklenmez. Özel Kurulum sırasında yükleme hatası görebilirsiniz. Yardım için lütfen IBM desteği ile iletişime geçin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure-SSIS IR 'nizi özelleştirmek için aşağıdaki noktalara ihtiyacınız vardır:

-   [Azure aboneliği](https://azure.microsoft.com/)

-   [Bir Azure SQL veritabanı veya yönetilen örnek sunucusu](https://ms.portal.azure.com/#create/Microsoft.SQLServer)

-   [Azure-SSIS IR 'nizi sağlama](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

-   [Bir Azure depolama hesabı](https://azure.microsoft.com/services/storage/). Özel kurulum için özel kurulum komut dosyanızı ve ilişkili dosyalarını bir blob kapsayıcısına yükler ve depolar. Özel Kurulum işlemi aynı zamanda yürütme günlüklerini aynı blob kapsayıcısına yükler.

## <a name="instructions"></a>Yönergeler

1. [Azure PowerShell](/powershell/azure/install-az-ps)indirin ve yükleyin.

1. Özel kurulum komut dosyanızı ve ilişkili dosyalarını (örneğin,. bat,. cmd,. exe,. dll,. msi veya. ps1 dosyaları) hazırlayın.

   1.  Özel kurulumlarınızın giriş noktası olan adlı `main.cmd`bir betik dosyanız olmalıdır.

   1.  Diğer araçların (örneğin, `msiexec.exe`) kapsayıcısına yüklenmek üzere oluşturulmasını istiyorsanız, betiklerinizde günlük klasörü `CUSTOM_SETUP_SCRIPT_LOG_DIR` olarak (örneğin, `msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`) önceden tanımlanmış ortam değişkenini belirtin.

1. [Azure Depolama Gezgini](https://storageexplorer.com/)indirin, yükleyin ve başlatın.

   1. **(Yerel ve bağlı)** altında, **depolama hesapları** ' nı sağ seçin ve **Azure depolama 'ya Bağlan**' ı seçin.

      ![Azure depolamaya bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

   1. **Depolama hesabı adı ve anahtarı kullan** ' ı seçin ve **İleri ' yi**seçin.

      ![Depolama hesabı adını ve anahtarını kullanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

   1. Azure depolama hesabınızın adını ve anahtarınızı girip, **İleri**' yi seçin ve ardından **Bağlan**' ı seçin.

      ![Mağaza hesabı adını ve anahtarını belirtin](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

   1. Bağlı Azure depolama hesabınız altında **BLOB kapsayıcıları**' na sağ tıklayın, **BLOB kapsayıcısı oluştur**' u seçin ve yeni kapsayıcıyı adlandırın.

      ![Blob kapsayıcısı oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

   1. Yeni kapsayıcıyı seçin ve özel kurulum komut dosyanızı ve ilişkili dosyalarını karşıya yükleyin. Herhangi bir klasörde değil, `main.cmd` kapsayıcının en üst düzeyinde karşıya yüklediğinizden emin olun. Ayrıca, Kapsayıcınızın yalnızca gerekli özel kurulum dosyalarını içerdiğinden emin olun. bu nedenle, Azure-SSIS IR 'nize indirmek uzun sürmeyecektir. Özel kurulum için en uzun süre şu anda zaman aşımına uğramadan önce 45 dakika içinde ayarlanmıştır ve bu, kapsayıcıınızdan tüm dosyaları indirme ve Azure-SSIS IR 'ye yükleme süresini de içerir. Daha uzun bir süre gerekiyorsa lütfen bir destek bileti yükseltin.

      ![Blob kapsayıcısına dosya yükleme](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

   1. Kapsayıcıya sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin.

      ![Kapsayıcı için paylaşılan erişim Imzasını al](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

   1. Kapsayıcınız için SAS URI 'sini, yeterince uzun bir süre sonu ve okuma + yazma + liste izinleriyle oluşturun. Azure-SSIS IR 'nizin herhangi bir düğümü yeniden başlatıldığında özel kurulum komut dosyanızı ve ilişkili dosyalarını indirmek ve çalıştırmak için SAS URI 'sine ihtiyacınız vardır. Kurulum yürütme günlüklerini karşıya yüklemek için yazma izninizin olması gerekir.

      > [!IMPORTANT]
      > Lütfen SAS URI 'sinin süresi dolana ve bu süre boyunca Azure-SSIS IR 'nizi düzenli olarak durdurup başlattığınızda, Azure-SSIS IR 'nizin tüm yaşam döngüsünün her zaman kullanılabilir olduğundan emin olun.

      ![Kapsayıcı için paylaşılan erişim Imzasını oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

   1. Kapsayıcının SAS URI 'sini kopyalayın ve kaydedin.

      ![Paylaşılan erişim Imzasını Kopyala ve Kaydet](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

   1. Azure-SSIS IR 'nizi Data Factory Kullanıcı arabirimi ile sağladığınızda veya yeniden yapılandırdığınızda, Azure-SSIS IR 'nizi başlatmaya başlamadan önce, **Gelişmiş ayarlar** panelinde ilgili alana kapsayıcının SAS URI 'sini girin:

      ![Paylaşılan erişim Imzasını girin](media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

      Azure-SSIS IR 'nizi PowerShell ile sağladığınızda veya yeniden yapılandırdığınızda, Azure-SSIS IR 'nizi başlatmaya başlamadan önce, `Set-AzDataFactoryV2IntegrationRuntime` cmdlet 'ini yeni `SetupScriptContainerSasUri` parametre değeri olarak kapsayıcının SAS URI 'si ile çalıştırın. Örneğin:

      ```powershell
      Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                -Name $MyAzureSsisIrName `
                                                -ResourceGroupName $MyResourceGroupName `
                                                -SetupScriptContainerSasUri $MySetupScriptContainerSasUri

      Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                  -Name $MyAzureSsisIrName `
                                                  -ResourceGroupName $MyResourceGroupName
      ```

   1. Özel Kurulum tamamlandıktan ve Azure-SSIS IR 'niz başladıktan sonra, `main.cmd` ve diğer yürütme günlüklerinin `main.cmd.log` standart çıkışını depolama kapsayıcının klasöründe bulabilirsiniz.

1. Diğer özel kurulum örneklerini görmek için, Azure Depolama Gezgini ile genel önizleme kapsayıcısına bağlanın.

   a.  **(Yerel ve bağlı)** altında **depolama hesapları**' na sağ tıklayın, **Azure depolama 'ya Bağlan**' ı seçin, **bir bağlantı dizesi veya paylaşılan erişim imzası URI 'Si kullan**' ı seçin ve ardından **İleri**' yi seçin.

      ![Paylaşılan erişim Imzasıyla Azure depolama 'ya bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

   b.  **SAS URI 'Si kullan** ' ı seçin ve genel önizleme kapsayıcısı IÇIN AŞAĞıDAKI SAS URI 'sini girin. **İleri**' yi ve ardından **Bağlan**' ı seçin.

      `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

      ![Kapsayıcı için paylaşılan erişim Imzasını sağlama](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

   c. Bağlı genel önizleme kapsayıcısını seçin ve `CustomSetupScript` klasöre çift tıklayın. Bu klasörde aşağıdaki öğeler vardır:

      1. Azure `Sample` -SSIS IR 'nizin her bir düğümüne temel bir görev yüklemeye yönelik özel bir kurulum içeren bir klasör. Görev hiçbir şey yapmaz, ancak birkaç saniye uyku moduna geçmez. Klasör ayrıca bir `gacutil` klasör içerir. (`gacutil.exe`, `gacutil.exe.config`, ve `1033\gacutlrc.dll`), kapsayıcınızda olduğu gibi, içeriğin tamamı de kopyalanabilir. Ayrıca, `main.cmd` dosya paylaşımları için erişim kimlik bilgilerini kalıcı hale getirmek için açıklamalar içerir.

      1. Gerçek `UserScenarios` kullanıcı senaryoları için birkaç özel kurulum içeren bir klasör.

   ![Genel Önizleme kapsayıcısının içeriği](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

   d. `UserScenarios` Klasöre çift tıklayın. Bu klasörde aşağıdaki öğeler vardır:

      1. Azure `.NET FRAMEWORK 3.5` -SSIS IR 'nizin her bir düğümündeki özel bileşenler için gerekebilecek .NET Framework önceki bir sürümünü yüklemek için özel bir kurulum içeren bir klasör.

      1. Azure `BCP` -SSIS IR 'nizin her bir düğümüne toplu kopyalama programı (`bcp`) dahil SQL Server`MsSqlCmdLnUtils.msi`komut satırı yardımcı programları () yüklemek için özel bir kurulum içeren bir klasör.

      1. Azure `EXCEL` -SSIS IR 'nizin her bir düğümüne açık kaynak derlemeler (`DocumentFormat.OpenXml.dll`, `ExcelDataReader.DataSet.dll`, ve `ExcelDataReader.dll`) yüklemek için özel bir kurulum içeren bir klasör.

      1. Tek `ORACLE ENTERPRISE` bir kurulum betiği (`main.cmd`) ve sessiz yükleme yapılandırma dosyası (`client.rsp`) içeren bir klasör, Azure-SSIS IR Enterprise Edition 'ın her bir düğümüne Oracle bağlayıcıları ve OCI sürücüsünü yüklemek için. Bu kurulum, Oracle bağlantı yöneticisini, kaynağını ve hedefini kullanmanıza olanak sağlar. İlk olarak, Microsoft [indirme merkezi](https://www.microsoft.com/en-us/download/details.aspx?id=55179) 'nden ve en son`AttunitySSISOraAdaptersSetup.msi` Oracle `AttunitySSISOraAdaptersSetup64.msi`istemcisinden (ve) Oracle için Microsoft bağlayıcıları v 5.0 ' ı indirin. `winx64_12102_client.zip` Örneğin, [Oracle 'dan](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html) `main.cmd` ve`client.rsp` kapsayıcınıza ekleyin. Oracle 'a bağlanmak için TNS kullanırsanız, kurulum sırasında Oracle yükleme klasörüne kopyalanabilmesi `tnsnames.ora`için bu dosyayı indirmeniz, düzenlemeniz ve kapsayıcısına yüklemeniz gerekir.

      1. Azure `ORACLE STANDARD ADO.NET` -SSIS IR 'nizin her bir düğümüne Oracle`main.cmd`ODP.net sürücüsünü yüklemek için özel bir kurulum betiği () içeren bir klasör. Bu kurulum, ADO.NET bağlantı yöneticisini, kaynağını ve hedefini kullanmanıza olanak sağlar. İlk olarak, en son Oracle odp.net sürücüsünü indirin (örneğin, `ODP.NET_Managed_ODAC122cR1.zip` [Oracle](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)'dan) ve ardından kapsayıcınıza ile `main.cmd` birlikte karşıya yükleyin.
       
      1. Oracle `ORACLE STANDARD ODBC` ODBC sürücüsünü yüklemek ve Azure-SSIS IR`main.cmd`'nizin her bir düğümünde DSN 'yi yapılandırmak için özel bir kurulum betiği () içeren bir klasör. Bu kurulum, Oracle sunucusuna bağlanmak için ODBC bağlantı Yöneticisi/kaynak/hedef veya Power Query bağlantı Yöneticisi/kaynağını ODBC veri kaynağı türü ile birlikte kullanmanıza olanak sağlar. İlk olarak, en son Oracle Instant Client 'ı (temel paket veya temel Lite paketi) ve ODBC paketini (örneğin, 64 bit paketleri [buradan](https://www.oracle.com/technetwork/topics/winx64soft-089540.html) ) indirin (temel paket: `instantclient-basic-windows.x64-18.3.0.0.0dbru.zip`, temel Lite paketi: `instantclient-basiclite-windows.x64-18.3.0.0.0dbru.zip`, ODBC paketi `instantclient-odbc-windows.x64-18.3.0.0.0dbru.zip`:) veya [buradan](https://www.oracle.com/technetwork/topics/winsoft-085727.html) 32 bitlik paketler (temel `instantclient-basic-nt-18.3.0.0.0dbru.zip`paket:, temel Lite paketi: `instantclient-basiclite-nt-18.3.0.0.0dbru.zip`, ODBC paketi: `instantclient-odbc-nt-18.3.0.0.0dbru.zip`) ve ardından bunları kapsayıcınıza birlikte `main.cmd` karşıya yükleyin.

      1. Azure `SAP BW` -SSIS IR Enterprise Edition ' ın her`main.cmd`bir düğümüne sap .net bağlayıcı derlemesini (`librfc32.dll`) yüklemek için özel bir kurulum betiği () içeren bir klasör. Bu kurulum, SAP BW bağlantı yöneticisini, kaynağını ve hedefini kullanmanıza olanak sağlar. İlk olarak, SAP yükleme klasöründen 64 bit veya 32 bit sürümünü `librfc32.dll` ile `main.cmd`birlikte kapsayıcınıza yükleyin. Daha sonra betik, kurulum sırasında SAP derlemesini `%windir%\SysWow64` veya `%windir%\System32` klasörüne kopyalar.

      1. Azure `STORAGE` -SSIS IR 'nizin her bir düğümüne Azure PowerShell yüklemek için özel bir kurulum içeren bir klasör. Bu kurulum, [Azure depolama hesabınızı işlemek Için PowerShell betikleri](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell)çalıştıran SSIS paketlerini dağıtmanıza ve çalıştırmanıza olanak tanır. Bir `main.cmd`örnek `AzurePowerShell.msi` ( veya`storage.ps1` en son sürümü), kapsayıcınıza kopyalayın. Paketleriniz için bir şablon olarak PowerShell. dtsx kullanın. Paket şablonu, değiştirilebilen bir PowerShell betiği olarak indirilen `storage.ps1` bir [Azure Blob indirme görevini](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task)ve komut dosyasını her düğümde yürüten bir [işlem Çalıştır görevi](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) birleştirir.

      1. Özel `TERADATA` bir kurulum betiği (`main.cmd`), onunla ilişkili dosya (`install.cmd`) ve yükleyici paketleri (`.msi`) içeren bir klasör. Bu dosyalar, Azure-SSIS IR Enterprise Edition ' ın her bir düğümüne Teradata bağlayıcıları, TPT API ve ODBC sürücüsü yükler. Bu kurulum, Teradata bağlantı yöneticisini, kaynağını ve hedefini kullanmanıza olanak sağlar. Öncelikle Teradata araçları ve yardımcı programlar (TTU) 15 `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`. x ZIP dosyasını (örneğin,) [Teradata](http://partnerintelligence.teradata.com)'dan indirip Yukarıdaki `.cmd` ve `.msi` dosyalarla birlikte kapsayıcınıza yükleyin.

   ![Kullanıcı senaryoları klasöründeki klasörler](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

   e. Bu özel kurulum örneklerini denemek için seçili klasördeki içeriği kopyalayıp kapsayıcınıza yapıştırın. Azure-SSIS IR 'nizi PowerShell ile sağladığınızda veya yeniden yapılandırdığınızda, `Set-AzDataFactoryV2IntegrationRuntime` cmdlet 'ini yeni `SetupScriptContainerSasUri` parametre değeri olarak kapsayıcının SAS URI 'siyle çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure-SSIS Integration Runtime Enterprise Edition](how-to-configure-azure-ssis-ir-enterprise-edition.md)

-   [Azure-SSIS tümleştirme çalışma zamanı için ücretli veya lisanslı özel bileşenler geliştirme](how-to-develop-azure-ssis-ir-licensed-components.md)
