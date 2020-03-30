---
title: include dosyası
description: include dosyası
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 12/13/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 70ac106995324c758bde942d12191a01e3457e6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188105"
---
> [!NOTE]
> Bu örnekler S2S/ExpressRoute birlikte yapılandırmaları için geçerli değildir.
> Ortak bir yapılandırmada ağ geçitleriyle çalışma hakkında daha fazla bilgi için, [birlikte varolan bağlantıları Yapılandırma'ya bakın.](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Ağ geçidi ekleme

Klasik kaynak modelini kullanarak sanal ağa ağ geçidi eklediğinizde, ağ geçidini oluşturmadan önce ağ yapılandırma dosyasını doğrudan değiştirirsiniz. Bir ağ geçidi oluşturmak için aşağıdaki örneklerdeki değerlerin dosyada bulunması gerekir. Sanal ağınızın daha önce bu ağla ilişkili bir ağ geçidi varsa, bu değerlerden bazıları zaten mevcut olacaktır. Aşağıdaki değerleri yansıtacak şekilde dosyayı değiştirin.

### <a name="download-the-network-configuration-file"></a>Ağ yapılandırma dosyasını indirin

1. Ağ yapılandırma dosyası makalesindeki adımları kullanarak ağ yapılandırma [dosyasını](../articles/virtual-network/virtual-networks-using-network-configuration-file.md) karşıdan yükleyin. Bir metin düzenleyicisi kullanarak dosyayı açın.
2. Dosyaya yerel bir ağ sitesi ekleyin. Geçerli adres önekini kullanabilirsiniz. VPN ağ geçidi için geçerli bir IP adresi ekleyebilirsiniz. Bu bölümdeki adres değerleri ExpressRoute işlemleri için kullanılmaz, ancak dosya doğrulama için gereklidir. Örnekte, "branch1" sitenin adıdır. Farklı bir ad kullanabilirsiniz, ancak dosyanın Ağ Geçidi bölümünde aynı değeri kullandığınızdan emin olun.

   ```
   <VirtualNetworkConfiguration>
    <Dns />
    <LocalNetworkSites>
      <LocalNetworkSite name="branch1">
        <AddressSpace>
          <AddressPrefix>165.3.1.0/27</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>3.2.1.4</VPNGatewayAddress>
    </LocalNetworkSite>
   ```
3. VirtualNetworkSites'e gidin ve alanları değiştirin.

   * Ağ Geçidi Alt Ağının sanal ağınız için var olduğunu doğrulayın. Değilse, şu anda bir ekleyebilirsiniz. Adı "GatewaySubnet" olmalıdır.
   * Dosyanın Ağ Geçidi bölümünün var olduğunu doğrulayın. Eğer değilse, ekleyin. Bu, sanal ağı yerel ağ sitesiyle ilişkilendirmek için gereklidir (bağlandığınız ağı temsil eder).
   * Bağlantı türünün = Adanmış olduğunu doğrulayın. Bu, ExpressRoute bağlantıları için gereklidir.

   ```
   </LocalNetworkSites>
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myAzureVNET" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="default">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.1.0/27</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="branch1">
              <Connection type="Dedicated" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
   </VirtualNetworkConfiguration>
   </NetworkConfiguration>
   ```
4. Dosyayı kaydedin ve Azure'a yükleyin.

### <a name="create-the-gateway"></a>Ağ geçidi oluşturma

Bir ağ geçidi oluşturmak için aşağıdaki komutu kullanın. Herhangi bir değeri kendi değerlerinin yerine.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Ağ geçidinin oluşturulduğunu doğrulama

Ağ geçidinin oluşturulduğunu doğrulamak için aşağıdaki komutu kullanın. Bu komut, diğer işlemler için gereken ağ geçidi kimliğini de alır.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Ağ geçidini yeniden boyutlandırma

[Ağ Geçidi SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md)bir dizi vardır. Ağ Geçidi SKU'yu istediğiniz zaman değiştirmek için aşağıdaki komutu kullanabilirsiniz.

> [!IMPORTANT]
> Bu komut UltraPerformance ağ geçidi için çalışmaz. Ağ geçidinizi bir UltraPerformance ağ geçidine değiştirmek için önce varolan ExpressRoute ağ geçidini kaldırın ve ardından yeni bir UltraPerformance ağ geçidi oluşturun. Ağ geçidinizi bir UltraPerformance ağ geçidinden düşürmek için önce UltraPerformance ağ geçidini kaldırın ve ardından yeni bir ağ geçidi oluşturun.
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Ağ geçitlerini kaldırma

Ağ geçidini kaldırmak için aşağıdaki komutu kullanın

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```