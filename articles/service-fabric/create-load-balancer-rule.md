---
title: Küme için Azure Yük Dengeleyici seçimi oluş
description: Azure Hizmet Kumaşı kümeniz için bağlantı noktalarını açacak şekilde bir Azure Yük Dengeleyicisi yapılandırın.
ms.topic: conceptual
ms.date: 12/06/2017
ms.openlocfilehash: f4599b2e0174381ab7df04aeeb33db7e3ee60f26
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025393"
---
# <a name="open-ports-for-a-service-fabric-cluster"></a>Service Fabric kümesi için bağlantı noktası açma

Azure Hizmet Kumaş kümenizle dağıtılan yük dengeleyicisi, trafiği bir düğüm üzerinde çalışan uygulamanıza yönlendirir. Uygulamanızı farklı bir bağlantı noktası kullanacak şekilde değiştirirseniz, söz konusu bağlantı noktasını (veya farklı bir bağlantı noktasını yönlendiren) Azure Yük Dengeleyicisi'nde ortaya çıkarmanız gerekir.

Service Fabric kümenizi Azure'a dağıttığınızda, sizin için otomatik olarak bir yük dengeleyici oluşturuldu. Yük dengeleyiciniz yoksa, [bkz.](../load-balancer/load-balancer-get-started-internet-portal.md)


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configure-service-fabric"></a>Hizmet dokusunun yapılandırılması

Servis Kumaşı uygulama **ServiceManifest.xml** config dosyanız uygulamanızın kullanmayı beklediği uç noktaları tanımlar. Config dosyası bir bitiş noktası tanımlamak için güncelleştirildikten sonra, yük dengeleyicisi bu (veya farklı) bağlantı noktasını ortaya çıkarmak için güncelleştirilmelidir. Hizmet dokusu bitiş noktasının nasıl oluşturulacığı hakkında daha fazla bilgi için [bkz.](service-fabric-service-manifest-resources.md)

## <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük Dengeleyici kuralı, internete bakan bir bağlantı noktası açar ve trafiği uygulamanız tarafından kullanılan iç düğümün bağlantı noktasına iletin. Yük dengeleyiciniz yoksa, [bkz.](../load-balancer/load-balancer-get-started-internet-portal.md)

Yük Dengeleyici kuralı oluşturmak için aşağıdaki bilgileri toplamanız gerekir:

- Yük dengeleyici adı.
- Yük dengeleyici ve hizmet kumaş kümesikaynak grubu.
- Dış bağlantı noktası.
- İç bağlantı noktası.

## <a name="azure-cli"></a>Azure CLI
**Azure CLI**ile yük dengeleyici kuralı oluşturmak için yalnızca tek bir komut gerekiyor. Yeni bir kural oluşturmak için hem yük dengeleyicisinin hem de kaynak grubunun adını bilmeniz gerekir.

>[!NOTE]
>Yük dengeleyicisinin adını belirlemeniz gerekiyorsa, tüm yük dengeleyicilerinin ve ilişkili kaynak gruplarının listesini hızlı bir şekilde almak için bu komutu kullanın.
>
>`az network lb list --query "[].{ResourceGroup: resourceGroup, Name: name}"`
>


```azurecli
az network lb rule create --backend-port 40000 --frontend-port 39999 --protocol Tcp --lb-name LB-svcfab3 -g svcfab_cli -n my-app-rule
```

Azure CLI komutu aşağıdaki tabloda açıklanan birkaç parametreye sahiptir:

| Parametre | Açıklama |
| --------- | ----------- |
| `--backend-port`  | Service Fabric uygulamasının dinlediği bağlantı noktası. |
| `--frontend-port` | Yük dengeleyicisinin dış bağlantılar için açığa çıkardığı bağlantı noktası. |
| `-lb-name` | Değiştirmek için yük dengeleyiciadı. |
| `-g`       | Hem yük dengeleyicisi hem de Hizmet Kumaşı kümesine sahip kaynak grubu. |
| `-n`       | Kuralın istenilen adı. |


>[!NOTE]
>Azure CLI ile yük dengeleyicisi oluşturma hakkında daha fazla bilgi için [bkz.](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)

## <a name="powershell"></a>PowerShell

PowerShell, Azure CLI'den biraz daha karmaşıktır. Bir kural oluşturmak için aşağıdaki kavramsal adımları izleyin:

1. Yük bakiyesini Azure'dan alın.
2. Bir kural oluşturun.
3. Kuralı yük dengeleyicisine ekleyin.
4. Yük dengeleyicisini güncelleştirin.

>[!NOTE]
>Yük bakiyesi sahibinin adını belirlemeniz gerekiyorsa, tüm yük dengeleyicilerinin ve ilişkili kaynak gruplarının listesini hızla almak için bu komutu kullanın.
>
>`Get-AzLoadBalancer | Select Name, ResourceGroupName`

```powershell
# Get the load balancer
$lb = Get-AzLoadBalancer -Name LB-svcfab3 -ResourceGroupName svcfab_cli

# Create the rule based on information from the load balancer.
$lbrule = New-AzLoadBalancerRuleConfig -Name my-app-rule7 -Protocol Tcp -FrontendPort 39990 -BackendPort 40009 `
                                            -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
                                            -BackendAddressPool  $lb.BackendAddressPools[0] `
                                            -Probe $lb.Probes[0]

# Add the rule to the load balancer
$lb.LoadBalancingRules.Add($lbrule)

# Update the load balancer on Azure
$lb | Set-AzLoadBalancer
```

Komutla `New-AzLoadBalancerRuleConfig` ilgili olarak, `-FrontendPort` yük dengeleyicisinin dış bağlantılar için açığa `-BackendPort` çıkardığı bağlantı noktasını ve servis kumaşı uygulamasının dinlediği bağlantı noktasını temsil eder.

>[!NOTE]
>PowerShell ile yük dengeleyicisi oluşturma hakkında daha fazla bilgi için [PowerShell ile yük dengeleyicisi oluşturma](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)konusuna bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Service Fabric'te ağ ağı](service-fabric-patterns-networking.md)hakkında daha fazla bilgi edinin.
