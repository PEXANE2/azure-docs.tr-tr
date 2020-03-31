---
title: PHP için Azure web ve çalışan rolleri oluşturma
description: Bir Azure bulut hizmetinde PHP web ve çalışan rolleri oluşturma ve PHP çalışma süresini yapılandırma kılavuzu.
services: ''
documentationcenter: php
author: msangapu
manager: cfowler
ms.assetid: 9f7ccda0-bd96-4f7b-a7af-fb279a9e975b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/11/2018
ms.author: msangapu
ms.openlocfilehash: 54410e1e70a2ec0d3a9e2f853dc9556cd05996ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79297263"
---
# <a name="create-php-web-and-worker-roles"></a>PHP web ve çalışan rolleri oluşturma

## <a name="overview"></a>Genel Bakış

Bu kılavuz, Windows geliştirme ortamında PHP web'i veya çalışan rollerini nasıl oluşturacağınızı, kullanılabilir "yerleşik" sürümlerden PHP'nin belirli bir sürümünü nasıl seçeceğinizi, PHP yapılandırmasını nasıl değiştireceğinizi, uzantıları etkinleştireceğinizi ve son olarak Azure'a nasıl dağıtacağınızı gösterir. Ayrıca, sağladığınız php çalışma zamanı (özel yapılandırma ve uzantılarla) kullanmak için bir web veya alt rolü nasıl yapılandırılabildiğini de açıklar.

Azure, uygulamaları çalıştırmak için üç bilgi işlem modeli sağlar: Azure Uygulama Hizmeti, Azure Sanal Makineler ve Azure Bulut Hizmetleri. Her üç model de PHP'yi destekler. Web ve çalışan rollerini içeren Bulut Hizmetleri, *hizmet olarak platform (PaaS)* sağlar. Bir bulut hizmeti içinde, web rolü ön uç web uygulamalarını barındırmak için özel bir Internet Bilgi Hizmetleri (IIS) web sunucusu sağlar. Bir alt rol, kullanıcı etkileşiminden veya girişinden bağımsız olarak eşzamanlı, uzun süreli veya sürekli görevler çalıştırabilir.

Bu seçenekler hakkında daha fazla bilgi için [Azure tarafından sağlanan İşlem barındırma seçeneklerine](cloud-services/cloud-services-choose-me.md)bakın.

## <a name="download-the-azure-sdk-for-php"></a>PHP için Azure SDK'yı indirin

[PHP için Azure SDK](https://github.com/Azure/azure-sdk-for-php) birkaç bileşenden oluşur. Bu makalede bunlardan ikisi kullanılacaktır: Azure PowerShell ve Azure emülatörleri. Bu iki bileşen Microsoft Web Platform Yükleyicisi aracılığıyla yüklenebilir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

## <a name="create-a-cloud-services-project"></a>Bulut Hizmetleri projesi oluşturma

PHP web veya çalışan rolü oluşturmanın ilk adımı bir Azure Hizmeti projesi oluşturmaktır. Azure Hizmeti projesi web ve çalışan rolleri için mantıksal bir kapsayıcı görevi sunar ve projenin [hizmet tanımı (.csdef)] ve [hizmet yapılandırma (.cscfg)] dosyalarını içerir.

Yeni bir Azure Hizmeti projesi oluşturmak için Azure PowerShell'i yönetici olarak çalıştırın ve aşağıdaki komutu uygulayın:

    PS C:\>New-AzureServiceProject myProject

Bu komut, web ve`myProject`çalışan rolleri ekleyebileceğiniz yeni bir dizin ( ) oluşturur.

## <a name="add-php-web-or-worker-roles"></a>PHP web veya çalışan rolleri ekleme

Projeye PHP web rolü eklemek için, projenin kök dizininden aşağıdaki komutu çalıştırın:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Bir alt rol için şu komutu kullanın:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> Parametre `roleName` isteğe bağlıdır. Atlanırsa, rol adı otomatik olarak oluşturulur. Oluşturulan ilk web rolü `WebRole1`olacak , `WebRole2`ikinci olacak , ve benzeri. Oluşturulan ilk işçi rolü `WorkerRole1`olacaktır , `WorkerRole2`ikinci olacak , ve benzeri.
>
>

## <a name="use-your-own-php-runtime"></a>Kendi PHP çalışma sürenizi kullanın

Bazı durumlarda, yerleşik bir PHP çalışma zamanı seçmek ve yukarıda açıklandığı gibi yapılandırmak yerine, kendi PHP çalışma zamanınızı sağlamak isteyebilirsiniz. Örneğin, geliştirme ortamınızda kullandığınız bir web veya çalışan rolünde aynı PHP çalışma süresini kullanabilirsiniz. Bu, uygulamanın üretim ortamınızdaki davranışı değiştirmemesini sağlamayı kolaylaştırır.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Kendi PHP çalışma sürenizi kullanacak bir web rolünü yapılandırma

Bir web rolünü sağladığınız PHP çalışma zamanını kullanacak şekilde yapılandırmak için aşağıdaki adımları izleyin:

1. Bir Azure Hizmeti projesi oluşturun ve bu konuda daha önce açıklandığı gibi bir PHP web rolü ekleyin.
2. Web `php` rolünüzün `bin` kök dizininde bulunan klasörde bir klasör oluşturun ve ardından PHP çalışma sürenizi (tüm ikili dosyalar, yapılandırma `php` dosyaları, alt klasörler, vb.) klasöre ekleyin.
3. (İsteğE bağlı) PHP çalışma süreniz [SQL Server için PHP için Microsoft Sürücüleri][sqlsrv drivers]kullanıyorsa, web rolünüzü SQL Server Native Client [2012'yi][sql native client] yüklemek için yapılandırmanız gerekir. Bunu yapmak için, web rolünüzün kök dizinindeki `bin` klasöre [sqlncli.msi x64 yükleyicisini] ekleyin. Bir sonraki adımda açıklanan başlangıç komut dosyası, rol sağlandığında yükleyiciyi sessizce çalıştıracaktır. PHP çalışma süreniz SQL Server için PHP için Microsoft Sürücüleri kullanmıyorsa, bir sonraki adımda gösterilen komut dosyasından aşağıdaki satırı kaldırabilirsiniz:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. [Internet Information Services 'i (IIS)][iis.net] sayfalar için istekleri işlemek için `.php` PHP çalışma sürenizi kullanacak şekilde yapılandıran bir başlangıç görevi tanımlayın. Bunu yapmak için, `setup_web.cmd` dosyayı `bin` (web rolünüzün kök dizininin dosyasında) bir metin düzenleyicisinde açın ve içeriğini aşağıdaki komut dosyasıyla değiştirin:

    ```cmd
    @ECHO ON
    cd "%~dp0"

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
    SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"
    ```
5. Uygulama dosyalarınızı web rolünüzdeki kök dizine ekleyin. Bu web sunucusunun kök dizini olacaktır.
6. Başvurunuzu aşağıdaki [uygulamanızı yayımla](#publish-your-application) bölümünde açıklandığı şekilde yayınlayın.

> [!NOTE]
> Komut `download.ps1` dosyası (web rolünün kök dizininin `bin` klasöründe) kendi PHP çalışma sürenizi kullanmak için yukarıda açıklanan adımları takip ettikten sonra silinebilir.
>
>

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Kendi PHP çalışma sürenizi kullanacak bir alt rol yapılandırma

Bir alt rolü sağladığınız PHP çalışma zamanını kullanacak şekilde yapılandırmak için aşağıdaki adımları izleyin:

1. Bir Azure Hizmeti projesi oluşturun ve bu konuda daha önce açıklandığı gibi bir PHP çalışanı rolü ekleyin.
2. Alt `php` rolün kök dizininde bir klasör oluşturun ve ardından PHP çalışma sürenizi (tüm ikilidosyalar, yapılandırma dosyaları, alt klasörler, vb.) klasöre `php` ekleyin.
3. (İsteğE bağlı) PHP çalışma süreniz [SQL Server için PHP için Microsoft Drivers][sqlsrv drivers]kullanıyorsa, uygun olduğunda SQL Server Native Client [2012'yi][sql native client] yüklemek için alt rolünüzü yapılandırmanız gerekir. Bunu yapmak için, [sqlncli.msi x64 yükleyicisini] alt rolün kök dizinine ekleyin. Bir sonraki adımda açıklanan başlangıç komut dosyası, rol sağlandığında yükleyiciyi sessizce çalıştıracaktır. PHP çalışma süreniz SQL Server için PHP için Microsoft Sürücüleri kullanmıyorsa, bir sonraki adımda gösterilen komut dosyasından aşağıdaki satırı kaldırabilirsiniz:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Rol sağlandığında, yürütülebilir rolünüzü `php.exe` işçi rolün PATH ortamı değişkenine ekleyen bir başlangıç görevi tanımlayın. Bunu yapmak için, `setup_worker.cmd` dosyayı (alt rolün kök dizininde) bir metin düzenleyicisinde açın ve içeriğini aşağıdaki komut dosyasıyla değiştirin:

    ```cmd
    @echo on

    cd "%~dp0"

    echo Granting permissions for Network Service to the web root directory...
    icacls ..\ /grant "Network Service":(OI)(CI)W
    if %ERRORLEVEL% neq 0 goto error
    echo OK

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    setx Path "%PATH%;%~dp0php" /M

    if %ERRORLEVEL% neq 0 goto error

    echo SUCCESS
    exit /b 0

    :error

    echo FAILED
    exit /b -1
    ```
5. Uygulama dosyalarınızı çalışan rolünüzün kök dizinine ekleyin.
6. Başvurunuzu aşağıdaki [uygulamanızı yayımla](#publish-your-application) bölümünde açıklandığı şekilde yayınlayın.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Uygulamanızı bilgi işlem ve depolama emülatörlerinde çalıştırın

Azure emülatörleri, Azure uygulamanızı buluta dağıtmadan önce test edebileceğiniz yerel bir ortam sağlar. Emülatörleri ve Azure ortamı arasında bazı farklar vardır. Bunu daha iyi anlamak [için](storage/common/storage-use-emulator.md)bkz.

İşlem emülatörü kullanmak için yerel olarak PHP yüklü olması gerektiğini unutmayın. Bilgi işlem emülatörü, uygulamanızı çalıştırmak için yerel PHP yüklemenizi kullanır.

Projenizi emülatörlerde çalıştırmak için, projenizin kök dizininden aşağıdaki komutu uygulayın:

    PS C:\MyProject> Start-AzureEmulator

Buna benzer çıktı göreceksiniz:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Bir web tarayıcısı açarak ve çıktıda gösterilen yerel adrese (yukarıdaki örnek`http://127.0.0.1:81` çıktıda) göz atarak uygulamanızın emülatörde çalıştığını görebilirsiniz.

Emülatörleri durdurmak için şu komutu uygulayın:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Uygulamanızı yayımlama

Uygulamanızı yayınlamak için önce [Alma-AzurePublishSettingsFile](https://docs.microsoft.com/powershell/module/servicemanagement/azure/import-azurepublishsettingsfile) cmdlet'ini kullanarak yayımlama ayarlarınızı almanız gerekir. Daha sonra [Publish-AzureServiceProject](https://docs.microsoft.com/powershell/module/servicemanagement/azure/publish-azureserviceproject) cmdlet'i kullanarak uygulamanızı yayımlayabilirsiniz. Oturum açma hakkında daha fazla bilgi için [Azure PowerShell'i nasıl yükleyip yapılandırılatırınız.](/powershell/azure/overview)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [PHP Geliştirici Merkezi'ne](https://azure.microsoft.com/develop/php/)bakın.

[install ps and emulators]: https://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[hizmet tanımı (.csdef)]: https://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[hizmet yapılandırması (.cscfg)]: https://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: https://www.iis.net/
[sql native client]: https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation
[sqlsrv drivers]: https://php.net/sqlsrv
[sqlncli.msi x64 yükleyici]: https://go.microsoft.com/fwlink/?LinkID=239648
