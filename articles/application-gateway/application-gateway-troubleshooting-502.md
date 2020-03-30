---
title: Sorun Giderme Hatalı Ağ Geçidi hataları - Azure Application Gateway
description: 'Application Gateway Server Error: 502 - Web sunucusu, ağ geçidi veya proxy sunucusu gibi hareket ederken geçersiz bir yanıt aldı.'
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/16/2019
ms.author: amsriva
ms.openlocfilehash: 17bed17b536f6e88fc821fd83e09a1d6ea218bc3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74130479"
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Application Gateway’deki hatalı ağ geçidi hataları ile ilgili sorunları giderme

Azure Application Gateway kullanırken alınan kötü ağ geçidi (502) hatalarını nasıl gidereceklerini öğrenin.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış

Bir uygulama ağ geçidini yapılandırdıktan sonra görebileceğiniz hatalardan biri "Sunucu Hatası: 502 - Web sunucusu, ağ geçidi veya proxy sunucusu gibi hareket ederken geçersiz bir yanıt aldı". Bu hata aşağıdaki ana nedenlerden dolayı oluşabilir:

* NSG, UDR veya Özel DNS arka uç havuz üyelerine erişimi engelliyor.
* Arka uç VM'ler veya sanal makine ölçeği kümesi örnekleri varsayılan sistem durumu sondasına yanıt vermiyor.
* Özel durum yoklamaları geçersiz veya hatalı şekilde yapılandırılmış.
* Azure Application Gateway'in [arka uç havuzu yapılandırılmamış veya boş değildir.](#empty-backendaddresspool)
* [Sanal makine ölçeği kümesindeki](#unhealthy-instances-in-backendaddresspool)VM'lerin veya örneklerin hiçbiri sağlıklı değildir.
* Kullanıcı istekleriyle ilgili [zaman ayarı veya bağlantı sorunları isteyin.](#request-time-out)

## <a name="network-security-group-user-defined-route-or-custom-dns-issue"></a>Ağ Güvenlik Grubu, Kullanıcı Tanımlı Rota veya Özel DNS sorunu

### <a name="cause"></a>Nedeni

NSG, UDR veya özel DNS nedeniyle arka uça erişim engellenirse, uygulama ağ geçidi örnekleri arka uç havuzuna erişemez. Bu, 502 hataya neden olan sonda hatalarına neden olur.

NSG/UDR, uygulama ağ geçidi alt ağına veya uygulama VM'lerinin dağıtıldığı alt ağda bulunabilir.

Benzer şekilde, VNet'te özel bir DNS'nin varlığı da sorunlara neden olabilir. Arka uç havuzu üyeleri için kullanılan bir FQDN, VNet için kullanıcı tarafından yapılandırılan DNS sunucusu tarafından doğru şekilde çözülmeyebilir.

### <a name="solution"></a>Çözüm

Aşağıdaki adımları izleyerek NSG, UDR ve DNS yapılandırmasını doğrulayın:

* Uygulama ağ geçidi alt ağıyla ilişkili NSG'leri denetleyin. Arka uçtaki iletişimin engellenmediğinden emin olun.
* Uygulama ağ geçidi alt ağıyla ilişkili UDR'yi denetleyin. UDR'nin trafiği arka uç alt ağdan uzağa yönlendirmediğinden emin olun. Örneğin, ExpressRoute/VPN üzerinden uygulama ağ geçidi alt ağına duyurulan sanal cihazlara veya varsayılan rotalara yönlendirme olup olma yeğleyin.

```azurepowershell
$vnet = Get-AzVirtualNetwork -Name vnetName -ResourceGroupName rgName
Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
```

* Arka uç VM ile etkili NSG'yi ve rotayı kontrol edin

```azurepowershell
Get-AzEffectiveNetworkSecurityGroup -NetworkInterfaceName nic1 -ResourceGroupName testrg
Get-AzEffectiveRouteTable -NetworkInterfaceName nic1 -ResourceGroupName testrg
```

* VNet'te özel DNS'nin varlığını denetleyin. DNS çıktıdaki VNet özelliklerinin ayrıntılarına bakarak kontrol edilebilir.

```json
Get-AzVirtualNetwork -Name vnetName -ResourceGroupName rgName 
DhcpOptions            : {
                           "DnsServers": [
                             "x.x.x.x"
                           ]
                         }
```
Varsa, DNS sunucusunun arka uç havuzu üyesinin FQDN'sini doğru şekilde çözebilmesini sağlayın.

## <a name="problems-with-default-health-probe"></a>Varsayılan sistem durumu sondasıyla ilgili sorunlar

### <a name="cause"></a>Nedeni

502 hata, varsayılan sistem durumu sondasının arka uç VM'lere ulaşamayabileceğinin sık göstergeleri de olabilir.

Bir uygulama ağ geçidi örneği sağlandığında, BackendHttpSetting özelliklerini kullanarak her BackendAddressPool için varsayılan bir sistem durumu sondası otomatik olarak yapılandırır. Bu sondayı ayarlamak için kullanıcı girişi gerekmez. Özellikle, bir yük dengeleme kuralı yapılandırıldığınızda, Bir BackendHttpSetting ve BackendAddressPool arasında bir ilişkilendirme yapılır. Varsayılan sonda bu ilişkilendirmelerin her biri için yapılandırılır ve uygulama ağ geçidi BackendHttpSetting öğesinde belirtilen bağlantı noktasındabackendAddressPool her örneği için bir periyodik sistem durumu denetimi bağlantısı başlar. 

Aşağıdaki tablo, varsayılan sistem durumu sondasıyla ilişkili değerleri listeler:

| Sonda özelliği | Değer | Açıklama |
| --- | --- | --- |
| Sonda URL'si |`http://127.0.0.1/` |URL yolu |
| Interval |30 |Saniye cinsinden sonda aralığı |
| Zaman aşımı |30 |Saniyeler içinde sonda zaman-out |
| Sağlıksız durum eşiği |3 |Sonda yeniden say. Arka uç sunucusu, ardışık sonda hata sayısı sağlıksız eşiğe ulaştıktan sonra aşağı işaretlenir. |

### <a name="solution"></a>Çözüm

* Varsayılan bir sitenin yapılandırıldığından ve 127.0.0.1'de dinlediğinden emin olun.
* BackendHttpSetting 80'den başka bir bağlantı noktası belirtirse, varsayılan site bu bağlantı noktasında dinleyecek şekilde yapılandırılmalıdır.
* Çağrı 200 bir HTTP sonuç kodu `http://127.0.0.1:port` döndürmelidir. Bu 30 saniyelik zaman dilimi içinde döndürülmelidir.
* Yapılandırılan bağlantı noktasının açık olduğundan ve yapılandırılan bağlantı noktasında gelen veya giden trafiği engelleyen güvenlik duvarı kuralları veya Azure Ağ Güvenlik Grupları olmadığından emin olun.
* Azure klasik VM'leri veya Bulut Hizmeti bir FQDN veya ortak bir IP ile kullanılıyorsa, ilgili [uç noktanın](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) açıldığından emin olun.
* VM, Azure Kaynak Yöneticisi aracılığıyla yapılandırılırsa ve uygulama ağ geçidinin dağıtıldığı VNet'in dışındaysa, bir [Ağ Güvenlik Grubu'nun](../virtual-network/security-overview.md) istenen bağlantı noktasında erişime izin verecek şekilde yapılandırılması gerekir.

## <a name="problems-with-custom-health-probe"></a>Özel sağlık sondası ile ilgili sorunlar

### <a name="cause"></a>Nedeni

Özel sistem durumu sondaları varsayılan sondalama davranışına ek esneklik sağlar. Özel sondalar kullandığınızda, sonda aralığını, URL'yi, sınanacak yolu ve arka uç havuzu örneğini sağlıksız olarak işaretlemeden önce kabul edilebilen kaç başarısız yanıtı yapılandırabilirsiniz.

Aşağıdaki ek özellikler eklenir:

| Sonda özelliği | Açıklama |
| --- | --- |
| Adı |Sondanın adı. Bu ad, arka uç HTTP ayarlarında sonda başvurmak için kullanılır. |
| Protokol |Protokol sondayı göndermek için kullanılırdı. Sonda, arka uç HTTP ayarlarında tanımlanan protokolü kullanır |
| Host |Sondayı göndermek için ana bilgisayar adı. Yalnızca uygulama ağ geçidinde çok siteli yapılandırıldıklarında uygulanabilir. Bu, VM ana bilgisayar adından farklıdır. |
| Yol |Sondanın göreli yolu. Geçerli yol '/' adresinden başlar. Sonda protokole \<\>gönderilir\<\>://\<\>\<ana bilgisayar : bağlantı noktası yolu\> |
| Interval |Sonda aralığı saniyeler içinde. Bu, art arda iki sonda arasındaki zaman aralığıdır. |
| Zaman aşımı |Saniyeler içinde sonda lama. Bu zaman ayırMadı süresi içinde geçerli bir yanıt alınmazsa, sonda başarısız olarak işaretlenir. |
| Sağlıksız durum eşiği |Sonda yeniden say. Arka uç sunucusu, ardışık sonda hata sayısı sağlıksız eşiğe ulaştıktan sonra aşağı işaretlenir. |

### <a name="solution"></a>Çözüm

Özel Durum Bamason'Un önceki tablo olarak doğru şekilde yapılandırıldığından doğrulayın. Önceki sorun giderme adımlarına ek olarak, aşağıdakileri de sağlayın:

* Sondanın [kılavuza](application-gateway-create-probe-ps.md)göre doğru şekilde belirtildiğinden emin olun.
* Uygulama ağ geçidi tek bir site için yapılandırılırsa, varsayılan `127.0.0.1`olarak Ana Bilgisayar adı , aksi takdirde özel sondada yapılandırılmadığı sürece belirtilmelidir.
* \<http:// ana bilgisayara\>yapılan\<\>\<bir\> çağrının: bağlantı noktası yolunun 200'lük bir HTTP sonuç kodunu döndürttürün.
* Aralık, Zaman Aralığı ve UnhealtyThreshold'un kabul edilebilir aralıklar içinde olduğundan emin olun.
* BIR HTTPS sondası kullanıyorsanız, arka uç sunucusunun arka uç sunucusunda bir geri dönüş sertifikası yapılandırarak SNI gerektirmediğinden emin olun.

## <a name="request-time-out"></a>Zaman dilimi isteği

### <a name="cause"></a>Nedeni

Bir kullanıcı isteği aldığında, uygulama ağ geçidi yapılandırılan kuralları isteğe uygular ve onu arka uç havuz örneğine yönlendirir. Arka uç örneğinden bir yanıt için yapılandırılabilir bir zaman aralığı bekler. Varsayılan olarak, bu aralık **20** saniyedir. Uygulama ağ geçidi bu aralıkta arka uç uygulamasından yanıt almazsa, kullanıcı isteği 502 hatası alır.

### <a name="solution"></a>Çözüm

Uygulama Ağ Geçidi, bu ayarı daha sonra farklı havuzlara uygulanabilen BackendHttpSetting aracılığıyla yapılandırmanızı sağlar. Farklı arka uç havuzları farklı BackendHttpSetting olabilir ve farklı bir istek zaman dışarı yapılandırılmıştır.

```azurepowershell
    New-AzApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="empty-backendaddresspool"></a>Boş BackendAddressPool

### <a name="cause"></a>Nedeni

Uygulama ağ geçidinde arka uç adres havuzunda yapılandırılan VM'ler veya sanal makine ölçeği kümesi yoksa, müşteri isteğini yönlendiremez ve kötü bir ağ geçidi hatası gönderir.

### <a name="solution"></a>Çözüm

Arka uç adres havuzunun boş olmadığından emin olun. Bu powershell, CLI veya portal üzerinden yapılabilir.

```azurepowershell
Get-AzApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

Önceki cmdlet çıkış boş olmayan arka uç adres havuzu içermelidir. Aşağıdaki örnek, arka uç VM'leri için bir FQDN veya IP adresleriyle yapılandırılan döndürülen iki havuzu gösterir. BackendAddressPool'un sağlama durumu 'Başarılı' olmalıdır.

BackendAddressPoolsText:

```json
[{
    "BackendAddresses": [{
        "ipAddress": "10.0.0.10",
        "ipAddress": "10.0.0.11"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool01",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
}, {
    "BackendAddresses": [{
        "Fqdn": "xyx.cloudapp.net",
        "Fqdn": "abc.cloudapp.net"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool02",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
}]
```

## <a name="unhealthy-instances-in-backendaddresspool"></a>BackendAddressPool'da sağlıksız durumlar

### <a name="cause"></a>Nedeni

BackendAddressPool'un tüm örnekleri sağlıksızsa, uygulama ağ geçidinde kullanıcı isteğini yönlendirmek için herhangi bir arka uç yoktur. Bu durum, arka uç örnekleri nin sağlıklı olduğu, ancak gerekli uygulamanın dağıtılmamadığında da olabilir.

### <a name="solution"></a>Çözüm

Örneklerin sağlıklı olduğundan ve uygulamanın düzgün şekilde yapılandırıldığından emin olun. Arka uç örneklerinin aynı VNet'teki başka bir VM'den gelen ping'e yanıt verilip verilemeyecini denetleyin. Genel bir bitiş noktasıyla yapılandırılırsa, web uygulamasına yönelik bir tarayıcı isteğinin servis edilebilir olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Önceki adımlar sorunu çözmezse, bir destek [bileti](https://azure.microsoft.com/support/options/)açın.

