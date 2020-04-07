---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/03/2020
ms.author: dacoulte
ms.openlocfilehash: 156f253e9559f493ba6cccde4de4744068a32fc4
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80758758"
---
|Adı |Açıklama |Etki(ler) |Sürüm |GitHub |
|---|---|---|---|---|
|[Sanal Makineler için Azure Yedekleme etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F013e242c-8828-4970-87b3-ab247555486d) |Bu ilke, tüm Sanal makineler için Azure Yedekleme hizmeti etkinse denetime yardımcı olur. Azure Yedekleme uygun maliyetli, tek tıklamayla yedekleme çözümü veri kurtarmayı basitleştirir ve etkinleştirmek diğer bulut yedekleme hizmetlerine göre daha kolaydır. |Auditifnotexists, Devre Dışı |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachines_EnableAzureBackup_Audit.json)
|[Bir konumun VM'lerinde yedeklemeyi aynı konumdaki mevcut merkezi Kasa'ya yapılandırma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F09ce66bc-1220-4153-8104-e3f51c936913) |Bu ilke, belirli bir konumdaki VM'lerde Azure Yedekleme korumasını aynı konumdaki varolan bir merkezi kasaya yapılandırır. Yalnızca yedekleme için zaten yapılandırılmamış VM'ler için geçerlidir. Bu ilkenin en fazla 200 VM'ye atanması önerilir. İlke 200'den fazla VM için atanmışsa, yedeklemenin tanımlanan zamanlamanın birkaç saat ötesinde tetiklenebilmiş olması yla sonuçlanabilir. Bu ilke, daha fazla VM görüntülerini desteklemek için geliştirilecektir. |deployIfNotExists, auditIfNotExists, devre dışı |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachineBackup_Backup_DeployIfNotExists.json)
