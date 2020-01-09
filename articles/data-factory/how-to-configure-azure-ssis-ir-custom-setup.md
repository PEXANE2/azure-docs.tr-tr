---
title: Azure-SSIS tümleştirme çalışma zamanı için kurulumu özelleştirme
description: Bu makalede, ek bileşenleri yüklemek veya ayarları değiştirmek için Azure-SSIS tümleştirme çalışma zamanı için özel kurulum arabirimi 'nin nasıl kullanılacağı açıklanır.
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
ms.date: 12/23/2019
ms.openlocfilehash: ccf7ba2fd27dabdb090be87c5438ad68471996da
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/26/2019
ms.locfileid: "75497065"
---
# <a name="customize-setup-for-the-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı için kurulumu özelleştirme

Azure-SSIS Integration Runtime için özel kurulum arabirimi, Azure-SSIS IR sağlama veya yeniden yapılandırma sırasında kendi kurulum adımlarınızı eklemek için bir arabirim sağlar. 

Özel Kurulum, varsayılan işletim yapılandırması veya ortamını değiştirmenize (örneğin, ek Windows Hizmetleri başlatmak veya dosya paylaşımları için erişim kimlik bilgilerini kalıcı hale getirmek) veya ek bileşenleri (örneğin, derlemeler, sürücüler veya Uzantılar) yüklemenizi sağlar Azure-SSIS IR.

Azure-SSIS IR özel kurulum yapmak için iki yol vardır: komut dosyaları olmadan hızlı özel ayarlar ve betiklerle standart özel ayarlar.

Hızlı özel kurulumlarla, bazı ortak sistem yapılandırma/Windows komutlarını çalıştırabilir veya herhangi bir betik kullanmadan popüler/önerilen ek bileşenleri yükleyebilirsiniz.  

Standart özel kurulumlarla, bir betiği ve ilişkili dosyalarını hazırlamanız ve bunları Azure Storage hesabınızdaki bir blob kapsayıcısında birlikte karşıya yüklemeniz gerekir. Daha sonra, Azure-SSIS IR sağladığınızda veya yeniden yapılandırdığınızda, kapsayıcınıza yönelik bir paylaşılan erişim Imzası (SAS) Tekdüzen Kaynak tanımlayıcısı (URI) sağlarsınız. Azure-SSIS IR her bir düğüm, komut dosyasını ve ilişkili dosyalarını kapsayıcınızdan indirir ve özel kurulumunuzu yükseltilmiş ayrıcalıklarla çalıştırır. Özel Kurulum tamamlandığında her düğüm, çalışma ve diğer günlüklerin standart çıkışını kapsayıcınıza karşıya yükler.

Hem ücretsiz/lisanssız hem de ücretli/lisanslı bileşenleri Express/standart özel kurulumlarla birlikte yükleyebilirsiniz. ISV iseniz, [Azure-SSIS IR için ücretli veya lisanslı bileşenlerin nasıl geliştirileceği](how-to-develop-azure-ssis-ir-licensed-components.md)hakkındaki belgelerimize bakın.

> [!IMPORTANT]
> Azure-SSIS IR v2 serisi düğümleri özel kurulum için uygun değildir, bu nedenle lütfen bunun yerine v3 serisi düğümlerini kullanın.  V2 serisi düğümlerini zaten kullanıyorsanız, lütfen v3 serisi düğümleri en kısa sürede kullanmaya geçin.

## <a name="current-limitations"></a>Geçerli sınırlamalar

Aşağıdaki sınırlamalar yalnızca standart özel kurulumlarda geçerlidir:

- Genel derleme önbelleği 'ne (GAC) derlemeleri yüklemek için komut dosyasında `gacutil.exe` kullanmak istiyorsanız, özel kurulumlarınızın bir parçası olarak `gacutil.exe` sağlamanız veya aşağıdaki genel önizleme kapsayıcımızda belirtilen kopyayı kullanmanız gerekir.

- Betiğinizdeki bir alt klasöre başvurmak istiyorsanız, `msiexec.exe` kök klasöre başvurmak için `.\` gösterimini desteklemez. `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`yerine `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` gibi bir komut kullanın.

- Yönetimsel paylaşımlar, yani Windows tarafından otomatik olarak oluşturulan gizli ağ paylaşımları Azure-SSIS IR Şu anda desteklenmiyor.

- IBM ıferies erişimi ODBC sürücüsü Azure-SSIS IR desteklenmez. Özel kurulumunuz sırasında yükleme hataları görebilirsiniz. Yardım için lütfen IBM desteği ile iletişime geçin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure-SSIS IR özelleştirmek için aşağıdaki işlemleri yapmanız gerekir:

- [Azure aboneliği](https://azure.microsoft.com/)

- [Azure-SSIS IR sağlama](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

- [Bir Azure depolama hesabı](https://azure.microsoft.com/services/storage/). Express özel kurulumları için gerekli değildir. Standart özel kurulumlarda, özel kurulum komut dosyanızı ve ilişkili dosyalarını bir blob kapsayıcısına yükler ve depolar. Özel Kurulum işlemi aynı zamanda yürütme günlüklerini aynı blob kapsayıcısına yükler.

## <a name="instructions"></a>Yönergeler

1. Azure-SSIS IR PowerShell ile sağlamak veya yeniden yapılandırmak istiyorsanız [Azure PowerShell](/powershell/azure/install-az-ps)indirin ve yükleyin. Express özel kurulumları için 4. adıma atlayın.

1. Özel kurulum komut dosyanızı ve ilişkili dosyalarını (örneğin,. bat,. cmd,. exe,. dll,. msi veya. ps1 dosyaları) hazırlayın.

   1. Özel kurulumlarınızın giriş noktası olan `main.cmd`adlı bir betik dosyanız olmalıdır.

   1. Betiğin sessizce yürütüldüğünden emin olmanız gerekir, önce betiği yerel makinede test etmeniz önerilir.

   1. Diğer araçlarla (örneğin, `msiexec.exe`), kapsayıcınıza karşıya yüklenecek ek günlüklerin oluşturulmasını istiyorsanız, betiklerinizde günlük klasörü olarak `CUSTOM_SETUP_SCRIPT_LOG_DIR` önceden tanımlanmış ortam değişkenini belirtin (örneğin, `msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`).

1. [Azure Depolama Gezgini](https://storageexplorer.com/)indirin, yükleyin ve başlatın.

   1. **(Yerel ve bağlı)** altında, **depolama hesapları** ' nı sağ seçin ve **Azure depolama 'ya Bağlan**' ı seçin.

      ![Azure depolamaya bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

   1. **Depolama hesabı adı ve anahtarı kullan** ' ı seçin ve **İleri ' yi**seçin.

      ![Depolama hesabı adını ve anahtarını kullanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

   1. Azure depolama hesabınızın adını ve anahtarınızı girip, **İleri**' yi seçin ve ardından **Bağlan**' ı seçin.

      ![Mağaza hesabı adını ve anahtarını belirtin](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

   1. Bağlı Azure depolama hesabınız altında **BLOB kapsayıcıları**' na sağ tıklayın, **BLOB kapsayıcısı oluştur**' u seçin ve yeni kapsayıcıyı adlandırın.

      ![Blob kapsayıcısı oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

   1. Yeni kapsayıcıyı seçin ve özel kurulum komut dosyanızı ve ilişkili dosyalarını karşıya yükleyin. `main.cmd` herhangi bir klasörde değil, kapsayıcının en üst düzeyinde karşıya yüklediğinizden emin olun. Ayrıca, kapsayıcınıza yalnızca gerekli özel kurulum dosyalarını içerdiğinden emin olun, böylece daha sonra Azure-SSIS IR indirmek uzun sürmeyecektir. Özel kurulum için en uzun süre, şu anda zaman aşımına uğramadan önce 45 dakika içinde ayarlanmıştır ve bu, kapsayıcıınızdan tüm dosyaları indirmek ve Azure-SSIS IR yüklemek için gereken süreyi de içerir. Daha uzun bir süre gerekiyorsa lütfen bir destek bileti yükseltin.

      ![Blob kapsayıcısına dosya yükleme](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

   1. Kapsayıcıya sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin.

      ![Kapsayıcı için paylaşılan erişim Imzasını al](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

   1. Kapsayıcınız için SAS URI 'sini, yeterince uzun bir süre sonu ve okuma + yazma + liste izinleriyle oluşturun. Azure-SSIS IR herhangi bir düğüm yeniden başlatıldığında özel kurulum komut dosyanızı ve ilişkili dosyalarını indirip çalıştırmak için SAS URI 'sine ihtiyacınız vardır. Kurulum yürütme günlüklerini karşıya yüklemek için yazma izninizin olması gerekir.

      > [!IMPORTANT]
      > Lütfen SAS URI 'sinin süresi dolana ve özel kurulum kaynaklarının Azure-SSIS IR, özellikle de bu süre boyunca düzenli olarak durdurup Azure-SSIS IR başlatırsanız, oluşturma işleminin silinmesine göre her zaman kullanılabilir olduğundan emin olun.

      ![Kapsayıcı için paylaşılan erişim Imzasını oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

   1. Kapsayıcının SAS URI 'sini kopyalayın ve kaydedin.

      ![Paylaşılan erişim Imzasını Kopyala ve Kaydet](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

1. Azure-SSIS IR Data Factory Kullanıcı arabirimi ile sağladığınızda veya yeniden yapılandırdığınızda, tümleştirme çalışma zamanı kurulum bölmesinin **Gelişmiş ayarlar** bölümünde bulunan **Azure-SSIS Integration Runtime ek sistem yapılandırmalarına/bileşen yüklemelerine göre Özelleştir** onay kutusunu seçerek özel kurulumları ekleyebilir/kaldırabilirsiniz. 

   Standart özel kurulumları eklemek istiyorsanız, **özel kurulum KAPSAYıCıSı SAS URI 'si** ALANıNA KAPSAYıCıNıN SAS URI 'sini girin. 
   
   Express özel kurulumları eklemek istiyorsanız, **Yeni** ' yi seçerek **hızlı** özel kurulum Ekle panelini açın ve ardından **hızlı özel kurulum türü** açılan menüsü altında herhangi bir tür seçin:

   1. **Cmdkey komut türünü Çalıştır** ' ı seçerseniz, hedeflenen bilgisayar/etki alanı adınızı, hesap adını/Kullanıcı adını ve hesap anahtarınızı/parolasını sırasıyla **/Add**, **/User**ve **/Pass** alanlarına girerek Azure-SSIS IR dosya paylaşımlarınız/Azure dosyalarınız için erişim kimlik bilgilerini kalıcı hale getirebilirsiniz. Bu, yerel makinenizde Windows [cmdkey](https://docs.microsoft.com/windows-server/administration/windows-commands/cmdkey) komutunu çalıştırmaya benzer.
   
   1. **Ortam değişken türü Ekle** ' yi seçerseniz, sırasıyla Azure-SSIS IR çalışan paketleriniz Içinde, **değişken adı** ve **değişken değer** alanlarına, sırasıyla, ortam değişkeni adı ve değeri girerek kullanılacak Windows ortam değişkenleri ekleyebilirsiniz. Bu, yerel makinenizde Windows [set](https://docs.microsoft.com/windows-server/administration/windows-commands/set_1) komutu çalıştırmaya benzer.

   1. **Lisanslı bileşen türünü yüklemeyi** seçerseniz, **bileşen adı** açılan menüsünde ISV iş ortaklarımızın tümleşik bileşenlerini seçebilirsiniz:

      1. **Sentryone 'ın 'ın görev fabrikası** bileşenini seçerseniz, **Lisans anahtarı** alanında onlardan satın aldığınız ürün lisans anahtarını girerek, Sentryone 'ın ' den gelen [görev fabrikası](https://www.sentryone.com/products/task-factory/high-performance-ssis-components) paketini Azure-SSIS IR yükleyebilirsiniz. Geçerli tümleşik sürüm **2019.4.3**.

      1. **Oh22's HEDDA ' yi seçerseniz. IO** bileşeni, Hedda 'yi yükleyebilirsiniz [. ](https://hedda.io/ssis-component/)Hizmet satın aldıktan sonra Azure-SSIS IR, GÇ veri kalitesi/oh22 'tan bileşeni Temizleme. Geçerli tümleşik sürüm **1.0.13**.

      1. **Oh22's SQLPhonetics.net** bileşenini seçerseniz, **Lisans anahtarı** alanında onlardan satın aldığınız ürün lisans anahtarını girerek Azure-SSIS IR [SQLPhonetics.net](https://sqlphonetics.oh22.is/sqlphonetics-net-for-microsoft-ssis/) veri kalitesini/eşleşen bileşeni yükleyebilirsiniz. Geçerli tümleşik sürüm **1.0.43**.
   
   Eklenmiş hızlı özel kurulumlarınız, **Gelişmiş ayarlar** bölümünde görüntülenir. Bunları kaldırmak için onay kutularını seçip **Sil**' i seçebilirsiniz.

   ![Özel kurulumlarla Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-custom.png)

   PowerShell ile Azure-SSIS IR sağladığınızda veya yeniden yapılandırdığınızda, Azure-SSIS IR başlamadan önce `Set-AzDataFactoryV2IntegrationRuntime` cmdlet 'ini çalıştırarak özel kurulumları ekleyebilir/kaldırabilirsiniz.
   
   Standart özel kurulumlarda, kapsayıcının SAS URI 'sini `SetupScriptContainerSasUri` parametresi değeri olarak sağlayabilirsiniz. Örneğin:

   ```powershell
   Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                         -Name $MyAzureSsisIrName `
                                         -ResourceGroupName $MyResourceGroupName `
                                         -SetupScriptContainerSasUri $MySetupScriptContainerSasUri

   Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                           -Name $MyAzureSsisIrName `
                                           -ResourceGroupName $MyResourceGroupName
   ```
   
   Standart özel kurulum tamamlandıktan ve Azure-SSIS IR başladıktan sonra, `main.cmd` ve diğer yürütme günlüklerinin standart çıkışını depolama kapsayıcının `main.cmd.log` klasöründe bulabilirsiniz.

1. Standart özel kurulumların bazı örneklerini görmek için, Azure Depolama Gezgini ile genel önizleme kapsayımuza bağlanın.

   1. **(Yerel ve bağlı)** altında **depolama hesapları**' na sağ tıklayın, **Azure depolama 'ya Bağlan**' ı seçin, **bir bağlantı dizesi veya paylaşılan erişim imzası URI 'Si kullan**' ı seçin ve ardından **İleri**' yi seçin.

      ![Paylaşılan erişim Imzasıyla Azure depolama 'ya bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

   1. **SAS URI 'Si kullan** ' ı seçin ve genel önizleme kapsayıcısı IÇIN AŞAĞıDAKI SAS URI 'sini girin. **İleri**' yi seçin ve ardından **Bağlan**' ı seçin.

      `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

      ![Kapsayıcı için paylaşılan erişim Imzasını sağlama](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

   1. Bağlı genel önizleme kapsayıcısını seçin ve `CustomSetupScript` klasöre çift tıklayın. Bu klasörde aşağıdaki öğeler vardır:

      1. Azure-SSIS IR her bir düğümüne temel bir görev yüklemeye yönelik özel bir kurulum içeren `Sample` klasörü. Görev hiçbir şey yapmaz, ancak birkaç saniye uyku moduna geçmez. Klasör ayrıca bir `gacutil` klasörü içerir, (`gacutil.exe`, `gacutil.exe.config`ve `1033\gacutlrc.dll`) tüm içeriği kapsayıcınızda olduğu gibi kopyalanabilir.

      1. Gerçek Kullanıcı senaryolarından birkaç özel kurulum örneği içeren `UserScenarios` klasörü.

      ![Genel Önizleme kapsayıcısının içeriği](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

   1. Aşağıdaki öğeleri bulmak için `UserScenarios` klasöre çift tıklayın:

      1. Azure-SSIS IR her bir düğümünde özel bileşenler için gerekebilecek .NET Framework daha önceki bir sürümünü yüklemek üzere özel bir kurulum içeren `.NET FRAMEWORK 3.5` klasörü.

      1. Azure-SSIS IR her bir düğümüne toplu kopyalama programı (`bcp`) dahil olmak üzere SQL Server komut satırı yardımcı programları (`MsSqlCmdLnUtils.msi`) yüklemek için özel bir kurulum içeren `BCP` klasörü.

      1. Azure-SSIS IR her bir düğümündeki Excel dosyalarını dinamik olarak okumak/yazmak için betik görevlerinde kullanabileceğiniz C# derlemeleri/kitaplıkları yüklemek için özel bir kurulum betiği (`main.cmd`) içeren bir `EXCEL` klasörü. İlk olarak [, buradan `ExcelDataReader.dll` ve buradan](https://www.nuget.org/packages/ExcelDataReader/) `DocumentFormat.OpenXml.dll` indirin [](https://www.nuget.org/packages/DocumentFormat.OpenXml/)ve ardından bunları kapsayıcınıza `main.cmd` birlikte karşıya yükleyin. Alternatif olarak, yalnızca standart Excel bağlantı yöneticisini/kaynağını/hedefini kullanmak istiyorsanız, gerekli erişim yeniden dağıtılabilir Azure-SSIS IR önceden yüklenmiş olduğundan, özel bir kuruluma gerek kalmaz.
      
      1. Azure-SSIS IR her bir düğümüne MySQL ODBC sürücülerini yüklemek için özel bir kurulum betiği (`main.cmd`) içeren `MYSQL ODBC` klasörü. Bu kurulum, MySQL sunucusuna bağlanmak için ODBC bağlantı yöneticisini/kaynağını/hedefini kullanmanıza olanak sağlar. İlk olarak, MySQL ODBC sürücü yükleyicilerinin en son 64-bit ve 32 bit sürümlerini indirin. Örneğin, `mysql-connector-odbc-8.0.13-winx64.msi` ve `mysql-connector-odbc-8.0.13-win32.msi`- [MySQL](https://dev.mysql.com/downloads/connector/odbc/)'ten `main.cmd` ve ardından bunları kapsayıcınıza birlikte karşıya yükleyin.

      1. Tek bir kurulum betiği (`main.cmd`) ve sessiz yükleme yapılandırma dosyası (`client.rsp`) içeren ve Azure-SSIS IR Enterprise Edition 'ın her bir düğümüne Oracle bağlayıcıları ve OCı sürücüsü yükleyen bir `ORACLE ENTERPRISE` klasörü. Bu kurulum, Oracle sunucusuna bağlanmak için Oracle bağlantı yöneticisini/kaynağını/hedefini kullanmanıza olanak sağlar. İlk olarak, [Microsoft Indirme merkezi](https://www.microsoft.com/en-us/download/details.aspx?id=55179) 'nden (`AttunitySSISOraAdaptersSetup.msi` ve `AttunitySSISOraAdaptersSetup64.msi`) Microsoft bağlayıcıları v 5.0 'ı ve en son Oracle istemcisini (örneğin `winx64_12102_client.zip`, [Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html)'dan) indirin ve ardından bunları kapsayıcınıza `main.cmd` ve `client.rsp` ile birlikte karşıya yükleyin. Oracle 'a bağlanmak için TNS kullanırsanız, Ayrıca, kurulum sırasında Oracle yükleme klasörüne kopyalanabilmesi için `tnsnames.ora`indirmeniz, düzenlemeniz ve kapsayıcıya yüklemeniz gerekir.

      1. Azure-SSIS IR her bir düğümüne Oracle ODP.NET sürücüsünü yüklemek için özel bir kurulum betiği (`main.cmd`) içeren `ORACLE STANDARD ADO.NET` klasörü. Bu kurulum, Oracle sunucusuna bağlanmak için ADO.NET bağlantı yöneticisini/kaynağını/hedefini kullanmanıza olanak sağlar. İlk olarak, en son Oracle ODP.NET sürücüsünü indirin (örneğin, [Oracle](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)'dan `ODP.NET_Managed_ODAC122cR1.zip`) ve ardından `main.cmd` ile birlikte kapsayıcınıza yükleyin.
       
      1. Oracle ODBC sürücüsünü yüklemek ve Azure-SSIS IR her bir düğümde DSN 'yi yapılandırmak için özel bir kurulum betiği (`main.cmd`) içeren `ORACLE STANDARD ODBC` klasörü. Bu kurulum, Oracle sunucusuna bağlanmak için ODBC bağlantı Yöneticisi/kaynak/hedef veya Power Query bağlantı Yöneticisi/kaynağını ODBC veri kaynağı türü ile birlikte kullanmanıza olanak sağlar. İlk olarak, en son Oracle anında Istemcisini (temel paket veya temel Lite paketi) ve ODBC paketini (örneğin,) indirin [. buradan 64 bitlik paketler (temel](https://www.oracle.com/technetwork/topics/winx64soft-089540.html) paket: `instantclient-basic-windows.x64-18.3.0.0.0dbru.zip`, temel Lite paketi: `instantclient-basiclite-windows.x64-18.3.0.0.0dbru.zip`, ODBC paketi: `instantclient-odbc-windows.x64-18.3.0.0.0dbru.zip`) veya 32 bit paketler [buradan](https://www.oracle.com/technetwork/topics/winsoft-085727.html) (temel paket: `instantclient-basic-nt-18.3.0.0.0dbru.zip`, temel Lite PAKETI: `instantclient-basiclite-nt-18.3.0.0.0dbru.zip`, ODBC paketi: `instantclient-odbc-nt-18.3.0.0.0dbru.zip`), bunları kapsayıcınıza `main.cmd` ile birlikte karşıya yükleyin.

      1. Azure-SSIS IR her bir düğümüne Oracle OLEDB sürücüsünü yüklemek için özel bir kurulum betiği (`main.cmd`) içeren bir `ORACLE STANDARD OLEDB` klasörü. Bu kurulum, Oracle sunucusuna bağlanmak için OLEDB bağlantı yöneticisini/kaynağını/hedefini kullanmanıza olanak sağlar. İlk olarak, en son Oracle OLEDB sürücüsünü indirin (örneğin, [Oracle](https://www.oracle.com/partners/campaign/index-090165.html)'dan `ODAC122010Xcopy_x64.zip`) ve ardından `main.cmd` ile birlikte kapsayıcınıza yükleyin.

      1. Azure-SSIS IR her bir düğümüne PostgreSQL ODBC sürücülerini yüklemek için özel bir kurulum betiği (`main.cmd`) içeren `POSTGRESQL ODBC` klasörü. Bu kurulum, PostgreSQL sunucusuna bağlanmak için ODBC bağlantı yöneticisini/kaynağını/hedefini kullanmanıza olanak sağlar. İlk olarak, PostgreSQL ODBC sürücü yükleyicilerinin en son 64-bit ve 32 bit sürümlerini (örneğin, `psqlodbc_x64.msi` ve `psqlodbc_x86.msi`- [PostgreSQL](https://www.postgresql.org/ftp/odbc/versions/msi/)' den yükleyin ve ardından bunları kapsayıcınıza `main.cmd` birlikte karşıya yükleyin.

      1. Azure-SSIS IR Enterprise Edition 'ın her bir düğümüne SAP .NET bağlayıcı derlemesini (`librfc32.dll`) yüklemek için özel bir kurulum betiği (`main.cmd`) içeren `SAP BW` klasörü. Bu kurulum, SAP BW sunucusuna bağlanmak için SAP BW bağlantı yöneticisini/kaynağını/hedefini kullanmanıza olanak sağlar. İlk olarak, `librfc32.dll` 64 bit veya 32 bit sürümünü SAP yükleme klasöründen `main.cmd` birlikte kapsayıcınıza yükleyin. Betik daha sonra, kurulum sırasında SAP derlemesini `%windir%\SysWow64` veya `%windir%\System32` klasörüne kopyalar.

      1. Azure-SSIS IR her bir düğümüne Azure PowerShell yüklemek için özel bir kurulum içeren `STORAGE` klasörü. Bu kurulum, [Azure depolama hesabınızı işlemek Için PowerShell betikleri](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell)çalıştıran SSIS paketlerini dağıtmanıza ve çalıştırmanıza olanak tanır. `main.cmd`kopyalayın, örnek bir `AzurePowerShell.msi` (veya en son sürümü kullanın) ve `storage.ps1` kapsayıcınıza. Paketleriniz için bir şablon olarak PowerShell. dtsx kullanın. Paket şablonu, `storage.ps1` değiştirilebilir bir PowerShell betiği olarak indiren bir [Azure Blob Indirme görevini](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task)ve komut dosyasını her düğümde yürüten bir [işlem Çalıştır görevi](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) birleştirir.

      1. Özel bir kurulum betiği (`main.cmd`), onunla ilişkili dosya (`install.cmd`) ve yükleyici paketleri (`.msi`) içeren `TERADATA` klasörü. Bu dosyalar, Azure-SSIS IR Enterprise Edition 'ın her bir düğümüne Teradata bağlayıcıları, TPT API ve ODBC sürücüsü yükler. Bu kurulum, Teradata sunucusuna bağlanmak için Teradata bağlantı yöneticisini/kaynağını/hedefini kullanmanıza olanak sağlar. İlk olarak, Teradata ' den Teradata araçları ve yardımcı programlar 15. x ZIP dosyasını (örneğin, `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`) [indirip, yukarıdaki](http://partnerintelligence.teradata.com)`.cmd` ve dosyaları kapsayıcınıza `.msi` birlikte karşıya yükleyin.

      1. Azure-SSIS IR her bir düğümüne Zulu OpenJDK 'yi yüklemek için özel bir kurulum betiği (`main.cmd`) ve PowerShell dosyası (`install_openjdk.ps1`) içeren bir `ZULU OPENJDK` klasörü. Bu kurulum, ORC/Parquet dosyalarını işlemek için Azure Data Lake Store/esnek dosya bağlayıcıları kullanmanıza olanak sağlar. daha fazla bilgi için [buraya](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-ver15#dependency-on-java) bakın. İlk olarak, en son Zulu OpenJDK 'yi indirin-Örneğin, [buradan](https://www.azul.com/downloads/zulu/zulu-windows/)`zulu8.33.0.1-jdk8.0.192-win_x64.zip` ve `main.cmd` ve `install_openjdk.ps1` birlikte kapsayıcınıza yükleyin.

      ![Kullanıcı senaryoları klasöründeki klasörler](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

   1. Bu özel kurulum örneklerini denemek için, Seçili klasörden içeriği kopyalayıp kapsayıcınıza yapıştırın.
   
      Data Factory Kullanıcı arabirimi ile Azure-SSIS IR sağladığınızda veya yeniden yapılandırdığınızda, **Gelişmiş ayarlar** bölümünde **Azure-SSIS Integration Runtime ek sistem yapılandırmalarına/bileşen yüklemelerine sahip olan özelleştirin** onay kutusunu seçin ve **özel kurulum kapsayıcısı SAS URI 'si** alanına kapsayıcının SAS URI 'sini girin.
   
      PowerShell ile Azure-SSIS IR sağladığınızda veya yeniden yapılandırdığınızda, `Set-AzDataFactoryV2IntegrationRuntime` cmdlet 'ini `SetupScriptContainerSasUri` parametresinin değeri olarak kapsayıcının SAS URI 'siyle çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure-SSIS Integration Runtime Enterprise sürümü](how-to-configure-azure-ssis-ir-enterprise-edition.md)

-   [Azure-SSIS tümleştirme çalışma zamanı için ücretli veya lisanslı özel bileşenler geliştirme](how-to-develop-azure-ssis-ir-licensed-components.md)
