---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/26/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: fc6c2a93892684463ab7b41ceca2ecd4c904e506
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82191192"
---
|Adı |Açıklama |Efekt (ler) |Sürüm |GitHub |
|---|---|---|---|---|
|[Sanal makineler için Azure Backup etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F013e242c-8828-4970-87b3-ab247555486d) |Bu ilke, tüm sanal makineler için Azure Backup hizmetinin etkin olup olmadığını denetlemeye yardımcı olur. Azure Backup uygun maliyetli bir yedekleme çözümü, veri kurtarmayı basitleştirir ve diğer bulut yedekleme hizmetlerinden daha kolay bir şekilde etkinleştirilir. |Auditınotexists, devre dışı |1.0.0 |[Bağlantısının](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachines_EnableAzureBackup_Audit.json) |
|[Bir konumun VM 'lerinde yedeklemeyi aynı konumdaki mevcut bir merkezi kasada yapılandırma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F09ce66bc-1220-4153-8104-e3f51c936913) |Bu ilke, belirli bir konumdaki sanal makinelerde bulunan VM 'lerde aynı konumdaki mevcut bir merkezi kasada Azure Backup koruma yapılandırır. Yalnızca, yedekleme için henüz yapılandırılmamış VM 'Ler için geçerlidir. Bu ilkenin 200 'den daha fazla VM 'ye atanması önerilir. İlke 200 ' den fazla VM için atanırsa, bu, yedeklemenin tanımlanan zamanlamanın ötesinde birkaç saat tetiklenmesi ile sonuçlanabilir. Bu ilke, daha fazla VM görüntüsünü destekleyecek şekilde geliştirilir. |deployIfNotExists, Auditınotexists, devre dışı |1.0.0 |[Bağlantısının](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachineBackup_Backup_DeployIfNotExists.json) |