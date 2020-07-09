---
title: include dosyası
description: include dosyası
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 07/07/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 837e82131b17fa02fca87da067eac6d77903b31d
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86111685"
---
Bu örnekler Azure İlkesinin, Azure Lighthouse’a eklenen aboneliklerle nasıl kullanılacağını gösterir.

| **Şablon** | **Açıklama** |
|---------|---------|
| [policy-add-or-replace-tag](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-add-or-replace-tag) | Temsilci aboneliğine etiket ekleyen veya kaldıran (değiştirme etkisini kullanarak) bir ilke atar. Daha fazla bilgi için bkz. [Temsilci aboneliği içinde düzeltilebilir bir ilke dağıtma](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-audit-delegation](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-audit-delegation) | Temsilci atamalarını denetleyecek bir ilke atar. |
| [policy-enforce-keyvault-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-keyvault-monitoring) | Temsilci aboneliklerindeki Azure Key Vault kaynakları üzerinde tanılamayı etkinleştiren (deployIfNotExists etkisini kullanarak) bir ilke atar. Daha fazla bilgi için bkz. [Temsilci aboneliği içinde düzeltilebilir bir ilke dağıtma](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-enforce-sub-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-sub-monitoring) | Temsilci aboneliğinde tanılamayı etkinleştirmek için çeşitli ilkeler atar ve tüm Windows ile Linux VM'lerini ilke tarafından oluşturulan Log Analytics çalışma alanına bağlar. Daha fazla bilgi için bkz. [Temsilci aboneliği içinde düzeltilebilir bir ilke dağıtma](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-initiative](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-initiative) | Temsilci aboneliğine bir girişim (birden fazla ilgili ilke tanımı) uygular. |

