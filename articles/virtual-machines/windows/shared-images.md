---
title: Azure PowerShell ile paylaşılan VM görüntüleri oluşturma
description: Azure'da paylaşılan sanal makine görüntüsü oluşturmak için Azure PowerShell'i nasıl kullanacağınızı öğrenin
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/06/2019
ms.author: cynthn
ms.custom: ''
ms.openlocfilehash: cfaf545ad2388688666a04076559ff82b7a5d120
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458071"
---
# <a name="create-a-shared-image-gallery-with-azure-powershell"></a>Azure PowerShell ile paylaşılan bir resim galerisi oluşturma 

[Paylaşılan Resim Galerisi,](shared-image-galleries.md) kuruluşunuz genelinde özel görüntü paylaşımını kolaylaştırır. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, önceden yükleme uygulamaları, uygulama yapılandırmaları ve diğer işletim sistemi yapılandırmaları gibi dağıtım görevlerini önyükleme de kullanılabilir. 

Paylaşılan Resim Galerisi, özel VM görüntülerinizi kuruluşunuzdaki, bölgeler içinde veya diğer bölgeler arasında, bir AAD kiracısı içinde başkalarıyla paylaşmanıza olanak tanır. Hangi resimleri paylaşmak istediğinizi, bunları hangi bölgelerde kullanılabilir hale getirmek istediğinizi ve bunları kiminle paylaşmak istediğinizi seçin. Paylaşılan görüntüleri mantıksal olarak gruplandırmak için birden çok galeri oluşturabilirsiniz. 

Galeri, tam rol tabanlı erişim denetimi (RBAC) sağlayan üst düzey bir kaynaktır. Görüntüler sürülebilir ve her resim sürümünü farklı bir Azure bölgesi kümesinde çoğaltmayı seçebilirsiniz. Galeri yalnızca Yönetilen Görüntüler ile çalışır.

Paylaşılan Resim Galerisi özelliğinin birden çok kaynak türü vardır. Bu makalede bunları kullanacak veya inşa edeceğiz:

| Kaynak | Açıklama|
|----------|------------|
| **Yönetilen görüntü** | Bu, tek başına kullanılabilen veya resim galerisinde **görüntü sürümü** oluşturmak için kullanılabilen temel bir görüntüdür. Yönetilen görüntüler genelleştirilmiş VM'lerden oluşturulur. Yönetilen görüntü, birden çok VM yapmak için kullanılabilen ve şimdi paylaşılan görüntü sürümleri oluşturmak için kullanılabilen özel bir VHD türüdür. |
| **Resim galerisi** | Azure Marketi gibi, **resim galerisi** de görüntüleri yönetmek ve paylaşmak için bir depodur, ancak kimin erişimi olduğunu siz denetlersiniz. |
| **Resim tanımı** | Görüntüler bir galeri içinde tanımlanır ve görüntü ve dahili olarak kullanmak için gereksinimleri hakkında bilgi taşır. Bu, görüntünün Windows veya Linux olup olmadığını, sürüm notlarını ve minimum ve maksimum bellek gereksinimlerini içerir. Bu bir görüntü türünün tanımıdır. |
| **Görüntü sürümü** | **Resim sürümü,** galeri kullanırken VM oluşturmak için kullandığınız sürümdür. Ortamınız için gerektiği gibi görüntünün birden çok sürümüne sahip olabilirsiniz. Yönetilen bir görüntü gibi, VM oluşturmak için bir **görüntü sürümü** kullandığınızda, görüntü sürümü VM için yeni diskler oluşturmak için kullanılır. Görüntü sürümleri birden çok kez kullanılabilir. |

Aynı anda oluşturduğunuz her 20 VM için bir yineleme bulundurmanızı öneririz. Örneğin, bir bölgede aynı görüntüyü aynı anda kullanarak 120 VM oluşturuyorsanız, görüntünüzün en az 6 kopyasını saklamanızı öneririz. Daha fazla bilgi için [ölçekleme'ye](/azure/virtual-machines/windows/shared-image-galleries#scaling)bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki örneği tamamlamak için varolan yönetilen bir görüntüye sahip olmalısınız. Öğretici'yi takip edebilirsiniz: Gerekirse bir azure VShell oluşturmak için [Azure PowerShell ile azure vm'nin özel bir görüntüsünü oluşturun.](tutorial-custom-images.md) Yönetilen görüntü bir veri diski içeriyorsa, veri diskboyutu 1 TB'den fazla olamaz.

Bu makale üzerinde çalışırken, gerektiğinde kaynak grubunu ve VM adlarını değiştirin.

[!INCLUDE [virtual-machines-common-shared-images-powershell](../../../includes/virtual-machines-common-shared-images-powershell.md)]

 
## <a name="create-vms-from-an-image"></a>Görüntüden VM oluşturma

Görüntü sürümü tamamlandıktan sonra, bir veya daha fazla yeni VM oluşturabilirsiniz. [Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet kullanarak. 

Bu örnek, *Güney Orta ABD* veri merkezindeki *myResourceGroup'ta* *myVMfromImage*adında bir VM oluşturur.


```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$location = "South Central US"
$vmName = "myVMfromImage"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group
New-AzResourceGroup -Name $resourceGroup -Location $location

# Network pieces
$subnetConfig = New-AzVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
$vnet = New-AzVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig
$pip = New-AzPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP
$nic = New-AzNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration using $imageVersion.Id to specify the shared image
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize Standard_D1_v2 | `
Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred | `
Set-AzVMSourceImage -Id $imageVersion.Id | `
Add-AzVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

[!INCLUDE [virtual-machines-common-gallery-list-ps](../../../includes/virtual-machines-common-gallery-list-ps.md)]

[!INCLUDE [virtual-machines-common-shared-images-update-delete-ps](../../../includes/virtual-machines-common-shared-images-update-delete-ps.md)]

## <a name="next-steps"></a>Sonraki adımlar
[Azure Image Builder (önizleme)](image-builder-overview.md) görüntü sürümü oluşturma otomatikleştirmek yardımcı olabilir, hatta güncelleme kullanabilirsiniz ve [varolan bir resim sürümünden yeni bir resim sürümü oluşturmak](image-builder-gallery-update-image-version.md). 

Şablonları kullanarak Paylaşılan Resim Galerisi kaynağını da oluşturabilirsiniz. Kullanılabilir birkaç Azure Quickstart Şablonu vardır: 

- [Paylaşılan Resim Galerisi Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan Resim Galerisinde Resim Tanımı Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan Resim Galerisinde Resim Sürümü Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Resim Sürümünden VM Oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Paylaşılan Resim Galerileri hakkında daha fazla bilgi için [Genel Bakış'a](shared-image-galleries.md)bakın. Sorunlarla karşılaştıysanız, [paylaşılan görüntü galerilerini sorun giderme](troubleshooting-shared-images.md)konusuna bakın.

