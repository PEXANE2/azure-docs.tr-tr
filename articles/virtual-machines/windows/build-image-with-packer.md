---
title: Azure'da Packer ile Windows VM Görüntüleri nasıl oluşturulur?
description: Azure'da Windows sanal makinelerinin görüntülerini oluşturmak için Packer'ı nasıl kullanacağınızı öğrenin
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
ms.date: 02/22/2019
ms.author: cynthn
ms.openlocfilehash: cb81cbb12605a9d4b8870aab4bb461c8af079cf5
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460758"
---
# <a name="how-to-use-packer-to-create-windows-virtual-machine-images-in-azure"></a>Azure'da Windows sanal makine resimleri oluşturmak için Packer nasıl kullanılır?
Azure'daki her sanal makine (VM), Windows dağıtımını ve işletim sistemi sürümünü tanımlayan bir görüntüden oluşturulur. Görüntüler önceden yüklenmiş uygulamaları ve yapılandırmaları içerebilir. Azure Marketi, en yaygın işletim sistemi ve uygulama ortamları için birçok birinci ve üçüncü taraf görüntü sağlar veya gereksinimlerinize göre uyarlanmış kendi özel resimlerinizi oluşturabilirsiniz. Bu makalede, Azure'da özel görüntüleri tanımlamak ve oluşturmak için açık kaynak araç [Packer'ın](https://www.packer.io/) nasıl kullanılacağı ayrıntılı olarak açıklanmaktadır.

Bu makale en son 21/21/2019 tarihinde [Az PowerShell modülü](https://docs.microsoft.com/powershell/azure/install-az-ps) sürüm 1.3.0 ve [Packer](https://www.packer.io/docs/install/index.html) sürümü 1.3.4 kullanılarak test edilmiştir.

> [!NOTE]
> Azure artık kendi özel resimlerinizi tanımlamak ve oluşturmak için Azure Image Builder (önizleme) hizmetine sahiptir. Azure Image Builder Packer üzerine kuruludur, böylece mevcut Packer kabuk provizyoner komut dosyalarınızı da onunla birlikte kullanabilirsiniz. Azure Image Builder'a başlamak için [bkz.](image-builder.md)

## <a name="create-azure-resource-group"></a>Azure kaynak grubu oluşturma
Oluşturma işlemi sırasında Packer, kaynak VM'yi oluştururken geçici Azure kaynakları oluşturur. Bu kaynak VM'yi görüntü olarak kullanmak üzere yakalamak için bir kaynak grubu tanımlamanız gerekir. Packer yapı işleminden elde edilen çıktı bu kaynak grubunda depolanır.

[New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)ile bir kaynak grubu oluşturun. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurepowershell
$rgName = "myResourceGroup"
$location = "East US"
New-AzResourceGroup -Name $rgName -Location $location
```

## <a name="create-azure-credentials"></a>Azure kimlik bilgilerini oluşturma
Packer, bir hizmet ilkesi ni kullanarak Azure ile kimlik doğrulaması verir. Azure hizmet prensibi, Packer gibi uygulamalar, hizmetler ve otomasyon araçlarıyla kullanabileceğiniz bir güvenlik kimliğidir. Hizmet sorumlusunun Azure'da hangi işlemleri gerçekleştirebileceğine göre izinleri denetler ve tanımlarsınız.

[New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal) ile bir hizmet sorumlusu oluşturun ve [Yeni-AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment)ile kaynak oluşturmak ve yönetmek için hizmet sorumlusuiçin izinler atayın. İhtiyaçların `-DisplayName` değeri benzersiz olmalıdır; gerektiğinde kendi değerinizle değiştirin.  

```azurepowershell
$sp = New-AzADServicePrincipal -DisplayName "PackerServicePrincipal"
$BSTR = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($sp.Secret)
$plainPassword = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($BSTR)
New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Sonra şifre ve uygulama kimliği çıktı.

```powershell
$plainPassword
$sp.ApplicationId
```


Azure'da kimlik doğrulaması yapmak [için, Get-AzSubscription](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription)ile Azure kiracınızı ve abonelik kimliklerinizi de edinmeniz gerekir:

```powershell
Get-AzSubscription
```


## <a name="define-packer-template"></a>Packer şablonu tanımla
Resim oluşturmak için JSON dosyası olarak bir şablon oluşturursunuz. Şablonda, gerçek yapı işlemini gerçekleştiren oluşturucuları ve geçici olarak tanımlarsınız. Packer'ın Azure için bir [oluşturucusu](https://www.packer.io/docs/builders/azure.html) vardır ve önceki adımda oluşturulan hizmet temel kimlik bilgileri gibi Azure kaynaklarını tanımlamanızı sağlar.

*windows.json* adlı bir dosya oluşturun ve aşağıdaki içeriği yapıştırın. Aşağıdakiler için kendi değerlerinizi girin:

| Parametre                           | Nereden temin edinilsin |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | Hizmet ana kimliğini`$sp.applicationId` |
| *client_secret*                     | Otomatik oluşturulan parolayı`$plainPassword` |
| *tenant_id*                         | Komuttan `$sub.TenantId` çıkış |
| *subscription_id*                   | Komuttan `$sub.SubscriptionId` çıkış |
| *managed_image_resource_group_name* | İlk adımda oluşturduğunuz kaynak grubunun adı |
| *managed_image_name*                | Oluşturulan yönetilen disk görüntüsünün adı |

```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "client_secret": "ppppppp-pppp-pppp-pppp-ppppppppppp",
    "tenant_id": "zzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz",
    "subscription_id": "yyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyy",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Windows",
    "image_publisher": "MicrosoftWindowsServer",
    "image_offer": "WindowsServer",
    "image_sku": "2016-Datacenter",

    "communicator": "winrm",
    "winrm_use_ssl": true,
    "winrm_insecure": true,
    "winrm_timeout": "5m",
    "winrm_username": "packer",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "type": "powershell",
    "inline": [
      "Add-WindowsFeature Web-Server",
      "& $env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /quiet /quit",
      "while($true) { $imageState = Get-ItemProperty HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Setup\\State | Select ImageState; if($imageState.ImageState -ne 'IMAGE_STATE_GENERALIZE_RESEAL_TO_OOBE') { Write-Output $imageState.ImageState; Start-Sleep -s 10  } else { break } }"
    ]
  }]
}
```

Bu şablon bir Windows Server 2016 VM oluşturur, IIS yükler, sonra Sysprep ile VM genelleştirir. IIS yüklemesi, powershell geçici sini ek komutları çalıştırmak için nasıl kullanabileceğinizi gösterir. Son Packer görüntüsü daha sonra gerekli yazılım yükleme ve yapılandırma içerir.


## <a name="build-packer-image"></a>Packer görüntüsü oluşturun
Yerel makinenizde Packer zaten yüklü değilseniz, [Packer yükleme yönergelerini uygulayın.](https://www.packer.io/docs/install/index.html)

Cmd istemini açarak ve Packer şablon dosyanızı aşağıdaki gibi belirterek resmi oluşturun:

```
./packer build windows.json
```

Önceki komutlardan çıktı örneği aşağıdaki gibidir:

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> task : Image deployment
==> azure-arm:  ->> dept : Engineering
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the certificate’s URL ...
==> azure-arm:  -> Key Vault Name        : ‘pkrkvpq0mthtbtt’
==> azure-arm:  -> Key Vault Secret Name : ‘packerKeyVaultSecret’
==> azure-arm:  -> Certificate URL       : ‘https://pkrkvpq0mthtbtt.vault.azure.net/secrets/packerKeyVaultSecret/8c7bd823e4fa44e1abb747636128adbb'
==> azure-arm: Setting the certificate’s URL ...
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.55.35’
==> azure-arm: Waiting for WinRM to become available...
==> azure-arm: Connected to WinRM!
==> azure-arm: Provisioning with Powershell...
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-powershell-provisioner902510110
    azure-arm: #< CLIXML
    azure-arm:
    azure-arm: Success Restart Needed Exit Code      Feature Result
    azure-arm: ------- -------------- ---------      --------------
    azure-arm: True    No             Success        {Common HTTP Features, Default Document, D...
    azure-arm: <Objs Version=“1.1.0.1” xmlns=“http://schemas.microsoft.com/powershell/2004/04"><Obj S=“progress” RefId=“0"><TN RefId=“0”><T>System.Management.Automation.PSCustomObject</T><T>System.Object</T></TN><MS><I64 N=“SourceId”>1</I64><PR N=“Record”><AV>Preparing modules for first use.</AV><AI>0</AI><Nil /><PI>-1</PI><PC>-1</PC><T>Completed</T><SR>-1</SR><SD> </SD></PR></MS></Obj></Objs>
==> azure-arm: Querying the machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-pq0mthtbtt/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Compute Name              : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```

Packer'ın VM'yi oluşturması, geçici maddeyi çalıştırması ve dağıtımı temizlemesi birkaç dakika sürer.


## <a name="create-a-vm-from-the-packer-image"></a>Packer görüntüsünden Bir VM oluşturma
Artık [Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm)ile Görüntünüzden bir VM oluşturabilirsiniz. Destekleyen ağ kaynakları zaten yoksa oluşturulur. İstendiğinde, VM'de oluşturulacak bir yönetim kullanıcı adı ve parola girin. Aşağıdaki örnek *myPackerImage* *myVM* adlı bir VM oluşturur:

```powershell
New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80 `
    -Image "myPackerImage"
```

Packer resminizden farklı bir kaynak grubunda veya bölgesinde VM oluşturmak istiyorsanız, görüntü adı yerine görüntü kimliğini belirtin. [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/Get-AzImage)ile resim kimliğini alabilirsiniz.

Packer resminizden VM'yi oluşturmak birkaç dakika nızı alır.


## <a name="test-vm-and-webserver"></a>Test VM ve web sunucusu
[Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress)ile VM'nizin genel IP adresini edinin. Aşağıdaki örnek, daha önce oluşturulan *myPublicIP* için IP adresini alır:

```powershell
Get-AzPublicIPAddress `
    -ResourceGroupName $rgName `
    -Name "myPublicIPAddress" | select "IpAddress"
```

Packer geçici maddesinden IIS yüklemesini içeren VM'nizi görmek için ortak IP adresini bir web tarayıcısına girin.

![Varsayılan IIS sitesi](./media/build-image-with-packer/iis.png) 


## <a name="next-steps"></a>Sonraki adımlar
[Azure Image Builder](image-builder.md)ile varolan Packer geçici komut dosyalarını da kullanabilirsiniz.
