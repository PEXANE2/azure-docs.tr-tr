---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 667d017e8febcf1abcc1cfe21ecfaa43ae75ea6d
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172754"
---
|Ad |Açıklama |Efekt (ler) |Sürüm |
|---|---|---|---|
|[İzin verilen sanal makine SKU 'Ları](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMSkusAllowed_Deny.json) |Bu ilke, kuruluşunuzun dağıtabileceği bir sanal makine SKU'ları kümesi belirtmenizi sağlar. |Reddet |1.0.0 |
|[Olağanüstü durum kurtarma yapılandırması olmadan sanal makineleri denetleme](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/RecoveryServices_DisasterRecovery_Audit.json) |Olağanüstü durum kurtarma yapılandırması olmayan sanal makineleri denetleyin. Olağanüstü durum kurtarma hakkında daha fazla bilgi için https://aka.ms/asr-docziyaret edin. |Auditınotexists |1.0.0 |
|[Yönetilen diskleri kullanmayan VM 'Leri denetleme](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMRequireManagedDisk_Audit.json) |Bu ilke yönetilen diskleri kullanmayan VM 'Leri denetler |denetlenmesini |1.0.0 |
|[Windows Server için varsayılan Microsoft ıaasantimalware uzantısını dağıt](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMAntimalwareExtension_Deploy.json) |Bu ilke, bir VM kötü amaçlı yazılımdan koruma uzantısıyla yapılandırılmadığı zaman bir Microsoft ıaasantimalware uzantısını varsayılan yapılandırmayla dağıtır. |deployIfNotExists |1.0.0 |
|[Sanal makine ölçek kümelerindeki tanılama günlükleri etkinleştirilmelidir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/ServiceFabric_and_VMSS_AuditVMSSDiagnostics.json) |Bir olay veya bir uzlaşmaya karşı araştırma gerektiğinde etkinlik izinin yeniden oluşturulması için günlüklerin etkinleştirilmesi önerilir. |Auditınotexists, devre dışı |1.0.0 |
|[Azure için Microsoft kötü amaçlı yazılımdan koruma imzaları otomatik olarak güncelleştirilecek şekilde yapılandırılmalıdır](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VirtualMachines_AntiMalwareAutoUpdate_AuditIfNotExists.json) |Bu ilke, Microsoft kötü amaçlı yazılımdan koruma imzalarının otomatik güncelleştirmesiyle yapılandırılmayan hiçbir Windows sanal makinesini denetler. |Auditınotexists, devre dışı |1.0.0 |
|[Microsoft ıaasantimalware uzantısı Windows Server 'lar üzerinde dağıtılmalıdır](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/WindowsServers_AntiMalware_AuditIfNotExists.json) |Bu ilke, Microsoft ıaasantimalware uzantısı dağıtılan tüm Windows Server VM 'leri denetler. |Auditınotexists, devre dışı |1.0.0 |
|[Yalnızca onaylanan VM uzantıları yüklenmelidir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VirtualMachines_ApprovedExtensions_Audit.json) |Bu ilke onaylanmamış sanal makine uzantılarını yönetir. |Denetim, reddetme, devre dışı |1.0.0 |
|[Sanal makine ölçek kümelerinde otomatik işletim sistemi görüntüsü düzeltme eki uygulamayı gerektir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMSSOSUpgradeHealthCheck_Deny.json) |Bu ilke, her ay en son güvenlik düzeltme eklerini güvenli bir şekilde uygulayarak sanal makineleri her zaman güvenli tutmak üzere sanal makine ölçek kümelerinde otomatik işletim sistemi görüntüsü düzeltme eki uygulamayı etkinleştirmeyi |reddedebilir |1.0.0 |
|[Eklenmemiş diskler şifrelenmelidir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/UnattachedDisk_Encryption_Audit.json) |Bu ilke, şifrelemeden önce eklenmemiş tüm diskleri denetler. |Denetim, devre dışı |1.0.0 |
|[Sanal makinelerin yeni Azure Resource Manager kaynaklara geçirilmesi gerekir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/ClassicCompute_Audit.json) |Sanal makineleriniz için yeni Azure Resource Manager kullanın: daha güçlü erişim denetimi (RBAC), daha iyi denetim, ARM tabanlı dağıtım ve idare, yönetilen kimliklere erişim, gizlilikler için anahtar kasasına erişim, Azure Daha kolay güvenlik yönetimi için, AD tabanlı kimlik doğrulaması ve Etiketler ve kaynak grupları desteği |Denetim, reddetme, devre dışı |1.0.0 |
