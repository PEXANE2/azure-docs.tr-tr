---
title: PowerShell - Azure Uygulama Ağ Geçidi'ni kullanarak özel bir sonda oluşturma
description: Klasik dağıtım modelinde PowerShell'i kullanarak Uygulama Ağ Geçidi için özel bir sonda oluşturmayı öğrenin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/13/2019
ms.author: victorh
ms.openlocfilehash: 0ba3e9ae7b5075d1f5457cb2960423ad1c737e94
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81312545"
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>PowerShell'i kullanarak Azure Uygulama Ağ Geçidi (klasik) için özel bir sonda oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Klasik PowerShell](application-gateway-create-probe-classic-ps.md)

Bu makalede, PowerShell ile varolan bir uygulama ağ geçidine özel bir sonda eklersiniz. Özel sondalar, belirli bir sistem durumu denetimi sayfası olan uygulamalar veya varsayılan web uygulamasında başarılı bir yanıt sağlamayan uygulamalar için yararlıdır.

> [!IMPORTANT]
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve Klasik.](../azure-resource-manager/management/deployment-models.md) Bu makalede, Klasik dağıtım modeli kullanılarak kapsar. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. [Bu adımları Resource Manager modeli kullanarak gerçekleştirmeyi](application-gateway-create-probe-ps.md) öğrenin.

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir uygulama ağ geçidi oluşturmak için:

1. Uygulama ağ geçidi kaynağı oluşturun.
2. Bir XML yapılandırma dosyası veya yapılandırma nesnesi oluşturun.
3. Yapılandırmayı, yeni oluşturulmuş uygulama ağ geçidi kaynağına uygulayın.

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a>Özel bir sonda içeren bir uygulama ağ geçidi kaynağı oluşturma

Ağ geçidini oluşturmak için, `New-AzureApplicationGateway` cmdlet’ini kullanın ve değerleri kendi değerlerinizle değiştirin. Ağ geçidinin faturalanması bu aşamada başlamaz. Daha sonra ağ geçidi başarıyla başlatıldığında faturalama da başlar. 

Aşağıdaki örnek, "testvnet1" adlı sanal ağı ve "subnet-1" aklı alt ağı kullanarak bir uygulama ağ geçidi oluşturur.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

Ağ geçidinin oluşturulduğunu doğrulamak için `Get-AzureApplicationGateway` cmdlet’ini kullanabilirsiniz.

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> *InstanceCount* için varsayılan değer 2 ile 10 arasıdır. *GatewaySize* için varsayılan değer Medium’dur. Küçük, Orta ve Büyük arasında seçim yapabilirsiniz.
> 
> 

Ağ geçidi daha başlatılmadığından dolayı *VirtualIPs* ve *DnsName* boş görünür. Bu değerler, ağ geçidi çalışan duruma geldikten sonra oluşturulur.

### <a name="configure-an-application-gateway-by-using-xml"></a>XML kullanarak bir uygulama ağ geçidini yapılandırma

Aşağıdaki örnekte, tüm uygulama ağ geçidi ayarlarını yapılandırmak için bir XML dosyası kullanır ve bu ayarları uygulama ağ geçidi kaynağına uygularsınız.  

Aşağıdaki metni Notepad’a kopyalayın.

```xml
<ApplicationGatewayConfiguration xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
<FrontendIPConfigurations>
    <FrontendIPConfiguration>
        <Name>fip1</Name>
        <Type>Private</Type>
    </FrontendIPConfiguration>
</FrontendIPConfigurations>
<FrontendPorts>
    <FrontendPort>
        <Name>port1</Name>
        <Port>80</Port>
    </FrontendPort>
</FrontendPorts>
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
    </Probes>
    <BackendAddressPools>
    <BackendAddressPool>
        <Name>pool1</Name>
        <IPAddresses>
            <IPAddress>1.1.1.1</IPAddress>
            <IPAddress>2.2.2.2</IPAddress>
        </IPAddresses>
    </BackendAddressPool>
</BackendAddressPools>
<BackendHttpSettingsList>
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
</BackendHttpSettingsList>
<HttpListeners>
    <HttpListener>
        <Name>listener1</Name>
        <FrontendIP>fip1</FrontendIP>
    <FrontendPort>port1</FrontendPort>
        <Protocol>Http</Protocol>
    </HttpListener>
</HttpListeners>
<HttpLoadBalancingRules>
    <HttpLoadBalancingRule>
        <Name>lbrule1</Name>
        <Type>basic</Type>
        <BackendHttpSettings>setting1</BackendHttpSettings>
        <Listener>listener1</Listener>
        <BackendAddressPool>pool1</BackendAddressPool>
    </HttpLoadBalancingRule>
</HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Parantez içindeki değerleri yapılandırma öğeleri için düzenleyin. Dosyası .xml. uzantısıyla kaydedin.

Aşağıdaki örnek, ortak bağlantı noktası 80'deki HTTP trafiğini yüklemek ve özel bir sonda kullanarak iki IP adresi arasındaki arka uç bağlantı noktası 80'e ağ trafiğini göndermek için uygulama ağ geçidini ayarlamak için bir yapılandırma dosyasının nasıl kullanılacağını gösterir.

> [!IMPORTANT]
> Http veya Https protokol öğesi büyük/küçük harf duyarlıdır.

Özel sondaları yapılandırmak için yeni bir yapılandırma öğesi \<Yoklama\> eklenir.

Yapılandırma parametreleri şunlardır:

|Parametre|Açıklama|
|---|---|
|**Adı** |Özel sonda için referans adı. |
| **Protokolü** | Kullanılan protokol (olası değerler HTTP veya HTTPS'dir).|
| **Ana Bilgisayar** ve **Yol** | Örneğin durumunu belirlemek için uygulama ağ geçidi tarafından çağrılan tam URL yolu. Örneğin, http:\//contoso.com/ bir web siteniz varsa, özel sonda başarılı\/bir HTTP yanıtı na sahip olmak için sonda denetimleri için "http: /contoso.com/path/custompath.htm" için yapılandırılabilir.|
| **Interval** | Sonda aralığı denetimlerini saniyeler içinde yapılandırır.|
| **Zaman aşımı** | HTTP yanıt denetimi için sonda zaman larını tanımlar.|
| **SağlıksızEşik** | Arka uç örneğini *sağlıksız*olarak işaretlemek için gereken başarısız HTTP yanıtlarının sayısı.|

Sonda adı, hangi \<arka uç\> havuzunun özel sonda ayarları kullandığını atamak için BackendHttpSettings yapılandırmasında başvurulmuştur.

## <a name="add-a-custom-probe-to-an-existing-application-gateway"></a>Varolan bir uygulama ağ geçidine özel bir sonda ekleme

Bir uygulama ağ geçidinin geçerli yapılandırmasını değiştirmek için üç adım gerekir: Geçerli XML yapılandırma dosyasını alın, özel bir sondaolacak şekilde değiştirin ve uygulama ağ geçidini yeni XML ayarlarıyla yapılandırın.

1. Kullanarak `Get-AzureApplicationGatewayConfig`XML dosyasını alın. Bu cmdlet, sonda ayarı eklemek için değiştirilecek xml yapılandırmasını dışa aktarım.

   ```powershell
   Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"
   ```

1. XML dosyasını metin düzenleyicisinde açın. 'den `<probe>` sonra `<frontendport>`bölüm ekle.

   ```xml
   <Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
   </Probes>
   ```

   XML'in arka uçHttpSettings bölümünde, aşağıdaki örnekte gösterildiği gibi sonda adını ekleyin:

   ```xml
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
   ```

   XML dosyasını kaydedin.

1. Yeni XML dosyasıyla uygulama ağ geçidi `Set-AzureApplicationGatewayConfig`yapılandırmasını kullanarak güncelleştirin. Bu cmdlet, uygulama ağ geçidinizi yeni yapılandırmayla güncelleştirir.

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"
```

## <a name="next-steps"></a>Sonraki adımlar

Daha önce Güvenli Soketkatmanı (SSL) boşaltma olarak bilinen Aktarım Katmanı Güvenliği'ni (TLS) yapılandırmak istiyorsanız, [TLS boşaltma için bir uygulama ağ geçidini yapılandırma'ya](application-gateway-ssl.md)bakın.

İç yük dengeleyiciyle kullanacağınız uygulama ağ geçidi yapılandırmak istiyorsanız, bkz. [İç yük dengeleyici (ILB) ile uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

