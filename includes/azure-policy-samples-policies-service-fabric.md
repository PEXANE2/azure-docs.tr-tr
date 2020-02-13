---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 0177f1ffbc21cf5508b86a9c0f3c625fead34faf
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172943"
---
|Ad |Açıklama |Efekt (ler) |Sürüm |
|---|---|---|---|
|[Service Fabric kümelerinde ClusterProtectionLevel özelliği EncryptAndSign olarak ayarlanmalıdır](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditClusterProtectionLevel_Audit.json) |Service Fabric, düğümden düğüme iletişim için bir birincil küme sertifikası kullanarak koruma (None, oturum ve EncryptAndSign) üç düzeyleri sağlar. Tüm düğümden düğüme mesajların şifrelendiğinden ve dijital olarak imzalandığından emin olmak için koruma düzeyini ayarlayın |Denetim, devre dışı |1.0.0 |
|[Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditADAuth_Audit.json) |İstemci kimlik doğrulamasının kullanımını yalnızca Service Fabric Azure Active Directory aracılığıyla denetle |Denetim, devre dışı |1.0.0 |
