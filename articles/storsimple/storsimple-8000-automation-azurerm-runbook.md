---
title: StorSimple aygıtlarını yönetmek için Azure Otomasyon Runbook'u kullanma
description: StorSimple işlerini otomatikleştirmek için Azure Otomasyon Runbook'u nasıl kullanacağınızı öğrenin
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 10/23/2017
ms.author: alkohli
ms.openlocfilehash: 727bebe0c190ed4dff4408884c45fe166ad541a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76276969"
---
# <a name="use-azure-automation-runbooks-to-manage-storsimple-devices"></a>StorSimple aygıtlarını yönetmek için Azure Otomasyon runkitaplarını kullanma

Bu makalede, Azure Otomasyon runbook'larının Azure portalındaki StorSimple 8000 serisi aygıtınızı yönetmek için nasıl kullanıldığı açıklanmaktadır. Bu runbook yürütmek için ortamınızı yapılandırma adımları üzerinden size yol için örnek bir runbook dahildir.


## <a name="configure-add-and-run-azure-runbook"></a>Azure runbook'u yapılandırma, ekleme ve çalıştırma

Bu bölümde StorSimple için bir örnek Windows PowerShell komut dosyası alınır ve komut dosyasını bir runbook'a aktarmak ve ardından runbook'u yayımlamak ve yürütmek için gereken çeşitli adımları ayrıntılarıyla anlatır.

### <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yaptığınızdan emin olun:

* StorSimple 8000 serisi bir cihaza kayıtlı StorSimple Device Manager hizmetinizle ilişkili etkin bir Azure aboneliği.

* Bilgisayarınızda Windows PowerShell 5.0 yüklenir (Veya kullanıyorsanız StorSimple'ınız için Windows Server ana bilgisayarınız).

### <a name="create-automation-runbook-module-in-windows-powershell"></a>Windows PowerShell'de otomasyon runbook modülü oluşturun

StorSimple 8000 serisi cihaz yönetimi için bir otomasyon modülü oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Windows PowerShell'i başlatın. Yeni bir klasör oluşturun ve dizini yeni klasöre değiştirin.

    ```powershell
        mkdir C:\scripts\StorSimpleSDKTools
        cd C:\scripts\StorSimpleSDKTools
    ```

2. [NuGet CLI'yi](https://www.nuget.org/downloads) önceki adımda oluşturulan klasörün altına indirin. _Nuget.exe_çeşitli sürümleri vardır. SDK'nıza karşılık gelen sürümü seçin. Her indirme bağlantısı doğrudan bir _.exe_ dosyasına işaret edin. Dosyayı tarayıcıdan çalıştırmak yerine sağ tıklayıp bilgisayarınıza kaydettiğinizden emin olun.

    Komut dosyasını daha önce oluşturduğunuz klasörü indirmek ve depolamak için aşağıdaki komutu da çalıştırabilirsiniz.

    ```
        wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```

3. Bağımlı SDK'yı indirin.

    ```
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```

4. Komut dosyasını örnek GitHub projesinden indirin.

    ```
        wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Monitor-Backups.ps1 -Out Monitor-Backups.ps1
    ```

5. StorSimple 8000 Serisi cihaz yönetimi için bir Azure Otomasyon Runbook Modülü oluşturun. Windows Powershell penceresinde aşağıdaki komutları yazın:

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

6. Bir otomasyon modülü zip dosyası `C:\scripts\StorSimpleSDKTools`nın 'da oluşturulduğunu doğrulayın.

    ![verify-otomasyon-modül](./media/storsimple-8000-automation-azurerm-runbook/verify-automation-module.png)

7. Otomasyon modülü Windows PowerShell üzerinden oluşturulduğunda aşağıdaki çıktı sunulur.

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

### <a name="import-publish-and-run-automation-runbook"></a>Otomasyon runbook'u alma, yayımlama ve çalıştırma

1. Azure portalında bir Azure Çalıştır otomasyon hesabı oluşturun. Bunu yapmak için Azure **pazar > Her Şey'e** gidin ve ardından **Otomasyon'u**arayın. **Otomasyon hesaplarını**seçin.

    ![arama otomasyonu](./media/storsimple-8000-automation-azurerm-runbook/automation1.png)

2. Ekle **Otomasyon** Hesabı'nda:

   1. Otomasyon hesabınızın **Adını** nızı temin edin.
   2. StorSimple Device Manager hizmetinize bağlı **Abonelik'i** seçin.
   3. Yeni bir kaynak grubu oluşturun veya varolan bir kaynak grubundan seçim yap.
   4. Bir **Konum** seçin (mümkünse hizmetinizin çalıştığı yerle aynı).
   5. Varsayılan **Create Run As hesabını** seçili bırakın.
   6. İsteğe bağlı olarak **Panoya Pin'i**denetleyin. **Oluştur'u**tıklatın.

       ![create-automation-account](./media/storsimple-8000-automation-azurerm-runbook/create-automation-account.png)

      Otomasyon hesabı başarıyla oluşturulduktan sonra size bildirilir. Otomasyon hesabı oluşturma hakkında daha fazla bilgi için, [Bir Run As hesabı oluştur'a](https://docs.microsoft.com/azure/automation/automation-create-runas-account)gidin.

3. Oluşturulan otomasyon hesabının StorSimple Device Manager hizmetine erişebilmesini sağlamak için otomasyon hesabına uygun izinler atamanız gerekir. StorSimple Device Manager hizmetinizde **Access denetimine** gidin. **+ Ekle'yi** tıklatın ve Azure Otomasyon Hesabınızın adını ekleyin. Ayarları **kaydedin.**

    ![eklenti-izinler-otomasyon-hesap](./media/storsimple-8000-automation-azurerm-runbook/goto-add-roles.png)

4. Yeni oluşturulan hesapta, **Paylaşılan Kaynaklar > Modülleri'ne** gidin ve **+ Ekle modülüne**tıklayın.

5. Modül **ekle** bıçağında, sıkıştırılmış modülün konumuna göz atın ve modülü seçin ve açın. **Tamam**'a tıklayın.

    ![ek modül](./media/storsimple-8000-automation-azurerm-runbook/add-module.png)

6. Çalışma **Kitapları > İşlem Otomasyonu'na gidin ve runbook ekle +'ya tıklayın.** **Runbook Ekle** çubuğunda, **varolan bir runbook'u içe aktar'ı**tıklatın. **Runbook dosyası**için Windows PowerShell komut dosyası dosyasına işaret edin. Runbook türü otomatik olarak seçilir. Runbook için bir ad ve isteğe bağlı bir açıklama sağlayın. **Oluştur'u**tıklatın.

    ![ek modül](./media/storsimple-8000-automation-azurerm-runbook/import-runbook.png)

7. Runbook runbook listesine eklenir. Bu runbook'u seçin ve tıklatın.

    ![tıklayın-yeni-runbook](./media/storsimple-8000-automation-azurerm-runbook/verify-runbook-created.png)

8. Runbook'u ve **Test bölmesini**tıklatın. StorSimple Aygıt Yöneticisi hizmetinizin adı, StorSimple aygıtının adı ve abonelik gibi parametreleri sağlayın. Testi **başlatın.** Çalışma tamamlandığında rapor oluşturulur. Daha fazla bilgi için, [bir runbook test etmek için nasıl](../automation/automation-first-runbook-textual-powershell.md#step-3---test-the-runbook)gidin.

    ![test-runbook](./media/storsimple-8000-automation-azurerm-runbook/test-runbook.png)

9. Test bölmesinde runbook'tan çıktıyı inceleyin. Memnun olursa, bölmeyi kapatın. **Yayımla'yı** ve onay istendiğinde runbook'u onaylayın ve yayımlayın'ı tıklatın.

    ![yayımlama-runbook](./media/storsimple-8000-automation-azurerm-runbook/publish-runbook.png)

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manager-service-administration.md)
