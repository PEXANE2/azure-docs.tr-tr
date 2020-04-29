---
title: WebSocket sunucusunu Application Gateway kullanıma sunma
description: Bu makalede, AKS kümeleri için giriş denetleyicisi ile Application Gateway bir WebSocket sunucusunu kullanıma sunma hakkında bilgi verilmektedir.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 1f068c9d98a827afd16da01bdc40cbb6ca5dc465
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79297841"
---
# <a name="expose-a-websocket-server-to-application-gateway"></a>WebSocket sunucusunu Application Gateway kullanıma sunma

Application Gateway v2 belgelerinde açıklandığı gibi [, WebSocket ve http/2 protokolleri için yerel destek sağlar](features.md#websocket-and-http2-traffic). Hem Application Gateway hem de Kubernetes girişi için, WebSocket desteğini seçmeli olarak etkinleştirmek veya devre dışı bırakmak için Kullanıcı tarafından yapılandırılabilir bir ayar olmadığına lütfen unutmayın.

Aşağıdaki Kubernetes dağıtımı YAML, bir WebSocket sunucusunu dağıtmak için kullanılan en düşük yapılandırmayı gösterir ve bu, normal bir Web sunucusunu dağıtmaya benzer:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: websocket-server
spec:
  selector:
    matchLabels:
      app: ws-app
  replicas: 2
  template:
    metadata:
      labels:
        app: ws-app
    spec:
      containers:
        - name: websocket-app
          imagePullPolicy: Always
          image: your-container-repo.azurecr.io/websockets-app
          ports:
            - containerPort: 8888
      imagePullSecrets:
        - name: azure-container-registry-credentials

---

apiVersion: v1
kind: Service
metadata:
  name: websocket-app-service
spec:
  selector:
    app: ws-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8888

---

apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: websocket-repeater
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
    - host: ws.contoso.com
      http:
        paths:
          - backend:
              serviceName: websocket-app-service
              servicePort: 80
```

Tüm önkoşulların yerine getirildiğini ve AKS 'larınızın bir Kubernetes tarafından denetlenen bir Application Gateway varsa yukarıdaki dağıtım, Application Gateway genel IP ve `ws.contoso.com` etki alanının 80 numaralı bağlantı noktasında kullanıma sunulan bir WebSockets sunucusu ile sonuçlanır.

Aşağıdaki kıvrımlı komutu WebSocket sunucu dağıtımını test edecektir:
```sh
curl -i -N -H "Connection: Upgrade" \
        -H "Upgrade: websocket" \
        -H "Origin: http://localhost" \
        -H "Host: ws.contoso.com" \
        -H "Sec-Websocket-Version: 13" \
        -H "Sec-WebSocket-Key: 123" \
        http://1.2.3.4:80/ws
```

## <a name="websocket-health-probes"></a>WebSocket sistem durumu araştırmaları

Dağıtımınız açıkça sistem durumu araştırmalarını tanımlamıyorsa, Application Gateway WebSocket sunucu uç noktanıza HTTP GET denemesi yapılır.
Sunucu uygulamasına bağlı olarak ([bir çok sevdiğimiz](https://github.com/gorilla/websocket/blob/master/examples/chat/main.go)) WebSocket 'e özgü üstbilgiler gerekli olabilir (`Sec-Websocket-Version` örneğin).
Application Gateway WebSocket üst bilgilerini eklemediğinden, Application Gateway 'ın WebSocket sunucusundan gelen sistem durumu araştırma yanıtı muhtemelen büyük olasılıkla olacaktır `400 Bad Request`.
Sonuç olarak Application Gateway, ayırımlarınızı sağlıksız olarak işaretleyecek ve sonuç olarak WebSocket sunucusunun tüketicilerine `502 Bad Gateway` neden olacak.
Bundan kaçınmak için, sunucunuza bir sistem durumu denetimi için HTTP GET işleyicisi eklemeniz gerekebilir (`/health` Örneğin, döndüren `200 OK`).
