---
title: dosya dahil etme
description: dosya dahil etme
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/02/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 6fc1f4152b2e16e1597c018e5af6e0245b075c3b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78255815"
---
## <a name="robots933456-in-logs"></a>günlüklerde robots933456

Kapsayıcı günlüklerinde şu iletiyi görebilirsiniz:

```
2019-04-08T14:07:56.641002476Z "-" - - [08/Apr/2019:14:07:56 +0000] "GET /robots933456.txt HTTP/1.1" 404 415 "-" "-"
```

Bu iletiyi güvenle yoksayabilirsiniz. `/robots933456.txt`, kapsayıcının istek sunma yeteneğine sahip olup olmadığını denetlemek için App Service tarafından kullanılan bir kukla URL yoludur. 404 yanıtı, yolun mevcut olmadığını gösterir, ancak kapsayıcının sağlıklı olduğunu ve isteklere yanıt vermeye hazırlandığını App Service sağlar.

