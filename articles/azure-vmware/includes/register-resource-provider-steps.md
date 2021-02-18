---
title: Azure VMware çözüm kaynağı sağlayıcısını kaydetme
description: Azure VMware Çözüm kaynak sağlayıcısını kaydetme adımları.
ms.topic: include
ms.date: 02/17/2021
ms.openlocfilehash: 80010a232f80865b20c2e3d953dc1d9d22ece1c6
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100653181"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-create-private-cloud.md -->

Azure VMware çözümünü kullanmak için, öncelikle kaynak sağlayıcısını aboneliğinize kaydetmeniz gerekir. Kaynak sağlayıcıları hakkında daha fazla bilgi için bkz. [Azure kaynak sağlayıcıları ve türleri](/azure/azure-resource-manager/management/resource-providers-and-types).

### <a name="azure-cli"></a>Azure CLI 

```azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

### <a name="azure-portal"></a>Azure portalı
 
1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portalı menüsünde **Tüm hizmetler**’i seçin.

1. **Tüm hizmetler** kutusunda, **abonelik** girin ve ardından **abonelikler**' i seçin.

1. Görüntülenecek abonelik listesinden aboneliği seçin.

1. **Kaynak sağlayıcıları** ' nı seçin ve aramaya **Microsoft. AVS** girin. 
 
1. Kaynak sağlayıcı kayıtlı değilse, **Kaydet**' i seçin.
