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
ms.openlocfilehash: d80786bbdfc4f53254dc98d79ac3badcf5dd3b24
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86111684"
---
Belirli ekleme senaryolarını gerçekleştirme için farklı şablonlar sağlarız. En uygun seçeneği kullanın ve parametre dosyasını ortamınızı yansıtacak şekilde değiştirmeyi unutmayın. Bu dosyaları dağıtımınızda nasıl kullanacağınız hakkında daha fazla bilgi için bkz. [Azure Lighthouse’a müşteri ekleme](../articles/lighthouse/how-to/onboard-customer.md).

| **Şablon** | **Açıklama** |
|---------|---------|
| [delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/delegated-resource-management) | Bir müşterinin aboneliğini, Azure Lighthouse’a ekler. Her abonelik için ayrı dağıtım yapılmalıdır. |
| [rg-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/rg-delegated-resource-management) | Bir veya daha fazla müşterinin kaynak grubunu Azure Lighthouse’a ekler. Tek kaynak grubu için **rgDelegatedResourceManagement** veya aynı aboneliğe birden fazla kaynak grubunu eklemek için **multipleRgDelegatedResourceManagement** kullanın. |
| [marketplace-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/marketplace-delegated-resource-management) | [Azure Market'te bir yönetilen hizmet teklifi yayımladıysanız](../articles/lighthouse/how-to/publish-managed-services-offers.md), teklifi kabul eden kullanıcılara kaynakları eklemek için isterseniz bu şablonu kullanabilirsiniz. Parametre dosyasındaki market değerleri teklifinizi yayımlarken kullandığınız değerlerle eşleşmelidir. |

Normalde eklenen her abonelik için ayrı dağıtım gerekir ama birden çok abonelikte şablonları dağıtabilirsiniz.

| **Şablon** | **Açıklama** |
|---------|---------|
| [cross-subscription-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/cross-subscription-deployment) | Azure Resource Manager şablonlarını birden fazla abonelikte dağıtır. |
