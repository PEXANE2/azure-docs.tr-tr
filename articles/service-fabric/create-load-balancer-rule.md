---
title: Küme için Azure Load Balancer kuralı oluşturma
description: Azure Service Fabric kümeniz için bağlantı noktaları açmak üzere bir Azure Load Balancer yapılandırın.
ms.topic: conceptual
ms.date: 12/06/2017
ms.openlocfilehash: 049afb88b5552c92294180069b54d49574a410fa
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259043"
---
# <a name="open-ports-for-a-service-fabric-cluster"></a>Service Fabric kümesi için bağlantı noktası açma

Azure Service Fabric kümeniz ile dağıtılan yük dengeleyici, trafiği bir düğümde çalışan uygulamanıza yönlendirir. Uygulamanızı farklı bir bağlantı noktası kullanacak şekilde değiştirirseniz, bu bağlantı noktasını (veya farklı bir bağlantı noktasını yönlendirmelisiniz) Azure Load Balancer.

Service Fabric kümenizi Azure 'a dağıttığınızda, sizin için otomatik olarak bir yük dengeleyici oluşturulur. Yük dengeleyiciniz yoksa, bkz. [Internet 'e yönelik yük dengeleyici yapılandırma](../load-balancer/quickstart-load-balancer-standard-public-portal.md).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configure-service-fabric"></a>Service Fabric 'i yapılandırma

Service Fabric uygulamanızın **ServiceManifest.xml** yapılandırma dosyası, uygulamanızın kullanmasını beklediği uç noktaları tanımlar. Yapılandırma dosyası bir uç nokta tanımlamak üzere güncelleştirildikten sonra, bu (veya farklı) bir bağlantı noktasını göstermek için yük dengeleyici 'nin güncellenmesi gerekir. Service Fabric uç noktası oluşturma hakkında daha fazla bilgi için bkz. [bir uç nokta kurma](service-fabric-service-manifest-resources.md).

## <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Bir Load Balancer kuralı, internet 'e yönelik bir bağlantı noktası açar ve trafiği, uygulamanız tarafından kullanılan iç düğümün bağlantı noktasına iletir. Yük dengeleyiciniz yoksa, bkz. [Internet 'e yönelik yük dengeleyici yapılandırma](../load-balancer/quickstart-load-balancer-standard-public-portal.md).

Load Balancer kuralı oluşturmak için aşağıdaki bilgileri toplamanız gerekir:

- Yük dengeleyici adı.
- Yük dengeleyici ve Service Fabric kümesinin kaynak grubu.
- Dış bağlantı noktası.
- İç bağlantı noktası.

## <a name="azure-cli"></a>Azure CLI
Yalnızca **Azure CLI**ile yük dengeleyici kuralı oluşturmak için tek bir komut alır. Yeni bir kural oluşturmak için yalnızca yük dengeleyici ve kaynak grubunun adını bilmeniz gerekir.

>[!NOTE]
>Yük dengeleyicinin adını belirlemeniz gerekiyorsa, tüm yük dengeleyiciler ve ilişkili kaynak gruplarının listesini hızlıca almak için bu komutu kullanın.
>
>`az network lb list --query "[].{ResourceGroup: resourceGroup, Name: name}"`
>


```azurecli
az network lb rule create --backend-port 40000 --frontend-port 39999 --protocol Tcp --lb-name LB-svcfab3 -g svcfab_cli -n my-app-rule
```

Azure CLı komutu, aşağıdaki tabloda açıklanan birkaç parametreye sahiptir:

| Parametre | Açıklama |
| --------- | ----------- |
| `--backend-port`  | Service Fabric uygulamanın dinlediği bağlantı noktası. |
| `--frontend-port` | Yük dengeleyicinin dış bağlantılar için sunduğu bağlantı noktası. |
| `-lb-name` | Değiştirilecek yük dengeleyicinin adı. |
| `-g`       | Hem yük dengeleyici hem de Service Fabric kümesine sahip olan kaynak grubu. |
| `-n`       | Kuralın istenen adı. |


>[!NOTE]
>Azure CLı ile yük dengeleyici oluşturma hakkında daha fazla bilgi için bkz. [Azure CLI ile yük dengeleyici oluşturma](../load-balancer/load-balancer-get-started-ilb-arm-cli.md).

## <a name="powershell"></a>PowerShell

PowerShell, Azure CLı 'den biraz daha karmaşıktır. Bir kural oluşturmak için bu kavramsal adımları izleyin:

1. Azure 'dan yük dengeleyiciyi alın.
2. Bir kural oluşturun.
3. Kuralı yük dengeleyiciye ekleyin.
4. Yük dengeleyiciyi güncelleştirin.

>[!NOTE]
>Yük dengeleyicinin adını belirlemeniz gerekiyorsa, tüm yük dengeleyiciler ve ilişkili kaynak gruplarının listesini hızlıca almak için bu komutu kullanın.
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

Komutuyla ilgili `New-AzLoadBalancerRuleConfig` olarak, `-FrontendPort` Yük dengeleyicinin dış bağlantılar için sunduğu bağlantı noktasını temsil eder ve `-BackendPort` Service Fabric uygulamasının dinlediği bağlantı noktasını temsil eder.

>[!NOTE]
>PowerShell ile yük dengeleyici oluşturma hakkında daha fazla bilgi için bkz. [PowerShell ile yük dengeleyici oluşturma](../load-balancer/load-balancer-get-started-ilb-arm-ps.md).

## <a name="next-steps"></a>Sonraki adımlar

[Service Fabric 'de ağ oluşturma](service-fabric-patterns-networking.md)hakkında daha fazla bilgi edinin.
