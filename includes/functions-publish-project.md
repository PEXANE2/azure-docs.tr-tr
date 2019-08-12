---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 9fefba9c79e9c58956243de8db67ca4cf316251c
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949955"
---
## <a name="deploy-the-function-app-project-to-azure"></a>İşlev uygulaması projesini Azure'a dağıtma

İşlev uygulaması Azure 'da oluşturulduktan sonra, proje kodunuzu Azure 'a dağıtmak için [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) çekirdek Araçlar komutunu kullanabilirsiniz. Bu örneklerde, önceki adımda `<APP_NAME>` uygulamanızın adıyla değiştirin.

### <a name="c--javascript"></a>C\# /JavaScript

```command
func azure functionapp publish <APP_NAME>
```

### <a name="typescript"></a>TypeScript

```command
npm run build:production 
func azure functionapp publish <APP_NAME>
```

Aşağıdakine benzer bir çıktı görürsünüz ve bu, okunabilirlik için kesildi:

```output
Getting site publishing info...
...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
Functions in myfunctionapp:
    HttpTrigger - [httpTrigger]
        Invoke url: https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....
```

Yalnızca Azure 'da işlevinizi `HttpTrigger`test etmek için kullanabileceğiniz değerinikopyalayın.`Invoke url` URL, işlev anahtarınız `code` olan bir sorgu dizesi değeri içeriyor. Bu anahtar başkalarının Azure 'da HTTP tetikleyici uç noktanızı aramasını zorlaştırır.