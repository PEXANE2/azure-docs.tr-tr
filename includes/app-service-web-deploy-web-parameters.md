---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 5bde217601d27129e044b64d90184727ea717950
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188179"
---
Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon, parametre değerlerinin tümünün bulunduğu Parametreler adlı bir bölüm içerir.
Dağıttığınız projeye veya dağıtmakta olduğunuz ortama göre değişen değerler için bir parametre tanımlamanız gerekir. Her zaman aynı kalacak değerler için parametreleri tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır. 

Parametreleri tanımlarken, bir kullanıcının dağıtım sırasında hangi değerleri sağlayabileceğini belirtmek için **izin verilen Değerler** alanını kullanın. Dağıtım sırasında değer sağlanmazsa, parametreye değer atamak için **varsayılan Değer** alanını kullanın.

Şablondaki her parametreyi açıklayacağız.

### <a name="sitename"></a>Sitename
Oluşturmak istediğiniz web uygulamasının adı.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
Web uygulamasını barındırmak için kullanılacak Uygulama Hizmeti'nin adı.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>Sku
Barındırma planının fiyatlandırma katmanı.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Şablon, bu parametre için izin verilen değerleri tanımlar ve değer belirtilmemişse varsayılan değer (S1) atar.

### <a name="workersize"></a>işçiBoyut
Barındırma planının örnek boyutu (küçük, orta veya büyük).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

Şablon, bu parametre için izin verilen değerleri tanımlar (0, 1 veya 2) ve değer belirtilmemişse varsayılan değer (0) atar. Değerler küçük, orta ve büyük karşılık gelir.

