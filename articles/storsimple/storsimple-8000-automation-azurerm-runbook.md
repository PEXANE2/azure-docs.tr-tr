---
title: StorSimple cihazlarını yönetmek için Azure Otomasyonu runbook 'U kullanma
description: Azure Otomasyonu runbook 'U kullanarak StorSimple işlerini otomatikleştirme hakkında bilgi edinin
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 10/23/2017
ms.author: alkohli
ms.openlocfilehash: 727bebe0c190ed4dff4408884c45fe166ad541a9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76276969"
---
# <a name="use-azure-automation-runbooks-to-manage-storsimple-devices"></a>StorSimple cihazlarını yönetmek için Azure Otomasyonu runbook 'larını kullanma

Bu makalede, Azure Otomasyonu runbook 'larının Azure portal ' de StorSimple 8000 serisi cihazınızı yönetmek için nasıl kullanılacağı açıklanır. Bu runbook 'u yürütmek için ortamınızı yapılandırma adımlarında size yol gösterecek örnek bir runbook dahil edilmiştir.


## <a name="configure-add-and-run-azure-runbook"></a>Azure runbook 'u yapılandırma, ekleme ve çalıştırma

Bu bölümde, StorSimple için bir Windows PowerShell betiği örneği alınır ve betiği bir runbook 'a aktarmak ve sonra runbook 'u yayımlamak ve yürütmek için gereken çeşitli adımların ayrıntıları yer alır.

### <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, şunları kullandığınızdan emin olun:

* StorSimple 8000 serisi cihazından kayıtlı StorSimple Aygıt Yöneticisi hizmetinize ilişkin etkin bir Azure aboneliği.

* Bilgisayarınızda yüklü olan Windows PowerShell 5,0 (veya kullanıyorsanız, StorSimple için Windows Server ana bilgisayarınız).

### <a name="create-automation-runbook-module-in-windows-powershell"></a>Windows PowerShell 'de Otomasyon Runbook modülü oluşturma

StorSimple 8000 serisi cihaz yönetimi için bir otomasyon modülü oluşturmak için aşağıdaki adımları uygulayın:

1. Windows PowerShell 'i başlatın. Yeni bir klasör oluşturun ve dizini yeni klasör ile değiştirin.

    ```powershell
        mkdir C:\scripts\StorSimpleSDKTools
        cd C:\scripts\StorSimpleSDKTools
    ```

2. Önceki adımda oluşturulan klasöre [NUGET CLI 'Yı indirin](https://www.nuget.org/downloads) . _NuGet. exe_' nin çeşitli sürümleri vardır. SDK 'nize karşılık gelen sürümü seçin. Her indirme bağlantı noktası doğrudan bir _. exe_ dosyasına bağlanır. Sağ tıklayıp dosyayı tarayıcıdan çalıştırmak yerine bilgisayarınıza kaydettiğinizden emin olun.

    Ayrıca, aşağıdaki komutu çalıştırarak betiği daha önce oluşturduğunuz aynı klasöre indirebilir ve kaydedebilirsiniz.

    ```
        wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```

3. Bağımlı SDK 'Yı indirin.

    ```
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```

4. Betiği örnek GitHub projesinden indirin.

    ```
        wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Monitor-Backups.ps1 -Out Monitor-Backups.ps1
    ```

5. StorSimple 8000 serisi cihaz yönetimi için bir Azure Otomasyonu runbook modülü oluşturun. Windows PowerShell penceresinde aşağıdaki komutları yazın:

    ```powershell
        # set path variables
        $downloadDir = "C:\scripts\StorSimpleSDKTools"
        $moduleDir = "$downloadDir\AutomationModule\Microsoft.Azure.Management.StorSimple8000Series"

        #don't change the folder name "Microsoft.Azure.Management.StorSimple8000Series"
        mkdir "$moduleDir"
        Copy-Item "$downloadDir\Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\Microsoft.IdentityModel.Clients.ActiveDirectory*.dll" $moduleDir
        Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\Microsoft.Rest.ClientRuntime.Azure*.dll" $moduleDir
        Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.2.3.8\lib\net452\Microsoft.Rest.ClientRuntime*.dll" $moduleDir
        Copy-Item "$downloadDir\Newtonsoft.Json.6.0.8\lib\net45\Newtonsoft.Json*.dll" $moduleDir
        Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.Authentication.2.2.9-preview\lib\net45\Microsoft.Rest.ClientRuntime.Azure.Authentication*.dll" $moduleDir
        Copy-Item "$downloadDir\Microsoft.Azure.Management.Storsimple8000series.1.0.0\lib\net452\Microsoft.Azure.Management.Storsimple8000series*.dll" $moduleDir

        #Don't change the name of the Archive
        compress-Archive -Path "$moduleDir" -DestinationPath Microsoft.Azure.Management.StorSimple8000Series.zip
    ```

6. Uygulamasında `C:\scripts\StorSimpleSDKTools`bir otomasyon modülü ZIP dosyası oluşturulduğunu doğrulayın.

    ![Verify-Automation-Module](./media/storsimple-8000-automation-azurerm-runbook/verify-automation-module.png)

7. Aşağıdaki çıktı, otomasyon modülü Windows PowerShell aracılığıyla oluşturulduğunda sunulur.

    ```powershell
    mkdir C:\scripts\StorSimpleSDKTools
    ```

    ```Output
        Directory: C:\scripts

    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d-----       10/18/2017   8:43 AM                StorSimpleSDKTools
    ```

    ```powershell
    wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```

    ```powershell
    C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
    ```

    ```Output
    -------------------------------------------
    CUT              CUT
    -------------------------------------------
    Successfully installed 'Microsoft.Azure.Management.Storsimple8000series 1.0.0' to C:\scripts\StorSimpleSDKTools
    Executing nuget actions took 1.77 sec
    ```

    ```powershell
    C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
    ```

    ```Output
    -------------------------------------------
    CUT              CUT
    -------------------------------------------
    Successfully installed 'Microsoft.IdentityModel.Clients.ActiveDirectory 2.28.3' to C:\scripts\StorSimpleSDKTools
    Executing nuget actions took 927.64 ms
    ```

    ```powershell
    C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```

    ```Output
    -------------------------------------------
    CUT              CUT
    -------------------------------------------
    Successfully installed 'Microsoft.Rest.ClientRuntime.Azure.Authentication 2.2.9-preview' to C:\scripts\StorSimpleSDKTools
    Executing nuget actions took 717.48 ms
    ```

    ```powershell
    wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Monitor-Backups.ps1 -Out Monitor-Backups.ps1
    # set path variables
    $downloadDir = "C:\scripts\StorSimpleSDKTools"
    $moduleDir = "$downloadDir\AutomationModule\Microsoft.Azure.Management.StorSimple8000Series"
    #don't change the folder name "Microsoft.Azure.Management.StorSimple8000Series"
    mkdir "$moduleDir"
    ```

    ```Output
        Directory: C:\scripts\StorSimpleSDKTools\AutomationModule

    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d-----       10/18/2017   8:48 AM                Microsoft.Azure.Management.StorSimple8000Series
    ```

    ```powershell
    Copy-Item "$downloadDir\Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\Microsoft.IdentityModel.Clients.ActiveDirectory*.dll" $moduleDir
    Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\Microsoft.Rest.ClientRuntime.Azure*.dll" $moduleDir
    Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.2.3.8\lib\net452\Microsoft.Rest.ClientRuntime*.dll" $moduleDir
    Copy-Item "$downloadDir\Newtonsoft.Json.6.0.8\lib\net45\Newtonsoft.Json*.dll" $moduleDir
    Copy-Item "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.Authentication.2.2.9-preview\lib\net45\Microsoft.Rest.ClientRuntime.Azure.Authentication*.dll" $moduleDir
    Copy-Item "$downloadDir\Microsoft.Azure.Management.Storsimple8000series.1.0.0\lib\net452\Microsoft.Azure.Management.Storsimple8000series*.dll" $moduleDir
    #Don't change the name of the Archive
    compress-Archive -Path "$moduleDir" -DestinationPath Microsoft.Azure.Management.StorSimple8000Series.zip
    ```

### <a name="import-publish-and-run-automation-runbook"></a>Otomasyon Runbook 'unu içeri aktarma, yayımlama ve çalıştırma

1. Azure portal Azure farklı çalıştır Otomasyon hesabı oluşturun. Bunu yapmak için **Azure Marketi 'Ne her şeyi >** gidin ve **Otomasyon**' ı arayın. **Otomasyon hesapları**' nı seçin.

    ![Arama-Otomasyon](./media/storsimple-8000-automation-azurerm-runbook/automation1.png)

2. **Otomasyon hesabı ekle** dikey penceresinde:

   1. Otomasyon hesabınızın **adını** sağlayın.
   2. StorSimple Aygıt Yöneticisi hizmetinize bağlı **aboneliği** seçin.
   3. Yeni bir kaynak grubu oluşturun veya var olan bir kaynak grubundan seçin.
   4. Bir **konum** seçin (mümkünse hizmetinizin çalıştığı gibi).
   5. Varsayılan **Farklı Çalıştır hesabı oluştur** seçeneğini seçili bırakın.
   6. İsteğe bağlı olarak **panoya sabitle ' yi**işaretleyin. **Oluştur**' a tıklayın.

       ![oluşturma-Otomasyon hesabı](./media/storsimple-8000-automation-azurerm-runbook/create-automation-account.png)

      Otomasyon hesabı başarıyla oluşturulduktan sonra bilgilendirirsiniz. Otomasyon hesabı oluşturma hakkında daha fazla bilgi için, [Farklı Çalıştır hesabı oluşturma](https://docs.microsoft.com/azure/automation/automation-create-runas-account)bölümüne gidin.

3. Oluşturulan Otomasyon hesabının StorSimple Aygıt Yöneticisi hizmetine erişebildiğinden emin olmak için, Otomasyon hesabına uygun izinleri atamanız gerekir. StorSimple Aygıt Yöneticisi hizmetinizde **erişim denetimi** ' ne gidin. **+ Ekle** ' ye tıklayın ve Azure Otomasyonu hesabınızın adını sağlayın. Ayarları **kaydedin** .

    ![Add-Permissions-Automation-Account](./media/storsimple-8000-automation-azurerm-runbook/goto-add-roles.png)

4. Yeni oluşturulan hesapta, **paylaşılan kaynaklar > modüller** ' e gidin ve **+ Modül Ekle**' ye tıklayın.

5. **Modül Ekle** dikey penceresinde, daraltılmış modülün konumuna gidin ve modülünü seçin ve açın. **Tamam**'a tıklayın.

    ![Add-Module](./media/storsimple-8000-automation-azurerm-runbook/add-module.png)

6. **Işlem otomasyonu > runbook 'larına gidin ve + Runbook Ekle ' ye tıklayın**. **Runbook Ekle** dikey penceresinde **mevcut bir runbook 'u içeri aktar**' a tıklayın. **Runbook dosyası**Için Windows PowerShell betik dosyasına işaret edin. Runbook türü otomatik olarak seçilir. Runbook için bir ad ve isteğe bağlı bir açıklama sağlayın. **Oluştur**' a tıklayın.

    ![Add-Module](./media/storsimple-8000-automation-azurerm-runbook/import-runbook.png)

7. Runbook, runbook 'ların listesine eklenir. Bu runbook 'u seçin ve tıklayın.

    ![tıklama-yeni-runbook](./media/storsimple-8000-automation-azurerm-runbook/verify-runbook-created.png)

8. Runbook 'u düzenleyin ve **Test bölmesi**' ne tıklayın. StorSimple Aygıt Yöneticisi hizmetinizin adı, StorSimple cihazının adı ve aboneliğin adı gibi parametreleri sağlayın. Testi **başlatın** . Çalışma tamamlandığında rapor oluşturulur. Daha fazla bilgi için, [runbook 'u test etme](../automation/automation-first-runbook-textual-powershell.md#step-3---test-the-runbook)konusuna gidin.

    ![test-runbook](./media/storsimple-8000-automation-azurerm-runbook/test-runbook.png)

9. Test bölmesindeki runbook 'tan çıktıyı inceleyin. Memnun olursa bölmeyi kapatın. **Yayımla** ' ya tıklayın ve onay sorulduğunda runbook 'u onaylayın ve yayımlayın.

    ![Yayımlama-runbook](./media/storsimple-8000-automation-azurerm-runbook/publish-runbook.png)

## <a name="next-steps"></a>Sonraki adımlar

StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini kullanın](storsimple-8000-manager-service-administration.md).
