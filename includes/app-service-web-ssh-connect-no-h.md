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
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188850"
---
Kapsayıcınızda doğrudan SSH oturumu başlatabilmek için uygulamanızın çalışıyor olması gerekir.

Aşağıdaki URL'yi tarayıcınıza yapıştırın ve \<app-name> yerine kendi uygulamanızın adını yazın:

```
https://<app-name>.scm.azurewebsites.net/webssh/host
```

Kimlik doğrulamasından geçmediyseniz bağlantıyı kurabilmek için Azure aboneliğinizle kimliğinizi doğrulamanız gerekir. Kimliğiniz doğrulandıktan sonra kapsayıcınızda komut çalıştırmak için kullanabileceğiniz tarayıcı içi kabuk ortamını görürsünüz.

![SSH bağlantısı](./media/app-service-web-ssh-connect-no-h/app-service-linux-ssh-connection.png)
