---
title: Azure VMware çözüm kaynağı sağlayıcısını kaydetme
description: Azure VMware Çözüm kaynak sağlayıcısını kaydetme adımları.
ms.topic: include
ms.date: 12/24/2020
ms.openlocfilehash: 7d24ce86f24c941c7d48d3b73576dcdfda120f51
ms.sourcegitcommit: 489ce69c0ff3f5188889ecfef5ffa76f7121e0d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/24/2020
ms.locfileid: "97770850"
---
<!-- Used in avs-deployment.md and tutorial-create-private-cloud.md -->

Azure VMware çözümünü kullanmak için, öncelikle kaynak sağlayıcısını aboneliğinize kaydetmeniz gerekir.  

### <a name="azure-cli"></a>Azure CLI 

```azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```


### <a name="azure-portal"></a>Azure portal
 
1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portalı menüsünde **Tüm hizmetler**’i seçin.

1. **Tüm hizmetler** kutusunda, **abonelik** girin ve ardından **abonelikler**' i seçin.

1. Görüntülenecek abonelik listesinden aboneliği seçin.

1. **Kaynak sağlayıcıları** ' nı seçin ve aramaya **Microsoft. AVS** girin. 
 
1. Kaynak sağlayıcı kayıtlı değilse, **Kaydet**' i seçin.