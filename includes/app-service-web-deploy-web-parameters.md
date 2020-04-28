---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 5bde217601d27129e044b64d90184727ea717950
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "67188179"
---
Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon, parametre değerlerinin tümünü içeren parametreler adlı bir bölüm içerir.
Dağıttığınız projeye göre veya dağıttığınız ortama göre farklılık gösteren değerler için bir parametre tanımlamalısınız. Her zaman aynı kalacak değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır. 

Parametreleri tanımlarken, bir kullanıcının dağıtım sırasında sağlayabileceği değerleri belirtmek için **allowedValues** alanını kullanın. Dağıtım sırasında hiçbir değer sağlanmazsa parametreye bir değer atamak için **DefaultValue** alanını kullanın.

Şablondaki her parametreyi anlayacağız.

### <a name="sitename"></a>siteName
Oluşturmak istediğiniz Web uygulamasının adı.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
Web uygulamasını barındırmak için kullanılacak App Service planının adı.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>isteyin
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

Şablon, bu parametre için izin verilen değerleri tanımlar ve hiçbir değer belirtilmemişse varsayılan bir değer (S1) atar.

### <a name="workersize"></a>workerSize
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

Şablon, bu parametre için izin verilen değerleri tanımlar (0, 1 veya 2) ve hiçbir değer belirtilmemişse varsayılan bir değer (0) atar. Değerler küçük, orta ve büyük bir değere karşılık gelir.

