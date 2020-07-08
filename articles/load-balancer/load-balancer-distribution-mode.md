---
title: Azure Load Balancer Dağıtım modunu yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, kaynak IP benzeşimini desteklemek üzere Azure Load Balancer için Dağıtım modunu yapılandırmaya başlayın.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/19/2019
ms.author: allensu
ms.openlocfilehash: 82c203322f1a417fa006c5228d957c178a706b3a
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85961022"
---
# <a name="configure-the-distribution-mode-for-azure-load-balancer"></a>Azure Load Balancer için Dağıtım modunu yapılandırın

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="hash-based-distribution-mode"></a>Karma tabanlı dağıtım modu

Azure Load Balancer’ın varsayılan dağıtım modu beşli tanımlama grubu olan karmadır. 

Tanımlama grubu şunlardan oluşur:
* **Kaynak IP 'si**
* **Kaynak bağlantı noktası**
* **Hedef IP**
* **Hedef bağlantı noktası**
* **Protokol türü**

Karma, trafiği kullanılabilir sunucularla eşlemek için kullanılır. Algoritma yalnızca bir aktarım oturumunda bir veya daha fazla sürekliliği sağlar. Aynı oturumdaki paketler, yük dengeli uç noktanın arkasındaki veri merkezi IP 'ye yönlendirilir. İstemci aynı kaynak IP 'den yeni bir oturum başlattığında, kaynak bağlantı noktası değişir ve trafiğin farklı bir veri merkezi uç noktasına geçmesine neden olur.

![Beş demet temelli karma tabanlı dağıtım modu](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

## <a name="source-ip-affinity-mode"></a>Kaynak IP benzeşim modu

Yük dengeleyici, kaynak IP benzeşimi dağıtım modu kullanılarak da yapılandırılabilir. Bu dağıtım modu oturum benzeşimi veya istemci IP’si benzeşimi olarak da bilinir. Bu mod, trafiği kullanılabilir sunucularla eşlemek için iki demet (kaynak IP ve hedef IP) ya da üç demet (kaynak IP, hedef IP ve protokol türü) karmasını kullanır. Kaynak IP benzeşimini kullanarak, aynı istemci bilgisayarından başlatılan bağlantılar aynı veri merkezi uç noktasına gider.

Aşağıdaki şekilde iki demet yapılandırması gösterilmektedir. İki demet, yük dengeleyici üzerinden sanal makine 1 ' e (VM1) nasıl çalışdığına dikkat edin. VM1 daha sonra VM2 ve VM3 tarafından yedeklenir.

![İki demet oturum benzeşimi dağıtım modu](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Kaynak IP benzeşimi modu Azure Load Balancer ve Uzak Masaüstü Ağ Geçidi (RD Ağ Geçidi) arasında bir uyumsuzluk çözer. Bu modu kullanarak, tek bir bulut hizmetinde bir RD Ağ Geçidi grubu oluşturabilirsiniz.

Başka bir kullanım örneği senaryosu medya karşıya yükleme 'dir. Karşıya veri yükleme işlemi UDP üzerinden yapılır, ancak denetim düzlemi TCP aracılığıyla gerçekleştirilir:

* İstemci, yük dengeli ortak adrese bir TCP oturumu başlatır ve belirli bir DIP 'ye yönlendirilir. Kanal, bağlantı durumunu izlemek için etkin bırakılır.
* Aynı istemci bilgisayardan gelen yeni bir UDP oturumu aynı yük dengeli ortak uç noktaya başlatılır. Bağlantı, önceki TCP bağlantısıyla aynı DIP uç noktasına yönlendirilir. Medya karşıya yüklemesi, TCP üzerinden bir denetim kanalını koruyarak yüksek aktarım düzeyinde yürütülebilir.

> [!NOTE]
> Bir sanal makineyi kaldırarak veya ekleyerek yük dengeli bir küme değiştirildiğinde, istemci isteklerinin dağıtımı yeniden hesaplanır. Mevcut istemcilerden gelen yeni bağlantılara, aynı sunucuda son vermek için bağlı yapamazsınız. Ayrıca, kaynak IP benzeşimi Dağıtım modunu kullanmak trafiğin eşit bir şekilde dağıtımına neden olabilir. Proxy 'nin arkasında çalışan istemciler tek bir benzersiz istemci uygulaması olarak görünebilir.

## <a name="configure-source-ip-affinity-settings"></a>Kaynak IP benzeşimi ayarlarını yapılandırma

### <a name="azure-portal"></a>Azure portal

Portalda Yük Dengeleme kuralını değiştirerek dağıtım modunun yapılandırmasını değiştirebilirsiniz.

1. Azure portal oturum açın ve **kaynak grupları**' na tıklayarak değiştirmek istediğiniz yük dengeleyiciyi Içeren kaynak grubunu bulun.
2. Yük Dengeleyiciye genel bakış ekranında **Ayarlar**' ın altındaki **Yük Dengeleme kuralları** ' na tıklayın.
3. Yük Dengeleme kuralları ekranında, Dağıtım modunu değiştirmek istediğiniz yük dengeleme kuralına tıklayın.
4. Kural altında, **oturum kalıcılığı** açılan kutusu değiştirilerek dağıtım modu değiştirilir.  Aşağıdaki seçenekler mevcuttur:
    
    * **Hiçbiri (karma tabanlı)** -aynı istemciden gelen ardışık isteklerin herhangi bir sanal makine tarafından işlenebileceğini belirtir.
    * **ISTEMCI IP (kaynak IP benzeşimi 2-kayıt düzeni)** -aynı istemci IP adresinden gelen isteklerin aynı sanal makine tarafından işleneceğini belirtir.
    * **İstemci IP 'si ve Protokolü (kaynak IP benzeşimi 3-kayıt)** -aynı istemci IP adresi ve protokol birleşimlerinden gelen ardışık isteklerin aynı sanal makine tarafından işleneceğini belirtir.

5. Dağıtım modunu seçin ve ardından **Kaydet**' e tıklayın.

### <a name="azure-powershell"></a>Azure PowerShell

Kaynak Yöneticisi ile dağıtılan sanal makineler için, var olan bir yük dengeleme kuralında yük dengeleyici dağıtım ayarlarını değiştirmek için PowerShell kullanın. Aşağıdaki komut Dağıtım modunu güncelleştirir: 

```azurepowershell-interactive
$lb = Get-AzLoadBalancer -Name MyLb -ResourceGroupName MyLbRg
$lb.LoadBalancingRules[0].LoadDistribution = 'sourceIp'
Set-AzLoadBalancer -LoadBalancer $lb
```

Klasik sanal makinelerde, dağıtım ayarlarını değiştirmek için Azure PowerShell kullanın. Bir sanal makineye Azure uç noktası ekleyin ve yük dengeleyici Dağıtım modunu yapılandırın:

```azurepowershell-interactive
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

`LoadBalancerDistribution`Gerekli Yük Dengeleme miktarı için öğesinin değerini ayarlayın. İki demet (kaynak IP ve hedef IP) yük dengelemesi için SourceIP 'yi belirtin. Üç demet için Sourceıpprotocol (kaynak IP, hedef IP ve protokol türü) yük dengelemesi belirleyin. Beş demet yük dengelemenin varsayılan davranışı için hiçbiri ' ni belirtin.

Bu ayarları kullanarak bir uç nokta yük dengeleyici dağıtım modu yapılandırması alın:

```azurepowershell
PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
LBSetName : MyLoadBalancedSet
LocalPort : 80
Name : HTTP
Port : 80
Protocol : tcp
Vip : 65.52.xxx.xxx
ProbePath :
ProbePort : 80
ProbeProtocol : tcp
ProbeIntervalInSeconds : 15
ProbeTimeoutInSeconds : 31
EnableDirectServerReturn : False
Acl : {}
InternalLoadBalancerName :
IdleTimeoutInMinutes : 15
LoadBalancerDistribution : sourceIP
```

`LoadBalancerDistribution`Öğe mevcut olmadığında, Azure Load Balancer varsayılan beş demet algoritmasını kullanır.

### <a name="configure-distribution-mode-on-load-balanced-endpoint-set"></a>Yük dengeli uç nokta kümesinde Dağıtım modunu yapılandırma

Uç noktalar yük dengeli bir uç nokta kümesinin parçasıysa, yük dengeli uç nokta kümesinde dağıtım modunun yapılandırılması gerekir:

```azurepowershell-interactive
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="configure-distribution-mode-for-cloud-services-endpoints"></a>Cloud Services uç noktaları için Dağıtım modunu yapılandırma

Bulut hizmetinizi güncelleştirmek için .NET 2,5 için Azure SDK 'sını kullanın. Cloud Services için uç nokta ayarları. csdef dosyasında yapılır. Bir Cloud Services dağıtımı için yük dengeleyici Dağıtım modunu güncelleştirmek için, bir dağıtım yükseltmesi gerekir.

Uç nokta ayarları için bir. csdef değişikliği örneği aşağıda verilmiştir:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
    </Endpoints>
</WorkerRole>
<NetworkConfiguration>
    <VirtualNetworkSite name="VNet"/>
    <AddressAssignments>
<InstanceAddress roleName="VMRolePersisted">
    <PublicIPs>
    <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
    </PublicIPs>
</InstanceAddress>
    </AddressAssignments>
</NetworkConfiguration>
```

## <a name="api-example"></a>API örneği

Aşağıdaki örnekte, bir dağıtımda belirtilen yük dengeli bir küme için yük dengeleyici dağıtım modunun nasıl yeniden yapılandırılacağı gösterilmektedir. 

### <a name="change-distribution-mode-for-deployed-load-balanced-set"></a>Dağıtılan yük dengeli küme için Dağıtım modunu değiştir

Mevcut bir dağıtım yapılandırmasını değiştirmek için klasik Azure dağıtım modelini kullanın. Üstbilgiyi ekleyin `x-ms-version` ve değeri sürüm 2014-09-01 veya üzeri olarak ayarlayın.

#### <a name="request"></a>İstek

```http
POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet   x-ms-version: 2014-09-01
Content-Type: application/xml
```

```xml
<LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
    <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
            <Port> port-assigned-to-probe </Port>
            <Protocol> probe-protocol </Protocol>
            <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
            <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
    </InputEndpoint>
</LoadBalancedEndpointList>
```

Daha önce açıklandığı gibi, `LoadBalancerDistribution` öğesini iki demet benzeşim Için SourceIP, üç demet benzeşim Için sourceIPProtocol veya hiçbir benzeşim (beş demet olmayan benzeşim) için hiçbiri olarak ayarlayın.

#### <a name="response"></a>Yanıt

```http
HTTP/1.1 202 Accepted
Cache-Control: no-cache
Content-Length: 0
Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
x-ms-servedbyregion: ussouth2
x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
Date: Thu, 16 Oct 2014 22:49:21 GMT
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Iç Load Balancer genel bakış](load-balancer-internal-overview.md)
* [İnternet 'e yönelik yük dengeleyici yapılandırma ile çalışmaya başlama](quickstart-create-standard-load-balancer-powershell.md)
* [Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
