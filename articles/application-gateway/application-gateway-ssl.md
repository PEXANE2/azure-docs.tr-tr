---
title: PowerShell kullanarak SSL boşaltma - Azure Uygulama Ağ Geçidi
description: Bu makalede, Azure klasik dağıtım modelini kullanarak SSL boşaltma ile bir uygulama ağ geçidi oluşturmak için yönergeler sağlar
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/13/2019
ms.author: victorh
ms.openlocfilehash: c456a0856adb0d36349b5f96ba0ab8bab3eec5c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74047912"
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Klasik dağıtım modelini kullanarak SSL boşaltma için bir uygulama ağ geçidini yapılandırma

> [!div class="op_single_selector"]
> * [Azure portalında](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure klasik PowerShell](application-gateway-ssl.md)
> * [Azure CLI](application-gateway-ssl-cli.md)

Azure Application Gateway, web grubunda maliyetli SSL şifre çözme görevlerinin oluşmasından kaçınmak için Güvenli Yuva Katmanı (SSL) oturumunu sonlandırmak amacıyla yapılandırılabilir. SSL yük boşaltımı ön uç sunucusunun kurulumunu ve web uygulamasının yönetimini de basitleştirir.

## <a name="before-you-begin"></a>Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Geçerli bir alt ağla çalışan bir sanal ağa sahip olduğunuzu doğrulayın. Ağ geçidi hiçbir sanal makinenin veya bulut dağıtımının kullanmadığından emin olun. Uygulama ağ geçidi tek başına bir sanal ağ alt ağında olmalıdır.
3. Uygulama ağ geçidini kullanmak üzere yapılandırdığınız sunucuların var olması veya sanal ağda veya genel bir IP adresi veya sanal IP adresi (VIP) atanmış olarak oluşturulan uç noktaları olmalıdır.

Bir uygulama ağ geçidinde SSL boşaltmayapılandırmak için listelenen sırada aşağıdaki adımları tamamlayın:

1. [Uygulama ağ geçidi oluşturma](#create-an-application-gateway)
2. [SSL sertifikalarını yükleme](#upload-ssl-certificates)
3. [Ağ geçidini yapılandırma](#configure-the-gateway)
4. [Ağ geçidi yapılandırmasını ayarlama](#set-the-gateway-configuration)
5. [Ağ geçidini başlatma](#start-the-gateway)
6. [Ağ geçidi durumunu doğrulama](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Ağ geçidioluşturmak `New-AzureApplicationGateway` için, kendi değerleri ile değiştirerek cmdlet girin. Ağ geçidinin faturalanması bu aşamada başlamaz. Daha sonra ağ geçidi başarıyla başlatıldığında faturalama da başlar. 

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

Ağ geçidinin oluşturulduğunu doğrulamak için `Get-AzureApplicationGateway` cmdlet'e girebilirsiniz.

Örnekte, **Açıklama,** **InstanceCount**ve **Ağ Geçidi Boyutu** isteğe bağlı parametrelerdir. **InstanceCount** için varsayılan değer **2,** en fazla **değeri 10**olan. **GatewaySize** için varsayılan değer **Orta'dır.** Küçük ve Büyük diğer kullanılabilir değerlerdir. Ağ geçidi henüz başlamadığı için **VirtualIPs** ve **DnsName** boş olarak gösterilir. Bu değerler ağ geçidi çalışan durumda sonra oluşturulur.

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a>SSL sertifikalarını yükleme

PFX formatındaki sunucu sertifikasını uygulama ağ geçidine yüklemek için girin. `Add-AzureApplicationGatewaySslCertificate` Sertifika adı kullanıcı tarafından seçilen bir addır ve uygulama ağ geçidi içinde benzersiz olmalıdır. Bu sertifika, uygulama ağ geçidindeki tüm sertifika yönetimi işlemlerinde bu adla anılır.

Aşağıdaki örnekcmdlet gösterir. Örnekteki değerleri kendi değerlerinizle değiştirin.

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>
```

Ardından, sertifika yüklemesini doğrulayın. Cmdlet girin. `Get-AzureApplicationGatewayCertificate`

Aşağıdaki örnek, ilk satırdaki cmdlet'i ve ardından çıktıyı gösterir:

```powershell
Get-AzureApplicationGatewaySslCertificate AppGwTest
```

```
VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
Name           : SslCert
SubjectName    : CN=gwcert.app.test.contoso.com
Thumbprint     : AF5ADD77E160A01A6......EE48D1A
ThumbprintAlgo : sha1RSA
State..........: Provisioned
```

> [!NOTE]
> Sertifika parolası, harf veya sayılardan oluşan 4 ila 12 karakter arasında olmalıdır. Özel karakterler kabul edilmez.

## <a name="configure-the-gateway"></a>Ağ geçidini yapılandırma

Uygulama ağ geçidi yapılandırması birden çok değerden oluşur. Yapılandırmayı oluşturmak için değerler birbirine bağlanabilir.

Değerler şunlardır:

* **Arka uç sunucu havuzu**: Arka uç sunucuların IP adresleri listesi. Listelenen IP adresleri sanal ağ alt ağına ait olmalı veya genel bir IP veya VIP adresi olmalıdır.
* **Arka uç sunucu havuzu ayarları**: Her havuzda bağlantı noktası, protokol ve çerez tabanlı yakınlık gibi ayarlar bulunur. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası**: Bu bağlantı noktası, uygulama ağ geçidinde açılan ortak bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici**: Dinleyicinin bir ön uç bağlantı noktası, bir protokolü (Http veya Https; bu değerler büyük/küçük harf duyarlıdır) ve SSL sertifika adı (bir SSL boşaltma yapılandırıyorsanız) vardır.
* **Kural**: Kural dinleyiciyi ve arka uç sunucu havuzunu bağlar ve trafiği belirli bir dinleyiciye çarptığında yönlendirecek arka uç sunucu havuzunu tanımlar. Şu anda yalnızca *temel* kural desteklenmektedir. *Temel* kural hepsini bir kez deneme yöntemiyle yük dağıtımıdır.

**Ek yapılandırma notları**

SSL sertifikaları yapılandırmada **HttpListener**’daki protokol **Https** (küçük/büyük harf duyarlı) ile değiştirilmelidir. **SslCert** öğesini [Upload SSL sertifikaları](#upload-ssl-certificates) bölümünde kullanılan değer kümesiyle **HttpListener'a** ekleyin. Ön uç bağlantı noktası **443**olarak güncellenmelidir.

**Çerez tabanlı yakınlığı etkinleştirmek için**: İstemci oturumundan gelen bir isteğin her zaman web çiftliğinde aynı VM'ye yönlendirilmesini sağlamak için bir uygulama ağ geçidi ni yapılandırabilirsiniz. Bunu gerçekleştirmek için, ağ geçidinin trafiği uygun şekilde yönlendirmesine izin veren bir oturum çerezi ekleyin. Tanımlama bilgisi temelli benzeşimi etkinleştirmek için, **CookieBasedAffinity**’yi **BackendHttpSetting** öğesindeki **Enabled**’a ayarlayın.

Yapılandırmanızı bir yapılandırma nesnesi oluşturarak veya bir yapılandırma XML dosyası kullanarak oluşturabilirsiniz.
Yapılandırma XML dosyasını kullanarak yapılandırmanızı oluşturmak için aşağıdaki örneği girin:


```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations />
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>443</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>BackendPool1</Name>
            <IPAddresses>
                <IPAddress>10.0.0.1</IPAddress>
                <IPAddress>10.0.0.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>BackendSetting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>HTTPListener1</Name>
            <FrontendPort>FrontendPort1</FrontendPort>
            <Protocol>Https</Protocol>
            <SslCert>GWCert</SslCert>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>HttpLBRule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
            <Listener>HTTPListener1</Listener>
            <BackendAddressPool>BackendPool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

## <a name="set-the-gateway-configuration"></a>Ağ geçidi yapılandırmasını ayarlama

Sonra, uygulama ağ geçidini kurun. Cmdlet'i `Set-AzureApplicationGatewayConfig` bir yapılandırma nesnesi veya yapılandırma XML dosyasıyla girebilirsiniz.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-the-gateway"></a>Ağ geçidini başlatma

Ağ geçidi yapılandırıldıktan sonra, `Start-AzureApplicationGateway` ağ geçidini başlatmak için cmdlet'i girin. Uygulama ağ geçidinin faturalanması ağ geçidi başarıyla başlatıldıktan sonra başlar.

> [!NOTE]
> `Start-AzureApplicationGateway` Cmdlet bitirmek için 15-20 dakika sürebilir.
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a>Ağ geçidi durumunu doğrulama

Ağ `Get-AzureApplicationGateway` geçidinin durumunu kontrol etmek için cmdlet'i girin. Önceki `Start-AzureApplicationGateway` adımda başarılı olduysanız, **Devlet** **Çalışıyor**olmalı ve **VirtualIPs** ve **DnsName** geçerli girişleri olmalıdır.

Bu örnek, trafiğe hazır, çalışan ve çalışmaya hazır bir uygulama ağ geçidini gösterir:

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest2
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
VirtualIPs    : {23.96.22.241}
DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net
```

## <a name="next-steps"></a>Sonraki adımlar

Genel olarak yük dengeleme seçenekleri hakkında daha fazla bilgi için bkz:

* [Azure Yük Dengeleyicisi](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
