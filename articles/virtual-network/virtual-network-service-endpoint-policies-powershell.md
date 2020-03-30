---
title: Azure Depolama ile veri sızma kısıtlama - Azure PowerShell
description: Bu makalede, Azure PowerShell'i kullanarak sanal ağ hizmeti uç nokta ilkeleriyle sanal ağ veri filtrelemeyi Azure Depolama kaynaklarıyla nasıl sınırlandırabileceğinizi ve sınırlandırabileceğinizi öğreneceksiniz.
services: virtual-network
documentationcenter: virtual-network
author: RDhillon
manager: narayan
editor: ''
tags: azure-resource-manager
Customer intent: I want only resources in a virtual network subnet to access an Azure PaaS resource, such as an Azure Storage account.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: rdhillon
ms.custom: ''
ms.openlocfilehash: 673431e2ddfc9a641bb1c640891daac79350cb3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78253032"
---
# <a name="manage-data-exfiltration-to-azure-storage-accounts-with-virtual-network-service-endpoint-policies-using-azure-powershell"></a>Azure PowerShell'i kullanarak Sanal ağ hizmeti bitiş noktası ilkeleriyle Azure Depolama hesaplarına veri sızma larını yönetme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Sanal ağ hizmeti bitiş noktası ilkeleri, hizmet bitiş noktaları üzerinden sanal ağ içinden Azure Depolama hesaplarına erişim denetimi uygulamanızı sağlar. Bu, iş yüklerinizi güvence altına almanın, hangi depolama hesaplarına izin verildiğini ve veri sızmasına izin verilen yerlerde yöneticilik için bir anahtardır.
Bu makalede şunları öğreneceksiniz:

* Sanal ağ oluşturun.
* Azure Depolama için bir alt ağ ekleyin ve hizmet bitiş noktasını etkinleştirin.
* İki Azure Depolama hesabı oluşturun ve yukarıda oluşturulan alt ağdan ağa erişime izin verin.
* Yalnızca depolama hesaplarından birine erişime izin vermek için bir hizmet bitiş noktası ilkesi oluşturun.
* Alt ağa sanal bir makine (VM) dağıtın.
* Alt ağdan izin verilen depolama hesabına erişimi onaylayın.
* Alt ağdan izin verilmeyen depolama hesabına erişimin reddedildiğini onaylayın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Sanal ağ oluşturmadan önce, sanal ağ ve bu makalede oluşturulan diğer tüm kaynaklar için bir kaynak grubu oluşturmanız gerekir. [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)ile bir kaynak grubu oluşturun. Aşağıdaki örnek, *myResourceGroup*adlı bir kaynak grubu oluşturur: 

```azurepowershell-interactive
New-AzResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS
```

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile sanal ağ oluşturun. Aşağıdaki örnek, adres öneki *10.0.0.0/16*ile *myVirtualNetwork* adlı bir sanal ağ oluşturur.

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

## <a name="enable-a-service-endpoint"></a>Hizmet uç noktasını girin

Sanal ağda bir alt ağ oluşturun. Bu örnekte, *Microsoft.Storage*için bir hizmet bitiş noktası ile *Özel* adlı bir alt ağ oluşturulur: 

```azurepowershell-interactive
$subnetConfigPrivate = Add-AzVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork `
  -ServiceEndpoint Microsoft.Storage

$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="restrict-network-access-for-the-subnet"></a>Alt ağ için ağ erişimini kısıtlama

[New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)ile ağ güvenlik grubu güvenlik kuralları oluşturun. Aşağıdaki kural, Azure Depolama hizmetine atanan genel IP adreslerine giden erişime izin verir: 

```azurepowershell-interactive
$rule1 = New-AzNetworkSecurityRuleConfig `
  -Name Allow-Storage-All `
  -Access Allow `
  -DestinationAddressPrefix Storage `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 100 -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

Aşağıdaki kural, tüm genel IP adreslerine erişimi reddeder. Önceki kural, Azure Depolama'nın genel IP adreslerine erişime izin veren daha yüksek önceliği nedeniyle bu kuralı geçersiz kılar.

```azurepowershell-interactive
$rule2 = New-AzNetworkSecurityRuleConfig `
  -Name Deny-Internet-All `
  -Access Deny `
  -DestinationAddressPrefix Internet `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 110 -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

Aşağıdaki kural, uzak masaüstü protokolü (RDP) trafiğinin her yerden alt ağa bağlanmasına izin verir. Uzak masaüstü bağlantılarına alt ağa izin verilir, böylece daha sonraki bir adımda bir kaynağa ağ erişimini onaylayabilirsiniz.

```azurepowershell-interactive
$rule3 = New-AzNetworkSecurityRuleConfig `
  -Name Allow-RDP-All `
  -Access Allow `
  -DestinationAddressPrefix VirtualNetwork `
  -DestinationPortRange 3389 `
  -Direction Inbound `
  -Priority 120 `
  -Protocol * `
  -SourceAddressPrefix * `
  -SourcePortRange *
```

[New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup)ile bir ağ güvenlik grubu oluşturun. Aşağıdaki örnek, *myNsgPrivate*adlı bir ağ güvenlik grubu oluşturur.

```azurepowershell-interactive
$nsg = New-AzNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myNsgPrivate `
  -SecurityRules $rule1,$rule2,$rule3
```

Ağ güvenlik grubunu [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) ile *Özel* alt ağla ilişkilendirin ve alt ağ yapılandırmasını sanal ağa yazın. Aşağıdaki örnek, *myNsgPrivate* ağ güvenlik grubunu *Özel* alt ağla ilişkilendirer:

```azurepowershell-interactive
Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.0.0/24 `
  -ServiceEndpoint Microsoft.Storage `
  -NetworkSecurityGroup $nsg

$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="restrict-network-access-to-azure-storage-accounts"></a>Azure Depolama hesaplarına ağ erişimini kısıtlama

Hizmet uç noktaları için etkinleştirilmiş Azure hizmetleri aracılığıyla oluşturulan kaynaklara ağ erişimini kısıtlamak için gereken adımlar, hizmetler arasında farklılık gösterir. Bir hizmete yönelik belirli adımlar için ilgili hizmetin belgelerine bakın. Bu makalenin geri kalanı, örnek olarak bir Azure Depolama hesabının ağ erişimini kısıtlamaya yönelik adımları içerir.

### <a name="create-two-storage-accounts"></a>İki depolama hesabı oluşturma

[New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount)ile bir Azure depolama hesabı oluşturun.

```azurepowershell-interactive
$storageAcctName1 = 'allowedaccount'

New-AzStorageAccount `
  -Location EastUS `
  -Name $storageAcctName1 `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Depolama hesabı oluşturulduktan sonra, depolama hesabı nın anahtarını [Get-AzStorageAccountKey](/powershell/module/az.storage/get-azstorageaccountkey)ile bir değişkene alın:

```azurepowershell-interactive
$storageAcctKey1 = (Get-AzStorageAccountKey -ResourceGroupName myResourceGroup -AccountName $storageAcctName1).Value[0]
```

Anahtar, daha sonraki bir adımda bir dosya paylaşımı oluşturmak için kullanılır. Dosya `$storageAcctKey` paylaşımını VM'deki bir sürücüyle eşlediğinizde daha sonraki bir adımda el ile girmeniz gerektiğinden, değeri girin ve not edin.

Şimdi ikinci bir depolama hesabı oluşturmak için yukarıdaki adımları tekrarlayın.

```azurepowershell-interactive
$storageAcctName2 = 'notallowedaccount'

New-AzStorageAccount `
  -Location EastUS `
  -Name $storageAcctName2 `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Ayrıca, daha sonra bir dosya paylaşımı oluşturmak için kullanmak için bu hesaptan depolama hesabı anahtarını alın.

```azurepowershell-interactive
$storageAcctKey2 = (Get-AzStorageAccountKey -ResourceGroupName myResourceGroup -AccountName $storageAcctName2).Value[0]
```

### <a name="create-a-file-share-in-each-of-the-storage-account"></a>Depolama hesabının her birinde dosya paylaşımı oluşturma

[New-AzStorageContext](/powershell/module/az.storage/new-AzStoragecontext)ile depolama hesabınız için bir bağlam ve anahtar oluşturun. Bağlam, depolama hesabı adı ve hesap anahtarını saklar:

```azurepowershell-interactive
$storageContext1 = New-AzStorageContext $storageAcctName1 $storageAcctKey1

$storageContext2 = New-AzStorageContext $storageAcctName2 $storageAcctKey2
```

[New-AzStorageShare](/powershell/module/az.storage/new-azstorageshare)ile dosya paylaşımı oluşturun:

```azurepowershell-interactive
$share1 = New-AzStorageShare my-file-share -Context $storageContext1

$share2 = New-AzStorageShare my-file-share -Context $storageContext2
```

### <a name="deny-all-network-access-to-a-storage-accounts"></a>Depolama hesaplarına tüm ağ erişimini reddetme

Varsayılan olarak, depolama hesapları herhangi bir ağdaki istemcilerden gelen ağ bağlantılarını kabul eder. Seçili ağlara erişimi sınırlamak için varsayılan eylemi [Update-AzStorageAccountNetworkRuleSet](/powershell/module/az.storage/update-azstorageaccountnetworkruleset)ile *Reddet* olarak değiştirin. Ağ erişimi reddedildiğinde depolama hesabı hiçbir ağdan erişilebilir olmaz.

```azurepowershell-interactive
Update-AzStorageAccountNetworkRuleSet `
  -ResourceGroupName myresourcegroup `
  -Name $storageAcctName1 `
  -DefaultAction Deny

Update-AzStorageAccountNetworkRuleSet  `
  -ResourceGroupName myresourcegroup `
  -Name $storageAcctName2 `
  -DefaultAction Deny
```

### <a name="enable-network-access-only-from-the-vnet-subnet"></a>Yalnızca VNet alt sonundan ağ erişimini etkinleştirme

[Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) ile oluşturulan sanal ağı alın ve sonra [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig)ile bir değişken içine özel subnet nesnealmak:

```azurepowershell-interactive
$privateSubnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Name myVirtualNetwork `
  | Get-AzVirtualNetworkSubnetConfig -Name Private
```

[Add-AzStorageAccountNetworkRule](/powershell/module/az.network/add-aznetworksecurityruleconfig)ile *Özel* alt ağdan depolama hesabına ağ erişimine izin verin.

```azurepowershell-interactive
Add-AzStorageAccountNetworkRule `
  -ResourceGroupName myresourcegroup `
  -Name $storageAcctName1 `
  -VirtualNetworkResourceId $privateSubnet.Id

Add-AzStorageAccountNetworkRule `
  -ResourceGroupName myresourcegroup `
  -Name $storageAcctName2 `
  -VirtualNetworkResourceId $privateSubnet.Id
```

## <a name="apply-policy-to-allow-access-to-valid-storage-account"></a>Geçerli depolama hesabına erişime izin vermek için ilke uygulayın

Sanal ağdaki kullanıcıların yalnızca güvenli ve izin verilen Azure Depolama hesaplarına erişebilmesini sağlamak için, tanımda izin verilen depolama hesaplarının listesini içeren bir Hizmet bitiş noktası ilkesi oluşturabilirsiniz. Bu ilke daha sonra hizmet bitiş noktaları üzerinden depolamaya bağlı sanal ağ alt ağına uygulanır.

### <a name="create-a-service-endpoint-policy"></a>Hizmet bitiş noktası ilkesi oluşturma

Bu bölümde, hizmet bitiş noktası üzerinden erişim için izin verilen kaynakların listesi ile ilke tanımı oluşturur

İlk (izin verilen) depolama hesabı için kaynak kimliğini alma 

```azurepowershell-interactive
$resourceId = (Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name $storageAcctName1).id
```

Yukarıdaki kaynağa izin vermek için ilke tanımını oluşturma

```azurepowershell-interactive
$policyDefinition = New-AzServiceEndpointPolicyDefinition -Name mypolicydefinition `
  -Description "Service Endpoint Policy Definition" `
  -Service "Microsoft.Storage" `
  -ServiceResource $resourceId
```

Yukarıda oluşturulan ilke tanımını kullanarak hizmet bitiş noktası ilkesini oluşturma

```azurepowershell-interactive
$sepolicy = New-AzServiceEndpointPolicy -ResourceGroupName myresourcegroup `
  -Name mysepolicy -Location EastUS
  -ServiceEndpointPolicyDefinition $policyDefinition
```

### <a name="associate-the-service-endpoint-policy-to-the-virtual-network-subnet"></a>Hizmet bitiş noktası ilkesini sanal ağ alt ağıyla ilişkilendirme

Hizmet bitiş noktası ilkesini oluşturduktan sonra, hedef alt ağile Azure Depolama için hizmet bitiş noktası yapılandırmasıyla ilişkilendireceksiniz.

```azurepowershell-interactive
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.0.0/24 `
  -NetworkSecurityGroup $nsg `
  -ServiceEndpoint Microsoft.Storage `
  -ServiceEndpointPolicy $sepolicy

$virtualNetwork | Set-AzVirtualNetwork
```
## <a name="validate-access-restriction-to-azure-storage-accounts"></a>Azure Depolama hesaplarına erişim kısıtlamasını doğrulama

### <a name="deploy-the-virtual-machine"></a>Sanal makineyi dağıtma

Bir depolama hesabına ağ erişimini test etmek için alt ağa bir VM dağıtın.

[New-AzVM](/powershell/module/az.compute/new-azvm)ile *Özel* alt ağda sanal bir makine oluşturun. Sonraki komutu çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır. `-AsJob` seçeneği, sonraki adıma devam edebilmeniz için arka planda sanal makineyi oluşturur.

```azurepowershell-interactive
New-AzVm -ResourceGroupName myresourcegroup `
  -Location "East US" `
  -VirtualNetworkName myVirtualNetwork `
  -SubnetName Private `
  -Name "myVMPrivate" -AsJob
```

Aşağıdaki örnek çıktıya benzer çıktı döndürülür:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM
```

### <a name="confirm-access-to-the-allowed-storage-account"></a>*İzin verilen* depolama hesabına erişimi onaylama

Bir VM'nin genel IP adresini döndürmek için [Get-AzPublicIpAddress'i](/powershell/module/az.network/get-azpublicipaddress) kullanın. Aşağıdaki örnek, *myVmPrivate* VM'nin genel IP adresini döndürür:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Aşağıdaki komuttaki `<publicIpAddress>` öğesini, önceki komuttan döndürülen genel IP adresiyle değiştirin ve sonra aşağıdaki komutu girin:

```powershell
mstsc /v:<publicIpAddress>
```

Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve bilgisayarınıza indirilir. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir. **Tamam'ı**seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarıyı alırsanız, bağlantıya devam etmek için **Evet** veya **Devam**et'i seçin.

*myVmPrivate* VM'de, PowerShell kullanarak Z'yi kullanmak için izin verilen depolama hesabından Azure dosya paylaşımını haritala. 

```powershell
$acctKey = ConvertTo-SecureString -String $storageAcctKey1 -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList ("Azure\allowedaccount"), $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\allowedaccount.file.core.windows.net\my-file-share" -Credential $credential
```

PowerShell aşağıdaki örnek çıktıya benzer bir çıktı döndürür:

```powershell
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
Z                                      FileSystem    \\allowedaccount.file.core.windows.net\my-f...
```

Z sürücüsüne başarıyla eşlenen Azure dosya paylaşımı.

*myVmPrivate* VM ile uzak masaüstü oturumunu kapatın.

### <a name="confirm-access-is-denied-to-non-allowed-storage-account"></a>*İzin verilmeyen* depolama hesabına erişimin reddedildiğini onaylayın

Aynı *myVmPrivate* VM'de, X'i sürmek için Azure dosya paylaşımını eşlemeye çalış. 

```powershell
$acctKey = ConvertTo-SecureString -String $storageAcctKey1 -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\notallowedaccount", $acctKey
New-PSDrive -Name X -PSProvider FileSystem -Root "\\notallowedaccount.file.core.windows.net\my-file-share" -Credential $credential
```

Paylaşıma erişim reddedilir ve bir `New-PSDrive : Access is denied` hata alırsınız. Depolama hesabı *izin verilmeyen hesap* hizmet bitiş noktası ilkesinde izin verilen kaynaklar listesinde olmadığından erişim reddedilir. 

*myVmPublic* VM ile uzak masaüstü oturumunu kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup) kullanabilirsiniz:

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Depolama'ya bir Azure sanal ağ hizmeti bitiş noktası üzerinden bir hizmet bitiş noktası ilkesi uyguladınız. Azure Depolama hesapları ve sanal ağ alt ağından yalnızca belirli depolama hesaplarına sınırlı ağ erişimi (ve dolayısıyla başkalarını reddettiniz). Hizmet bitiş noktası ilkeleri hakkında daha fazla bilgi edinmek için [Hizmet uç noktaları ilkelerine genel bakış'e](virtual-network-service-endpoint-policies-overview.md)bakın.
