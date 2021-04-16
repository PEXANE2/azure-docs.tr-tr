---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 64505883273e21cda6b3abb1c8a08b79798b3663
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2021
ms.locfileid: "107511947"
---
|Name<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL Server üzerinde denetim etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9) |SQL Server denetim, sunucudaki tüm veritabanları genelinde veritabanı etkinliklerini izlemek ve bir denetim günlüğüne kaydetmek için etkinleştirilmelidir. |Auditınotexists, devre dışı |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditing_Audit.json) |
|[SQL denetim ayarları, kritik etkinlikleri yakalamak için yapılandırılmış Action-Groups sahip olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7ff426e2-515f-405a-91c8-4f2333442eb5) |AuditActionsAndGroups özelliği, kapsamlı bir denetim günlüğü sağlamak için en az SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP, FAILED_DATABASE_AUTHENTICATION_GROUP, BATCH_COMPLETED_GROUP içermelidir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditing_ActionsAndGroups_Audit.json) |
