---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/10/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: ab9ca05db0e6def896ec242df74ae531d33293bf
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90022241"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Sanal makineler için Azure Backup etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F013e242c-8828-4970-87b3-ab247555486d) |Azure Backup etkinleştirerek Azure sanal makinelerinizin korunmasını sağlayın. Azure Backup, Azure için güvenli ve uygun maliyetli bir veri koruma çözümüdür. |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachines_EnableAzureBackup_Audit.json) |
|[Bir konumun VM 'lerinde yedeklemeyi aynı konumdaki mevcut bir merkezi kasada yapılandırma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F09ce66bc-1220-4153-8104-e3f51c936913) |Bu ilke, belirli bir konumdaki sanal makinelerde bulunan VM 'lerde aynı konumdaki mevcut bir merkezi kasada Azure Backup koruma yapılandırır. Yalnızca, yedekleme için henüz yapılandırılmamış VM 'Ler için geçerlidir. Bu ilkenin 200 'den daha fazla VM 'ye atanması önerilir. İlke 200 ' den fazla VM için atanırsa, bu, yedeklemenin tanımlanan zamanlamanın ötesinde birkaç saat tetiklenmesi ile sonuçlanabilir. Bu ilke, daha fazla VM görüntüsünü destekleyecek şekilde geliştirilir. |deployIfNotExists, Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachineBackup_Backup_DeployIfNotExists.json) |
|[Kaynak özel kategoriler için Log Analytics çalışma alanına kurtarma hizmetleri Kasası için tanılama ayarlarını dağıtın.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc717fb0c-d118-4c43-ab3d-ece30ac81fb3) |Kaynağa özgü kategoriler için Log Analytics çalışma alanına akışa yönelik kurtarma hizmetleri Kasası için tanılama ayarlarını dağıtın. Kaynağa özgü kategorilerden herhangi biri etkinleştirilmemişse, yeni bir tanılama ayarı oluşturulur. |deployIfNotExists |[1.0.1-Önizleme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/EnableRecoveryServiceVaultDiagnosticSetting_Backup_DeployIfNotExist.json) |
