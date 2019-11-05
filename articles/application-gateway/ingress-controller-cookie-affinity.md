---
title: Application Gateway tanımlama bilgisine dayalı benzeşimi etkinleştir
description: Bu makalede, tanımlama bilgisi tabanlı Benzeşimin bir Application Gateway nasıl etkinleştirileceği hakkında bilgi sağlanır.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 10/23/2019
ms.author: caya
ms.openlocfilehash: cb1a403888f8dca78d1af7395095aa2bac39935e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73513581"
---
# <a name="enable-cookie-based-affinity-with-an-application-gateway"></a>Application Gateway tanımlama bilgisi tabanlı benzeşimi etkinleştirme
[Azure Application Gateway belgelerinde](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#http-settings)özetlenen Application Gateway, tanımlama bilgisi tabanlı benzeşimi destekler, yani sonraki trafiği işlenmek üzere bir kullanıcı oturumundan aynı sunucuya yönlendirebilir.

## <a name="example"></a>Örnek
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: guestbook
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: frontend
          servicePort: 80
```
