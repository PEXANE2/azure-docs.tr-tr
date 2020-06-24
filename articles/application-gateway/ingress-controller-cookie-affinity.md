---
title: Application Gateway tanımlama bilgisine dayalı benzeşimi etkinleştir
description: Bu makalede, tanımlama bilgisi tabanlı Benzeşimin bir Application Gateway nasıl etkinleştirileceği hakkında bilgi sağlanır.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 3af2705fedbb9c19d4f128e8e997d3fa73f8b5a7
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84807962"
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
