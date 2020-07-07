---
title: dosya dahil etme
description: dosya dahil etme
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/29/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 060bc1039982cc0a77214d5dbe2a08de7a839c84
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188850"
---
Kapsayıcınıza doğrudan bir SSH oturumu açmak için uygulamanızın çalışıyor olması gerekir.

Aşağıdaki URL 'YI tarayıcınıza yapıştırın ve \<app-name> uygulamanızın adıyla değiştirin:

```
https://<app-name>.scm.azurewebsites.net/webssh/host
```

Henüz kimlik doğrulamasından ayrılmadıysanız, bağlanmak için Azure aboneliğinizle kimlik doğrulaması yapmanız gerekir. Kimliği doğrulandıktan sonra, bir tarayıcı içi kabuğu görürsünüz ve burada, Kapsayıcınız içindeki komutları çalıştırabilirsiniz.

![SSH bağlantısı](./media/app-service-web-ssh-connect-no-h/app-service-linux-ssh-connection.png)
