---
title: Azure bağımsız bulutlarında verileri yönetmek için PowerShell'i kullanın
titleSuffix: Azure Storage
description: Azure PowerShell'i Kullanarak Çin Bulutu, Devlet Bulutu ve Alman Bulutu'nda Depolamayı Yönetme.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 5fa515515c06466e121a5c0ee925fd4d14245363
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74895230"
---
# <a name="managing-storage-in-the-azure-independent-clouds-using-powershell"></a>PowerShell'i kullanarak Azure bağımsız bulutlarında Depolamayı Yönetme

Çoğu kişi, küresel Azure dağıtımı için Azure Genel Bulutu'nunu kullanır. Ayrıca, egemenlik ve benzeri nedenlerle Microsoft Azure'un bazı bağımsız dağıtımları da vardır. Bu bağımsız dağıtımlara "ortamlar" adı verilir. Aşağıdaki liste, şu anda kullanılabilen bağımsız bulutları ayrıntılarıyla açıklar.

* [Azure Devlet Bulutu](https://azure.microsoft.com/features/gov/)
* [Azure China 21Vianet Cloud Çin'de 21Vianet tarafından işletilen](http://www.windowsazure.cn/)
* [Azure Alman Bulutu](../../germany/germany-welcome.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="using-an-independent-cloud"></a>Bağımsız bir bulut kullanma

Azure Depolama'yı bağımsız bulutlardan birinde kullanmak için Azure Geneli yerine bu buluta bağlanırsınız. Azure Geneli yerine bağımsız bulutlardan birini kullanmak için:

* Bağlanabileceğiniz *ortamı* belirtirsiniz.
* Kullanılabilir bölgeleri belirler ve kullanırsınız.
* Azure Genel'den farklı olan doğru bitiş noktası sonekini kullanırsınız.

Örnekler, Azure PowerShell modülü Az sürüm 0.7 veya daha sonra gerektirir. PowerShell penceresinde, sürümü `Get-Module -ListAvailable Az` bulmak için çalıştırın. Hiçbir şey listelenmemişse veya yükseltmeniz gerekiyorsa Azure [PowerShell modüllerini](/powershell/azure/install-Az-ps)yükleyin'e bakın.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Kullanılabilir Azure ortamlarını görmek için [Get-AzEnvironment](/powershell/module/az.accounts/get-azenvironment) cmdlet'ini çalıştırın:

```powershell
Get-AzEnvironment
```

Bağlanmak ve ortamı ayarlamak istediğiniz buluta erişimi olan hesabınızda oturum açın. Bu örnek, Azure Kamu Bulutu'nu kullanan bir hesapta nasıl oturum açılabildiğini gösterir.   

```powershell
Connect-AzAccount –Environment AzureUSGovernment
```

Çin Bulutu'na erişmek için **AzureChinaCloud**ortamını kullanın. Alman Bulutu'na erişmek için **AzureGermanCloud'u**kullanın.

Bu noktada, bir depolama hesabı veya başka bir kaynak oluşturmak için konumların listesine ihtiyacınız varsa, [Get-AzLocation'ı](/powershell/module/az.resources/get-azlocation)kullanarak seçili bulut için kullanılabilir konumları sorgulayabilirsiniz.

```powershell
Get-AzLocation | select Location, DisplayName
```

Aşağıdaki tablo, Alman bulutu için döndürülen konumları gösterir.

|Konum | Görünen Ad |
|----|----|
| `germanycentral` | Orta Almanya|
| `germanynortheast` | Kuzeydoğu Almanya |


## <a name="endpoint-suffix"></a>Bitiş noktası soneki

Bu ortamların her biri için uç nokta soneki Azure Genel bitiş noktasından farklıdır. Örneğin, Azure Genel için blob uç nokta soneki **blob.core.windows.net.** Hükümet Bulutu için, blob uç nokta soneki **blob.core.usgovcloudapi.net.**

### <a name="get-endpoint-using-get-azenvironment"></a>Get-AzEnvironment'ı kullanarak uç noktayı elde edin

[Get-AzEnvironment'ı](/powershell/module/az.accounts/get-azenvironment)kullanarak bitiş noktası sonekini alın. Bitiş noktası, ortamın *StorageEndpointSuffix* özelliğidir.

Aşağıdaki kod parçacıkları bitiş noktası sonekinin nasıl alınır olduğunu gösterir. Tüm bu komutlar "core.cloudapp.net" veya "core.cloudapi.de" gibi bir şey döndürür. Bu hizmete erişmek için depolama hizmetine sonek ekle. Örneğin, "queue.core.cloudapi.de" Alman Bulut'taki kuyruk hizmetine erişecektir.

Bu kod snippet, her biri için tüm ortamları ve bitiş noktası sonekini alır.

```powershell
Get-AzEnvironment | select Name, StorageEndpointSuffix 
```

Bu komut aşağıdaki sonuçları döndürür.

| Adı| DepolamaEndpointSuffix|
|----|----|
| AzureChinaCloud | core.chinacloudapi.cn|
| AzureCloud | core.windows.net |
| AzureGermanCloud | core.cloudapi.de|
| AzureUSGovernment | core.usgovcloudapi.net |

Belirtilen ortama ait tüm özellikleri almak için **Get-AzEnvironment'ı** arayın ve bulut adını belirtin. Bu kod snippet özelliklerinin listesini döndürür; listede **StorageEndpointSuffix** arayın. Aşağıdaki örnek Alman Bulutu içindir.

```powershell
Get-AzEnvironment -Name AzureGermanCloud
```

Sonuçlar aşağıdaki değerlere benzer:

|Özellik Adı|Değer|
|----|----|
| Adı | `AzureGermanCloud` |
| EtkinleştirmeAdfsOrijinalasyon | `False` |
| ActiveDirectoryServiceEndpointResourceI | `http://management.core.cloudapi.de/` |
| GaleriURL | `https://gallery.cloudapi.de/` |
| YönetimPortalUrl | `https://portal.microsoftazure.de/` |
| ServiceManagementUrl | `https://manage.core.cloudapi.de/` |
| PublishSettingsFileUrl| `https://manage.microsoftazure.de/publishsettings/index` |
| Kaynak YöneticisiUrl | `http://management.microsoftazure.de/` |
| SqlDatabaseDnsSuffix | `.database.cloudapi.de` |
| **DepolamaEndpointSuffix** | `core.cloudapi.de` |
| ... | ... |
Sadece depolama bitiş noktası sonek özelliğini almak için, belirli bulutu alın ve sadece bir özelliği isteyin.

```powershell
$environment = Get-AzEnvironment -Name AzureGermanCloud
Write-Host "Storage EndPoint Suffix = " $environment.StorageEndpointSuffix
```

Bu komut aşağıdaki bilgileri döndürür:

`Storage Endpoint Suffix = core.cloudapi.de`

### <a name="get-endpoint-from-a-storage-account"></a>Bir depolama hesabından bitiş noktası alma

Uç noktaları almak için bir depolama hesabının özelliklerini de inceleyebilirsiniz:

```powershell
# Get a reference to the storage account.
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"
$storageAccount = Get-AzStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName 
  # Output the endpoints.
Write-Host "blob endpoint = " $storageAccount.PrimaryEndPoints.Blob 
Write-Host "file endpoint = " $storageAccount.PrimaryEndPoints.File
Write-Host "queue endpoint = " $storageAccount.PrimaryEndPoints.Queue
Write-Host "table endpoint = " $storageAccount.PrimaryEndPoints.Table
```

Devlet Bulutu'ndaki bir depolama hesabı için bu komut aşağıdaki çıktıyı döndürür:

```
blob endpoint = http://myexistingstorageaccount.blob.core.usgovcloudapi.net/
file endpoint = http://myexistingstorageaccount.file.core.usgovcloudapi.net/
queue endpoint = http://myexistingstorageaccount.queue.core.usgovcloudapi.net/
table endpoint = http://myexistingstorageaccount.table.core.usgovcloudapi.net/
```

## <a name="after-setting-the-environment"></a>Ortamı ayarladıktan sonra

Buradan itibaren, depolama hesaplarınızı yönetmek ve [Azure Depolama ile Azure PowerShell'i Kullanma](storage-powershell-guide-full.md)makalesinde açıklandığı gibi veri düzlemine erişmek için kullanılan powershell'i kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu alıştırma için yeni bir kaynak grubu ve depolama hesabı oluşturduysanız, kaynak grubunu silerek her iki varlığı da kaldırabilirsiniz. Kaynak grubunu sildiğinizde o grupta bulunan tüm kaynaklar da silinir.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell oturumları arasında kullanıcı oturumlarını sürdürme](/powershell/azure/context-persistence)
* [Azure Devlet depolama](../../azure-government/documentation-government-services-storage.md)
* [Microsoft Azure Kamu Geliştirici Kılavuzu](../../azure-government/documentation-government-developer-guide.md)
* [Azure China 21Vianet Uygulamaları için Geliştirici Notları](https://msdn.microsoft.com/library/azure/dn578439.aspx)
* [Azure Almanya Dokümantasyon](../../germany/germany-welcome.md)
