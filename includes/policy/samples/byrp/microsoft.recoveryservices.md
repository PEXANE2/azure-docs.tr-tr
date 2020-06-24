---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 06/10/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 7baa1c030c8d70b76eb661d6a6fac7429a80a50e
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84709920"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Sanal makineler için Azure Backup etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F013e242c-8828-4970-87b3-ab247555486d) |Bu ilke, tüm sanal makineler için Azure Backup hizmetinin etkin olup olmadığını denetlemeye yardımcı olur. Azure Backup uygun maliyetli bir yedekleme çözümü, veri kurtarmayı basitleştirir ve diğer bulut yedekleme hizmetlerinden daha kolay bir şekilde etkinleştirilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachines_EnableAzureBackup_Audit.json) |
|[Bir konumun VM 'lerinde yedeklemeyi aynı konumdaki mevcut bir merkezi kasada yapılandırma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F09ce66bc-1220-4153-8104-e3f51c936913) |Bu ilke, belirli bir konumdaki sanal makinelerde bulunan VM 'lerde aynı konumdaki mevcut bir merkezi kasada Azure Backup koruma yapılandırır. Yalnızca, yedekleme için henüz yapılandırılmamış VM 'Ler için geçerlidir. Bu ilkenin 200 'den daha fazla VM 'ye atanması önerilir. İlke 200 ' den fazla VM için atanırsa, bu, yedeklemenin tanımlanan zamanlamanın ötesinde birkaç saat tetiklenmesi ile sonuçlanabilir. Bu ilke, daha fazla VM görüntüsünü destekleyecek şekilde geliştirilir. |deployIfNotExists, Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachineBackup_Backup_DeployIfNotExists.json) |
|[Kaynak özel kategoriler için Log Analytics çalışma alanına kurtarma hizmetleri Kasası için tanılama ayarlarını dağıtın.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc717fb0c-d118-4c43-ab3d-ece30ac81fb3) |Kaynağa özgü kategoriler için Log Analytics çalışma alanına akışa yönelik kurtarma hizmetleri Kasası için tanılama ayarlarını dağıtın. Kaynağa özgü kategorilerden herhangi biri etkinleştirilmemişse, yeni bir tanılama ayarı oluşturulur. |deployIfNotExists |[1.0.0-Önizleme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/EnableRecoveryServiceVaultDiagnosticSetting_Backup_DeployIfNotExist.json) |
