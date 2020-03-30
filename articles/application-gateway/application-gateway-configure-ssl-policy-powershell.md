---
title: PowerShell kullanarak SSL ilkesini yapılandırın
titleSuffix: Azure Application Gateway
description: Bu makale, Azure Uygulama Ağ Geçidi'nde SSL İlkesini yapılandırmak için yönergeler sağlar
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: victorh
ms.openlocfilehash: 105b0b3e40e6e9433ee456914cd5babc1d17d036
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74075242"
---
# <a name="configure-ssl-policy-versions-and-cipher-suites-on-application-gateway"></a>Uygulama Ağ Geçidi'nde SSL ilke sürümlerini ve şifreleme paketlerini yapılandırma

Application Gateway'de SSL ilke sürümlerini ve şifreleme paketlerini nasıl yapılandıracağız öğrenin. SSL ilkesi sürümlerinin ve etkin şifreleme paketlerinin farklı yapılandırmalarını içeren önceden tanımlanmış ilkelerin listesinden seçim yapabilirsiniz. İhtiyaçlarınıza göre özel bir [SSL ilkesi](#configure-a-custom-ssl-policy) tanımlama da abilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-available-ssl-options"></a>Kullanılabilir SSL seçeneklerini alın

Cmdlet, `Get-AzApplicationGatewayAvailableSslOptions` önceden tanımlanmış kullanılabilir ilkelerin, kullanılabilir şifreleme paketlerinin ve yapılandırılabilen protokol sürümlerinin bir listesini sağlar. Aşağıdaki örnek, cmdlet'in çalıştırılmasına neden olan bir örnek çıktıyı gösterir.

```
DefaultPolicy: AppGwSslPolicy20150501
PredefinedPolicies:
    /subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups//providers/Microsoft.Network/ApplicationGatewayAvailableSslOptions/default/Applic
ationGatewaySslPredefinedPolicy/AppGwSslPolicy20150501
    /subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups//providers/Microsoft.Network/ApplicationGatewayAvailableSslOptions/default/Applic
ationGatewaySslPredefinedPolicy/AppGwSslPolicy20170401
    /subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups//providers/Microsoft.Network/ApplicationGatewayAvailableSslOptions/default/Applic
ationGatewaySslPredefinedPolicy/AppGwSslPolicy20170401S

AvailableCipherSuites:
    TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
    TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
    TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
    TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
    TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
    TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
    TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
    TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
    TLS_DHE_RSA_WITH_AES_256_CBC_SHA
    TLS_DHE_RSA_WITH_AES_128_CBC_SHA
    TLS_RSA_WITH_AES_256_GCM_SHA384
    TLS_RSA_WITH_AES_128_GCM_SHA256
    TLS_RSA_WITH_AES_256_CBC_SHA256
    TLS_RSA_WITH_AES_128_CBC_SHA256
    TLS_RSA_WITH_AES_256_CBC_SHA
    TLS_RSA_WITH_AES_128_CBC_SHA
    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
    TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
    TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
    TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
    TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
    TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
    TLS_DHE_DSS_WITH_AES_256_CBC_SHA
    TLS_DHE_DSS_WITH_AES_128_CBC_SHA
    TLS_RSA_WITH_3DES_EDE_CBC_SHA
    TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

AvailableProtocols:
    TLSv1_0
    TLSv1_1
    TLSv1_2
```

## <a name="list-pre-defined-ssl-policies"></a>Önceden tanımlanmış SSL İlkeleri listele

Uygulama ağ geçidi, kullanılabilecek üç önceden tanımlanmış ilke yle birlikte gelir. Cmdlet `Get-AzApplicationGatewaySslPredefinedPolicy` bu ilkeleri alır. Her ilke nin farklı protokol sürümleri ve şifreli paketleri etkindir. Önceden tanımlanmış bu ilkeler, uygulama ağ geçidinizde bir SSL ilkesini hızla yapılandırmak için kullanılabilir. Varsayılan olarak **AppGwSslPolicy20150501** belirli bir SSL ilkesi tanımlanmamışsa seçilir.

Aşağıdaki çıktı çalışan `Get-AzApplicationGatewaySslPredefinedPolicy`bir örnektir.

```
Name: AppGwSslPolicy20150501
MinProtocolVersion: TLSv1_0
CipherSuites:
    TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
    TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
    TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
    TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
    TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
    TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
    TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
    TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
    TLS_DHE_RSA_WITH_AES_256_CBC_SHA
    TLS_DHE_RSA_WITH_AES_128_CBC_SHA
    TLS_RSA_WITH_AES_256_GCM_SHA384
 ...
Name: AppGwSslPolicy20170401
MinProtocolVersion: TLSv1_1
CipherSuites:
    TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
    TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
    TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
    TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
...
```

## <a name="configure-a-custom-ssl-policy"></a>Özel bir SSL ilkesini yapılandırma

Özel bir SSL ilkesini yapılandırırken aşağıdaki parametreleri geçersiniz: PolicyType, MinProtocolVersion, CipherSuite ve ApplicationGateway. Diğer parametreleri geçirmeye çalışırsanız, Uygulama Ağ Geçidi'ni oluştururken veya güncellerken bir hata alırsınız. 

Aşağıdaki örnek, bir uygulama ağ geçidinde özel bir SSL ilkesi ni belirler. Minimum protokol sürümünü ayarlar `TLSv1_1` ve aşağıdaki şifreleme paketlerini sağlar:

* TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256

> [!IMPORTANT]
> TLS_RSA_WITH_AES_256_CBC_SHA256 özel bir SSL ilkesi ni yapılandırırken seçilmelidir. Uygulama ağ geçidi arka uç yönetimi için bu şifreleme paketini kullanır. Bunu diğer tümsüitlerle birlikte kullanabilirsiniz, ancak bu paketin de seçilmesi gerekir. 

```powershell
# get an application gateway resource
$gw = Get-AzApplicationGateway -Name AdatumAppGateway -ResourceGroup AdatumAppGatewayRG

# set the SSL policy on the application gateway
Set-AzApplicationGatewaySslPolicy -ApplicationGateway $gw -PolicyType Custom -MinProtocolVersion TLSv1_1 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"

# validate the SSL policy locally
Get-AzApplicationGatewaySslPolicy -ApplicationGateway $gw

# update the gateway with validated SSL policy
Set-AzApplicationGateway -ApplicationGateway $gw
```

## <a name="create-an-application-gateway-with-a-pre-defined-ssl-policy"></a>Önceden tanımlanmış bir SSL ilkesiyle bir uygulama ağ geçidi oluşturma

Önceden tanımlanmış bir SSL ilkesini yapılandırırken aşağıdaki parametreleri geçersiniz: İlke Türü, İlke Adı ve ApplicationGateway. Diğer parametreleri geçirmeye çalışırsanız, Uygulama Ağ Geçidi'ni oluştururken veya güncellerken bir hata alırsınız.

Aşağıdaki örnek, önceden tanımlanmış bir SSL ilkesiyle yeni bir uygulama ağ geçidi oluşturur.

```powershell
# Create a resource group
$rg = New-AzResourceGroup -Name ContosoRG -Location "East US"

# Create a subnet for the application gateway
$subnet = New-AzVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network with a 10.0.0.0/16 address space
$vnet = New-AzVirtualNetwork -Name appgwvnet -ResourceGroupName $rg.ResourceGroupName -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve the subnet object for later use
$subnet = $vnet.Subnets[0]

# Create a public IP address
$publicip = New-AzPublicIpAddress -ResourceGroupName $rg.ResourceGroupName -name publicIP01 -location "East US" -AllocationMethod Dynamic

# Create an ip configuration object
$gipconfig = New-AzApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Create a backend pool for backend web servers
$pool = New-AzApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

# Define the backend http settings to be used.
$poolSetting = New-AzApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

# Create a new port for SSL
$fp = New-AzApplicationGatewayFrontendPort -Name frontendport01  -Port 443

# Upload an existing pfx certificate for SSL offload
$password = ConvertTo-SecureString -String "P@ssw0rd" -AsPlainText -Force
$cert = New-AzApplicationGatewaySslCertificate -Name cert01 -CertificateFile C:\folder\contoso.pfx -Password $password

# Create a frontend IP configuration for the public IP address
$fipconfig = New-AzApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Create a new listener with the certificate, port, and frontend ip.
$listener = New-AzApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

# Create a new rule for backend traffic routing
$rule = New-AzApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Define the size of the application gateway
$sku = New-AzApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Configure the SSL policy to use a different pre-defined policy
$policy = New-AzApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

# Create the application gateway.
$appgw = New-AzApplicationGateway -Name appgwtest -ResourceGroupName $rg.ResourceGroupName -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

## <a name="update-an-existing-application-gateway-with-a-pre-defined-ssl-policy"></a>Varolan bir uygulama ağ geçidini önceden tanımlanmış bir SSL ilkesiyle güncelleştirme

Özel bir SSL ilkesi ayarlamak için aşağıdaki parametreleri geçirin: **PolicyType,** **MinProtocolVersion**, **CipherSuite**ve **ApplicationGateway**. Önceden tanımlanmış bir SSL ilkesi ayarlamak için aşağıdaki parametreleri geçirin: **PolicyType**, **PolicyName**ve **ApplicationGateway**. Diğer parametreleri geçirmeye çalışırsanız, Uygulama Ağ Geçidi'ni oluştururken veya güncellerken bir hata alırsınız.

Aşağıdaki örnekte, hem Özel İlke hem de Önceden Tanımlanmış İlke için kod örnekleri vardır. Kullanmak istediğiniz ilkeyi açıklamamaya bırakın.

```powershell
# You have to change these parameters to match your environment.
$AppGWname = "YourAppGwName"
$RG = "YourResourceGroupName"

$AppGw = get-Azapplicationgateway -Name $AppGWname -ResourceGroupName $RG

# Choose either custom policy or predefined policy and uncomment the one you want to use.

# SSL Custom Policy
# Set-AzApplicationGatewaySslPolicy -PolicyType Custom -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_RSA_WITH_AES_128_CBC_SHA256" -ApplicationGateway $AppGw

# SSL Predefined Policy
# Set-AzApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName "AppGwSslPolicy20170401S" -ApplicationGateway $AppGW

# Update AppGW
# The SSL policy options are not validated or updated on the Application Gateway until this cmdlet is executed.
$SetGW = Set-AzApplicationGateway -ApplicationGateway $AppGW
```

## <a name="next-steps"></a>Sonraki adımlar

HTTP trafiğini https bitiş noktasına nasıl yönlendirilenöğrenmek için Uygulama Ağ Geçidi'ni ziyaret [edin.](application-gateway-redirect-overview.md)
