---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/12/2020
ms.author: dacoulte
ms.openlocfilehash: 221633fcb459e39a4c11e869b9214d985cd5ef0f
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77192529"
---
|Ad |Açıklama |Efekt (ler) |Sürüm |
|---|---|---|---|
|[Service Fabric kümelerinde ClusterProtectionLevel özelliği EncryptAndSign olarak ayarlanmalıdır](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditClusterProtectionLevel_Audit.json) |Service Fabric, düğümden düğüme iletişim için bir birincil küme sertifikası kullanarak koruma (None, oturum ve EncryptAndSign) üç düzeyleri sağlar. Tüm düğümden düğüme mesajların şifrelendiğinden ve dijital olarak imzalandığından emin olmak için koruma düzeyini ayarlayın |Denetim, devre dışı |1.0.0 |
|[Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditADAuth_Audit.json) |İstemci kimlik doğrulamasının kullanımını yalnızca Service Fabric Azure Active Directory aracılığıyla denetle |Denetim, devre dışı |1.0.0 |
