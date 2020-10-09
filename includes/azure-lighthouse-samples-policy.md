---
title: include dosyası
description: include dosyası
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 09/24/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: a9c9b5081232988cb4ab80bfcbb9f2d92d1d025f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91336590"
---
Bu örnekler Azure İlkesinin, Azure Lighthouse’a eklenen aboneliklerle nasıl kullanılacağını gösterir.

| **Şablon** | **Açıklama** |
|---------|---------|
| [policy-add-or-replace-tag](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-add-or-replace-tag) | Temsilci aboneliğine etiket ekleyen veya kaldıran (değiştirme etkisini kullanarak) bir ilke atar. Daha fazla bilgi için bkz. [Temsilci aboneliği içinde düzeltilebilir bir ilke dağıtma](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-audit-delegation](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-audit-delegation) | Temsilci atamalarını denetleyecek bir ilke atar. |
| [policy-delegate-management-groups](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-delegate-management-groups) | Bir yönetim grubundaki aboneliklerin yönetim kiracısına temsilci olarak atandığını onaylamaya yönelik bir ilke atar, yoksa atama oluşturur.
| [policy-enforce-keyvault-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-keyvault-monitoring) | Temsilci aboneliklerindeki Azure Key Vault kaynakları üzerinde tanılamayı etkinleştiren (deployIfNotExists etkisini kullanarak) bir ilke atar. Daha fazla bilgi için bkz. [Temsilci aboneliği içinde düzeltilebilir bir ilke dağıtma](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-enforce-sub-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-sub-monitoring) | Temsilci aboneliğinde tanılamayı etkinleştirmek için çeşitli ilkeler atar ve tüm Windows ile Linux VM'lerini ilke tarafından oluşturulan Log Analytics çalışma alanına bağlar. Daha fazla bilgi için bkz. [Temsilci aboneliği içinde düzeltilebilir bir ilke dağıtma](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-initiative](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-initiative) | Temsilci aboneliğine bir girişim (birden fazla ilgili ilke tanımı) uygular. |

