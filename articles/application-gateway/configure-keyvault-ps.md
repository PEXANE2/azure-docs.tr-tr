---
title: Key Vault sertifikaları ile SSL sonlandırma yapılandırma - PowerShell
titleSuffix: Azure Application Gateway
description: HTTPS özellikli dinleyicilere bağlı sunucu sertifikaları için Azure Uygulama Ağ Geçidi'ni Key Vault ile nasıl entegre edebileceğinizi öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 02/27/2020
ms.author: victorh
ms.openlocfilehash: 15e10d34120ab5475f241235bbebeb0c7689ca14
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371220"
---
# <a name="configure-ssl-termination-with-key-vault-certificates-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak SSL sonlandırmaişlemini Key Vault sertifikalarıyla yapılandırın

[Azure Key Vault,](../key-vault/key-vault-overview.md) sırları, anahtarları ve SSL sertifikalarını korumak için kullanabileceğiniz platform tarafından yönetilen gizli bir mağazadır. Azure Application Gateway, HTTPS özellikli dinleyicilere bağlı sunucu sertifikaları için Key Vault ile tümleştirmeyi destekler. Bu destek Uygulama Ağ Geçidi v2 SKU ile sınırlıdır.

Daha fazla bilgi için [Key Vault sertifikalarıile SSL sonlandırma'ya](key-vault-certs.md)bakın.

Bu makalede, anahtar kasanızı SSL sonlandırma sertifikaları için uygulama ağ geçidinizle tümleştirmek için azure PowerShell komut dosyasının nasıl kullanılacağı gösterilmektedir.

Bu makalede, Azure PowerShell modülü sürümü 1.0.0 veya sonrası gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). Bu makaledeki komutları çalıştırmak için, çalıştırarak `Connect-AzAccount`Azure ile bir bağlantı oluşturmanız da gerekir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce ManagedServiceIdentity modüllerini yüklemiş olmalısınız:

```azurepowershell
Install-Module -Name Az.ManagedServiceIdentity
Connect-AzAccount
Select-AzSubscription -Subscription <your subscription>
```

## <a name="example-script"></a>Örnek betik

### <a name="set-up-variables"></a>Değişkenleri ayarlama

```azurepowershell
$rgname = "KeyVaultTest"
$location = "East US"
$kv = "TestKeyVaultAppGw"
$appgwName = "AppGwKVIntegration"
```

### <a name="create-a-resource-group-and-a-user-managed-identity"></a>Kaynak grubu ve kullanıcı tarafından yönetilen bir kimlik oluşturma

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location
$identity = New-AzUserAssignedIdentity -Name "appgwKeyVaultIdentity" `
  -Location $location -ResourceGroupName $rgname
```

### <a name="create-a-key-vault-policy-and-certificate-to-be-used-by-the-application-gateway"></a>Uygulama ağ geçidi tarafından kullanılacak anahtar kasası, ilke ve sertifika oluşturma

```azurepowershell
$keyVault = New-AzKeyVault -Name $kv -ResourceGroupName $rgname -Location $location -EnableSoftDelete 
Set-AzKeyVaultAccessPolicy -VaultName $kv -PermissionsToSecrets get -ObjectId $identity.PrincipalId

$policy = New-AzKeyVaultCertificatePolicy -ValidityInMonths 12 `
  -SubjectName "CN=www.contoso11.com" -IssuerName self `
  -RenewAtNumberOfDaysBeforeExpiry 30
Set-AzKeyVaultAccessPolicy -VaultName $kv -EmailAddress <your email address> -PermissionsToCertificates create,get,list
$certificate = Add-AzKeyVaultCertificate -VaultName $kv -Name "cert1" -CertificatePolicy $policy
$certificate = Get-AzKeyVaultCertificate -VaultName $kv -Name "cert1"
$secretId = $certificate.SecretId.Replace($certificate.Version, "")
```
> [!NOTE]
> SSL sonlandırmanın düzgün çalışması için -EnableSoftDelete bayrağı kullanılmalıdır. [Key Vault soft-delete'i Portal üzerinden](../key-vault/key-vault-ovw-soft-delete.md#soft-delete-behavior)yapılandırıyorsanız, bekletme süresi varsayılan değer olan 90 gün olarak tutulmalıdır. Uygulama Ağ Geçidi henüz farklı bir bekletme süresini desteklemez. 

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

```azurepowershell
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzVirtualNetworkSubnetConfig -Name "backendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzvirtualNetwork -Name "Vnet1" -ResourceGroupName $rgname -Location $location `
  -AddressPrefix "10.0.0.0/16" -Subnet @($sub1, $sub2)
```

### <a name="create-a-static-public-virtual-ip-vip-address"></a>Statik bir genel sanal IP (VIP) adresi oluşturma

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name "AppGwIP" `
  -location $location -AllocationMethod Static -Sku Standard
```

### <a name="create-pool-and-front-end-ports"></a>Havuz ve ön uç bağlantı noktaları oluşturma

```azurepowershell
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -VirtualNetwork $vnet

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "AppGwIpConfig" -Subnet $gwSubnet
$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "fipconfig" -PublicIPAddress $publicip
$pool = New-AzApplicationGatewayBackendAddressPool -Name "pool1" `
  -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com
$fp01 = New-AzApplicationGatewayFrontendPort -Name "port1" -Port 443
$fp02 = New-AzApplicationGatewayFrontendPort -Name "port2" -Port 80
```

### <a name="point-the-ssl-certificate-to-your-key-vault"></a>SSL sertifikasını anahtar kasanıza doğru işaretle

```azurepowershell
$sslCert01 = New-AzApplicationGatewaySslCertificate -Name "SSLCert1" -KeyVaultSecretId $secretId
```

### <a name="create-listeners-rules-and-autoscale"></a>Dinleyiciler, kurallar ve otomatik ölçeklendirme oluşturun

```azurepowershell
$listener01 = New-AzApplicationGatewayHttpListener -Name "listener1" -Protocol Https `
  -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $sslCert01
$listener02 = New-AzApplicationGatewayHttpListener -Name "listener2" -Protocol Http `
  -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp02
$poolSetting01 = New-AzApplicationGatewayBackendHttpSetting -Name "setting1" -Port 80 `
  -Protocol Http -CookieBasedAffinity Disabled
$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType basic `
  -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool
$rule02 = New-AzApplicationGatewayRequestRoutingRule -Name "rule2" -RuleType basic `
  -BackendHttpSettings $poolSetting01 -HttpListener $listener02 -BackendAddressPool $pool
$autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 3
$sku = New-AzApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
```

### <a name="assign-the-user-managed-identity-to-the-application-gateway"></a>Kullanıcı tarafından yönetilen kimliği uygulama ağ geçidine atama

```azurepowershell
$appgwIdentity = New-AzApplicationGatewayIdentity -UserAssignedIdentityId $identity.Id
```

### <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

```azurepowershell
$appgw = New-AzApplicationGateway -Name $appgwName -Identity $appgwIdentity -ResourceGroupName $rgname `
  -Location $location -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 `
  -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfig01 `
  -FrontendPorts @($fp01, $fp02) -HttpListeners @($listener01, $listener02) `
  -RequestRoutingRules @($rule01, $rule02) -Sku $sku `
  -SslCertificates $sslCert01 -AutoscaleConfiguration $autoscaleConfig
```

## <a name="next-steps"></a>Sonraki adımlar

[SSL sonlandırma hakkında daha fazla bilgi edinin](ssl-overview.md)
