---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 733fc9620cbd17e5e4d8bb101c54ff646a06d6a4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "86050381"
---
### <a name="app-service-plan"></a>App Service planı
Web uygulamasını barındırmak için hizmet planı oluşturur. **Hostingplanname** parametresi aracılığıyla planın adını sağlarsınız. Planın konumu, kaynak grubu için kullanılan konumdur. Fiyatlandırma Katmanı ve çalışan boyutu, **SKU** ve **workersize** parametrelerinde belirtilir

```config
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('hostingPlanName')]",
  "type": "Microsoft.Web/serverfarms",
  "location": "[resourceGroup().location]",
  "sku": {
    "name": "[parameters('sku')]",
    "capacity": "[parameters('workerSize')]"
  },
  "properties": {
    "name": "[parameters('hostingPlanName')]"
  }
},
```
