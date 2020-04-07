---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/03/2020
ms.author: dacoulte
ms.openlocfilehash: 77b011300d7673d4d96d89fda3f19dacf9c635e9
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80758712"
---
|Adı |Açıklama |Etki(ler) |Sürüm |GitHub |
|---|---|---|---|---|
|[Service Fabric kümeleri ClusterProtectionLevel özelliğini EncryptAndSign olarak ayarlamalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F617c02be-7f02-4efd-8836-3180d47b6c68) |Service Fabric, birincil küme sertifikası kullanarak düğümden düğüme iletişim için üç koruma düzeyi (Yok, İşaret ve ŞifrelemeVe İşaret) sağlar. Tüm düğümden düğüme iletilerin şifrelenmiş ve dijital olarak imzalanmış olduğundan emin olmak için koruma düzeyini ayarlama |Denetim, Engelli |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditClusterProtectionLevel_Audit.json)
|[Hizmet Kumaş kümeleri yalnızca müşteri kimlik doğrulaması için Azure Etkin Dizini kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb54ed75b-3e1a-44ac-a333-05ba39b99ff0) |Hizmet Dokusunda Azure Active Directory aracılığıyla yalnızca istemci kimlik doğrulamasını denetleme |Denetim, Engelli |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditADAuth_Audit.json)
