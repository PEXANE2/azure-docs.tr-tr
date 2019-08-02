---
title: Azure Avustralya 'da Azure VPN Gateway
description: Azure Avustralya 'da VPN Gateway uygulama, HARONLE uyumlu olmak ve Avustralya kamu kuruluşlarını etkin bir şekilde korumak için kullanılır
author: galey801
ms.service: azure-australia
ms.topic: article
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 442ad6334a1775033018005d4a85875dbcb08ada
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571854"
---
# <a name="azure-vpn-gateway-in-azure-australia"></a>Azure Avustralya 'da Azure VPN Gateway

Herhangi bir genel buluta sahip kritik bir hizmet, bulut kaynakları ve Hizmetleri 'nin mevcut şirket içi sistemlere güvenli bağlantısıdır.  Azure 'da bu özelliği sağlayan hizmet Azure VPN Gateway (VPN Gateway). Bu makalede, VPN Gateway, Avustralya sinyalleri (ASD) [bilgi güvenliği El Ile denetim](https://acsc.gov.au/infosec/ism/) (ISM) ile uyumlu olacak şekilde yapılandırmayla ilgili önemli noktalar özetlenmektedir.

Azure 'daki bir sanal ağ ve başka bir ağ arasında şifrelenmiş trafik göndermek için bir VPN Gateway kullanılır.  VPN ağ geçitleri tarafından giderilen üç senaryo vardır:

- **Siteden siteye** S2S
- **Noktadan siteye** P2S
- **VNet-VNet**

Bu makale S2S VPN ağ geçitlerine odaklanacaktır. Diyagram 1, bir siteden siteye VPN ağ geçidi yapılandırmasını gösterir.

![Çok siteli bağlantılarla VPN Gateway](media/vpngateway-multisite-connection-diagram.png)

*Diyagram 1 – Azure siteden siteye VPN Gateway*

## <a name="key-design-considerations"></a>Önemli tasarım konuları

Azure 'ı Avustralya kamu müşterilerine bağlamak için üç ağ seçeneği vardır:

- **SIMG**
- **ExpressRoute**
- **Genel internet**

Azure için Avustralya Cyber Güvenlik Merkezi 'nin [Tüketici Kılavuzu](https://servicetrust.microsoft.com/viewpage/Australia) , VPN Gateway (veya eşit korumalı sertifikalı üçüncü taraf hizmeti), bağlantıların şu şekilde uyumlu olmasını sağlamak için üç ağ seçeneğiyle birlikte kullanılmasını önerir. Şifreleme ve bütünlük için ıSM denetimleri.

### <a name="encryption-and-integrity"></a>Şifreleme ve bütünlük

Varsayılan olarak VPN, ıKE el sıkışmalarının bir parçası olarak bağlantı kurulurken şifreleme ve bütünlük algoritmalarından ve parametrelerine göre anlaşır.  IKE el sıkışması sırasında, tercihe ait yapılandırma ve sıra, VPN Gateway Başlatıcı veya Yanıtlayıcı (NB: VPN cihazı aracılığıyla denetlenir) olmasına bağlıdır.  Bağlantının son yapılandırması VPN cihazının yapılandırması tarafından denetlenir.  Doğrulanan VPN cihazlarının ayrıntıları ve yapılandırmaları için buraya bakın: [VPN cihazları hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices)

VPN ağ geçitleri, bağlantıda özel bir IPSec/ıKE ilkesi yapılandırarak şifrelemeyi ve bütünlüğü denetleyebilir.

### <a name="resource-operations"></a>Kaynak işlemleri

VPN ağ geçitleri, Azure ve Azure olmayan ortamlar arasında genel İnternet üzerinden bir bağlantı oluşturur.  ISM, bağlantıların açık yetkilendirmesiyle ilgili denetimleri içerir.  Varsayılan olarak, güvenli ortamlarda yetkisiz tüneller oluşturmak için VPN ağ geçitleri kullanmak mümkündür.  Bu nedenle, kuruluşların VPN Gateway 'leri ve bağlantılarını kimlerin oluşturup değiştirebilmelerini denetlemek için Azure rol tabanlı Access Control (RBAC) kullanması önemlidir.  Azure 'da VPN ağ geçitlerini yönetmek için "yerleşik" rolü yoktur, bu nedenle özel bir rol gerekir.

"Sahip", "katkıda bulunan" ve "ağ katılımcısı" rollerine erişim sıkı bir şekilde denetlenir.  Daha ayrıntılı erişim denetimi için Azure AD Privileged Identity Management kullanılması da önerilir.

### <a name="high-availability"></a>Yüksek kullanılabilirlik

Azure VPN ağ geçitlerinin birden çok bağlantısı olabilir (bkz. diyagram 1) ve aynı şirket içi ortama birden çok şirket içi VPN cihazını destekleyebilir.  

Azure 'daki sanal ağlarda bağımsız, etkin-Pasif veya etkin-etkin yapılandırmalarda dağıtılabilecek birden fazla VPN ağ geçidi olabilir.

Tüm VPN ağ geçitlerinin [yüksek kullanılabilirliğe sahip bir yapılandırmada](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable)dağıtılması önerilir: Örneğin, etkin-Pasif veya etkin-etkin modda Iki VPN ağ geçidine bağlı iki ŞIRKET içi VPN cihazı (bkz. diyagram 2).

![VPN Gateway yedekli bağlantılar](media/dual-redundancy.png)

*Diyagram 2 – etkin-etkin VPN ağ geçitleri ve iki VPN cihazı*

### <a name="forced-tunneling"></a>Zorlamalı tünel oluşturma

Zorlamalı tünel yeniden yönlendirmeleri veya Internet 'e bağlı tüm trafiği, İnceleme ve denetim için VPN Gateway aracılığıyla şirket içi ortama geri yönlendirir. Zorlamalı tünel olmadan, Azure 'daki VM 'lerden Internet 'e yönelik trafik, trafiği İnceleme veya denetleme seçeneği olmadan Azure ağ altyapısını doğrudan genel Internet 'e erişir.  Kuruluşun bir ortam için güvenli bir Internet ağ geçidi (SıG) kullanması gerektiğinde bu kritik öneme sahiptir.

## <a name="detailed-configuration"></a>Ayrıntılı yapılandırma

### <a name="service-attributes"></a>Hizmet öznitelikleri

Avustralya kamu sektörü için yapılandırılmış olan S2S bağlantılarında VPN ağ geçitlerinin aşağıdaki özniteliklere sahip olması gerekir:

|Öznitelik | SEÇILMELIDIR|
|--- | --- |
|gatewayType | SANAL|
|

Korunan için ıSM denetimleriyle uyum sağlamak için gereken öznitelik ayarları şunlardır:

|Öznitelik | SEÇILMELIDIR|
|--- |---|
|vpnType |RouteBased|
|vpnClientConfiguration/vpnClientProtocols | Ike|
|

Azure VPN ağ geçitleri, IPSec ve ıKE protokol standartlarından bir dizi şifreleme algoritmasını destekler.  Varsayılan ilke, çok çeşitli üçüncü taraf VPN cihazlarıyla birlikte çalışabilirliği belirler.  Sonuç olarak, ıKE el sıkışması sırasında uyumlu olmayan bir yapılandırmaya anlaşılırsa mümkündür.  Bu nedenle, bağlantıların Azure 'a şirket içi ortam bağlantıları için ıSM denetimlerini karşıladığından emin olmak üzere VPN ağ geçitlerinde vpnClientConfiguration 'a [Özel IPSec/IKE ilkesi](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-ipsecikepolicy-rm-powershell) parametrelerinin uygulanması önemle önerilir.  Anahtar öznitelikleri şunlardır:

|Öznitelik|GEREKIR|SEÇILMELIDIR|
|---|---|---|
|saLifeTimeSeconds|< anlamına gelen 14400 saniye|> 300 saniye|
|saDataSizeKilobytes| |> 1024 KB|
|ıpsecencryption şifreleme| |AES256-GCMAES256|
|ıpsecıntegrity| |SHA256-GCMAES256|
|ıkeencryptıon| |AES256-GCMAES256|
|ıkeıntegrity| |SHA256-GCMAES256|
|dhGroup|DHGroup14, DHGroup24, ECP256, ECP384|DHGroup2|
|pfsGroup|PFS2048, PFS24, ECP256, ECP384||
|

*Yukarıdaki tabloda dhGroup ve pfsGroup için, diğer ayarların kullanılmasına karşın ECP256 ve ECP384 tercih edilir*

### <a name="related-services"></a>İlgili hizmetler

Azure VPN Gateway tasarlarken ve yapılandırırken, Ayrıca var olan ve yapılandırılmış bir dizi ilgili hizmet vardır:

|Hizmet | Eylem Gereklidir|
|--- | ---|
|Sanal Ağ | VPN ağ geçitleri bir sanal ağa eklenir.  Yeni bir VPN Gateway oluşturmadan önce bir sanal ağın oluşturulması gerekir.|
|Genel IP Adresi | S2S VPN ağ geçitlerinin, şirket içi VPN cihazı ve VPN Gateway arasında bağlantı kurması için genel bir IP adresi olması gerekir.  S2S VPN Gateway oluşturmadan önce genel IP adresinin oluşturulması gerekir.|
|Subnet | VPN Gateway için sanal ağ alt ağının oluşturulması gerekir.|
|

## <a name="implementation-steps-using-powershell"></a>PowerShell kullanarak uygulama adımları

### <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)

1. Özel rol oluşturun (örneğin, virtualNetworkGateway katılımcısı).  VPN ağ geçitleri oluşturmalarına ve değiştirmesine izin verilecek kullanıcılara atanacak bir rol oluşturun. Özel rol aşağıdaki işlemlere izin verir:

   Microsoft. Network/Virtualnetworkgateway/*  
   Microsoft. Network/Connections/*  
   Microsoft. Network/localnetworkgateway/*  
   Microsoft. Network/virtualNetworks/alt ağları/*  
   Microsoft. Network/publicIPAddresses/*  
   Microsoft. Network/Publicipöneklerini/*  
   Microsoft. Network/routeTables/*  

2. VPN ağ geçitlerini ve şirket içi ortamlara bağlantıları oluşturmalarına ve yönetmesine izin verilen kullanıcılara özel rol ekleyin.

### <a name="create-vpn-gateway"></a>VPN Gateway oluştur

*Bu adımlar, bir sanal ağın zaten oluşturulduğunu varsayar*

1. Yeni bir genel IP adresi oluştur
2. VPN Gateway alt ağı oluşturma
3. VPN Gateway IP yapılandırması oluşturma
4. VPN Gateway oluşturun
5. Şirket içi VPN cihazı için yerel ağ geçidi oluşturma
6. IPSec Ilkesi oluşturma (Özel IPSec/ıKE ilkeleri kullanmayı varsayarsak)
7. IPSec Ilkesi kullanarak VPN Gateway ve yerel ağ geçidi arasında bağlantı oluşturma

### <a name="enforce-tunneling"></a>Tüneli zorla

Zorlamalı tünel gerekiyorsa VPN Gateway oluşturmadan önce:

1. Yol tablosu ve yol kuralları oluştur
2. Yol tablosunu uygun alt ağlarla ilişkilendir

VPN Gateway oluşturduktan sonra:

1. VPN Gateway GatewayDefaultSite 'yi şirket içi ortama ayarlayın

### <a name="example-powershell-script"></a>Örnek PowerShell betiği

Avustralya KORUMALı güvenlik sınıflandırması için ıSM denetimleriyle uyumlu özel bir ıPSEC/ıKE ilkesi oluşturmak için örnek bir PowerShell betiği.

Sanal ağın, VPN Gateway ve yerel ağ geçitlerinin var olduğunu varsayar.

#### <a name="create-an-ipsecike-policy"></a>IPSec/ıKE ilkesi oluşturma

Aşağıdaki örnek betik, aşağıdaki algoritmalara ve parametrelere sahip bir IPSec/ıKE ilkesi oluşturur:

- Ike AES256, SHA256, DHGroup ECP256
- Bkz AES256, SHA256, PFS ECP256, SA yaşam 14.400 saniye & 102400000 KB

```powershell
$custompolicy = New-AzIpsecPolicy `
                    -IkeEncryption AES256 `
                    -IkeIntegrity SHA256 `
                    -DhGroup ECP256 `
                    -IpsecEncryption AES256 `
                    -IpsecIntegrity SHA256 `
                    -PfsGroup ECP256 `
                    -SALifeTimeSeconds 14400 `
                    -SADataSizeKilobytes 102400000
```

#### <a name="create-a-s2s-vpn-connection-with-the-custom-ipsecike-policy"></a>Özel IPSec/ıKE ilkesiyle bir S2S VPN bağlantısı oluşturma

```powershell
$vpngw = Get-AzVirtualNetworkGateway `
                    -Name "<yourVPNGatewayName>" `
                    -ResourceGroupName "<yourResourceGroupName>"
$localgw = Get-AzLocalNetworkGateway  `
                    -Name "<yourLocalGatewayName>" `
                    -ResourceGroupName "<yourResourceGroupName>"

New-AzVirtualNetworkGatewayConnection `
                    -Name "ConnectionName" `
                    -ResourceGroupName "<yourResourceGroupName>" `
                    -VirtualNetworkGateway1 $vpngw `
                    -LocalNetworkGateway2 $localgw `
                    -Location "Australia Central" `
                    -ConnectionType IPsec `
                    -IpsecPolicies $custompolicy `
                    -SharedKey "AzureA1b2C3"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, aktarım sırasında Avustralya kamu KORUMALı verilerinin güvenliğini sağlamak için bilgi güvenliği El Ile (ıSM) belirtilen gereksinimleri karşılamak üzere VPN Gateway belirli bir yapılandırması ele alınmıştır. VPN Gateway yapılandırmaya yönelik ayrıntılı adımlar için:

- [Azure sanal ağ geçidine genel bakış](https://docs.microsoft.com/azure/vpn-gateway/)  
- [VPN Gateway nedir?](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)  
- [PowerShell kullanarak siteden siteye VPN bağlantısı ile VNet oluşturma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell)  
- [VPN ağ geçidi oluşturma ve yönetme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-tutorial-create-gateway-powershell)