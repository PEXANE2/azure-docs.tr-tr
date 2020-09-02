---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 07/22/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 93dcd58550d16a25c9d886107692c03c782596ff
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379266"
---
## <a name="azure-security-benchmark"></a>Azure Güvenlik Karşılaştırması

[Azure Güvenlik kıyaslaması](../../../../articles/security/benchmarks/overview.md) , bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. Bu hizmetin Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için bkz. [Azure Güvenlik kıyaslaması eşleme dosyaları](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

Tüm Azure hizmetleri için kullanılabilen Azure Ilkesi 'nin Bu uyumluluk standardına nasıl eşlendiğini gözden geçirmek için bkz. [Azure Ilke mevzuatı uyumluluğu-Azure Güvenlik kıyaslaması](../../../../articles/governance/policy/samples/azure-security-benchmark.md).

|Etki alanı |Denetim KIMLIĞI |Denetim başlığı |İlke<br /><sub>(Azure portal)</sub> |İlke sürümü<br /><sub>GitHub</sub>  |
|---|---|---|---|---|
|Ağ Güvenliği |1.1 |Sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruma |[Tüm Internet trafiği, dağıtılan Azure Güvenlik duvarınız aracılığıyla yönlendirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffc5e4038-4584-4632-8c85-c0448d374b2c) |[2.0.0-Önizleme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/ASC_All_Internet_traffic_should_be_routed_via_Azure_Firewall.json) |
|Ağ Güvenliği |1.1 |Sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruma |[Alt ağlar bir ağ güvenlik grubuyla ilişkilendirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe71308d3-144b-4262-b144-efdc3cc90517) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_NetworkSecurityGroupsOnSubnets_Audit.json) |
|Ağ Güvenliği |1.1 |Sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruma |[Sanal makinelerin onaylanan bir sanal ağa bağlanması gerekir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd416745a-506c-48b6-8ab1-83cb814bcaa3) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/ApprovedVirtualNetwork_Audit.json) |
|Ağ Güvenliği |1.1 |Sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruma |[Sanal ağlar belirtilen sanal ağ geçidini kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff1776c76-f58c-4245-a8d0-2b207198dc8b) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/VirtualNetwork_ApprovedVirtualNetworkGateway_AuditIfNotExists.json) |
|Ağ Güvenliği |1.2 |VNET 'ler, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin |[Ağ Izleyicisi etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb6e2945c-0b7b-40f5-9233-7a5323b5cdc6) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkWatcher_Enabled_Audit.json) |
|Ağ Güvenliği |1.4 |Bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme |[Tüm Internet trafiği, dağıtılan Azure Güvenlik duvarınız aracılığıyla yönlendirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffc5e4038-4584-4632-8c85-c0448d374b2c) |[2.0.0-Önizleme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/ASC_All_Internet_traffic_should_be_routed_via_Azure_Firewall.json) |
|Ağ Güvenliği |1.4 |Bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme |[Azure DDoS koruma standardı etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa7aca53f-2ed4-4466-a25e-0b45ade68efd) |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableDDoSProtection_Audit.json) |
|Ağ Güvenliği |1,5 |Ağ paketlerini ve akış günlüklerini kaydetme |[Ağ Izleyicisi etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb6e2945c-0b7b-40f5-9233-7a5323b5cdc6) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkWatcher_Enabled_Audit.json) |

## <a name="cis-microsoft-azure-foundations-benchmark"></a>CIS Microsoft Azure Foundations Benchmark

Tüm Azure hizmetleri için kullanılabilir olan Azure Ilkesi 'nin Bu uyumluluk standardına nasıl eşlendiğini gözden geçirmek için bkz. [Azure Ilke mevzuatı uyumluluğu-cıs Microsoft Azure temelleri kıyaslama 1.1.0](../../../../articles/governance/policy/samples/cis-azure-1-1-0.md).
Bu uyumluluk standardı hakkında daha fazla bilgi için bkz. [cıs Microsoft Azure kuruluş kıyaslaması](https://www.cisecurity.org/benchmark/azure/).

|Etki alanı |Denetim KIMLIĞI |Denetim başlığı |İlke<br /><sub>(Azure portal)</sub> |İlke sürümü<br /><sub>GitHub</sub>  |
|---|---|---|---|---|
|Güvenlik Merkezi |2.9 |"Sonraki nesil güvenlik duvarı 'Nı etkinleştir (NGFW) Izlemenin" "devre dışı" olarak Izlenmesi için ASC varsayılan ilke ayarını doğrulayın |[Alt ağlar bir ağ güvenlik grubuyla ilişkilendirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe71308d3-144b-4262-b144-efdc3cc90517) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_NetworkSecurityGroupsOnSubnets_Audit.json) |
|Ağ |6.1 |RDP erişiminin internet 'ten kısıtlanmasını sağlayın |[Internet 'ten RDP erişimi engellenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe372f825-a257-4fb8-9175-797a8a8627d6) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkSecurityGroup_RDPAccess_Audit.json) |
|Ağ |6.2 |SSH erişiminin internet 'ten kısıtlanmasını sağlayın |[Internet 'ten SSH erişimi engellenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2c89a2e5-7285-40fe-afe0-ae8654b92fab) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkSecurityGroup_SSHAccess_Audit.json) |
|Ağ |6.5 |Ağ Izleyicisi 'nin ' etkin ' olduğundan emin olun |[Ağ Izleyicisi etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb6e2945c-0b7b-40f5-9233-7a5323b5cdc6) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkWatcher_Enabled_Audit.json) |

## <a name="nist-sp-800-171-r2"></a>NIST SP 800-171 R2

Tüm Azure hizmetleri için kullanılabilen Azure Ilkesinin Bu uyumluluk standardına nasıl eşlendiğini gözden geçirmek için bkz. [Azure Ilke mevzuatı uyumluluğu-NıST SP 800-171 R2](../../../../articles/governance/policy/samples/nist-sp-800-171-r2.md).
Bu uyumluluk standardı hakkında daha fazla bilgi için bkz. [NıST SP 800-171 R2](https://csrc.nist.gov/publications/detail/sp/800-171/rev-2/final).

|Etki alanı |Denetim KIMLIĞI |Denetim başlığı |İlke<br /><sub>(Azure portal)</sub> |İlke sürümü<br /><sub>GitHub</sub>  |
|---|---|---|---|---|
|Sistem ve bilgi bütünlüğü |3.14.6 |Gelen ve giden iletişim trafiği dahil olmak üzere kuruluş sistemlerini izleyip olası saldırıların saldırılarını ve göstergelerini tespit edin. |[Ağ Izleyicisi etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb6e2945c-0b7b-40f5-9233-7a5323b5cdc6) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkWatcher_Enabled_Audit.json) |

## <a name="nist-sp-800-53-r4"></a>NIST SP 800-53 R4

Tüm Azure hizmetleri için kullanılabilen Azure Ilkesi 'nin Bu uyumluluk standardına nasıl eşlendiğini gözden geçirmek için bkz. [Azure Ilke mevzuatı uyumluluğu-NıST SP 800-53 R4](../../../../articles/governance/policy/samples/nist-sp-800-53-r4.md).
Bu uyumluluk standardı hakkında daha fazla bilgi için bkz. [NıST SP 800-53 R4](https://nvd.nist.gov/800-53).

|Etki alanı |Denetim KIMLIĞI |Denetim başlığı |İlke<br /><sub>(Azure portal)</sub> |İlke sürümü<br /><sub>GitHub</sub>  |
|---|---|---|---|---|
|Sistem ve Iletişim koruması |SC-5 |Hizmet reddi koruması |[Azure DDoS koruma standardı etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa7aca53f-2ed4-4466-a25e-0b45ade68efd) |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableDDoSProtection_Audit.json) |

