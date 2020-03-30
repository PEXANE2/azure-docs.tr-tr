---
title: Uygulama Ağ Geçidi ile çerez tabanlı yakınlığı etkinleştirme
description: Bu makalede, bir Uygulama Ağ Geçidi ile çerez tabanlı yakınlığı etkinleştirmek için nasıl hakkında bilgi sağlar.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: adacd805e736524fd7956c4bbc0ad402980b4cd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73795988"
---
# <a name="enable-cookie-based-affinity-with-an-application-gateway"></a>Uygulama Ağ Geçidi ile Çerez tabanlı yakınlığı etkinleştirme
Azure Uygulama [Ağ Geçidi Belgeleri'nde](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#http-settings)belirtildiği gibi, Application Gateway çerez tabanlı yakınlığı destekler, bu da sonraki trafiği bir kullanıcı oturumundan işleme için aynı sunucuya yönlendirebileceği anlamına gelir.

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
