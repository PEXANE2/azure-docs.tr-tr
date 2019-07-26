---
title: Office 'i ana VHD görüntüsüne yükler-Azure
description: Windows sanal masaüstü önizleme ana görüntüsünde Office 'i Azure 'a yüklemek ve özelleştirmek.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: v-chjenk
ms.openlocfilehash: 9243b3e6ae9e164930373960757109ae8a4512e4
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501184"
---
# <a name="install-office-on-a-master-vhd-image"></a>Ana VHD görüntüsü üzerinde Office yükleme

Bu makalede, Azure 'a yüklemek üzere Office 365 ProPlus, OneDrive ve diğer yaygın uygulamaların bir ana sanal sabit disk (VHD) görüntüsüne nasıl yükleneceği açıklanır. Kullanıcılarınızın belirli iş kolu (LOB) uygulamalarına erişmesi gerekiyorsa, bu makaledeki yönergeleri tamamladıktan sonra bunları yüklemenizi öneririz.

Bu makalede, zaten bir sanal makine (VM) oluşturmuş olduğunuz varsayılmaktadır. Aksi takdirde, bkz. [ana VHD görüntüsünü hazırlama ve özelleştirme](set-up-customize-master-image.md#create-a-vm)

Bu makalede ayrıca, Azure 'da veya Hyper-V Yöneticisi 'nde sağlanmasından bağımsız olarak, VM 'de yükseltilmiş erişiminizin olduğunu varsaymaktadır. Aksi takdirde, [tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişimi yükseltme](https://docs.microsoft.com/azure/role-based-access-control/elevate-access-global-admin)bölümüne bakın.

>[!NOTE]
>Bu yönergeler, kuruluşunuzun mevcut işlemleriyle birlikte kullanılabilecek Windows sanal masaüstü önizlemesine özgü bir yapılandırma içindir.

## <a name="install-office-in-shared-computer-activation-mode"></a>Office 'i paylaşılan bilgisayar etkinleştirme moduna yüklerken

Paylaşılan bilgisayar etkinleştirmesi, kuruluşunuzda birden çok kullanıcı tarafından erişilen bir bilgisayara Office 365 ProPlus dağıtmanızı sağlar. Paylaşılan bilgisayar etkinleştirmesi hakkında daha fazla bilgi için bkz. [Office 365 ProPlus için paylaşılan bilgisayar etkinleştirmeye genel bakış](https://docs.microsoft.com/DeployOffice/overview-of-shared-computer-activation-for-office-365-proplus).

Office 'i yüklemek için [Office dağıtım aracı](https://www.microsoft.com/download/details.aspx?id=49117) 'nı kullanın. Windows 10 Enterprise çoklu oturum yalnızca aşağıdaki Office sürümlerini destekler:
- Office 365 ProPlus
- Microsoft 365 İş abonelikle birlikte sunulan Office 365 Business

Office dağıtım aracı bir yapılandırma XML dosyası gerektirir. Aşağıdaki örneği özelleştirmek için, [Office dağıtım aracı yapılandırma seçeneklerine](https://docs.microsoft.com/deployoffice/configuration-options-for-the-office-2016-deployment-tool)bakın.

Sağladığımız Bu örnek yapılandırma XML 'i şu işlemleri yapar:

- Insiders kanalından Office 'i yükleyip, yürütüldüğü sırada Insiders kanalından güncelleştirmeleri sunun.
- X64 mimarisini kullanın.
- Otomatik güncelleştirmeleri devre dışı bırakın.
- Visio ve Project 'i yükler.
- Mevcut Office yüklemelerini kaldırın ve ayarlarını geçirin.
- Paylaşılan bilgisayar etkinleştirmesini etkinleştirin.

>[!NOTE]
>Visio 'daki kalıp arama özelliği, önizleme yapılandırması sırasında Windows sanal masaüstü 'nde çalışmaz.

Bu örnek yapılandırma XML 'i şunları yapamayacaklarınız:

- Skype Kurumsal 'ı yükler
- OneDrive 'ı Kullanıcı başına moda yükleyin. Daha fazla bilgi için bkz. [OneDrive 'ı makine başına moda yükleyin](#install-onedrive-in-per-machine-mode).

>[!NOTE]
>Paylaşılan bilgisayar etkinleştirme, grup ilkesi nesneleri (GPO 'Lar) veya kayıt defteri ayarları aracılığıyla ayarlanabilir. GPO, **Yönetim Şablonları\\\\\\MicrosoftOffice2016(makine) lisanslama ayarlarındaki bilgisayar yapılandırma ilkeleri ' nde bulunur\\**

Office dağıtım aracı Setup. exe dosyasını içerir. Office 'i yüklemek için komut satırında aşağıdaki komutu çalıştırın:

```batch
Setup.exe /configure configuration.xml
```

#### <a name="sample-configurationxml"></a>Örnek Configuration. xml

Aşağıdaki XML örneği, Insiders sürümünü yükler.

```xml
<Configuration>
    <Add OfficeClientEdition="64" SourcePath="https://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f">
        <Product ID="O365ProPlusRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Groove" />
            <ExcludeApp ID="Lync" />
            <ExcludeApp ID="OneDrive" />
            <ExcludeApp ID="Teams" />
        </Product>
        <Product ID="VisioProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" /> 
        </Product>
        <Product ID="ProjectProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" />
        </Product>
    </Add>
    <RemoveMSI All="True" />
    <Updates Enabled="FALSE" UpdatePath="https://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f" />
    <Display Level="None" AcceptEULA="TRUE" />
    <Logging Level="Verbose" Path="%temp%\WVDOfficeInstall" />
    <Property Value="TRUE" Name="FORCEAPPSHUTDOWN"/>
    <Property Value="1" Name="SharedComputerLicensing"/>
    <Property Value="TRUE" Name="PinIconsToTaskbar"/>
</Configuration>
```

>[!NOTE]
>Office ekibi **OfficeClientEdition** parametresi için 64 bitlik yüklemeyi kullanmayı önerir.

Office 'i yükledikten sonra varsayılan Office davranışını güncelleştirebilirsiniz. Davranışı güncelleştirmek için aşağıdaki komutları tek tek veya bir toplu iş dosyasında çalıştırın.

```batch
rem Mount the default user registry hive
reg load HKU\TempDefault C:\Users\Default\NTUSER.DAT
rem Must be executed with default registry hive mounted.
reg add HKU\TempDefault\SOFTWARE\Policies\Microsoft\office\16.0\common /v InsiderSlabBehavior /t REG_DWORD /d 2 /f
rem Set Outlook's Cached Exchange Mode behavior
rem Must be executed with default registry hive mounted.
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v enable /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v syncwindowsetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSettingMonths  /t REG_DWORD /d 1 /f
rem Unmount the default user registry hive
reg unload HKU\TempDefault

rem Set the Office Update UI behavior.
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideupdatenotifications /t REG_DWORD /d 1 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideenabledisableupdates /t REG_DWORD /d 1 /f
```

## <a name="install-onedrive-in-per-machine-mode"></a>OneDrive 'ı makine başına moduna yükleyin

OneDrive, normalde Kullanıcı başına yüklenir. Bu ortamda makine başına yüklenmelidir.

OneDrive 'ı makine başına modunda yüklemek için şu adımları uygulayın:

1. İlk olarak, OneDrive yükleyicisinin aşamasına kadar bir konum oluşturun. Yerel disk klasörü veya [\\\\UNC] (File://UNC) konumu iyidir.

2. OneDriveSetup. exe dosyasını bu bağlantıyla hazırlanan konumunuza indirin:<https://aka.ms/OneDriveWVD-Installer>

3. Office 'i,  **\<excludeapp ID =\>"OneDrive"** seçeneğini atlayarak OneDrive 'a yüklediyseniz, aşağıdaki komutu çalıştırarak yükseltilmiş bir komut isteminden mevcut OneDrive Kullanıcı başına yüklemelerini kaldırın:
    
    ```batch
    "[staged location]\OneDriveSetup.exe" /uninstall
    ```

4. **Allusersınstall** kayıt defteri değerini ayarlamak için bu komutu yükseltilmiş bir komut isteminden çalıştırın:

    ```batch
    REG ADD "HKLM\Software\Microsoft\OneDrive" /v "AllUsersInstall" /t REG_DWORD /d 1 /reg:64
    ```

5. OneDrive 'ı makine başına modunda yüklemek için şu komutu çalıştırın:

    ```batch
    Run "[staged location]\OneDriveSetup.exe" /allusers
    ```

6. OneDrive 'ı tüm kullanıcılar için oturum açma sırasında başlatılacak şekilde yapılandırmak için bu komutu çalıştırın:

    ```batch
    REG ADD "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v OneDrive /t REG_SZ /d "C:\Program Files (x86)\Microsoft OneDrive\OneDrive.exe /background" /f
    ```

7. Aşağıdaki komutu çalıştırarak **sessizce Kullanıcı hesabını Yapılandır** özelliğini etkinleştirin.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "SilentAccountConfig" /t REG_DWORD /d 1 /f
    ```

8. Aşağıdaki komutu çalıştırarak Windows bilinen klasörlerini yeniden yönlendirin ve OneDrive 'a taşıyın.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "KFMSilentOptIn" /t REG_SZ /d "<your-AzureAdTenantId>" /f
    ```

## <a name="teams-and-skype"></a>Takımlar ve Skype

Windows sanal masaüstü, Skype Kurumsal ve ekiplerini desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

Artık Office 'i görüntüye eklemişseniz, ana VHD görüntünüzü özelleştirmeye devam edebilirsiniz. Bkz. [ana VHD görüntüsünü hazırlama ve özelleştirme](set-up-customize-master-image.md).
