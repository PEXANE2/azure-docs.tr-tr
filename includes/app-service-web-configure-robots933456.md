---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/02/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 6fc1f4152b2e16e1597c018e5af6e0245b075c3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255815"
---
## <a name="robots933456-in-logs"></a>robotlar933456 günlükleri

Kapsayıcı günlüklerinde aşağıdaki iletiyi görebilirsiniz:

```
2019-04-08T14:07:56.641002476Z "-" - - [08/Apr/2019:14:07:56 +0000] "GET /robots933456.txt HTTP/1.1" 404 415 "-" "-"
```

Bu iletiyi güvenle yoksayabilirsiniz. `/robots933456.txt`Uygulama Hizmeti'nin kapsayıcının isteklere hizmet verebildiğini denetlemek için kullandığı sahte bir URL yoludur. 404 yanıtı yalnızca yolun var olmadığını gösterir, ancak Uygulama Hizmeti'ne kapsayıcının sağlıklı ve isteklere yanıt vermeye hazır olduğunu bildirir.

