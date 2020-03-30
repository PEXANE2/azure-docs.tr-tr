---
title: 'Azure VPN Ağ Geçidi: Bir ağ geçidini silme: PowerShell'
description: Kaynak Yöneticisi dağıtım modelinde PowerShell'i kullanarak sanal ağ ağ ağ geçidini silin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.date: 02/07/2019
ms.author: cherylmc
ms.topic: conceptual
ms.openlocfilehash: f351f14796ec736bd5525f139a518c9a0dd3d19f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162115"
---
# <a name="delete-a-virtual-network-gateway-using-powershell"></a>PowerShell'i kullanarak sanal ağ ağ geçidini silme
> [!div class="op_single_selector"]
> * [Azure portalında](vpn-gateway-delete-vnet-gateway-portal.md)
> * [Powershell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [PowerShell (klasik)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

VPN ağ geçidi yapılandırması için sanal ağ ağ ağ geçidini silmek istediğinizde kullanabileceğiniz birkaç farklı yaklaşım vardır.

- Her şeyi silmek ve baştan başlamak istiyorsanız, bir test ortamında olduğu gibi, kaynak grubunu silebilirsiniz. Bir kaynak grubunu sildiğinizde, grup içindeki tüm kaynakları siler. Bu yöntem yalnızca kaynak grubundaki kaynaklardan herhangi birini tutmak istemiyorsanız önerilir. Bu yaklaşımı kullanarak yalnızca birkaç kaynağı seçarak silemezsiniz.

- Kaynak grubunuzdaki bazı kaynakları tutmak istiyorsanız, sanal ağ ağ ağ geçidini silerseniz biraz daha karmaşık hale gelir. Sanal ağ ağ ağ geçidini silmeden önce ağ geçidine bağlı olan kaynakları silmeniz gerekir. İzlediğiniz adımlar, oluşturduğunuz bağlantı türüne ve her bağlantı için bağımlı kaynaklara bağlıdır.

## <a name="before-beginning"></a>Başlamadan önce



### <a name="1-download-the-latest-azure-resource-manager-powershell-cmdlets"></a>1. En son Azure Kaynak Yöneticisi PowerShell cmdlets'i indirin.

Azure Kaynak Yöneticisi PowerShell cmdlets'in en son sürümünü indirin ve yükleyin. PowerShell cmdlets indirme ve yükleme hakkında daha fazla bilgi için [Azure PowerShell'i nasıl yükleyip yapılandırılatırınız.](/powershell/azure/overview)

### <a name="2-connect-to-your-azure-account"></a>2. Azure hesabınıza bağlanın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzAccount
```

Hesapla ilişkili abonelikleri kontrol edin.

```powershell
Get-AzSubscription
```

Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="delete-a-site-to-site-vpn-gateway"></a><a name="S2S"></a>Siteden Siteye VPN ağ geçidini silme

Bir S2S yapılandırması için sanal ağ ağ ağ geçidini silmek için, öncelikle sanal ağ ağ ağ geçidiyle ilgili her kaynağı silmeniz gerekir. Kaynaklar, bağımlılıklar nedeniyle belirli bir sırada silinmelidir. Aşağıdaki örneklerle çalışırken, bazı değerler belirtilmeli, diğer değerler ise çıktı sonucutur. Örneklerde aşağıdaki özel değerleri gösteri amacıyla kullanırız:

VNet adı: VNet1<br>
Kaynak Grubu adı: RG1<br>
Sanal ağ ağ geçidi adı: GW1<br>

Aşağıdaki adımlar Kaynak Yöneticisi dağıtım modeli için geçerlidir.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Silmek istediğiniz sanal ağ ağ ağ geçidini alın.

```powershell
$GW=get-Azvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Sanal ağ ağ geçidinin bağlantısı olup olmadığını kontrol edin.

```powershell
get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
$Conns=get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

### <a name="3-delete-all-connections"></a>3. Tüm bağlantıları silin.

Bağlantıların her birinin silinmesini onaylamanız istenebilir.

```powershell
$Conns | ForEach-Object {Remove-AzVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="4-delete-the-virtual-network-gateway"></a>4. Sanal ağ ağ ağ geçidini silin.

Ağ geçidinin silinmesini onaylamanız istenebilir. S2S yapılandırmanıza ek olarak bu VNet'te bir P2S yapılandırmanız varsa, sanal ağ ağ ağ geçidini silerseniz, uyarı yapmadan tüm P2S istemcilerinin bağlantısını otomatik olarak keser.


```powershell
Remove-AzVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Bu noktada, sanal ağ ağ ağ geçidiniz silindi. Artık kullanılmayan kaynakları silmek için sonraki adımları kullanabilirsiniz.

### <a name="5-delete-the-local-network-gateways"></a>5 Yerel ağ ağ geçitlerini silin.

İlgili yerel ağ ağ ağ geçitlerinin listesini alın.

```powershell
$LNG=Get-AzLocalNetworkGateway -ResourceGroupName "RG1" | where-object {$_.Id -In $Conns.LocalNetworkGateway2.Id}
```

Yerel ağ ağ ağ geçitlerini silin. Yerel ağ ağ geçidinin her birinin silinmesini onaylamanız istenebilir.

```powershell
$LNG | ForEach-Object {Remove-AzLocalNetworkGateway -Name $_.Name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="6-delete-the-public-ip-address-resources"></a>6. Genel IP adresi kaynaklarını silin.

Sanal ağ ağ geçidinin IP yapılandırmalarını alın.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

Bu sanal ağ ağ geçidi için kullanılan Genel IP adresi kaynaklarının listesini alın. Sanal ağ ağ ağ geçidi etkinse, iki Genel IP adresi görürsünüz.

```powershell
$PubIP=Get-AzPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Genel IP kaynaklarını silin.

```powershell
$PubIP | foreach-object {remove-AzpublicIpAddress -Name $_.Name -ResourceGroupName "RG1"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a>7. Ağ geçidi alt netini silin ve yapılandırmayı ayarlayın.

```powershell
$GWSub = Get-AzVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="delete-a-vnet-to-vnet-vpn-gateway"></a><a name="v2v"></a>VNet'den VNet VPN ağ geçidine silme

V2V yapılandırması için sanal ağ ağ ağ geçidini silmek için, öncelikle sanal ağ ağ ağ geçidiyle ilgili her kaynağı silmeniz gerekir. Kaynaklar, bağımlılıklar nedeniyle belirli bir sırada silinmelidir. Aşağıdaki örneklerle çalışırken, bazı değerler belirtilmeli, diğer değerler ise çıktı sonucutur. Örneklerde aşağıdaki özel değerleri gösteri amacıyla kullanırız:

VNet adı: VNet1<br>
Kaynak Grubu adı: RG1<br>
Sanal ağ ağ geçidi adı: GW1<br>

Aşağıdaki adımlar Kaynak Yöneticisi dağıtım modeli için geçerlidir.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Silmek istediğiniz sanal ağ ağ ağ geçidini alın.

```powershell
$GW=get-Azvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Sanal ağ ağ geçidinin bağlantısı olup olmadığını kontrol edin.

```powershell
get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
Sanal ağ ağ ağ geçidine farklı bir kaynak grubunun parçası olan başka bağlantılar da olabilir. Her ek kaynak grubunda ek bağlantılar olup olup deyanabın. Bu örnekte, RG2'den gelen bağlantıları kontrol ediyoruz. Bunu, sanal ağ ağ geçidine bağlantısı olabilecek her kaynak grubu için çalıştırın.

```powershell
get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG2" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
```

### <a name="3-get-the-list-of-connections-in-both-directions"></a>3. Her iki yöndeki bağlantıların listesini alın.

Bu bir VNet-to-VNet yapılandırması olduğundan, her iki yöndeki bağlantıların listesine ihtiyacınız vardır.

```powershell
$ConnsL=get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
Bu örnekte, RG2'den gelen bağlantıları kontrol ediyoruz. Bunu, sanal ağ ağ geçidine bağlantısı olabilecek her kaynak grubu için çalıştırın.

```powershell
 $ConnsR=get-Azvirtualnetworkgatewayconnection -ResourceGroupName "<NameOfResourceGroup2>" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
 ```

### <a name="4-delete-all-connections"></a>4. Tüm bağlantıları silin.

Bağlantıların her birinin silinmesini onaylamanız istenebilir.

```powershell
$ConnsL | ForEach-Object {Remove-AzVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
$ConnsR | ForEach-Object {Remove-AzVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="5-delete-the-virtual-network-gateway"></a>5. Sanal ağ ağ ağ geçidini silin.

Sanal ağ ağ geçidinin silinmesini onaylamanız istenebilir. V2V yapılandırmanıza ek olarak VNet'lerinizde P2S yapılandırmaları varsa, sanal ağ ağ geçitlerini silerseniz, uyarı yapmadan tüm P2S istemcilerinin bağlantısını otomatik olarak keser.

```powershell
Remove-AzVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Bu noktada, sanal ağ ağ ağ geçidiniz silindi. Artık kullanılmayan kaynakları silmek için sonraki adımları kullanabilirsiniz.

### <a name="6-delete-the-public-ip-address-resources"></a>6. Genel IP adresi kaynaklarını silme

Sanal ağ ağ geçidinin IP yapılandırmalarını alın.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

Bu sanal ağ ağ geçidi için kullanılan Genel IP adresi kaynaklarının listesini alın. Sanal ağ ağ ağ geçidi etkinse, iki Genel IP adresi görürsünüz.

```powershell
$PubIP=Get-AzPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Genel IP kaynaklarını silin. Genel IP'nin silinmesini onaylamanız istenebilir.

```powershell
$PubIP | foreach-object {remove-AzpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a>7. Ağ geçidi alt netini silin ve yapılandırmayı ayarlayın.

```powershell
$GWSub = Get-AzVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="delete-a-point-to-site-vpn-gateway"></a><a name="deletep2s"></a>Bir Noktaya Sayfa VPN ağ geçidini silme

Bir P2S yapılandırması için sanal ağ ağ ağ geçidini silmek için, öncelikle sanal ağ ağ ağ geçidiyle ilgili her kaynağı silmeniz gerekir. Kaynaklar, bağımlılıklar nedeniyle belirli bir sırada silinmelidir. Aşağıdaki örneklerle çalışırken, bazı değerler belirtilmeli, diğer değerler ise çıktı sonucutur. Örneklerde aşağıdaki özel değerleri gösteri amacıyla kullanırız:

VNet adı: VNet1<br>
Kaynak Grubu adı: RG1<br>
Sanal ağ ağ geçidi adı: GW1<br>

Aşağıdaki adımlar Kaynak Yöneticisi dağıtım modeli için geçerlidir.


>[!NOTE]
> VPN ağ geçidini sildiğinizde, bağlı tüm istemcilerin uyarı yapılmadan VNet'ten bağlantısı kesilir.
>
>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Silmek istediğiniz sanal ağ ağ ağ geçidini alın.

```powershell
$GW=get-Azvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-delete-the-virtual-network-gateway"></a>2. Sanal ağ ağ ağ geçidini silin.

Sanal ağ ağ geçidinin silinmesini onaylamanız istenebilir.

```powershell
Remove-AzVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Bu noktada, sanal ağ ağ ağ geçidiniz silindi. Artık kullanılmayan kaynakları silmek için sonraki adımları kullanabilirsiniz.

### <a name="3-delete-the-public-ip-address-resources"></a>3. Genel IP adresi kaynaklarını silme

Sanal ağ ağ geçidinin IP yapılandırmalarını alın.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

Bu sanal ağ ağ geçidi için kullanılan Genel IP adreslerinin listesini alın. Sanal ağ ağ ağ geçidi etkinse, iki Genel IP adresi görürsünüz.

```powershell
$PubIP=Get-AzPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Genel IP'leri silin. Genel IP'nin silinmesini onaylamanız istenebilir.

```powershell
$PubIP | foreach-object {remove-AzpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="4-delete-the-gateway-subnet-and-set-the-configuration"></a>4. Ağ geçidi alt netini silin ve yapılandırmayı ayarlayın.

```powershell
$GWSub = Get-AzVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="delete-a-vpn-gateway-by-deleting-the-resource-group"></a><a name="delete"></a>Kaynak grubunu silerek VPN ağ geçidini silme

Kaynaklarınızdan herhangi birini kaynak grubunda tutmakla ilgilenmiyorsanız ve baştan başlamak istiyorsanız, tüm kaynak grubunu silebilirsiniz. Bu her şeyi kaldırmak için hızlı bir yoldur. Aşağıdaki adımlar yalnızca Kaynak Yöneticisi dağıtım modeli için geçerlidir.

### <a name="1-get-a-list-of-all-the-resource-groups-in-your-subscription"></a>1. Aboneliğinizdeki tüm kaynak gruplarının listesini alın.

```powershell
Get-AzResourceGroup
```

### <a name="2-locate-the-resource-group-that-you-want-to-delete"></a>2. Silmek istediğiniz kaynak grubunu bulun.

Silmek istediğiniz kaynak grubunu bulun ve bu kaynak grubundaki kaynak listesini görüntüleyin. Örnekte, kaynak grubunun adı RG1'dir. Tüm kaynakların listesini almak için örneği değiştirin.

```powershell
Find-AzResource -ResourceGroupNameContains RG1
```

### <a name="3-verify-the-resources-in-the-list"></a>3. Listedeki kaynakları doğrulayın.

Liste döndürüldüğünde, kaynak grubundaki tüm kaynakların yanı sıra kaynak grubunun kendisini de silmek istediğinizi doğrulamak için listeyi gözden geçirin. Kaynak grubundaki bazı kaynakları tutmak istiyorsanız, ağ geçidinizi silmek için bu makalenin önceki bölümlerindeki adımları kullanın.

### <a name="4-delete-the-resource-group-and-resources"></a>4. Kaynak grubunu ve kaynakları silin.

Kaynak grubunu ve kaynak grubunda bulunan tüm kaynağı silmek için, örneği değiştirin ve çalıştırın.

```powershell
Remove-AzResourceGroup -Name RG1
```

### <a name="5-check-the-status"></a>5. Durumu kontrol edin.

Azure'un tüm kaynakları silmesi biraz zaman alır. Bu cmdlet'i kullanarak kaynak grubunuzun durumunu kontrol edebilirsiniz.

```powershell
Get-AzResourceGroup -ResourceGroupName RG1
```

Döndürülen sonuç 'Başarılı' olarak gösterilmektedir.

```
ResourceGroupName : RG1
Location          : eastus
ProvisioningState : Succeeded
```
