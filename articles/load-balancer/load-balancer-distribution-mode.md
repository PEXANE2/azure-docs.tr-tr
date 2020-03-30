---
title: Azure Yük Bakiyesi dağıtım modunu yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, kaynak IP yakınlığını destekleyecek Azure Yük Dengeleyicisi'nin dağıtım modunu yapılandırmaya başlayın.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/19/2019
ms.author: allensu
ms.openlocfilehash: 5c50186692438be5d0922cd329c28e665310e5c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023540"
---
# <a name="configure-the-distribution-mode-for-azure-load-balancer"></a>Azure Yük Dengeleyicisi için dağıtım modunu yapılandırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="hash-based-distribution-mode"></a>Karma tabanlı dağıtım modu

Azure Load Balancer’ın varsayılan dağıtım modu beşli tanımlama grubu olan karmadır. 

Tuple oluşur:
* **Kaynak IP**
* **Kaynak bağlantı noktası**
* **Hedef IP**
* **Hedef bağlantı noktası**
* **Protokol türü**

Karma, trafiği kullanılabilir sunuculara eşlemek için kullanılır. Algoritma yalnızca bir aktarım oturumu içinde yapışkanlık sağlar. Aynı oturumda bulunan paketler, yük dengeli bitiş noktasının arkasındaki aynı veri merkezi IP'sine yönlendirilir. İstemci aynı kaynak IP'den yeni bir oturum başlattığında, kaynak bağlantı noktası değişir ve trafiğin farklı bir veri merkezi bitiş noktasına gitmesine neden olur.

![Beş tuple karma tabanlı dağıtım modu](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

## <a name="source-ip-affinity-mode"></a>Kaynak IP yakınlık modu

Yük dengeleyici si, kaynak IP yakınlık dağıtım modu kullanılarak da yapılandırılabilir. Bu dağıtım modu oturum benzeşimi veya istemci IP’si benzeşimi olarak da bilinir. Mod, trafiği kullanılabilir sunuculara eşlemek için iki tuple (kaynak IP ve hedef IP) veya üç tuple (kaynak IP, hedef IP ve protokol türü) karma kullanır. Kaynak IP yakınlığını kullanarak, aynı istemci bilgisayardan başlatılan bağlantılar aynı veri merkezi bitiş noktasına gider.

Aşağıdaki şekil iki tuple yapılandırmasını göstermektedir. İki tuple'ın yük dengeleyiciden sanal makine 1'e (VM1) nasıl çalıştığına dikkat edin. VM1 daha sonra VM2 ve VM3 tarafından yedeklenir.

![İki tuple oturum afinite dağılım modu](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Kaynak IP yakınlık modu, Azure Yük Dengeleyicisi ile Uzak Masaüstü Ağ Geçidi (RD Ağ Geçidi) arasındaki uyumsuzluğu giderır. Bu modu kullanarak, tek bir bulut hizmetinde bir RD Ağ Geçidi çiftliği oluşturabilirsiniz.

Başka bir kullanım örneği senaryosu medya yüklemesidir. Veri yüklemesi UDP üzerinden gerçekleştirilir, ancak kontrol düzlemi TCP üzerinden elde edilir:

* İstemci, yük dengeli genel adresine bir TCP oturumu başlatır ve belirli bir DIP'e yönlendirilir. Kanal bağlantı durumunu izlemek için etkin bırakılır.
* Aynı istemci bilgisayardan yeni bir UDP oturumu aynı yük dengeli ortak bitiş noktasına başlatılır. Bağlantı, önceki TCP bağlantısıyla aynı DIP bitiş noktasına yönlendirilir. TCP üzerinden bir kontrol kanalı korurken medya yüklemesi yüksek iş seviyesinde yürütülebilir.

> [!NOTE]
> Sanal makineyi kaldırarak veya ekleyerek yük dengeli bir küme değiştiğinde, istemci isteklerinin dağıtımı yeniden hesaplanır. Varolan istemcilerin yeni bağlantılarının aynı sunucuda sona erdirilene güvenemezsiniz. Ayrıca, kaynak IP yakınlık dağıtım modunu kullanarak trafik eşit olmayan bir dağıtıma neden olabilir. Yakınlık ların arkasında çalışan istemciler benzersiz bir istemci uygulaması olarak görülebilir.

## <a name="configure-source-ip-affinity-settings"></a>Kaynak IP afinite ayarlarını yapılandırma

### <a name="azure-portal"></a>Azure portalında

Portaldaki yük dengeleme kuralını değiştirerek dağıtım modunun yapılandırmasını değiştirebilirsiniz.

1. Azure portalında oturum açın ve **Kaynak Grupları'nı**tıklayarak değiştirmek istediğiniz yük bakiyesini içeren Kaynak Grubunu bulun.
2. Yük dengeleyiciye genel bakış ekranında Ayarlar **altında**Yük dengeleme **kurallarına** tıklayın.
3. Yük dengeleme kuralları ekranında, dağıtım modunu değiştirmek istediğiniz yük dengeleme kuralını tıklatın.
4. Kural altında, Oturum **kalıcılığı** açılır kutusunu değiştirerek dağıtım modu değiştirilir.  Aşağıdaki seçenekler mevcuttur:
    
    * **Yok (karma tabanlı)** - Aynı istemciden art arda gelen isteklerin herhangi bir sanal makine tarafından işlenebilir olduğunu belirtir.
    * **İstemci IP (kaynak IP afinite 2-tuple)** - Aynı istemci IP adresinden art arda gelen isteklerin aynı sanal makine tarafından işleneceğini belirtir.
    * **İstemci IP ve protokolü (kaynak IP yakınlığı 3-tuple)** - Aynı istemci IP adresi ve protokol kombinasyonundan art arda gelen isteklerin aynı sanal makine tarafından işleneceğini belirtir.

5. Dağıtım modunu seçin ve ardından **Kaydet'i**tıklatın.

### <a name="azure-powershell"></a>Azure PowerShell

Kaynak Yöneticisi ile dağıtılan sanal makineler için, varolan bir yük dengeleme kuralındaki yük dengeleyici dağıtım ayarlarını değiştirmek için PowerShell'i kullanın. Aşağıdaki komut dağıtım modunu güncelleştirir: 

```azurepowershell-interactive
$lb = Get-AzLoadBalancer -Name MyLb -ResourceGroupName MyLbRg
$lb.LoadBalancingRules[0].LoadDistribution = 'sourceIp'
Set-AzLoadBalancer -LoadBalancer $lb
```

Klasik sanal makineler için dağıtım ayarlarını değiştirmek için Azure PowerShell'i kullanın. Sanal makineye Azure bitiş noktası ekleyin ve yük dengeleyici dağıtım modunu yapılandırın:

```azurepowershell-interactive
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

Gerekli yük dengeleme `LoadBalancerDistribution` miktarı için öğenin değerini ayarlayın. İki tuple (kaynak IP ve hedef IP) yük dengelemesi için sourceIP belirtin. Üç tuple (kaynak IP, hedef IP ve protokol türü) yük dengelemesi için sourceIPProtocol'u belirtin. Beş tuple yük dengelemenin varsayılan davranışı için hiçbirini belirtmeyin.

Bu ayarları kullanarak bir uç nokta yük dengeleyici dağıtım modu yapılandırması alın:

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

`LoadBalancerDistribution` Öğe olmadığında, Azure Yük Dengeleyici varsayılan beş-tuple algoritmasını kullanır.

### <a name="configure-distribution-mode-on-load-balanced-endpoint-set"></a>Yük dengeli uç nokta kümesinde dağıtım modunu yapılandırma

Uç noktalar yük dengeli uç noktası kümesinin bir parçası olduğunda, dağıtım modu yük dengeli uç nokta kümesi üzerinde yapılandırılmalıdır:

```azurepowershell-interactive
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="configure-distribution-mode-for-cloud-services-endpoints"></a>Bulut Hizmetleri uç noktaları için dağıtım modunu yapılandırma

Bulut hizmetinizi güncellemek için .NET 2.5 için Azure SDK'yı kullanın. Bulut Hizmetleri için uç nokta ayarları .csdef dosyasında yapılır. Bulut Hizmetleri dağıtımı için yük bakiyesi dağıtım modunu güncelleştirmek için dağıtım yükseltmesi gereklidir.

Bitiş noktası ayarları için .csdef değişikliklerine bir örnek aşağıda verilmiştir:

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

Aşağıdaki örnek, dağıtımda belirli bir yük dengeli kümesi için yük dengeleyici dağıtım modunun nasıl yeniden yapılandırılabildiğini gösterir. 

### <a name="change-distribution-mode-for-deployed-load-balanced-set"></a>Dağıtılan yük dengeli kümesi için dağıtım modunu değiştirme

Varolan bir dağıtım yapılandırmasını değiştirmek için Azure klasik dağıtım modelini kullanın. Üstbilgi `x-ms-version` ekleyin ve değeri sürüm 2014-09-01 veya sonraki sürümolarak ayarlayın.

#### <a name="request"></a>İstek

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet   x-ms-version: 2014-09-01
    Content-Type: application/xml

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

Daha önce açıklandığı gibi, `LoadBalancerDistribution` iki tuple yakınlık için sourceIP için öğeyi ayarlayın, üç tuple yakınlık için sourceIPProtocol veya hiçbir yakınlık (beş-tuple yakınlık) için hiçbiri.

#### <a name="response"></a>Yanıt

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Dahili Yük Dengeleyicisi'ne genel bakış](load-balancer-internal-overview.md)
* [İnternete bakan yük dengeleyicisi yapılandırmaya başlayın](quickstart-create-standard-load-balancer-powershell.md)
* [Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
