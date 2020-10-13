---
title: Azure VMware çözüm kaynağı sağlayıcısını kaydetme
description: Azure VMware Çözüm kaynak sağlayıcısını kaydetme adımları.
ms.topic: include
ms.date: 09/21/2020
ms.openlocfilehash: 96d15546c5102a69a0b19f92de33d35d2e9ab6c2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91575750"
---
<!-- Used in avs-deployment.md and tutorial-create-private-cloud.md -->

Azure VMware çözümünü kullanmak için, öncelikle kaynak sağlayıcısını aboneliğinize kaydetmeniz gerekir.

```azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

>[!TIP]
>Alternatif olarak, **Microsoft. AVS** kaynak sağlayıcısını kaydetmek için GUI 'yi de kullanabilirsiniz.  Daha fazla bilgi için bkz. [kaynak sağlayıcısı ve türleri kaydetme](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider) makalesi.  
