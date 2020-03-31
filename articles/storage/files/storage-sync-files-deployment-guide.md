---
title: Azure Dosya Eşitlemeyi Dağıtma | Microsoft Dokümanlar
description: Azure Dosya Eşitlemeyi'ni baştan sona nasıl dağıtılayabilirsiniz öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 07/19/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: f2c4e762ebf10a5ca2120c13a52750a7781d60b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268073"
---
# <a name="deploy-azure-file-sync"></a>Azure Dosya Eşitleme’yi dağıtma
Kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda merkezileştirmek ve şirket içi bir dosya sunucusunun esnekliğini, performansını ve uyumluluğunu korumak için Azure Dosya Eşitlemeyi'ni kullanın. Azure Dosya Eşitleme, Windows Server’ı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. SMB, NFS ve FTPS dahil olmak üzere verilerinize yerel olarak erişmek için Windows Server'da kullanılabilen tüm protokolleri kullanabilirsiniz. Dünya çapında ihtiyacınız olduğu kadar önbelleke sahip olabilirsiniz.

Bu makalede açıklanan adımları tamamlamadan önce [Azure Dosyaları dağıtımı için Planlama](storage-files-planning.md) ve Azure Dosya [Eşitleme dağıtımı planlamayı](storage-sync-files-planning.md) okumanızı öneririz.

## <a name="prerequisites"></a>Ön koşullar
* Azure Dosya Eşitle'yi dağıtmak istediğiniz aynı bölgede bir Azure dosya paylaşımı. Daha fazla bilgi için bkz:
    - Azure Dosya Eşitlemi için [bölge kullanılabilirliği.](storage-sync-files-planning.md#azure-file-sync-region-availability)
    - Dosya [paylaşımının](storage-how-to-create-file-share.md) nasıl oluşturulabildiğini adım adım açıklaması için dosya paylaşımı oluşturun.
* Azure Dosya Eşitlemi ile eşitlemek için windows server veya Windows Server kümesinin desteklenen en az bir örneği. Windows Server'ın desteklenen sürümleri hakkında daha fazla bilgi için Windows [Server ile Birlikte Çalışabilirlik](storage-sync-files-planning.md#windows-file-server-considerations)bölümüne bakın.
* Az PowerShell modülü PowerShell 5.1 veya PowerShell 6+ ile kullanılabilir. Windows olmayan sistemler de dahil olmak üzere desteklenen herhangi bir sistemde Azure Dosya Eşitlemesi için Az PowerShell modüllerini kullanabilirsiniz, ancak sunucu kayıt cmdlet'in her zaman kaydettiğiniz Windows Server örneğinde çalıştırılması gerekir (bu doğrudan veya PowerShell üzerinden yapılabilir remoting). Windows Server 2012 R2'de en az PowerShell 5.1 çalıştırdığınızı doğrulayabilirsiniz. \* **$PSVersionTable** nesnenin **PSVersion** özelliğinin değerine bakarak:

    ```powershell
    $PSVersionTable.PSVersion
    ```

    PSVersion değeriniz 5.1'den küçükse. \*, Windows Server 2012 R2'nin en yeni kurulumlarında olduğu gibi, Windows [Yönetim Çerçevesi (WMF) 5.1'i](https://www.microsoft.com/download/details.aspx?id=54616)indirip yükleyerek kolayca yükseltebilirsiniz. Windows Server 2012 R2 için indirmek ve yüklemek için uygun paket **\*\*\*\*\*\*\*Win8.1AndW2K12R2-KB -x64.msu**olduğunu. 

    PowerShell 6+ desteklenen herhangi bir sistemle kullanılabilir ve [GitHub sayfası](https://github.com/PowerShell/PowerShell#get-powershell)üzerinden indirilebilir. 

    > [!Important]  
    > Doğrudan PowerShell'den kaydolmak yerine Sunucu Kayıt UI'sini kullanmayı planlıyorsanız PowerShell 5.1'i kullanmanız gerekir.

* PowerShell 5.1'i kullanmayı seçtiyseniz, en az .NET 4.7.2'nin yüklü olduğundan emin olun. [.NET Framework sürümleri ve](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies) sisteminizdeki bağımlılıklar hakkında daha fazla bilgi edinin.

    > [!Important]  
    > Windows Server Core'a .NET 4.7.2+ yüklüyorsanız, `quiet` `norestart` bayraklarla yüklemeniz gerekir veya yükleme başarısız olur. Örneğin, .NET 4.8 yüklüyorsanız, komut aşağıdaki gibi görünür:
    > ```PowerShell
    > Start-Process -FilePath "ndp48-x86-x64-allos-enu.exe" -ArgumentList "/q /norestart" -Wait
    > ```

* Buradaki yönergeleri izleyerek kurulabilen Az PowerShell modülü: [Azure PowerShell'i yükleyin ve yapılandırın.](https://docs.microsoft.com/powershell/azure/install-Az-ps)
     
    > [!Note]  
    > Az PowerShell modüllerini yüklediğinizde Az.StorageSync modülü artık otomatik olarak yüklenir.

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Windows Server’ı Azure Dosya Eşitleme ile kullanmaya hazırlama
Failover Cluster'daki her sunucu düğümü de dahil olmak üzere Azure Dosya Eşitlemesi ile kullanmak istediğiniz her sunucu için **Internet Explorer Gelişmiş Güvenlik Yapılandırmasını**devre dışı kaldırın. Bu yalnızca ilk sunucu kaydı için gereklidir. Sunucu kaydedildikten sonra özelliği yeniden etkinleştirebilirsiniz.

# <a name="portal"></a>[Portal](#tab/azure-portal)
> [!Note]  
> Windows Server Core'da Azure Dosya Eşitlemi dağıtıyorsanız bu adımı atlayabilirsiniz.

1. Sunucu Yöneticisi'ni açın.
2. **Yerel Sunucu'ya**tıklayın:  
    ![Sunucu Yöneticisi kullanıcı arabiriminin sol tarafındaki "Yerel Sunucu"](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
3. **Özellikler** alt bölmesinde **IE Artırılmış Güvenlik Yapılandırması** bağlantısını seçin.  
    ![Sunucu Yöneticisi kullanıcı arabirimindeki "IE Artırılmış Güvenlik Yapılandırması" bölmesi](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
4. Internet **Explorer Gelişmiş Güvenlik Yapılandırması** iletişim kutusunda, **Yöneticiler** ve **Kullanıcılar**için **Kapalı'yı** seçin:  
    !["Kapalı" seçeneğinin işaretli olduğu Internet Explorer Artırılmış Güvenlik Yapılandırması açılır penceresi](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
Internet Explorer Gelişmiş Güvenlik Yapılandırmasını devre dışı kılabilir, yükseltilmiş bir PowerShell oturumundan aşağıdakileri uygulayın:

```powershell
$installType = (Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\").InstallationType

# This step is not required for Server Core
if ($installType -ne "Server Core") {
    # Disable Internet Explorer Enhanced Security Configuration 
    # for Administrators
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force
    
    # Disable Internet Explorer Enhanced Security Configuration 
    # for Users
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force
    
    # Force Internet Explorer closed, if open. This is required to fully apply the setting.
    # Save any work you have open in the IE browser. This will not affect other browsers,
    # including Microsoft Edge.
    Stop-Process -Name iexplore -ErrorAction SilentlyContinue
}
``` 

---

## <a name="deploy-the-storage-sync-service"></a>Depolama Eşitleme Hizmetini Dağıtma 
Azure Dosya Eşitlemi dağıtımı, bir **Depolama Eşitleme Hizmeti** kaynağının seçili aboneliğinizin kaynak grubuna yerleştirilmesiyle başlar. Gerektiğinde bunlardan çok azını sağlamanızı öneririz. Sunucularınız ile bu kaynak arasında bir güven ilişkisi oluşturursunuz ve sunucu yalnızca bir Depolama Eşitleme Hizmetine kaydedilebilir. Sonuç olarak, sunucu gruplarını ayırmak için gereken kadar depolama eşitleme hizmeti dağıtmanız önerilir. Farklı depolama eşitleme hizmetlerinden sunucuların birbirleriyle eşitlemeyeceğini unutmayın.

> [!Note]
> Depolama Eşitleme Hizmeti, dağıtıldığı abonelik ve kaynak grubundan erişim izinlerini devralır. Kimlere erişimi olduğunu dikkatlice kontrol etmenizi öneririz. Yazma erişimine sahip varlıklar, bu depolama eşitleme hizmetine kayıtlı sunuculardan yeni dosya kümelerini eşitlemeye başlayabilir ve verilerin kendileri tarafından erişilebilen Azure depolama alanına akmasına neden olabilir.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Depolama Eşitleme Hizmeti dağıtmak için [Azure portalına](https://portal.azure.com/)gidin , *kaynak oluştur'u* tıklatın ve ardından Azure Dosya Eşitlemeyi'ni arayın. Arama sonuçlarında **Azure Dosya Eşitle'sini**seçin ve ardından **Depolama Eşitlemeyi** Dağıt sekmesini açmak için **Oluştur'u** seçin.

Açılan bölmeye aşağıdaki bilgileri girin:

- **Adı**: Depolama Eşitleme Hizmeti için benzersiz bir ad (abonelik başına).
- **Abonelik**: Depolama Eşitleme Hizmetini oluşturmak istediğiniz abonelik. Kuruluşunuzun yapılandırma stratejisine bağlı olarak, bir veya daha fazla aboneye erişiminiz olabilir. Azure aboneliği, her bulut hizmeti (Azure Dosyaları gibi) için faturalandırma için en temel kapsayıcıdır.
- Kaynak grubu : Kaynak **grubu,** depolama hesabı veya Depolama Eşitleme Hizmeti gibi mantıksal bir Azure kaynakları grubudur. Yeni bir kaynak grubu oluşturabilir veya Azure Dosya Eşitlemi için varolan bir kaynak grubu kullanabilirsiniz. (Kaynak gruplarını, kuruluşunuz için belirli bir proje için İk kaynaklarını veya kaynaklarını gruplandırma gibi mantıksal olarak kaynak yalıtmak için kapsayıcı olarak kullanmanızı öneririz.)
- **Konum**: Azure Dosya Eşitlemeyi dağıtmak istediğiniz bölge. Bu listede yalnızca desteklenen bölgeler kullanılabilir.

İşi bittiğinde, Depolama Eşitleme Hizmetini dağıtmak için **Oluştur'u** seçin.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
`<Az_Region>`Değiştirin `<RG_Name>`, `<my_storage_sync_service>` ve kendi değerlerinizle, ardından bir Depolama Eşitleme Hizmeti oluşturmak ve dağıtmak için aşağıdaki komutları kullanın:

```powershell
$hostType = (Get-Host).Name

if ($installType -eq "Server Core" -or $hostType -eq "ServerRemoteHost") {
    Connect-AzAccount -UseDeviceAuthentication
}
else {
    Connect-AzAccount
}

# this variable holds the Azure region you want to deploy 
# Azure File Sync into
$region = '<Az_Region>'

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = @()
Get-AzLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the selected Azure Region or the region is mistyped.")
}

# the resource group to deploy the Storage Sync Service into
$resourceGroup = '<RG_Name>'

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = @()
Get-AzResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    New-AzResourceGroup -Name $resourceGroup -Location $region
}

$storageSyncName = "<my_storage_sync_service>"
$storageSync = New-AzStorageSyncService -ResourceGroupName $resourceGroup -Name $storageSyncName -Location $region
```

---

## <a name="install-the-azure-file-sync-agent"></a>Azure Dosya Eşitleme aracısını yükleme
Azure Dosya Eşitleme aracısı, Windows Server’ın bir Azure dosya paylaşımı ile eşitlenmesini sağlayan indirilebilir bir pakettir. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Aracıyı [Microsoft İndirme Merkezi'nden](https://go.microsoft.com/fwlink/?linkid=858257)indirebilirsiniz. İndirme işlemi tamamlandığında, Azure Dosya Eşitleme aracısı yüklemesini başlatmak için MSI paketini çift tıklatın.

> [!Important]  
> Azure Dosya Eşitlemeyi'ni Failover Kümesi ile kullanmayı planlıyorsanız, Azure Dosya Eşitleme aracısının kümedeki her düğüme yüklenmesi gerekir. Kümedeki her düğüm, Azure Dosya Eşitlemesi ile çalışmak için kaydedilmelidir.

Aşağıdakileri yapmanızı öneririz:
- Sorun giderme ve sunucu bakımını kolaylaştırmak için varsayılan yükleme yolunu (C:\Program Files\Azure\StorageSyncAgent) bırakın.
- Azure Dosya Eşitlemesini güncel tutmak için Microsoft Update'i etkinleştirin. Özellik güncelleştirmeleri ve düzeltmeler de dahil olmak üzere Azure Dosya Eşitleme aracısına yapılan tüm güncelleştirmeler Microsoft Update'ten oluşur. En son güncelleştirmeyi Azure Dosya Eşitle'ye yüklemenizi öneririz. Daha fazla bilgi için [Azure Dosya Eşitleme güncelleştirme ilkesine](storage-sync-files-planning.md#azure-file-sync-agent-update-policy)bakın.

Azure Dosya Eşitleme aracısı yüklemesi tamamlandığında, Sunucu Kayıt UI'sı otomatik olarak açılır. Kaydolmadan önce bir Depolama Eşitleme Hizmetiniz olmalıdır; Depolama Eşitleme Hizmeti oluşturma yla ilgili sonraki bölüme bakın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
İşletim sisteminiz için Azure Dosya Eşitleme aracısının uygun sürümünü indirmek ve sisteminize yüklemek için aşağıdaki PowerShell kodunu çalıştırın.

> [!Important]  
> Azure Dosya Eşitlemeyi'ni Failover Kümesi ile kullanmayı planlıyorsanız, Azure Dosya Eşitleme aracısının kümedeki her düğüme yüklenmesi gerekir. Kümedeki her düğüm, Azure Dosya Eşitlemesi ile çalışmak için kaydedilmelidir.

```powershell
# Gather the OS version
$osver = [System.Environment]::OSVersion.Version

# Download the appropriate version of the Azure File Sync agent for your OS.
if ($osver.Equals([System.Version]::new(10, 0, 17763, 0))) {
    Invoke-WebRequest `
        -Uri https://aka.ms/afs/agent/Server2019 `
        -OutFile "StorageSyncAgent.msi" 
} elseif ($osver.Equals([System.Version]::new(10, 0, 14393, 0))) {
    Invoke-WebRequest `
        -Uri https://aka.ms/afs/agent/Server2016 `
        -OutFile "StorageSyncAgent.msi" 
} elseif ($osver.Equals([System.Version]::new(6, 3, 9600, 0))) {
    Invoke-WebRequest `
        -Uri https://aka.ms/afs/agent/Server2012R2 `
        -OutFile "StorageSyncAgent.msi" 
} else {
    throw [System.PlatformNotSupportedException]::new("Azure File Sync is only supported on Windows Server 2012 R2, Windows Server 2016, and Windows Server 2019")
}

# Install the MSI. Start-Process is used to PowerShell blocks until the operation is complete.
# Note that the installer currently forces all PowerShell sessions closed - this is a known issue.
Start-Process -FilePath "StorageSyncAgent.msi" -ArgumentList "/quiet" -Wait

# Note that this cmdlet will need to be run in a new session based on the above comment.
# You may remove the temp folder containing the MSI and the EXE installer
Remove-Item -Path ".\StorageSyncAgent.msi" -Recurse -Force
```

---

## <a name="register-windows-server-with-storage-sync-service"></a>Windows Server’ı Depolama Eşitleme Hizmetine kaydetme
Windows Server’ı bir Depolama Eşitleme Hizmeti’ne kaydetmek, sunucunuz (veya kümeniz) ile Depolama Eşitleme Hizmeti arasında bir güven ilişkisi kurar. Bir sunucu yalnızca bir Depolama Eşitleme Hizmeti’ne kaydedilebilir ve aynı Depolama Eşitleme Hizmeti ile ilişkili diğer sunucular ve Azure dosya paylaşımları ile eşitlenebilir.

> [!Note]
> Sunucu kaydı, Depolama Eşitleme Hizmeti ile Windows Server'ınız arasında bir güven ilişkisi oluşturmak için Azure kimlik bilgilerinizi kullanır, ancak daha sonra sunucu kayıtlı kaldığı sürece geçerli olan kendi kimliğini oluşturur ve kullanır geçerli Paylaşılan Erişim İmza belirteci (Storage SAS) geçerlidir. Sunucu kayıt dışı olduğunda sunucuya yeni bir SAS belirteci verilemez ve böylece sunucunun Azure dosya paylaşımlarınıza erişme özelliği kaldırılabilir ve eşitlemeleri durdurur.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Sunucu Kayıt UI'si, Azure Dosya Eşitleme aracısını yüklendikten sonra otomatik olarak açılmalıdır. Açılmazsa, şu dosya konumundan kendiniz açabilirsiniz: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe. Sunucu Kayıt UI açıldığında, başlamak üzere **Oturum Aç'ı** seçin.

Oturum açtıktan sonra aşağıdaki bilgiler istenir:

![Sunucu Kaydı kullanıcı arabiriminin ekran görüntüsü](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure Aboneliği**: Depolama Eşitleme Hizmetini içeren abonelik [(bkz.](#deploy-the-storage-sync-service) 
- **Kaynak Grubu**: Depolama Eşitleme Hizmeti içeren kaynak grubu.
- **Depolama Eşitleme Hizmeti**: Kaydolmak istediğiniz Depolama Eşitleme Hizmetinin adı.

Uygun bilgileri seçtikten sonra, sunucu kaydını tamamlamak için **Kaydol'u** seçin. Kayıt işleminin bir parçası olarak bir kez daha oturum açmanız istenir.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
```powershell
$registeredServer = Register-AzStorageSyncServer -ParentObject $storageSync
```

---

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>Bir eşitleme grubu ve bir bulut uç noktası oluşturma
Eşitleme grubu, bir dosya kümesi için eşitleme topolojisini tanımlar. Bir eşitleme grubu içindeki uç noktalar, birbiriyle eşitlenmiş durumda tutulur. Eşitleme grubu, bir Azure dosya paylaşımı ve en az bir sunucu uç noktasını temsil eden bir bulut uç noktası içermelidir. Sunucu bitiş noktası, kayıtlı bir sunucudaki yolu temsil eder. Bir sunucu, birden çok eşitleme grubunda sunucu uç noktalarına sahip olabilir. İstediğiniz eşitleme topolojisini uygun şekilde açıklamak için istediğiniz kadar eşitleme grubu oluşturabilirsiniz.

Bulut bitiş noktası, Azure dosya paylaşımıiçin işaretçidir. Tüm sunucu uç noktaları bulut bitiş noktasıyla eşitlenecek ve bulut uç noktası hub'ı sağlar. Azure dosya paylaşımının depolama hesabı, Depolama Eşitleme Hizmeti ile aynı bölgede bulunmalıdır. Azure dosya paylaşımının tamamı, bir istisna dışında eşitlenir: NTFS birimindeki gizli "Sistem Hacmi Bilgileri" klasörüne karşılaştırılabilir özel bir klasör sağlanacaktır. Bu dizin "denir. SystemShareInformation". Diğer uç noktalarla eşitlemeyecek önemli eşitleme meta verileri içerir. Kullanmayın veya silmeyin!

> [!Important]  
> Eşitleme grubundaki herhangi bir bulut bitiş noktasında veya sunucu bitiş noktasında değişiklik yapabilir ve dosyalarınızın eşitleme grubundaki diğer uç noktalarla eşitlemesine neden olabilirsiniz. Bulut bitiş noktasında (Azure dosya paylaşımı) doğrudan bir değişiklik yaparsanız, öncelikle değişikliklerin bir Azure Dosya Eşitlemi değiştirme algılama işi tarafından keşfedilmesi gerekir. Bulut bitiş noktası için her 24 saatte bir değişiklik algılama işi başlatılır. Daha fazla bilgi için Azure [Dosyaları'nın sık sorulan sorularına](storage-files-faq.md#afs-change-detection)bakın.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure [portalında](https://portal.azure.com/)bir eşitleme grubu oluşturmak için Depolama Eşitleme Hizmetinize gidin ve ardından **+ Eşitle grubunu**seçin:

![Azure portalda yeni bir eşitleme grubu oluşturma](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Açılan bölmede, bulut uç noktası olan bir eşitleme grubu oluşturmak için aşağıdaki bilgileri girin:

- **Eşitleme grubu adı**: Oluşturulacak eşitleme grubunun adı. Bu ad Depolama Eşitleme Hizmetinde benzersiz olmalıdır, ancak size mantıklı gelen herhangi bir ad olabilir.
- **Abonelik**: [Depolama Eşitleme Hizmetini Dağıt'ta](#deploy-the-storage-sync-service)Depolama Eşitleme Hizmetini dağıttığınız abonelik.
- **Depolama hesabı**: **Depolama hesabını seç**seçeneğini belirlerseniz, eşitlemek istediğiniz Azure dosya paylaşımına sahip depolama hesabını seçebileceğiniz başka bir bölme görüntülenir.
- **Azure dosya paylaşımı**: Eşitlemek istediğiniz Azure dosya paylaşımının adı.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
Eşitleme grubunu oluşturmak için aşağıdaki PowerShell'i çalıştırın. Eşitleme `<my-sync-group>` grubunun istenilen adı ile değiştirmeyi unutmayın.

```powershell
$syncGroupName = "<my-sync-group>"
$syncGroup = New-AzStorageSyncGroup -ParentObject $storageSync -Name $syncGroupName
```

Eşitleme grubu başarıyla oluşturulduktan sonra bulut bitiş noktanızı oluşturabilirsiniz. Beklenen değerleri `<my-storage-account>` değiştirdiğinden ve `<my-file-share>` değiştirdiğinden emin olun.

```powershell
# Get or create a storage account with desired name
$storageAccountName = "<my-storage-account>"
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup | Where-Object {
    $_.StorageAccountName -eq $storageAccountName
}

if ($storageAccount -eq $null) {
    $storageAccount = New-AzStorageAccount `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        -Location $region `
        -SkuName Standard_LRS `
        -Kind StorageV2 `
        -EnableHttpsTrafficOnly:$true
}

# Get or create an Azure file share within the desired storage account
$fileShareName = "<my-file-share>"
$fileShare = Get-AzStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    $fileShare = New-AzStorageShare -Context $storageAccount.Context -Name $fileShareName
}

# Create the cloud endpoint
New-AzStorageSyncCloudEndpoint `
    -Name $fileShare.Name `
    -ParentObject $syncGroup `
    -StorageAccountResourceId $storageAccount.Id `
    -AzureFileShareName $fileShare.Name
```

---

## <a name="create-a-server-endpoint"></a>Sunucu uç noktası oluşturma
Sunucu uç noktası, bir sunucu birimi üzerindeki klasör gibi kayıtlı bir sunucu üzerindeki belirli bir noktayı temsil eder. Sunucu bitiş noktası kayıtlı bir sunucuda (monte edilmiş bir paylaşım yerine) bir yol olmalıdır ve bulut katmanlama kullanmak için, yol sistem dışı bir birimde olmalıdır. Ağ ekli depolama (NAS) desteklenmez.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Sunucu bitiş noktası eklemek için, yeni oluşturulan eşitleme grubuna gidin ve ardından **sunucu bitiş noktası ekle'yi**seçin.

![Eşitleme grubu bölmesine yeni bir sunucu uç noktası ekleme](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

**Sunucu uç noktası ekle** bölmesinde bir sunucu uç noktası oluşturmak için aşağıdaki bilgileri girin:

- **Kayıtlı sunucu**: Sunucu bitiş noktasını oluşturmak istediğiniz sunucu veya kümenin adı.
- **Yol**: Eşitleme grubunun bir parçası olarak eşitlenecek Windows Server yolu.
- **Bulut Katmanlama**: Bulut katmanlamayı etkinleştirmek veya devre dışı etmek için bir anahtar. Bulut katmanlama ile, seyrek kullanılan veya erişilen dosyalar Azure Dosyaları'na katmanlandırılabilir.
- **Hacim Boş Alan**: Sunucu bitiş noktasının bulunduğu birimde rezerve edilebilen boş alan miktarıdır. Örneğin, birim boş alanı tek bir sunucu bitiş noktasına sahip bir birimde %50 olarak ayarlanmışsa, veri miktarının kabaca yarısı Azure Dosyaları'na katmanlanır. Bulut katmanlamanın etkin olup olmadığına bakılmaksızın, Azure dosya paylaşımınızda her zaman eşitleme grubundaki verilerin tam bir kopyası vardır.

Sunucu bitiş noktasını eklemek için **Oluştur'u**seçin. Dosyalarınız artık Azure dosya paylaşımınızda ve Windows Server'da eşitolarak tutulur. 

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
Sunucu bitiş noktasını oluşturmak için aşağıdaki PowerShell komutlarını uygulayın `<your-volume-free-space>` ve istediğiniz değerleri değiştirdiğinden ve değiştirdiğinden `<your-server-endpoint-path>` emin olun.

```powershell
$serverEndpointPath = "<your-server-endpoint-path>"
$cloudTieringDesired = $true
$volumeFreeSpacePercentage = <your-volume-free-space>

if ($cloudTieringDesired) {
    # Ensure endpoint path is not the system volume
    $directoryRoot = [System.IO.Directory]::GetDirectoryRoot($serverEndpointPath)
    $osVolume = "$($env:SystemDrive)\"
    if ($directoryRoot -eq $osVolume) {
        throw [System.Exception]::new("Cloud tiering cannot be enabled on the system volume")
    }

    # Create server endpoint
    New-AzStorageSyncServerEndpoint `
        -Name $registeredServer.FriendlyName `
        -SyncGroup $syncGroup `
        -ServerResourceId $registeredServer.ResourceId `
        -ServerLocalPath $serverEndpointPath `
        -CloudTiering `
        -VolumeFreeSpacePercent $volumeFreeSpacePercentage
} else {
    # Create server endpoint
    New-AzStorageSyncServerEndpoint `
        -Name $registeredServer.FriendlyName `
        -SyncGroup $syncGroup `
        -ServerResourceId $registeredServer.ResourceId `
        -ServerLocalPath $serverEndpointPath 
}
```

---

## <a name="configure-firewall-and-virtual-network-settings"></a>Güvenlik duvarını ve sanal ağ ayarlarını kısıtla

### <a name="portal"></a>Portal
Azure Dosya eşitlemenizi güvenlik duvarı ve sanal ağ ayarlarıyla çalışacak şekilde yapılandırmak istiyorsanız, aşağıdakileri yapın:

1. Azure portalından, güvenliğini sağlamak istediğiniz depolama hesabına gidin.
1. Sol menüdeki **Güvenlik Duvarları ve sanal ağlar** düğmesini seçin.
1. **Erişime izin ver'in**altında **Seçili ağları** seçin.
1. Sunucularınızın IP veya sanal ağının uygun bölümün altında listelenmiş olduğundan emin olun.
1. Bu **depolama hesabına erişmek için güvenilir Microsoft hizmetlerine İzin Ver'in** denetlendiğinden emin olun.
1. Ayarlarınızı kaydetmek için **Kaydet'i** seçin.

![Güvenlik duvarı ve sanal ağ ayarlarını Azure Dosya eşitlemeyle çalışacak şekilde yapılandırma](media/storage-sync-files-deployment-guide/firewall-and-vnet.png)

## <a name="onboarding-with-azure-file-sync"></a>Azure Dosya Eşitleme ile Onboarding
Tam dosya doğruluğu ve erişim denetim listesi (ACL) korurken sıfır kapalı kalma süresi ile ilk için Azure Dosya Eşitleme üzerinde onboard için önerilen adımlar şunlardır:
 
1. Depolama Eşitleme Hizmeti dağıtın.
2. Eşitleme grubu oluşturun.
3. Tam veri kümesiyle sunucuya Azure Dosya Eşitleme aracısını yükleyin.
4. Bu sunucuyu kaydedin ve paylaşımda bir sunucu bitiş noktası oluşturun. 
5. Azure dosya paylaşımına (bulut bitiş noktası) tam yüklemeyi eşitleme yapalım.  
6. İlk yükleme tamamlandıktan sonra, kalan sunucuların her birine Azure Dosya Eşitleme aracısını yükleyin.
7. Kalan sunucuların her birinde yeni dosya paylaşımları oluşturun.
8. İstenirse bulut katmanlama ilkesiyle yeni dosya paylaşımlarında sunucu uç noktaları oluşturun. (Bu adım, ilk kurulum için kullanılabilir olması için ek depolama alanı gerektirir.)
9. Azure Dosya Eşitleme aracısının gerçek veri aktarımı olmadan tam ad alanını hızlı bir şekilde geri yüklemesine izin verin. Tam ad alanı eşitledikten sonra, eşitleme altyapısı sunucu bitiş noktası için bulut katmanlama ilkesini temel alan yerel disk alanını doldurur. 
10. Senkronizasyonun tamamlandığından emin olun ve topolojinizi istediğiniz gibi test edin. 
11. Kullanıcıları ve uygulamaları bu yeni paylaşıma yönlendirin.
12. Sunucularda yinelenen paylaşımları isteğe bağlı olarak silebilirsiniz.
 
İlk biniş için ek depolama alanınız yoksa ve varolan paylaşımlara eklemek istiyorsanız, Azure dosyalarının paylaşımlarında verileri önceden tohumlayabilirsiniz. Bu yaklaşım, yalnızca kapalı kalma süresini kabul edebilirseniz ve ilk onboarding işlemi sırasında sunucu paylaşımlarında veri değişikliği olmadığını kesinlikle garanti edebilirseniz önerilir. 
 
1. Herhangi bir sunucudaki verilerin onboarding işlemi sırasında değiştirilemediğinden emin olun.
2. Ön tohumAzure dosyası, Örneğin Robocopy, doğrudan Kobİ kopyası gibi Kobİ üzerinden herhangi bir veri aktarım aracı nı kullanarak sunucu verileriyle paylaşır. AzCopy, ön tohumlama için kullanılamadığı için SMB üzerinden veri yüklemediği için.
3. Varolan paylaşımları gösteren istenilen sunucu uç noktalarıyla Azure Dosya Eşitleme topolojisini oluşturun.
4. Tüm uç noktalarda eşitleme mutabakat işlemini bitirin. 
5. Mutabakat tamamlandıktan sonra, paylaşımları değişiklikler için açabilirsiniz.
 
Şu anda, ön tohumlama yaklaşımı birkaç sınırlamaları vardır - 
- Dosyalardaki tam doğruluk korunmuyor. Örneğin, dosyalar ALA'ları ve zaman damgalarını kaybeder.
- Eşitleme topolojisi tam olarak dolmadan ve çalışan sunucu uç noktalarında çakışmalara neden olabilir önce sunucuda veri değişiklikleri.  
- Bulut bitiş noktası oluşturulduktan sonra, Azure Dosya Eşitlemi ilk eşitlemeye başlamadan önce buluttaki dosyaları algılamak için bir işlem çalıştırAr. Bu işlemi tamamlamak için geçen süre, ağ hızı, kullanılabilir bant genişliği ve dosya ve klasör sayısı gibi çeşitli etkenlere bağlı olarak değişir. Önizleme sürümündeki kaba tahmin için algılama işlemi yaklaşık olarak 10 dosya/sn'de çalışır.  Bu nedenle, ön tohumlama hızlı sürse bile, veriler bulutta önceden tohumlandığında tam olarak çalışan bir sisteme ulaşmak için gereken genel süre önemli ölçüde daha uzun olabilir.

## <a name="self-service-restore-through-previous-versions-and-vss-volume-shadow-copy-service"></a>Önceki Sürümler ve VSS (Cilt Gölge Kopyalama Hizmeti) ile self servis geri yükleme

> [!IMPORTANT]
> Aşağıdaki bilgiler yalnızca depolama eşitleme aracısının sürüm 9 (veya üstü) ile kullanılabilir. 9'dan küçük sürümlerde StorageSyncSelfService cmdlets olmaz.

Önceki Sürümler, bir dosyanın geri getirilebilir sürümlerini bir SMB istemcisine sunmak için bir birimin sunucu tarafındaki VSS anlık görüntülerini kullanmanıza olanak tanıyan bir Windows özelliğidir.
Bu, bt yöneticisinin geri yüklemesine bağlı kalmak yerine doğrudan bilgi çalışanları için self servis geri yükleme olarak adlandırılan güçlü bir senaryosağlar.

VSS anlık görüntüleri ve Önceki Sürümler Azure Dosya Eşitleme bağımsız çalışır. Ancak, bulut katmanlama uyumlu bir modda ayarlanmalıdır. Aynı birimde birçok Azure Dosya Eşitleme sunucusu bitiş noktası bulunabilir. Bulut katmanlamayı planladığınız veya bulut katmanlamayı kullandığınız bir sunucu bitiş noktası bile olan birim başına aşağıdaki PowerShell aramasını yapmak zorundasınız.

```powershell
Import-Module ‘<SyncAgentInstallPath>\StorageSync.Management.ServerCmdlets.dll’
Enable-StorageSyncSelfServiceRestore [-DriveLetter] <string> [[-Force]] 
```

VSS anlık görüntüleri tüm bir birim alınır. Varsayılan olarak, anlık görüntüleri depolamak için yeterli alan verilen, belirli bir birim için 64'e kadar anlık görüntü bulunabilir. VSS bunu otomatik olarak işler. Varsayılan anlık görüntü zamanlaması, Pazartesi'den Cuma'ya kadar günde iki anlık görüntü alır. Bu zamanlama, Windows Zamanlanmış Görev aracılığıyla yapılandırılabilir. Yukarıdaki PowerShell cmdlet iki şey yapar:
1. Belirtilen birimdeki Azure Dosya Eşitlemebulut katmanlamasını önceki sürümlerle uyumlu olacak şekilde yapılandırır ve sunucuda buluta katmanlı olsa bile bir dosyanın önceki sürümden geri yüklenebileceğini garanti eder. 
2. Varsayılan VSS zamanlamasını sağlar. Daha sonra değiştirmeye karar verebilirsiniz. 

> [!Note]  
> Unutulmaması gereken iki önemli nokta vardır:
>- -Kuvvet parametresini kullanırsanız ve VSS şu anda etkinse, geçerli VSS anlık görüntü çizelgesinin üzerine yazar ve varsayılan zamanlamayla değiştirir. Cmdlet'i çalıştırmadan önce özel yapılandırmanızı kaydettiğinizden emin olun.
> - Bu cmdlet'i bir küme düğümünde kullanıyorsanız, kümedeki diğer düğümlerde de çalıştırmanız gerekir! 

Self servis geri yükleme uyumluluğunun etkin olup olmadığını görmek için aşağıdaki cmdlet'i çalıştırabilirsiniz.

```powershell
    Get-StorageSyncSelfServiceRestore [[-Driveletter] <string>]
```

Sunucudaki tüm birimlerin yanı sıra her biri için bulut katmanlama uyumlu gün sayısını listeler. Bu sayı, birim başına mümkün olan en yüksek anlık görüntü ve varsayılan anlık görüntü zamanlaması na göre otomatik olarak hesaplanır. Bu nedenle, varsayılan olarak, bir bilgi çalışanına sunulan tüm önceki sürümler geri yüklemek için kullanılabilir. Varsayılan zamanlamayı daha fazla anlık görüntü almak için değiştirirseniz, aynı durum geçerlidir.
Ancak, zamanlamayı, uyumlu gün değerinden daha eski olan birimde kullanılabilir bir anlık görüntüyle sonuçlanacak şekilde değiştirirseniz, kullanıcılar bu eski anlık görüntüden (önceki sürüm) geri yüklemek için kullanamaz.

> [!Note]
> Self servis geri yüklemeyi etkinleştirmek, Azure depolama tüketiminiz ve faturanız üzerinde etkili olabilir. Bu etki, sunucuda şu anda katmanlı dosyalarla sınırlıdır. Bu özelliği etkinleştirmek, bulutta önceki sürümlerden (VSS anlık görüntüsü) giriş yoluyla başvurulan bir dosya sürümü olmasını sağlar.
>
> Özelliği devre dışı bederseniz, uyumlu günler penceresi geçene kadar Azure depolama alanı tüketimi yavaş yavaş azalır. Bunu hızlandırmanın bir yolu yok. 

Birim başına varsayılan maksimum VSS anlık görüntüsü sayısı (64) ve bunları almak için varsayılan zamanlama, birimde depolayabildiğiniz vsS anlık görüntüsünün sayısına bağlı olarak, bir bilgi çalışanının geri yükleyebileceği en fazla 45 günlük önceki sürümlerle sonuçlanır.

Eğer max. Birim başına 64 VSS anlık görüntü sizin için doğru ayar değil, [bir kayıt defteri anahtarı ile bu değeri değiştirebilirsiniz.](https://docs.microsoft.com/windows/win32/backup/registry-keys-for-backup-and-restore#maxshadowcopies)
Yeni sınırın etkili olması için cmdlet'i daha önce etkinleştirilen her birimde önceki sürüm uyumluluğunu etkinleştirmek için yeniden çalıştırmanız gerekir ve -Force bayrağı ses başına yeni maksimum VSS anlık görüntü sayısını hesaba katabilmeniz ilerler. Bu, yeni hesaplanan uyumlu gün sayısıyla sonuçlanır. Bu değişikliğin yalnızca yeni katmanlı dosyalar üzerinde etkili olacağını ve yapmış olabileceğiniz VSS zamanlamasındaki özelleştirmelerin üzerine yazacağını lütfen unutmayın.

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>DFS Çoğaltma (DFS-R) dağıtımını Azure Dosya Eşitlemesine geçirme
BIR DFS-R dağıtımını Azure Dosya Eşitleme'sine geçirmek için:

1. Değiştirmekte olduğunuz DFS-R topolojisini temsil edecek bir eşitleme grubu oluşturun.
2. Geçiş yapmak için DFS-R topolojinizde tam veri kümesine sahip sunucudan başlayın. Azure Dosya Eşitlemeyi'yi bu sunucuya yükleyin.
3. Bu sunucuyu kaydedin ve ilk sunucunun geçirilme noktası için bir sunucu bitiş noktası oluşturun. Bulut katmanlamayı etkinleştirme.
4. Tüm verilerin Azure dosyanızla eşitlemesine izin verin (bulut bitiş noktası).
5. Kalan DFS-R sunucularının her birine Azure Dosya Eşitleme aracısını yükleyin ve kaydedin.
6. DFS-R'yi devre dışı. 
7. DFS-R sunucularının her birinde bir sunucu bitiş noktası oluşturun. Bulut katmanlamayı etkinleştirme.
8. Senkronizasyonun tamamlandığından emin olun ve topolojinizi istediğiniz gibi test edin.
9. EMEKLI DFS-R.
10. Bulut katmanlama artık istediğiniz gibi herhangi bir sunucu bitiş noktasında etkinleştirilebilir.

Daha fazla bilgi için Bkz. [Dağıtılmış Dosya Sistemi (DFS) ile Azure Dosya Eşitlemi interop.](storage-sync-files-planning.md#distributed-file-system-dfs)

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Dosya Eşitleme Sunucusu Bitiş Noktası ekleme veya kaldırma](storage-sync-files-server-endpoint.md)
- [Azure Dosya Eşitlemi ile bir sunucuya kaydolun veya kaydını kaldırın](storage-sync-files-server-registration.md)
- [Azure Dosya Eşitleme’yi izleme](storage-sync-files-monitoring.md)
