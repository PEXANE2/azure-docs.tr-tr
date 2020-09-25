---
title: Azure VMware çözüm kaynağı sağlayıcısını kaydetme
description: Azure VMware Çözüm kaynak sağlayıcısını kaydetme adımları.
ms.topic: include
ms.date: 09/21/2020
ms.openlocfilehash: 380cb513ec389293db757e667ed4681778e29348
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91254660"
---
<!-- Used in avs-deployment.md and tutorial-create-private-cloud.md -->

Azure VMware çözümünü kullanmak için, öncelikle kaynak sağlayıcısını aboneliğinize kaydetmeniz gerekir.

```
azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

>[!TIP]
>Alternatif olarak, **Microsoft. AVS** kaynak sağlayıcısını kaydetmek için GUI 'yi de kullanabilirsiniz.  Daha fazla bilgi için bkz. [kaynak sağlayıcısı ve türleri kaydetme](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider) makalesi.  
