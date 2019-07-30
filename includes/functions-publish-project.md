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
ms.openlocfilehash: fbb537c9584c948af37694b3bfc77a7c345e084d
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639146"
---
## <a name="deploy-the-function-app-project-to-azure"></a>İşlev uygulaması projesini Azure'a dağıtma

İşlev uygulaması Azure 'da oluşturulduktan sonra, proje kodunuzu Azure 'a dağıtmak için [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) çekirdek Araçlar komutunu kullanabilirsiniz. Aşağıdaki komutta, yerine önceki adımdaki `<APP_NAME>` uygulamanızın adıyla değiştirin.

```bash
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