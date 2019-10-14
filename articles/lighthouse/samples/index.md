---
title: Azure Lighthouse örnekleri ve şablonları
description: Bu örnekler ve Azure Resource Manager şablonları, Azure temsilcili kaynak yönetimi için müşterilerin nasıl ekleneceğini ve Azure Lighthouse senaryolarının nasıl destekleneceğini gösterir.
author: JnHs
manager: carmonm
ms.service: lighthouse
ms.topic: sample
ms.date: 07/11/2019
ms.author: jenhayes
ms.openlocfilehash: d888fb0d50bf0336f8fe830c567586e66065871f
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286143"
---
# <a name="azure-lighthouse-samples"></a>Azure Lighthouse örnekleri

Aşağıdaki tablo, Azure Lighthouse için önemli Azure Resource Manager şablonlarının bağlantılarını içerir. Bu dosyalar ve daha fazlası [Azure Lighthouse örnekleri deposunda](https://github.com/Azure/Azure-Lighthouse-samples/) da bulunabilir.

| **Şablon** | **Açıklama** |
|---------|---------|
| [create-multiple-rgs](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/create-multiple-rgs) | Tek bir Azure Resource Manager şablonu kullanarak birden çok kaynak grubu oluşturur. |
| [cross-rg-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/cross-rg-deployment) | Depolama hesaplarını iki farklı kaynak grubuna dağıtır. |
| [cross-subscription-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/cross-subscription-deployment) | Azure Resource Manager şablonlarını birden fazla abonelikte dağıtır. |
| [delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management) | Bir müşterinin aboneliğini, Azure tarafından atanan temsilcinin kaynak yönetimine ekler. |
| [deploy-azure-mgmt-services](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-azure-mgmt-services) | Azure yönetim hizmetlerini oluşturur, bunları birbirine bağlar ve ek çözümler dağıtır. |
| [deploy-azure-security-center](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-azure-security-center) | Hedeflenen Azure aboneliğinde Azure Güvenlik Merkezi’ni etkinleştirir ve yapılandırır. |
| [marketplace-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management) | Market’te yayımlanan yönetilen hizmet teklifine göre bir müşterinin aboneliğini, Azure tarafından atanan temsilcinin kaynak yönetimine ekler. |
| [rg-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management) | Bir veya daha fazla müşterinin kaynak grubunu Azure tarafından atanan temsilcinin kaynak yönetimine ekler. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure tarafından atanan temsilcinin kaynak yönetimi](../concepts/azure-delegated-resource-management.md) hakkında bilgi edinin.
- [Azure tarafından atanan temsilcinin kaynak yönetimine müşterileri eklemek](../how-to/onboard-customer.md) için Azure Resource Manager şablonlarını kullanmayı öğrenin.
