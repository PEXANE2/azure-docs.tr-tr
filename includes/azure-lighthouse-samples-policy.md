---
title: include dosyası
description: include dosyası
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 12/19/2019
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 55eee87ed1e07b61a7f8287e86ddc0a7a6aa4df9
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75456789"
---
Bu örnekler Azure İlkesinin, Azure tarafından atanan temsilcinin kaynak yönetimi için eklenen aboneliklerle nasıl kullanılacağını gösterir.

| **Şablon** | **Açıklama** |
|---------|---------|
| [policy-add-or-replace-tag](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-add-or-replace-tag) | Temsilci aboneliğine etiket ekleyen veya kaldıran (değiştirme etkisini kullanarak) bir ilke atar. Daha fazla bilgi için bkz. [Temsilci aboneliği içinde düzeltilebilir bir ilke dağıtma](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-audit-delegation](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-audit-delegation) | Temsilci atamalarını denetleyecek bir ilke atar. |
| [policy-enforce-keyvault-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-enforce-keyvault-monitoring) | Temsilci aboneliklerindeki Azure Key Vault kaynakları üzerinde tanılamayı etkinleştiren (deployIfNotExists etkisini kullanarak) bir ilke atar. Daha fazla bilgi için bkz. [Temsilci aboneliği içinde düzeltilebilir bir ilke dağıtma](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-enforce-sub-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-enforce-sub-monitoring) | Temsilci aboneliğinde tanılamayı etkinleştirmek için çeşitli ilkeler atar ve tüm Windows ile Linux VM'lerini ilke tarafından oluşturulan Log Analytics çalışma alanına bağlar. Daha fazla bilgi için bkz. [Temsilci aboneliği içinde düzeltilebilir bir ilke dağıtma](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-initiative](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-initiative) | Temsilci aboneliğine bir girişim (birden fazla ilgili ilke tanımı) uygular. |

