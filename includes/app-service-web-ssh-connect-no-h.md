---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/29/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 060bc1039982cc0a77214d5dbe2a08de7a839c84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188850"
---
Kapsayıcınızla doğrudan bir SSH oturumu açmak için uygulamanızın çalışıyor olması gerekir.

Aşağıdaki URL'yi tarayıcınıza yapıştırın ve uygulama adı> uygulama adınız ile değiştirin: \<

```
https://<app-name>.scm.azurewebsites.net/webssh/host
```

Henüz kimliğinizi doğrulamadıysanız, bağlanmak için Azure aboneliğinizde kimlik doğrulamanız gerekir. Kimlik doğrulaması alındıktan sonra, kapsayıcınızın içinde komutları çalıştırabileceğiniz tarayıcı içi bir kabuk görürsünüz.

![SSH bağlantısı](./media/app-service-web-ssh-connect-no-h/app-service-linux-ssh-connection.png)
