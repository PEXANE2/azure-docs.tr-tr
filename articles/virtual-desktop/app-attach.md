---
title: Windows Sanal Masaüstü MSIX uygulaması eklemek - Azure
description: Windows Sanal Masaüstü için MSIX uygulaması nasıl ayarlanır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/14/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 20a82cbd7de4b5678648bac19ab9b59bf557b0ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128325"
---
# <a name="set-up-msix-app-attach"></a>MSIX uygulama iliştirmeyi ayarlama

> [!IMPORTANT]
> MSIX uygulama eklemek şu anda genel önizleme de.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Bu konu, Windows Sanal Masaüstü ortamında MSIX uygulaması eklemeyi nasıl ayarlayaceksiniz size yol gösterecektir.

## <a name="requirements"></a>Gereksinimler

Başlamadan önce, MSIX uygulaması eklemek yapılandırmak için gerekenler şunlardır:

- MSIX uygulaması api'leri için destek ile Windows 10 sürümünü elde etmek için Windows Insider portalına erişim.
- Çalışan bir Windows Sanal Masaüstü dağıtımı. Daha fazla bilgi için windows [sanal masaüstünde kiracı oluşturma](tenant-setup-azure-active-directory.md)'ya bakın.
- MSIX paketleme aracı
- MSIX paketinin depolanacağı Windows Sanal Masaüstü dağıtımınızda bir ağ paylaşımı

## <a name="get-the-os-image"></a>İşletim sistemi görüntüsünü alın

İlk olarak, MSIX uygulaması için kullanacağınız işletim sistemi görüntüsünü almanız gerekir. İşletim sistemi görüntüsünü almak için:

1. Windows [Insider portalını](https://www.microsoft.com/software-download/windowsinsiderpreviewadvanced?wa=wsignin1.0) açın ve oturum açın.

     >[!NOTE]
     >Windows Insider portalına erişmek için Windows Insider programının üyesi olmalısınız. Windows Insider programı hakkında daha fazla bilgi edinmek için [Windows Insider belgelerimize](/windows-insider/at-home/)göz atın.

2. **Sürümü Seç** bölümüne gidin ve **Windows 10 Insider Preview Enterprise (FAST) - Build 19035** veya daha sonra'yı seçin.

3. **Onaylamak'ı**seçin, ardından kullanmak istediğiniz dili seçin ve sonra yeniden **Onayla'yı** seçin.
    
     >[!NOTE]
     >Şu anda, İngilizce özelliği ile test edilmiştir tek dildir. Diğer dilleri seçebilirsiniz, ancak bunlar beklendiği gibi görüntülemeyebilir.
    
4. İndirme bağlantısı oluşturulduğunda, **64 bit İndir'i** seçin ve yerel sabit diskinize kaydedin.

## <a name="prepare-the-vhd-image-for-azure"></a>Azure için VHD görüntüsünü hazırlayın 

Başlamadan önce, bir ana VHD görüntü oluşturmanız gerekir. Henüz ana VHD görüntünüzü oluşturmadıysanız, [hazırlayın ve bir ana VHD görüntüsünü özelleştirin](set-up-customize-master-image.md) ve talimatları izleyin. 

Ana VHD resminizi oluşturduktan sonra, MSIX uygulama ekleme uygulamaları için otomatik güncelleştirmeleri devre dışı bmelisiniz. Otomatik güncelleştirmeleri devre dışı kalmanız için, aşağıdaki komutları yükseltilmiş bir komut isteminde çalıştırmanız gerekir:

```cmd
rem Disable Store auto update:

reg add HKLM\Software\Policies\Microsoft\WindowsStore /v AutoDownload /t REG_DWORD /d 0 /f
Schtasks /Change /Tn "\Microsoft\Windows\WindowsUpdate\Automatic app update" /Disable
Schtasks /Change /Tn "\Microsoft\Windows\WindowsUpdate\Scheduled Start" /Disable

rem Disable Content Delivery auto download apps that they want to promote to users:

reg add HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v PreInstalledAppsEnabled /t REG_DWORD /d 0 /f

reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\Debug /v ContentDeliveryAllowedOverride /t REG_DWORD /d 0x2 /f

rem Disable Windows Update:

sc config wuauserv start=disabled
```

Ardından, Azure için VM VHD'yi hazırlayın ve ortaya çıkan VHD diskini Azure'a yükleyin. Daha fazla bilgi edinmek için ana [VHD görüntü hazırlayın ve özelleştirin.](set-up-customize-master-image.md)

VHD'yi Azure'a yükledikten sonra, Azure Marketi öğreticisini [kullanarak ana bilgisayar havuzu oluştur'daki](create-host-pools-azure-marketplace.md) yönergeleri izleyerek bu yeni resmi temel alan bir ana bilgisayar havuzu oluşturun.

## <a name="prepare-the-application-for-msix-app-attach"></a>MSIX uygulama eklemek için uygulama hazırlayın 

Zaten bir MSIX paketiniz varsa, [Windows Sanal Masaüstü altyapısını Yapılandırmak](#configure-windows-virtual-desktop-infrastructure)için ileri ye geçin. Eski uygulamaları test etmek istiyorsanız, eski uygulamayı Bir MSIX paketine dönüştürmek için [VM'deki bir masaüstü yükleyicisinden MSIX paketi oluştur'daki](/windows/msix/packaging-tool/create-app-package-msi-vm/) yönergeleri izleyin.

## <a name="generate-a-vhd-or-vhdx-package-for-msix"></a>MSIX için bir VHD veya VHDX paketi oluşturun

Paketler performansı optimize etmek için VHD veya VHDX formatındadır. MSIX, VHD veya VHDX paketlerinin düzgün çalışmasını gerektirir.

MSIX için bir VHD veya VHDX paketi oluşturmak için:

1. [msixmgr aracını indirin](https://aka.ms/msixmgr) ve .zip klasörünü oturum ana bilgisayar VM içindeki bir klasöre kaydedin.

2. Msixmgr aracı .zip klasörünü açın.

3. Kaynak MSIX paketini, msixmgr aracının fermuarını açtığınızda aynı klasöre koyun.

4. Bir VHD oluşturmak için PowerShell'de aşağıdaki cmdlet'i çalıştırın:

    ```powershell
    New-VHD -SizeBytes <size>MB -Path c:\temp\<name>.vhd -Dynamic -Confirm:$false
    ```

    >[!NOTE]
    >VHD boyutunun genişletilmiş MSIX'yi tutacak kadar büyük olduğundan emin olun.*

5. Yeni oluşturulan VHD'yi monte etmek için aşağıdaki cmdlet'i çalıştırın:

    ```powershell
    $vhdObject = Mount-VHD c:\temp\<name>.vhd -Passthru
    ```

6. VHD'yi başlatmak için bu cmdlet'i çalıştırın:

    ```powershell
    $disk = Initialize-Disk -Passthru -Number $vhdObject.Number
    ```

7. Yeni bir bölüm oluşturmak için bu cmdlet çalıştırın:

    ```powershell
    $partition = New-Partition -AssignDriveLetter -UseMaximumSize -DiskNumber $disk.Number
    ```

8. Bölümü biçimlendirmek için bu cmdlet'i çalıştırın:

    ```powershell
    Format-Volume -FileSystem NTFS -Confirm:$false -DriveLetter $partition.DriveLetter -Force
    ```

9. Monte edilen VHD'de bir üst klasör oluşturun. MSIX uygulaması eklemek bir üst klasör gerektirdiğinden bu adım zorunludur. Ana klasörü istediğiniz gibi adlandırabilirsiniz.

### <a name="expand-msix"></a>MSIX'i Genişlet

Bundan sonra, msix görüntü "genişletmek" açmak gerekir. MSIX görüntüsünü açmak için:

1. Yönetici olarak bir komut istemi açın ve msixmgr aracını indirip fermuarını açtığınız klasöre gidin.

2. MSIX'yi bir önceki bölümde oluşturduğunuz ve monte ettiğiniz VHD'ye açmak için aşağıdaki cmdlet'i çalıştırın.

    ```powershell
    msixmgr.exe -Unpack -packagePath <package>.msix -destination "f:\<name of folder you created earlier>" -applyacls
    ```

    Ambalajı açma yapıldıktan sonra aşağıdaki ileti görünmelidir:

    `Successfully unpacked and applied ACLs for package: <package name>.msix`

    >[!NOTE]
    > Ağınızdaki Microsoft İş Mağazası'ndan (veya Eğitim için) paketleri kullanıyorsanız veya internete bağlı olmayan cihazlarda, paketi niçin Mağaza'dan almanız ve uygulamayı başarılı bir şekilde çalıştırmak için yüklemeniz gerekir. Bkz. [Paketleri çevrimdışı kullan.](#use-packages-offline)

3. Monte edilen VHD'ye gidin ve uygulama klasörünü açın ve paket içeriğinin mevcut olduğunu onaylayın.

4. VHD'yi çıkarın.

## <a name="configure-windows-virtual-desktop-infrastructure"></a>Windows Sanal Masaüstü altyapılarını yapılandırma

Tasarım gereği, vhd'ler salt okunur modunda eklenirken, tek bir MSIX genişletilmiş paket (önceki bölümde oluşturduğunuz VHD) birden çok oturum ana bilgisayarı VM arasında paylaşılabilir.

Başlamadan önce ağ payınızın bu gereksinimleri karşıladığından emin olun:

- Paylaşım SMB uyumludur.
- Oturum ana bilgisayar havuzunun bir parçası olan VM'lerin payına NTFS izinleri vardır.

### <a name="set-up-an-msix-app-attach-share"></a>Bir MSIX uygulama ekleme paylaşımı ayarlama 

Windows Sanal Masaüstü ortamınızda bir ağ paylaşımı oluşturun ve paketi oraya taşıyın.

>[!NOTE]
> MSIX ağ paylaşımları oluşturmak için en iyi yöntem, ağ payını NTFS salt okuma izinleriyle ayarlamaktır.

## <a name="install-certificates"></a>Sertifikaları yükleme

Uygulamanız herkese açık olmayan veya kendi imzası olan bir sertifika kullanıyorsa, şu şekilde yükleyebilirsiniz:

1. Pakete sağ tıklayın ve **Özellikler'i**seçin.
2. Görünen pencerede **Dijital imzalar** sekmesini seçin. Aşağıdaki resimde gösterildiği gibi, sekmede listede yalnızca bir öğe olmalıdır. Öğeyi vurgulamak için bu öğeyi seçin, ardından **Ayrıntılar'ı**seçin.
3. Dijital imza ayrıntıları penceresi göründüğünde, **Genel** sekmesini seçin ve **ardından sertifikayı yükle'yi**seçin.
4. Yükleyici açıldığında, depolama konumunuz olarak **yerel makineyi** seçin ve **ardından İleri'yi**seçin.
5. Yükleyici, uygulamanın cihazınızda değişiklik yapmasına izin vermek isteyip istemediğinizi sorarsa, **Evet'i**seçin.
6. **Aşağıdaki mağazadaki tüm sertifikaları yerleştir'i**seçin, ardından **Gözat'ı**seçin.
7. Sertifika deposu penceresi seçgörüntülenirken, **Güvenilen kişileri**seçin ve **ardından Tamam'ı**seçin.
8. **Bitiş'i**seçin.

## <a name="prepare-powershell-scripts-for-msix-app-attach"></a>MSIX uygulaması eklemek için PowerShell komut dosyaları hazırlayın

MSIX uygulama eki aşağıdaki sırada yapılması gereken dört ayrı aşamaya sahiptir:

1. Aşama
2. Kaydettir
3. Kayıt silme
4. Sahne Silme

Her aşama bir PowerShell komut dosyası oluşturur. Her aşama için örnek komut dosyaları [burada](https://github.com/Azure/RDS-Templates/tree/master/msix-app-attach)mevcuttur.

### <a name="stage-the-powershell-script"></a>PowerShell komut dosyasını sahnele

PowerShell komut dosyalarını güncellemeden önce VHD'de ses düzeyi GUID'ine sahip olduğundan emin olun. Cilt GUID almak için:

1.  Komut dosyasını çalıştıracağınız VHD'nin VM içinde bulunduğu ağ paylaşımını açın.

2.  VHD'ye sağ tıklayın ve **Mount'u**seçin. Bu bir sürücü harfi VHD monte edecektir.

3.  VHD'yi monte ettikten sonra **Dosya Gezgini** penceresi açılır. Üst klasörü yakalayın ve **$parentFolder** değişkenini güncelleştirin

    >[!NOTE]
    >Bir üst klasör görmüyorsanız, Bu MSIX düzgün genişletilmediği anlamına gelir. Önceki bölümü yeniden yapın ve yeniden deneyin.

4.  Üst klasörü açın. Doğru şekilde genişletilirse, paketle aynı ada sahip bir klasör görürsünüz. Bu klasörün adı eşleşecek **şekilde $packageName** değişkenini güncelleştirin.

    Örneğin, `VSCodeUserSetup-x64-1.38.1_1.38.1.0_x64__8wekyb3d8bbwe`.

5.  Komut istemini açın ve **mountvol**girin. Bu komut, birimlerin ve GUID'lerinin bir listesini görüntüler. Sürücü harfinin VHD'nizi adım 2'ye monte ettiğiniz sürücüyle eşleştiği ses düzeyinin GUID'ini kopyalayın.

    Örneğin, mountvol komutu için bu örnek çıktı, C sürücü için VHD monte ederseniz, `C:\`yukarıdaki değeri kopyalamak isteyeceksiniz:

    ```cmd
    Possible values for VolumeName along with current mount points are:

    \\?\Volume{a12b3456-0000-0000-0000-10000000000}\
    *** NO MOUNT POINTS ***

    \\?\Volume{c78d9012-0000-0000-0000-20000000000}\
        E:\

    \\?\Volume{d34e5678-0000-0000-0000-30000000000}\
        C:\

    ```


6.  **$volumeGuid** değişkenini az önce kopyaladığınız GUID hacmiyle güncelleştirin.

7. Bir Admin PowerShell istemi açın ve ortamınız için geçerli değişkenlerle aşağıdaki PowerShell komut dosyasını güncelleyin.

    ```powershell
    #MSIX app attach staging sample

    #region variables

    $vhdSrc="<path to vhd>"

    $packageName = "<package name>"

    $parentFolder = "<package parent folder>"

    $parentFolder = "\" + $parentFolder + "\"

    $volumeGuid = "<vol guid>"

    $msixJunction = "C:\temp\AppAttach\"

    #endregion

    #region mountvhd

    try

    {

    Mount-Diskimage -ImagePath $vhdSrc -NoDriveLetter -Access ReadOnly

    Write-Host ("Mounting of " + $vhdSrc + " was completed!") -BackgroundColor Green

    }

    catch

    {

    Write-Host ("Mounting of " + $vhdSrc + " has failed!") -BackgroundColor Red

    }

    #endregion

    #region makelink

    $msixDest = "\\?\Volume{" + $volumeGuid + "}\"

    if (!(Test-Path $msixJunction))

    {

    md $msixJunction

    }

    $msixJunction = $msixJunction + $packageName

    cmd.exe /c mklink /j $msixJunction $msixDest

    #endregion

    #region stage

    [Windows.Management.Deployment.PackageManager,Windows.Management.Deployment,ContentType=WindowsRuntime]
    | Out-Null

    Add-Type -AssemblyName System.Runtime.WindowsRuntime

    $asTask = ([System.WindowsRuntimeSystemExtensions].GetMethods() | Where {
    $_.ToString() -eq 'System.Threading.Tasks.Task`1[TResult]
    AsTask[TResult,TProgress](Windows.Foundation.IAsyncOperationWithProgress`2[TResult,TProgress])'})[0]

    $asTaskAsyncOperation =
    $asTask.MakeGenericMethod([Windows.Management.Deployment.DeploymentResult],
    [Windows.Management.Deployment.DeploymentProgress])

    $packageManager = [Windows.Management.Deployment.PackageManager]::new()

    $path = $msixJunction + $parentFolder + $packageName # needed if we do the pbisigned.vhd

    $path = ([System.Uri]$path).AbsoluteUri

    $asyncOperation = $packageManager.StagePackageAsync($path, $null, "StageInPlace")

    $task = $asTaskAsyncOperation.Invoke($null, @($asyncOperation))

    $task

    #endregion
    ```

### <a name="register-powershell-script"></a>PowerShell komut dosyasını kaydedin

Kayıt komut dosyasını çalıştırmak için, ortamınız için geçerli değerlerle değiştirilen yer tutucu değerleriyle aşağıdaki PowerShell cmdlets'i çalıştırın.

```powershell
#MSIX app attach registration sample

#region variables

$packageName = "<package name>"

$path = "C:\Program Files\WindowsApps\" + $packageName + "\AppxManifest.xml"

#endregion

#region register

Add-AppxPackage -Path $path -DisableDevelopmentMode -Register

#endregion
```

### <a name="deregister-powershell-script"></a>PowerShell komut dosyasının kaydını silme

Bu komut dosyası için, **$packageName** için yer tutucuyu test ettiğiniz paketin adı ile değiştirin.

```powershell
#MSIX app attach deregistration sample

#region variables

$packageName = "<package name>"

#endregion

#region deregister

Remove-AppxPackage -PreserveRoamableApplicationData $packageName

#endregion
```

### <a name="destage-powershell-script"></a>Destage PowerShell komut dosyası

Bu komut dosyası için, **$packageName** için yer tutucuyu test ettiğiniz paketin adı ile değiştirin.

```powershell
#MSIX app attach de staging sample

#region variables

$packageName = "<package name>"

$msixJunction = "C:\temp\AppAttach\"

#endregion

#region deregister

Remove-AppxPackage -AllUsers -Package $packageName

cd $msixJunction

rmdir $packageName -Force -Verbose

#endregion
```

## <a name="set-up-simulation-scripts-for-the-msix-app-attach-agent"></a>MSIX uygulama ekle aracısı için simülasyon komut dosyaları ayarlama

Komut dosyalarını oluşturduktan sonra, kullanıcılar bunları el ile çalıştırabilir veya başlatma, oturum açma, oturum kapatma ve kapatma komut dosyaları olarak otomatik olarak çalışacak şekilde ayarlayabilir. Bu tür komut dosyaları hakkında daha fazla bilgi edinmek için [bkz.](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn789196(v=ws.11)/)

Bu otomatik komut dosyalarının her biri uygulamanın bir aşamasını çalıştırAn komut dosyaları:

- Başlangıç komut dosyası sahne komut dosyasını çalıştırın.
- Oturum açma komut dosyası kayıt komut dosyasını çalıştırZ.
- Oturum kapatma komut dosyası, kayıt silme komut dosyasını çalıştırıyor.
- Kapatma komut dosyası, sahne yiyle'in devre-son komut dosyasını çalıştırın.

## <a name="use-packages-offline"></a>Paketleri çevrimdışı kullanma

Ağınızdaki Microsoft İş [Mağazası'ndan](https://businessstore.microsoft.com/) veya [Microsoft Eğitim Mağazası'ndan](https://educationstore.microsoft.com/) veya internete bağlı olmayan cihazlarda paketleri kullanıyorsanız, uygulamayı başarılı bir şekilde çalıştırmak için paket lisanslarını Microsoft Mağazası'ndan almanız ve cihazınıza yüklemeniz gerekir. Cihazınız çevrimiçiyse ve Microsoft Store for Business'a bağlanabiliyorsa, gerekli lisansların otomatik olarak karşıdan yüklenir, ancak çevrimdışıysanız, lisansları el ile ayarlamanız gerekir. 

Lisans dosyalarını yüklemek için, WMI Bridge Provider'daki MDM_EnterpriseModernAppManagement_StoreLicenses02_01 sınıfı çağıran bir PowerShell komut dosyası kullanmanız gerekir.  

Lisansları çevrimdışı kullanım için şu şekilde ayarlayabilirsiniz: 

1. Uygulama paketini, lisansları ve gerekli çerçeveleri Microsoft İş Mağazası'ndan indirin. Hem kodlanmış hem de kodlanmamış lisans dosyalarına ihtiyacınız vardır. Detaylı indirme [talimatlarına buradan](/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app)ulabilirsiniz.
2. Adım 3 için komut dosyasında aşağıdaki değişkenleri güncelleştirin:
      1. `$contentID`Kodlanmamış lisans dosyasındaki ContentID değeridir (.xml). Lisans dosyasını seçtiğiniz bir metin düzenleyicisinde açabilirsiniz.
      2. `$licenseBlob`Kodlanmış lisans dosyasındaki (.bin) lisans blob'u için tüm dizedir. Kodlanmış lisans dosyasını seçtiğiniz bir metin düzenleyicisinde açabilirsiniz. 
3. Aşağıdaki komut dosyasını Bir Yönetici PowerShell komut isteminden çalıştırın. Lisans yükleme gerçekleştirmek için iyi bir yer de bir Yönetici istemi çalıştırılması gereken [evreleme komut dosyasının](#stage-the-powershell-script) sonunda yer alır.

```powershell
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_EnterpriseModernAppManagement_StoreLicenses02_01"
$methodName = "AddLicenseMethod"
$parentID = "./Vendor/MSFT/EnterpriseModernAppManagement/AppLicenses/StoreLicenses"

#TODO - Update $contentID with the ContentID value from the unencoded license file (.xml)
$contentID = "{'ContentID'_in_unencoded_license_file}"

#TODO - Update $licenseBlob with the entire String in the encoded license file (.bin)
$licenseBlob = "{Entire_String_in_encoded_license_file}"

$session = New-CimSession 

#The final string passed into the AddLicenseMethod should be of the form <License Content="encoded license blob" />
$licenseString = '<License Content='+ '"' + $licenseBlob +'"' + ' />' 

$params = New-Object Microsoft.Management.Infrastructure.CimMethodParametersCollection
$param = [Microsoft.Management.Infrastructure.CimMethodParameter]::Create("param",$licenseString ,"String", "In")
$params.Add($param) 


try
{
   $instance = New-CimInstance -Namespace $namespaceName -ClassName $className -Property @{ParentID=$parentID;InstanceID=$contentID}
   $session.InvokeMethod($namespaceName, $instance, $methodName, $params)

}
catch [Exception]
{
     write-host $_ | out-string
}  
```

## <a name="next-steps"></a>Sonraki adımlar

Bu özellik şu anda desteklenmez, ancak [Windows Virtual Desktop TechCommunity'de](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop)topluluğa sorular sorabilirsiniz.

Windows [Sanal Masaüstü geri bildirim hub'ına](https://aka.ms/MRSFeedbackHub)geri bildirim de bırakabilir veya [MSIX uygulamasındaki MSIX uygulaması](https://aka.ms/msixappattachfeedback) ve paketleme aracı için geri bildirim bırakabilirsiniz geri bildirim merkezi ve [MSIX paketleme aracı geri bildirim merkezi.](https://aka.ms/msixtoolfeedback)
