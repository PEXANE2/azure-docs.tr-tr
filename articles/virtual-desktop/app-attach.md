---
title: Windows sanal masaüstü MALTı uygulama iliştirme-Azure
description: Windows sanal masaüstü için MSIX uygulama iliştirme 'yi ayarlama.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/14/2019
ms.author: helohr
ms.openlocfilehash: 8e8eec8af81832992a27206efcd7b7e7051a83b8
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772559"
---
# <a name="set-up-msix-app-attach"></a>MSIX uygulama iliştirmeyi ayarlama

> [!IMPORTANT]
> MSIX uygulama iliştirme Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu konu başlığı altında, bir Windows sanal masaüstü ortamında MSIX uygulama iliştirme 'yi ayarlama işleminde size yol gösterilir.

## <a name="requirements"></a>Gereksinimler

Başlamadan önce, MSIX uygulama iliştirme 'yi yapılandırmak için gerekenler şunlardır:

- MSIX uygulama iliştirme API 'Leri desteğiyle Windows 10 sürümünü edinmek için Windows Insider portalına erişim.
- Çalışan bir Windows sanal masaüstü dağıtımı. Bilgi için bkz. [Windows sanal masaüstü 'nde kiracı oluşturma](tenant-setup-azure-active-directory.md).
- MSIX paketleme aracı
- Windows sanal masaüstü dağıtımınızda MSIX paketinin depolanacağı bir ağ paylaşımıdır

## <a name="get-the-os-image"></a>İşletim sistemi görüntüsünü al

İlk olarak, MSIX uygulaması için kullanacağınız işletim sistemi görüntüsünü almanız gerekir. İşletim sistemi görüntüsünü almak için:

1. [Windows Insider portalını](https://www.microsoft.com/software-download/windowsinsiderpreviewadvanced?wa=wsignin1.0) açın ve oturum açın.

     >[!NOTE]
     >Windows Insider portalına erişmek için Windows Insider programının üyesi olmanız gerekir. Windows Insider programı hakkında daha fazla bilgi edinmek için [Windows Insider belgelerimize](https://docs.microsoft.com/windows-insider/at-home/)göz atın.

2. **Seç sürümü** bölümüne gidin ve **Windows 10 Insider PREVIEW Enterprise (Fast) – Build 19035** veya üzeri ' i seçin.

3. **Onayla**' yı seçin, ardından kullanmak istediğiniz dili seçin ve sonra yeniden **Onayla** ' yı seçin.
    
     >[!NOTE]
     >Şu anda, özelliği ile test edilen tek dil Ingilizce 'dir. Diğer dilleri seçebilirsiniz, ancak tasarlanan gibi görünmeyebilir.
    
4. İndirme bağlantısı oluşturulduğunda, **64 bit indirmeyi** seçin ve yerel sabit diskinize kaydedin.

## <a name="prepare-the-vhd-image-for-azure"></a>Azure için VHD görüntüsünü hazırlama 

Başlamadan önce, bir ana VHD görüntüsü oluşturmanız gerekir. Ana VHD görüntünüzü henüz oluşturmadıysanız, [ana VHD görüntüsünü hazırla ve özelleştirme](set-up-customize-master-image.md) bölümüne gidin ve yönergeleri izleyin. 

Ana VHD görüntünüzü oluşturduktan sonra, MSIX uygulama iliştirme uygulamaları için otomatik güncelleştirmeleri devre dışı bırakmanız gerekir. Otomatik güncelleştirmeleri devre dışı bırakmak için, yükseltilmiş bir komut isteminde aşağıdaki komutları çalıştırmanız gerekir:

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

Ardından, Azure için VM VHD 'yi hazırlayın ve elde edilen VHD diskini Azure 'a yükleyin. Daha fazla bilgi için bkz. [ana VHD görüntüsünü hazırlama ve özelleştirme](set-up-customize-master-image.md).

VHD 'yi Azure 'a yükledikten sonra [Azure Marketi öğreticisini kullanarak bir konak havuzu oluşturma](create-host-pools-azure-marketplace.md) bölümünde yer alan yönergeleri izleyerek bu yeni görüntüye dayalı bir konak havuzu oluşturun.

## <a name="prepare-the-application-for-msix-app-attach"></a>Uygulamayı MALTı uygulama iliştirme için hazırlama 

Zaten bir MSIX paketiniz varsa [Windows sanal masaüstü altyapısını yapılandırma](#configure-windows-virtual-desktop-infrastructure)bölümüne atlayın. Eski uygulamaları test etmek istiyorsanız, eski uygulamayı bir MSIX paketine dönüştürmek için [BIR sanal makinede bulunan bir masaüstü yükleyicisinden msix paketi oluşturma](https://docs.microsoft.com/windows/msix/packaging-tool/create-app-package-msi-vm) bölümündeki yönergeleri izleyin.

## <a name="generate-a-vhd-or-vhdx-package-for-msix"></a>MSIX için bir VHD veya VHDX paketi oluşturma

Paketler, performansı iyileştirmek için VHD veya VHDX biçimindedir. MSIX, VHD veya VHDX paketlerinin düzgün şekilde çalışmasını gerektirir.

MSIX için bir VHD veya VHDX paketi oluşturmak için:

1. [Msıxmgr aracını indirin](https://aka.ms/msixmgr) ve. zip klasörünü bir oturum ana bilgisayar VM 'si içindeki bir klasöre kaydedin.

2. Msıxmgr Tool. zip klasörünü sıkıştırmayı açın.

3. Kaynak MDıX paketini, msıxmgr aracının sıkıştırkonunkından aynı klasöre yerleştirin.

4. Bir VHD oluşturmak için PowerShell 'de aşağıdaki cmdlet 'i çalıştırın:

    ```powershell
    New-VHD -SizeBytes <size>MB -Path c:\temp\<name>.vhd -Dynamic -Confirm:$false
    ```

    >[!NOTE]
    >VHD boyutunun genişletilmiş MALTıYı tutabilecek kadar büyük olduğundan emin olun. *

5. Yeni oluşturulan VHD 'YI bağlamak için aşağıdaki cmdlet 'i çalıştırın:

    ```powershell
    $vhdObject = Mount-VHD c:\temp\<name>.vhd -Passthru
    ```

6. VHD 'YI başlatmak için bu cmdlet 'i çalıştırın:

    ```powershell
    $disk = Initialize-Disk -Passthru -Number $vhdObject.Number
    ```

7. Yeni bir bölüm oluşturmak için bu cmdlet 'i çalıştırın:

    ```powershell
    $partition = New-Partition -AssignDriveLetter -UseMaximumSize -DiskNumber $disk.Number
    ```

8. Bölümü biçimlendirmek için bu cmdlet 'i çalıştırın:

    ```powershell
    Format-Volume -FileSystem NTFS -Confirm:$false -DriveLetter $partition.DriveLetter -Force
    ```

9. Bağlı VHD üzerinde bir üst klasör oluşturun. Bu adım, MALTıLıK uygulama iliştirme bir üst klasör gerektirdiğinden zorunludur. Üst klasörü dilediğiniz gibi adlandırabilirsiniz.

### <a name="expand-msix"></a>MALTıYı Genişlet

Bundan sonra, bu görüntüyü genişleterek MSIX 'yi "genişletmeniz" gerekir. MSIX görüntüsünün paketini açmak için:

1. Yönetici olarak bir komut istemi açın ve msixmgr aracını indirdiğiniz ve sıkıştırkından geri yüklediğiniz klasöre gidin.

2. Oluşturduğunuz ve önceki bölümde bağladığınız VHD 'de MSIX 'yi açmak için aşağıdaki cmdlet 'i çalıştırın.

    ```powershell
    msixmgr.exe -Unpack -packagePath <package>.msix -destination "f:\<name of folder you created earlier>" -applyacls
    ```

    Aşağıdaki ileti, paketten sonra görünmelidir:

    `Successfully unpacked and applied ACLs for package: <package name>.msix`

    >[!NOTE]
    > Ağınız içinde Iş için Microsoft Store (veya eğitim) veya internet 'e bağlı olmayan cihazlarda paketler kullanıyorsanız, paket lisanslarını mağazadan edinmeniz ve uygulamayı başarılı bir şekilde çalıştırmak için yüklemeniz gerekir. Bkz. [paketleri çevrimdışı kullanma](#use-packages-offline).

3. Bağlı VHD 'ye gidin ve uygulama klasörünü açın ve paket içeriğinin mevcut olduğunu doğrulayın.

4. VHD'yi çıkarın.

## <a name="configure-windows-virtual-desktop-infrastructure"></a>Windows sanal masaüstü altyapısını yapılandırma

Tasarıma göre, VHD 'Ler salt okuma modunda eklendiği için, tek bir MSIX genişletilmiş paket (önceki bölümde oluşturduğunuz VHD) birden çok oturum ana makinesi arasında paylaşılabilir.

Başlamadan önce, ağ paylaşımınızın bu gereksinimleri karşıladığından emin olun:

- Paylaşımda SMB uyumlu.
- Oturum Ana bilgisayar havuzunun parçası olan sanal makinelerin, paylaşımda NTFS izinleri vardır.

### <a name="set-up-an-msix-app-attach-share"></a>MALTı uygulama iliştirme paylaşma ayarlama 

Windows sanal masaüstü ortamınızda bir ağ paylaşma oluşturun ve paketi buraya taşıyın.

>[!NOTE]
> MSIX ağ paylaşımları oluşturmak için en iyi yöntem, ağ paylaşımını NTFS salt okuma izinleriyle ayarlamaya yöneliktir.

## <a name="install-certificates"></a>Sertifikaları yükler

Uygulamanız ortak güvenilir olmayan ya da kendinden imzalı bir sertifika kullanıyorsa, bunu nasıl yükleyeceksiniz:

1. Pakete sağ tıklayın ve **Özellikler**' i seçin.
2. Görüntülenen pencerede **dijital imzalar** sekmesini seçin. Aşağıdaki görüntüde gösterildiği gibi sekmedeki listede yalnızca bir öğe olmalıdır. Öğeyi vurgulamak için bu öğeyi seçin, sonra **Ayrıntılar**' ı seçin.
3. Dijital imza ayrıntıları penceresi göründüğünde **genel** sekmesini seçin ve ardından **sertifikayı yükler**' i seçin.
4. Yükleyici açıldığında, depolama konumunuz olarak **yerel makine** ' yi seçin ve ardından **İleri**' yi seçin.
5. Yükleyici, uygulamanın cihazınızda değişiklik yapmasına izin vermek isteyip istemediğinizi isterse, **Evet**' i seçin.
6. **Tüm sertifikaları aşağıdaki depolama alanına yerleştir**' i seçin ve ardından da **Araştır**' ı seçin.
7. Sertifika deposu Seç penceresi göründüğünde, **Güvenilen Kişiler**' i seçin ve ardından **Tamam**' ı seçin.
8. **Son**’u seçin.

## <a name="prepare-powershell-scripts-for-msix-app-attach"></a>MSIX uygulama iliştirme için PowerShell betikleri hazırlama

MSIX uygulama iliştirme, aşağıdaki sırayla gerçekleştirilmesi gereken dört farklı aşamaya sahiptir:

1. Stage
2. Kaydol
3. Kaydını silmek
4. Gerçekleştirilen

Her aşama bir PowerShell betiği oluşturur. Her aşama için örnek betikler [burada](https://github.com/Azure/RDS-Templates/tree/master/msix-app-attach)bulunabilir.

### <a name="stage-the-powershell-script"></a>PowerShell betiğini hazırlama

PowerShell betiklerini güncelleştirmeden önce, VHD 'deki birimin birim GUID 'sine sahip olduğunuzdan emin olun. Birim GUID 'sini almak için:

1.  VHD 'nin betiği çalıştıracağınız VM 'nin içinde bulunduğu ağ payını açın.

2.  VHD 'ye sağ tıklayın ve **bağla**' yı seçin. Bu, VHD 'yi bir sürücü harfine bağlayacaktır.

3.  VHD 'yi takduktan sonra **Dosya Gezgini** penceresi açılır. Üst klasörü yakala ve **$parentFolder** değişkenini Güncelleştir

    >[!NOTE]
    >Üst klasör görmüyorsanız, MSIX 'nin doğru genişletilmediği anlamına gelir. Önceki bölümü yineleyin ve yeniden deneyin.

4.  Üst klasörü açın. Doğru şekilde genişletilmişse, paketiyle aynı ada sahip bir klasör görürsünüz. **$PackageName** değişkenini bu klasörün adıyla eşleşecek şekilde güncelleştirin.

    Örneğin, `VSCodeUserSetup-x64-1.38.1_1.38.1.0_x64__8wekyb3d8bbwe`.

5.  Bir komut istemi açın ve **mountvol**yazın. Bu komut, birimlerin ve GUID 'Lerinin bir listesini görüntüler. 2\. adımda sürücü harfinin VHD 'nizi bağladığınız sürücüyle eşleşen birimin GUID 'sini kopyalayın.

    Örneğin, mountvol komutuna yönelik bu örnek çıktıda, VHD 'nizi C sürücüsüne bağladıysanız, `C:\`yukarıdaki değeri kopyalamak isteyeceksiniz:

    ```cmd
    Possible values for VolumeName along with current mount points are:

    \\?\Volume{a12b3456-0000-0000-0000-10000000000}\
    *** NO MOUNT POINTS ***

    \\?\Volume{c78d9012-0000-0000-0000-20000000000}\
        E:\

    \\?\Volume{d34e5678-0000-0000-0000-30000000000}\
        C:\

    ```


6.  **$VolumeGuid** değişkenini yeni kopyaladığınız birim GUID 'si ile güncelleştirin.

7. Bir yönetici PowerShell istemi açın ve aşağıdaki PowerShell betiğini ortamınıza uygulanan değişkenlerle güncelleştirin.

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

### <a name="register-powershell-script"></a>PowerShell betiğini Kaydet

Kayıt betiğini çalıştırmak için aşağıdaki PowerShell cmdlet 'lerini ortamınıza uygulanan değerlerle birlikte yer tutucu değerleriyle birlikte çalıştırın.

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

### <a name="deregister-powershell-script"></a>PowerShell betiğini kaydını kaldırma

Bu komut dosyası için **$PackageName** yer tutucusunu, Sınadığınızı paketin adıyla değiştirin.

```powershell
#MSIX app attach deregistration sample

#region variables

$packageName = "<package name>"

#endregion

#region deregister

Remove-AppxPackage -PreserveRoamableApplicationData $packageName

#endregion
```

### <a name="destage-powershell-script"></a>PowerShell betiği önbellekten taşıma

Bu komut dosyası için **$PackageName** yer tutucusunu, Sınadığınızı paketin adıyla değiştirin.

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

## <a name="set-up-simulation-scripts-for-the-msix-app-attach-agent"></a>MSIX uygulama iliştirme Aracısı için benzetim betikleri ayarlama

Betikleri oluşturduktan sonra kullanıcılar el ile çalıştırabilir veya başlatma, oturum açma, oturum kapatma ve kapatma betikleri olarak otomatik olarak çalışacak şekilde ayarlayabilir. Bu komut dosyaları türleri hakkında daha fazla bilgi edinmek için bkz. [Grup İlkesi başlatma, kapatma, oturum açma ve oturum kapatma betikleri kullanma](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn789196(v=ws.11)).

Bu otomatik betiklerin her biri, uygulama komut dosyalarının bir aşamasını çalıştırır:

- Başlangıç betiği, aşama betiğini çalıştırır.
- Oturum açma betiği, kayıt betiğini çalıştırır.
- Oturumu kapatma betiği, kaydı kaldırma betiğini çalıştırır.
- Kapatılma betiği, taşıma betiğini çalıştırır.

## <a name="use-packages-offline"></a>Paketleri çevrimdışı kullanma

[İş için Microsoft Store](https://businessstore.microsoft.com/) veya ağınıza veya internet 'e bağlı olmayan cihazlarda [eğitim Microsoft Store](https://educationstore.microsoft.com/) kullanıyorsanız, uygulamayı başarılı bir şekilde çalıştırmak için Microsoft Store paket lisanslarını almanız ve cihazınıza yüklemeniz gerekir. Cihazınız çevrimiçiyse ve Iş için Microsoft Store bağlanabildiğinizde, gerekli lisanslar otomatik olarak indirilmelidir, ancak çevrimdışı çalışıyorsanız, lisansları el ile ayarlamanız gerekir. 

Lisans dosyalarını yüklemek için, WMI Köprüsü sağlayıcısında MDM_EnterpriseModernAppManagement_StoreLicenses02_01 sınıfını çağıran bir PowerShell betiği kullanmanız gerekir.  

Lisansları çevrimdışı kullanım için ayarlama: 

1. Iş için Microsoft Store uygulama paketini, lisansları ve gerekli çerçeveleri indirin. Kodlanmış ve kodlanmamış lisans dosyalarının her ikisi de gereklidir. Ayrıntılı indirme yönergeleri [burada](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app)bulunabilir.
2. 3\. adım için komut dosyasında aşağıdaki değişkenleri güncelleştirin:
      1. `$contentID`, kodlanmamış lisans dosyasından (. xml) ContentID değeridir. Lisans dosyasını dilediğiniz bir metin düzenleyicisinde açabilirsiniz.
      2. `$licenseBlob`, kodlanmış lisans dosyasında (. bin), lisans blobu için tüm dizedir. Kodlanmış lisans dosyasını dilediğiniz bir metin düzenleyicisinde açabilirsiniz. 
3. Yönetici PowerShell isteminden aşağıdaki betiği çalıştırın. Lisans yükleme işlemini gerçekleştirmek için iyi bir yer, bir yönetici isteminden çalıştırılması gereken [hazırlama betiğinin](#stage-the-powershell-script) sonunda yer alan bir yerdir.

```powershell
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_EnterpriseModernAppManagement_StoreLicenses02_01"
$methodName = "AddLicenseMethod"
$parentID = "./Vendor/MSFT/EnterpriseModernAppManagement/AppLicenses/StoreLicenses"

#TODO - Update $contentID with the ContentID value from the unencoded license file (.xml)
$contentID = "{‘ContentID’_in_unencoded_license_file}"

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

Bu özellik şu anda desteklenmemektedir, ancak [Windows sanal masaüstü techcommunity](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop)'de topluluğa soru sorabilirsiniz.

Windows sanal masaüstü [Geri Bildirim Hub 'ında](https://aka.ms/MRSFeedbackHub)Windows sanal masaüstü geri bildirimini de bırakabilir veya msix uygulama ve paketleme aracı hakkında geri bildirim ' i, [maltıya Ekle geri bildirim](https://aka.ms/msixappattachfeedback) hub 'ında ve [msix paketleme aracı geri bildirim hub 'ında](https://aka.ms/msixtoolfeedback)da bırakabilirsiniz.
