---
title: Office'i ana VHD görüntüsüne yükleme - Azure
description: Office'i Windows Sanal Masaüstü ana resmine Azure'a yükleme ve özelleştirme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: b93f26a6799a50868feb1f3350a3dc4a73a0b2e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127843"
---
# <a name="install-office-on-a-master-vhd-image"></a>Ana VHD görüntüsü üzerinde Office yükleme

Bu makalede, Azure'a yüklemek için office 365 ProPlus, OneDrive ve diğer yaygın uygulamaları ana sanal sabit disk (VHD) görüntüsüne nasıl yükleyebilirsiniz. Kullanıcılarınızın belirli iş alanı (LOB) uygulamalarına erişmeleri gerekiyorsa, bu makaledeki yönergeleri tamamladıktan sonra bunları yüklemenizi öneririz.

Bu makalede, zaten bir sanal makine (VM) oluşturduğunuz varsayar. Değilse, [bkz.](set-up-customize-master-image.md#create-a-vm)

Bu makalede, Azure veya Hyper-V Manager'da sağlanan vm erişiminin yüksek olduğu da varsayar. Değilse, bkz. [Tüm Azure abonelik ve yönetim gruplarını yönetmek için erişimi yükselt.](../role-based-access-control/elevate-access-global-admin.md)

>[!NOTE]
>Bu yönergeler, kuruluşunuzun varolan işlemleriyle kullanılabilecek Windows Sanal Masaüstüne özgü bir yapılandırma içindir.

## <a name="install-office-in-shared-computer-activation-mode"></a>Paylaşılan bilgisayar etkinleştirme modunda Office'i yükleme

Paylaşılan bilgisayar etkinleştirmesi, Office 365 ProPlus'ı kuruluşunuzdaki birden çok kullanıcı tarafından erişilen bir bilgisayara dağıtmanıza olanak tanır. Paylaşılan bilgisayar etkinleştirme hakkında daha fazla bilgi için Office [365 ProPlus için paylaşılan bilgisayar etkinleştirme genel görünümüne](/deployoffice/overview-of-shared-computer-activation-for-office-365-proplus/)bakın.

Office'i yüklemek için [Office Dağıtım Aracı'nı](https://www.microsoft.com/download/details.aspx?id=49117) kullanın. Windows 10 Enterprise çoklu oturumyalnızca Office'in aşağıdaki sürümlerini destekler:
- Office 365 ProPlus
- Microsoft 365 Business aboneliğiyle birlikte gelen Office 365 İşletme

Office Dağıtım Aracı yapılandırma XML dosyası gerektirir. Aşağıdaki örneği özelleştirmek için [Office Dağıtım Aracı için Yapılandırma Seçenekleri'ne](/deployoffice/configuration-options-for-the-office-2016-deployment-tool/)bakın.

Sağladığımız bu örnek yapılandırma XML aşağıdaki leri yapacaktır:

- Office'i aylık kanaldan yükleyin ve yürütüldüğünde aylık kanaldan güncelleştirmeler teslim edin.
- x64 mimarisini kullanın.
- Otomatik güncelleştirmeleri devre dışı.
- Office'in varolan tüm yüklemelerini kaldırın ve ayarlarını geçirin.
- Paylaşılan bilgisayar etkinleştirmesini etkinleştirin.

>[!NOTE]
>Visio'nun şablon arama özelliği Windows Sanal Masaüstü'nde beklendiği gibi çalışmayabilir.

XML'in bu örnek yapılandırması şunları yapmaz:

- İşletmeler için Skype'ı yükleyin
- OneDrive'ı kullanıcı başına modda yükleyin. Daha fazla bilgi [için, makine başına yükleme modunda OneDrive'ı yükleyin'](#install-onedrive-in-per-machine-mode)e bakın.

>[!NOTE]
>Paylaşılan Bilgisayar Etkinleştirme Grup İlkesi Nesneleri (GPO) veya kayıt defteri ayarları aracılığıyla ayarlanabilir. GPO, **Bilgisayar Yapılandırma\\İlkeleri\\Yönetim Şablonları\\Microsoft Office 2016 (Makine)\\LisansLama Ayarları'nda** bulunur

Office Dağıtım Aracı setup.exe içerir. Office'i yüklemek için aşağıdaki komutu bir komut satırında çalıştırın:

```batch
Setup.exe /configure configuration.xml
```

#### <a name="sample-configurationxml"></a>Örnek configuration.xml

Aşağıdaki XML örneği aylık sürümü yükler.

```xml
<Configuration>
  <Add OfficeClientEdition="64" Channel="Monthly">
    <Product ID="O365ProPlusRetail">
      <Language ID="en-US" />
      <Language ID="MatchOS" />
      <ExcludeApp ID="Groove" />
      <ExcludeApp ID="Lync" />
      <ExcludeApp ID="OneDrive" />
      <ExcludeApp ID="Teams" />
    </Product>
  </Add>
  <RemoveMSI/>
  <Updates Enabled="FALSE"/>
  <Display Level="None" AcceptEULA="TRUE" />
  <Logging Level=" Standard" Path="%temp%\WVDOfficeInstall" />
  <Property Name="FORCEAPPSHUTDOWN" Value="TRUE"/>
  <Property Name="SharedComputerLicensing" Value="1"/>
</Configuration>
```

>[!NOTE]
>Office ekibi, **OfficeClientEdition** parametresi için 64 bit yükleme kullanmanızı önerir.

Office'i yükledikten sonra varsayılan Office davranışını güncelleştirebilirsiniz. Davranışı güncelleştirmek için aşağıdaki komutları tek tek veya toplu iş dosyasında çalıştırın.

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

## <a name="install-onedrive-in-per-machine-mode"></a>OneDrive'ı makine başına modda yükleme

OneDrive normalde kullanıcı başına yüklenir. Bu ortamda, makine başına kurulmalıdır.

OneDrive'ı makine başına modda şu şekilde yükleyebilirsiniz:

1. İlk olarak, OneDrive yükleyicisini sahnelemek için bir konum oluşturun. Yerel bir disk\\\\klasörü veya [ unc] (file://unc) konumu iyi.

2. OneDriveSetup.exe'yi bu bağlantı ile sahnelenen konumunuza indirin:<https://aka.ms/OneDriveWVD-Installer>

3. OneDrive ile ofis kurduysanız, **Dışlanan App\>ID="OneDrive" / , aşağıdaki komutu çalıştırarak kullanıcı başına mevcut OneDrive yüklemelerini yükseltilmiş bir komut isteminden kaldırın: \<**
    
    ```batch
    "[staged location]\OneDriveSetup.exe" /uninstall
    ```

4. **AllUsersInstall** kayıt defteri değerini ayarlamak için bu komutu yükseltilmiş bir komut isteminden çalıştırın:

    ```batch
    REG ADD "HKLM\Software\Microsoft\OneDrive" /v "AllUsersInstall" /t REG_DWORD /d 1 /reg:64
    ```

5. OneDrive'ı makine başına modda yüklemek için bu komutu çalıştırın:

    ```batch
    Run "[staged location]\OneDriveSetup.exe" /allusers
    ```

6. OneDrive'ı tüm kullanıcılar için oturum açmabaşlatmak üzere yapılandırmak için bu komutu çalıştırın:

    ```batch
    REG ADD "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v OneDrive /t REG_SZ /d "C:\Program Files (x86)\Microsoft OneDrive\OneDrive.exe /background" /f
    ```

7. Aşağıdaki komutu çalıştırarak **kullanıcı hesabını sessizce yapılandırmayı** etkinleştirin.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "SilentAccountConfig" /t REG_DWORD /d 1 /f
    ```

8. Aşağıdaki komutu çalıştırarak Windows bilinen klasörleri OneDrive'a yönlendirin ve taşıyın.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "KFMSilentOptIn" /t REG_SZ /d "<your-AzureAdTenantId>" /f
    ```

## <a name="teams-and-skype"></a>Takımlar ve Skype

Windows Sanal Masaüstü, Skype kurumsal ve takımlar için desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar

Görüntüye Office eklediğinize göre, ana VHD görüntünüzü özelleştirmeye devam edebilirsiniz. Bkz. [Bir ana VHD görüntü hazırlayın ve özelleştirin.](set-up-customize-master-image.md)
