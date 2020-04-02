---
title: Windows şablon makinesi kurma kılavuzu | Microsoft Dokümanlar
description: Laboratuvar Hizmetleri'nde Windows şablon uyrama makinesi hazırlamak için genel adımlar.  Bu adımlar, Windows Update zamanlamasını ayarlamayı, OneDrive'ı yüklemeyi ve Office'i yüklemeyi içerir.
services: lab-services
documentationcenter: na
author: EMaher
manager: ''
editor: ''
ms.service: lab-services
ms.topic: article
ms.date: 11/21/2019
ms.author: enewman
ms.openlocfilehash: c1aaf588f61b329fa3b838b8a92f3e287897315b
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521182"
---
# <a name="guide-to-setting-up-a-windows-template-machine-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'nde Windows şablon makinesi kurma kılavuzu

Azure Lab Hizmetleri için bir Windows 10 şablon uymaktadırsanız, göz önünde bulundurulması gereken en iyi uygulamalar ve ipuçları aşağıda verilmiştir. Aşağıdaki yapılandırma adımlarının tümü isteğe bağlıdır.  Ancak, bu hazırlık adımları öğrencilerinizin daha üretken olmalarını, sınıf zaman kesintilerini en aza indirmelerine ve en son teknolojileri kullandıklarından emin olmalarına yardımcı olabilir.

>[!IMPORTANT]
>Bu makalede, makine şablonu değişiklik işlemini kolaylaştırmak için PowerShell parçacıkları içerir.  Gösterilen tüm PowerShell komut dosyaları için, bunları Windows PowerShell'de yönetici ayrıcalıklarıyla çalıştırmak isteyeceksiniz. Windows 10'da, bunu yapmanın hızlı bir yolu Başlat Menüsü'ne sağ tıklatmak ve "Windows PowerShell (Yönetici)"yi seçmektir.

## <a name="install-and-configure-onedrive"></a>OneDrive'ı yükleme ve yapılandırma

Sanal bir makine sıfırlanırsa öğrenci verilerinin kaybolmasını korumak için öğrencilerin verilerini buluta yedeklemelerini öneririz.  Microsoft OneDrive, öğrencilerin verilerini korumalarına yardımcı olabilir.  

### <a name="install-onedrive"></a>OneDrive'ı Yükleyin

OneDrive'ı el ile indirmek ve yüklemek için İş için [OneDrive](https://onedrive.live.com/about/download/) veya [OneDrive](https://onedrive.live.com/about/business/) indirme sayfalarına bakın.

Aşağıdaki PowerShell komut dosyasını da kullanabilirsiniz.  OneDrive'ın en son sürümünü otomatik olarak indirip yükler.  OneDrive istemcisi yüklendikten sonra yükleyiciyi çalıştırın.  Örneğimizde, makinedeki `/allUsers` tüm kullanıcılar için OneDrive'ı yüklemek için anahtarı kullanırız. Biz de `/silent` sessizce OneDrive yüklemek için anahtarı kullanın.

```powershell
Write-Host "Downloading OneDrive Client..."
$DownloadPath = "$env:USERPROFILE/Downloads/OneDriveSetup.exe"
if((Test-Path $DownloadPath) -eq $False )
{
    Write-Host "Downloading OneDrive..."
    $web = new-object System.Net.WebClient
    $web.DownloadFile("https://go.microsoft.com/fwlink/p/?LinkId=248256",$DownloadPath)
} else {
    Write-Host "OneDrive installer already exists at " $DownloadPath
}

Write-Host "Installing OneDrive..."
& $env:USERPROFILE/Downloads/OneDriveSetup.exe /allUsers /silent
```

### <a name="onedrive-customizations"></a>OneDrive özelleştirmeleri

[OneDrive için yapılabilecek](https://docs.microsoft.com/onedrive/use-group-policy)birçok özelleştirme vardır. Bazı daha yaygın özelleştirmeler kapağıyapalım.

#### <a name="silently-move-windows-known-folders-to-onedrive"></a>Windows bilinen klasörleri sessizce OneDrive'a taşıyın

Belgeler, İndirmeler ve Resimler gibi klasörler genellikle öğrenci dosyalarını depolamak için kullanılır. Bu klasörlerin OneDrive'a yedeklenmediğinden emin olmak için, bu klasörleri OneDrive'a taşımanızı öneririz.

Active Directory kullanmayan bir makinedeyseniz, kullanıcılar bu klasörleri OneDrive'a kimlik doğruladıktan sonra el ile OneDrive'a taşıyabilir.

1. Dosya Gezgini'ni Aç
2. Belgeler, İndirmeler veya Resimler klasörüne sağ tıklayın.
3. Özellikler > Konumu'na gidin.  Klasörü OneDrive dizinindeki yeni bir klasöre taşıyın.

Sanal makineniz Active Directory'ye bağlıysa, şablon makinesini öğrencilerinizin bilinen klasörleri OneDrive'a taşımalarını otomatik olarak isteyecek şekilde ayarlayabilirsiniz.  

Önce Office Kiracı Kimliğinizi almanız gerekir.  Diğer talimatlar için [bkz.](https://docs.microsoft.com/onedrive/find-your-office-365-tenant-id)  Aşağıdaki PowerShell'i kullanarak Office 365 Kiracı Kimliğini de alabilirsiniz.

```powershell
Install-Module MSOnline -Confirm
Connect-MsolService
$officeTenantID = Get-MSOLCompanyInformation |
    Select-Object -expand objectID |
    Select-Object -expand Guid
```

Office 365 Kiracı Kimliğiniz aldıktan sonra OneDrive'ı aşağıdaki PowerShell'i kullanarak bilinen klasörleri OneDrive'a taşıması için ayarla.

```powershell
if ($officeTenantID -eq $null)
{
        Write-Error "Variable `$officeTenantId must be set to your Office Tenant Id before continuing."
}
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\OneDrive"
New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\OneDrive"
    -Name "KFMSilentOptIn" -Value $officeTenantID -PropertyType STRING
```

### <a name="use-onedrive-files-on-demand"></a>OneDrive dosyalarını isteğe bağlı kullanma

Öğrencilerin OneDrive hesaplarında birçok dosya olabilir. Makinede yer tasarrufu ve indirme süresini kısaltmak için, öğrencinin OneDrive hesabında depolanan tüm dosyaların isteğe bağlı olmasını öneririz.  İsteğe bağlı dosyalar yalnızca kullanıcı dosyaya eriştikten sonra karşıdan yüklenebilir.

```powershell
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\OneDrive" -Force
New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\OneDrive"
    -Name "FilesOnDemandEnabled" -Value "00000001" -PropertyType DWORD
```

### <a name="silently-sign-in-users-to-onedrive"></a>Kullanıcıları OneDrive'da sessizce oturum açın

OneDrive, oturum açmış kullanıcının Windows kimlik bilgileriyle otomatik olarak oturum açacak şekilde ayarlanabilir.  Otomatik oturum açma, öğrencinin Office 365 okul kimlik bilgilerini oturum alabildiği sınıflar için yararlıdır.

```powershell
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\OneDrive"
New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\OneDrive"
    -Name "SilentAccountConfig" -Value "00000001" -PropertyType DWORD
```

### <a name="disable-the-tutorial-that-appears-at-the-end-of-onedrive-setup"></a>OneDrive kurulumunun sonunda görünen öğreticiyi devre dışı

Bu ayar, öğreticinin OneDrive Kurulumu'nun sonundabir web tarayıcısında başlatılmasını engellemenize olanak tanır.

```powershell
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\OneDrive" -Force
New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\OneDrive"
    -Name "DisableTutorial" -Value "00000001" -PropertyType DWORD -Force
```

### <a name="set-the-maximum-size-of-a-file-that-to-be-download-automatically"></a>Otomatik olarak indirilecek bir dosyanın maksimum boyutunu ayarlama

Bu ayar, OneDrive Files On-Demand etkin olmayan cihazlarda Windows kimlik bilgileri ile OneDrive eşitleme istemcisinde sessiz oturum kullanıcıları ile birlikte kullanılır. Belirtilen eşikten daha büyük bir OneDrive'a sahip olan tüm kullanıcıdan dosyaları indirmeden önce eşitlemek istedikleri klasörleri seçmeleri istenir.  Örneğimizde, "1111-2222-3333-4444" Office 365 kiracı kimliği ve 0005000 5 GB bir eşik ayarlar.

```powershell
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\OneDrive"
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\OneDrive\DiskSpaceCheckThresholdMB"
New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\OneDrive\DiskSpaceCheckThresholdMB"
    -Name "1111-2222-3333-4444" -Value "0005000" -PropertyType DWORD
```

## <a name="install-and-configure-microsoft-office-365"></a>Microsoft Office 365'i yükleme ve yapılandırma

### <a name="install-microsoft-office-365"></a>Microsoft Office 365'i yükleme

Şablon makinenizin Office'e ihtiyacı varsa, [Office Dağıtım Aracı (ODT)](https://www.microsoft.com/download/details.aspx?id=49117 )aracılığıyla Office'in yüklenmesini öneririz. Hangi mimarinin, Office'ten hangi özelliklere ihtiyacınız olacağını ve ne sıklıkta güncellediğinizi seçmek için [Office 365 İstemci Yapılandırma Hizmeti'ni](https://config.office.com/) kullanarak yeniden kullanılabilir bir yapılandırma dosyası oluşturmanız gerekir.

1. Office [365 İstemci Yapılandırma Hizmeti'ne](https://config.office.com/) gidin ve kendi yapılandırma dosyanızı indirin.
2. [Office Dağıtım Aracını](https://www.microsoft.com/download/details.aspx?id=49117)İndir.  İndirilen dosya `setup.exe`.
3. Office `setup.exe /download configuration.xml` bileşenlerini indirmek için çalıştırın.
4. Office `setup.exe /configure configuration.xml` bileşenlerini yüklemek için çalıştırın.

### <a name="change-the-microsoft-office-365-update-channel"></a>Microsoft Office 365 güncelleştirme kanalını değiştirme

Office Yapılandırma Aracı'nı kullanarak Office'in güncelleştirmeleri ne sıklıkta aldığını ayarlayabilirsiniz. Ancak, Office'in yüklemeden sonra güncelleştirmeleri alma sıklıkla ne sıklıkta aldığını değiştirmeniz gerekiyorsa, güncelleştirme kanalı NıN URL'sini değiştirebilirsiniz. Kanal URL adreslerini güncelleştir, [kuruluşunuzdaki aygıtlar için Office 365 ProPlus güncelleme kanalını değiştir'de](https://docs.microsoft.com/deployoffice/change-update-channels)bulunabilir. Aşağıdaki örnekte, Aylık Güncelleştirme Kanalı'nı kullanacak Office 365'in nasıl ayarlanış edilebildiğini gösterilmektedir.

```powershell
# Update to the Office 365 Monthly Channel
Set-ItemProperty
    -Path "HKLM:\SOFTWARE\Microsoft\Office\ClickToRun\Configuration\CDNBaseUrl"
    -Name "CDNBaseUrl"
    -Value "http://officecdn.microsoft.com/pr/492350f6-3a01-4f97-b9c0-c7c6ddf67d60"
```

## <a name="install-and-configure-updates"></a>Güncelleştirmeleri yükleme ve yapılandırma

### <a name="install-the-latest-windows-updates"></a>En son Windows Güncelleştirmelerini Yükleme

Şablon VM'yi yayımlamadan önce şablon makinesine güvenlik amacıyla en son Microsoft güncelleştirmelerini yüklemenizi öneririz.  Ayrıca, güncelleştirmeler beklenmeyen zamanlarda çalıştırıldığında öğrencilerin çalışmalarında kesintiye uğramalarını da önler.

1. Başlat Menüsünden **Ayarları** Başlat
2. Güvenlik & **Güncelleştir'e** tıklayın
3. **Güncelleştirmeleri Denetle'yi** tıklatın
4. Güncelleştirmeler karşıdan yüklenir ve yüklenir.

Şablon makinesini güncellemek için PowerShell'i de kullanabilirsiniz.

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Confirm
Install-Module PSWindowsUpdate -Confirm
Install-WindowsUpdate -MicrosoftUpdate
Set-ExecutionPolicy default -Force
```

>[!NOTE]
>Bazı güncelleştirmeler makinenin yeniden başlatılmasını gerektirebilir.  Yeniden başlatma gerekirse istenirsiniz.

### <a name="install-the-latest-updates-for-microsoft-store-apps"></a>Microsoft Mağazası uygulamaları için en son güncelleştirmeleri yükleme

Tüm Microsoft Mağazası uygulamalarının en son sürümlerine güncellenmenizi öneririz.  Microsoft Mağazası'ndaki uygulamaları el ile güncelleştirmek için gereken yönergeler aşağıda verilmiştir.  

1. Microsoft Mağazası uygulamasını **başlatın.**
2. Uygulamanın üst köşesindeki kullanıcı fotoğrafının yanındaki elipsi (...) tıklatın.
3. Açılan menüden **İndir** ve güncelleştirmeleri seçin.
4. **Güncelleştir düğmesini al** düğmesini tıklatın.

Zaten yüklenmiş olan Microsoft Mağazası uygulamalarını güncelleştirmek için PowerShell'i de kullanabilirsiniz.

```powershell
(Get-WmiObject -Namespace "root\cimv2\mdm\dmmap" -Class "MDM_EnterpriseModernAppManagement_AppManagement01").UpdateScanMethod()
```

### <a name="stop-automatic-windows-updates"></a>Otomatik Windows Güncelleştirmelerini Durdur

Windows'u en son sürüme güncelledikten sonra, Windows Güncelleştirmeleri'ni durdurmayı düşünebilirsiniz.  Otomatik güncelleştirmeler zamanlanan sınıf saatini etkileyebilir.  Kursanız daha uzun sürüyorsa, öğrencilerden güncelleştirmeleri el ile kontrol etmelerini veya planlanan ders saatleri dışında bir süre için otomatik güncelleştirmeleri ayarlamalarını istemeyi düşünün.  Windows Update için özelleştirme seçenekleri hakkında daha fazla bilgi için [ek Windows Update ayarlarını yönet'e](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings)bakın.

Otomatik Windows Güncelleştirmeleri aşağıdaki PowerShell komut dosyası kullanılarak durdurulabilir.

```powershell
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\AU"
New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\AU"
    -Name "NoAutoUpdate" -Value "1" -PropertyType DWORD
```

## <a name="install-foreign-language-packs"></a>Yabancı dil paketleri yükleme

Sanal makineye ek dillerin yüklenmesi gerekiyorsa, bunları Microsoft Mağazası üzerinden ekleyebilirsiniz.

1. Microsoft Store'u başlatın
2. "Dil paketi" arama
3. Yüklemek için dil seçin

VM şablonuna zaten giriş yaptıysanız, doğrudan uygun ayarlar sayfasına gitmek için ["Dil paketini yükle" kısayolu](ms-settings:regionlanguage?activationSource=SMC-IA-4027670) kullanın.

## <a name="remove-unneeded-built-in-apps"></a>Gereksiz yerleşik uygulamaları kaldırma

Windows 10, özel sınıfınız için gerekli olmayabilen birçok yerleşik uygulamayla birlikte gelir. Öğrenciler için makine görüntüsünü basitleştirmek için şablon makinenizden bazı uygulamaları kaldırmak isteyebilirsiniz.  Yüklü uygulamaların listesini görmek için PowerShell `Get-AppxPackage` cmdlet'i kullanın.  Aşağıdaki örnek, kaldırılabilen tüm yüklü uygulamaları gösterir.

```powershell
Get-AppxPackage | Where {$_.NonRemovable -eq $false} | select Name
```

Bir uygulamayı kaldırmak için Remove-Appx cmdlet'i kullanın.  Aşağıdaki örnek, XBox ile ilgili her şeyin nasıl kaldırılır olduğunu gösterir.

```powershell
Get-AppxPackage -Name *xbox* | foreach { if (-not $_.NonRemovable) { Remove-AppxPackage $_} }
```

## <a name="install-common-teaching-related-applications"></a>Öğretimle ilgili yaygın uygulamaları yükleme

Windows Mağazası uygulaması üzerinden öğretim için yaygın olarak kullanılan diğer uygulamaları yükleyin. Öneriler [Microsoft Whiteboard uygulaması](https://www.microsoft.com/store/productId/9MSPC6MP8FM4)gibi uygulamalar dahil, [Microsoft Teams](https://www.microsoft.com/store/productId/9MSPC6MP8FM4), ve Minecraft [Education Edition](https://education.minecraft.net/). Bu uygulamalar, Windows Mağazası veya kendi web siteleri aracılığıyla vm şablonu üzerinden el ile yüklenmelidir.

## <a name="conclusion"></a>Sonuç

Bu makalede, etkili bir sınıf için Windows şablonu VM hazırlamak için isteğe bağlı adımlar göstermiştir.  Adımlar arasında OneDrive'ı yükleme ve Office 365'i yükleme, Windows güncelleştirmelerini yükleme ve Microsoft Mağazası uygulamaları için güncelleştirmeleri yükleme yer alıyor.  Güncelleştirmeleri sınıfınız için en uygun zamanlamaya nasıl ayarlayabileceğimizi de tartıştık.  

## <a name="next-steps"></a>Sonraki adımlar
Maliyetleri yönetmeye yardımcı olmak için Windows kapatma davranışını nasıl denetleyeceklerine ilişkin makaleye bakın: [Windows kapatma davranışını denetleme kılavuzu](how-to-windows-shutdown.md)
