---
title: Windows VM 'Leri için bir görüntü Galerisi ile Azure Image Builder kullanma (Önizleme)
description: Azure Image Builder ve Azure PowerShell kullanarak Azure Paylaşılan Galeri görüntüsü sürümlerini oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 01/14/2020
ms.topic: article
ms.service: virtual-machines-windows
manager: gwallace
ms.openlocfilehash: d5856780d0d9f1a1943bca1c2f076bb3ec914e1d
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76263362"
---
# <a name="preview-create-a-windows-image-and-distribute-it-to-a-shared-image-gallery"></a>Önizleme: bir Windows görüntüsü oluşturun ve paylaşılan bir görüntü galerisine dağıtın 

Bu makale, Azure görüntü Oluşturucu 'Yu nasıl kullanabileceğinizi ve Azure PowerShell [paylaşılan bir görüntü galerisinde](shared-image-galleries.md)görüntü sürümü oluşturmayı ve sonra görüntüyü küresel olarak dağıtmayı gösterir. Bunu [Azure CLI](../linux/image-builder-gallery.md)kullanarak da yapabilirsiniz.

Görüntüyü yapılandırmak için bir. JSON şablonu kullanacağız. Kullandığımız. JSON dosyası şu şekildedir: [Armtemplatewinsig. JSON](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/armTemplateWinSIG.json). Şablonun yerel bir sürümünü indirip düzenlemenizi sağlayacak ve bu makalede yerel PowerShell oturumu kullanılarak yazılmıştır.

Görüntüyü paylaşılan bir görüntü galerisine dağıtmak için, şablon, şablonun `distribute` bölümü [değeri olarak parçalama](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#distribute-sharedimage) .

Azure görüntü Oluşturucu görüntüyü genelleştirmek için otomatik olarak Sysprep çalıştırır, bu, gerekirse [geçersiz kılabileceğiniz](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#vms-created-from-aib-images-do-not-create-successfully) genel bir Sysprep komutu. 

Özelleştirmelerin kaç kez özelleştirmeler olduğunu unutmayın. Sysprep komutunu tek bir Windows görüntüsünde en fazla 8 kez çalıştırabilirsiniz. Sysprep 8 kez çalıştırıldıktan sonra, Windows görüntünüzü yeniden oluşturmanız gerekir. Daha fazla bilgi için bkz. [Sysprep 'i kaç kez çalıştıracağınızı gösteren sınırlamalar](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation#limits-on-how-many-times-you-can-run-sysprep). 

> [!IMPORTANT]
> Azure görüntü Oluşturucu Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-features"></a>Özellikleri kaydetme
Önizleme sırasında Azure Image Builder 'ı kullanmak için yeni özelliği kaydetmeniz gerekir.

```powershell
Register-AzProviderFeature -FeatureName VirtualMachineTemplatePreview -ProviderNamespace Microsoft.VirtualMachineImages
```

Özellik kaydının durumunu denetleyin.

```powershell
Get-AzProviderFeature -FeatureName VirtualMachineTemplatePreview -ProviderNamespace Microsoft.VirtualMachineImages
```

Sonraki adıma geçmeden önce `RegistrationState` `Registered` bitinceye kadar bekleyin.

Sağlayıcı kayıtlarınızı denetleyin. Her birinin `Registered`döndürdüğünden emin olun.

```powershell
Get-AzResourceProvider -ProviderNamespace Microsoft.VirtualMachineImages | Format-table -Property ResourceTypes,RegistrationState
Get-AzResourceProvider -ProviderNamespace Microsoft.Storage | Format-table -Property ResourceTypes,RegistrationState 
Get-AzResourceProvider -ProviderNamespace Microsoft.Compute | Format-table -Property ResourceTypes,RegistrationState
Get-AzResourceProvider -ProviderNamespace Microsoft.KeyVault | Format-table -Property ResourceTypes,RegistrationState
```

`Registered`döndürmezse, sağlayıcıları kaydetmek için aşağıdakileri kullanın:

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.VirtualMachineImages
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
Register-AzResourceProvider -ProviderNamespace Microsoft.KeyVault
```

## <a name="create-variables"></a>Değişken oluşturma

Bazı bilgi parçalarını sürekli olarak kullanacağız. bu nedenle, bu bilgileri depolamak için bazı değişkenler oluşturacağız. `username` ve `vmpassword`gibi değişkenlerin değerlerini kendi bilgileriniz ile değiştirin.

```powershell
# Get existing context
$currentAzContext = Get-AzContext

# Get your current subscription ID. 
$subscriptionID=$currentAzContext.Subscription.Id

# Destination image resource group
$imageResourceGroup="aibwinsig"

# Location
$location="westus"

# Image distribution metadata reference name
$runOutputName="aibCustWinManImg02ro"

# Image template name
$imageTemplateName="helloImageTemplateWin02ps"

# Distribution properties object name (runOutput).
# This gives you the properties of the managed image on completion.
$runOutputName="winclientR01"
```



## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Kaynak grubu oluşturun ve bu kaynak grubunda kaynak oluşturmak için Azure Image Builder 'a izin verin.

```powershell
New-AzResourceGroup `
   -Name $imageResourceGroup `
   -Location $location
New-AzRoleAssignment `
   -ObjectId ef511139-6170-438e-a6e1-763dc31bdf74 `
   -Scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup `
   -RoleDefinitionName Contributor
```



## <a name="create-the-shared-image-gallery"></a>Paylaşılan görüntü galerisini oluşturma

Görüntü Oluşturucuyu paylaşılan bir görüntü Galerisi ile birlikte kullanmak için, var olan bir görüntü Galerisi ve görüntü tanımınız olması gerekir. Görüntü Oluşturucu, sizin için görüntü Galerisi ve görüntü tanımı oluşturmaz.

Kullanmak üzere bir galeri ve görüntü tanımınız yoksa, bunları oluşturarak başlayın. İlk olarak, bir görüntü galerisi oluşturun.

```powershell
# Image gallery name
$sigGalleryName= "myIBSIG"

# Image definition name
$imageDefName ="winSvrimage"

# additional replication region
$replRegion2="eastus"

# Create the gallery
New-AzGallery `
   -GalleryName $sigGalleryName `
   -ResourceGroupName $imageResourceGroup  `
   -Location $location

# Create the image definition
New-AzGalleryImageDefinition `
   -GalleryName $sigGalleryName `
   -ResourceGroupName $imageResourceGroup `
   -Location $location `
   -Name $imageDefName `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myCompany' `
   -Offer 'WindowsServer' `
   -Sku 'WinSrv2019'
```



## <a name="download-and-configure-the-template"></a>Şablonu indirme ve yapılandırma

. JSON şablonunu indirin ve değişkenleriniz ile yapılandırın.

```powershell

$templateFilePath = "armTemplateWinSIG.json"

Invoke-WebRequest `
   -Uri "https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/armTemplateWinSIG.json" `
   -OutFile $templateFilePath `
   -UseBasicParsing

(Get-Content -path $templateFilePath -Raw ) `
   -replace '<subscriptionID>',$subscriptionID | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<rgName>',$imageResourceGroup | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<runOutputName>',$runOutputName | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<imageDefName>',$imageDefName | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<sharedImageGalName>',$sigGalleryName | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<region1>',$location | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<region2>',$replRegion2 | Set-Content -Path $templateFilePath

```


## <a name="create-the-image-version"></a>Görüntü sürümü oluşturma

Şablonunuz hizmete gönderilmesi gerekir, bu komut dosyaları gibi bağımlı yapıtları indirir ve bunları hazırlama kaynak grubunda depolar, *IT_* ön ekine sahip olur.

```powershell
New-AzResourceGroupDeployment `
   -ResourceGroupName $imageResourceGroup `
   -TemplateFile $templateFilePath `
   -api-version "2019-05-01-preview" `
   -imageTemplateName $imageTemplateName `
   -svclocation $location
```

Şablonu oluşturmak için şablonda ' Run ' komutunu çağırmanız gerekir.

```powershell
Invoke-AzResourceAction `
   -ResourceName $imageTemplateName `
   -ResourceGroupName $imageResourceGroup `
   -ResourceType Microsoft.VirtualMachineImages/imageTemplates `
   -ApiVersion "2019-05-01-preview" `
   -Action Run
```

Görüntünün oluşturulması ve her iki bölgeye çoğaltılmasının biraz zaman alabilir. VM oluşturma işlemine geçmeden önce Bu bölüm bitene kadar bekleyin.

Görüntü oluşturma durumunun otomatikleştirilmesine yönelik seçenekler hakkında bilgi için GitHub 'da bu şablonun [Benioku dosyasına](https://github.com/danielsollondon/azvmimagebuilder/blob/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/readme.md#get-status-of-the-image-build-and-query) bakın.


## <a name="create-the-vm"></a>Sanal makine oluşturma

Azure Image Builder tarafından oluşturulan görüntü sürümünden bir VM oluşturun.

Oluşturduğunuz görüntü sürümünü alın.
```powershell
$imageVersion = Get-AzGalleryImageVersion `
   -ResourceGroupName $imageResourceGroup `
   -GalleryName $sigGalleryName `
   -GalleryImageDefinitionName $imageDefName
```

Görüntünün çoğaltılan ikinci bölgede sanal makineyi oluşturun.

```powershell
$vmResourceGroup = "myResourceGroup"
$vmName = "myVMfromImage"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group
New-AzResourceGroup -Name $vmResourceGroup -Location $replRegion2

# Network pieces
$subnetConfig = New-AzVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
$vnet = New-AzVirtualNetwork -ResourceGroupName $vmResourceGroup -Location $replRegion2 `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig
$pip = New-AzPublicIpAddress -ResourceGroupName $vmResourceGroup -Location $replRegion2 `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $vmResourceGroup -Location $replRegion2 `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP
$nic = New-AzNetworkInterface -Name myNic -ResourceGroupName $vmResourceGroup -Location $replRegion2 `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration using $imageVersion.Id to specify the shared image
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize Standard_D1_v2 | `
Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred | `
Set-AzVMSourceImage -Id $imageVersion.Id | `
Add-AzVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzVM -ResourceGroupName $vmResourceGroup -Location $replRegion2 -VM $vmConfig
```

## <a name="verify-the-customization"></a>Özelleştirmeyi doğrulama
VM 'yi oluştururken ayarladığınız Kullanıcı adını ve parolayı kullanarak VM 'ye bir Uzak Masaüstü bağlantısı oluşturun. VM 'nin içinde bir komut istemi açın ve şunu yazın:

```console
dir c:\
```

Görüntü özelleştirmesi sırasında oluşturulan `buildActions` adlı bir dizin görmeniz gerekir.


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık aynı görüntünün yeni bir sürümünü oluşturmak için görüntü sürümünü yeniden özelleştirmeyi denemek istiyorsanız, **Bu adımı atlayın** ve [başka bir görüntü sürümü oluşturmak Için Azure Image Builder 'ı kullanma](image-builder-gallery-update-image-version.md)sayfasına gidin.


Bu işlem, oluşturulan görüntüyü ve diğer tüm kaynak dosyalarını siler. Kaynakları silmeden önce bu dağıtımla bitdiğinizden emin olun.

İlk olarak kaynak grubu şablonunu silin, aksi takdirde AıB tarafından kullanılan hazırlama kaynak grubu (*IT_* ) temizlenmeyecektir.

Görüntü şablonunun RESOURCEID 'sini alın. 

```powerShell
$resTemplateId = Get-AzResource -ResourceName $imageTemplateName -ResourceGroupName $imageResourceGroup -ResourceType Microsoft.VirtualMachineImages/imageTemplates -ApiVersion "2019-05-01-preview"
```

Görüntü şablonunu silin.

```powerShell
Remove-AzResource -ResourceId $resTemplateId.ResourceId -Force
```

Kaynak grubunu silin.

```powerShell
Remove-AzResourceGroup $imageResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki Adımlar

Oluşturduğunuz görüntü sürümünü güncelleştirme hakkında bilgi edinmek için bkz. [Azure Image Builder 'ı kullanarak başka bir görüntü sürümü oluşturma](image-builder-gallery-update-image-version.md).
