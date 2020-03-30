---
title: Bir WebSocket sunucusunun Uygulama Ağ Geçidi'ne maruz bırakma
description: Bu makalede, AKS kümeleri için giriş denetleyicisi ile Uygulama Ağ Geçidi bir WebSocket sunucu maruz kalmak için nasıl hakkında bilgi sağlar.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 1f068c9d98a827afd16da01bdc40cbb6ca5dc465
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79297841"
---
# <a name="expose-a-websocket-server-to-application-gateway"></a>Bir WebSocket sunucusunun Uygulama Ağ Geçidi'ne maruz bırakma

Application Gateway v2 belgelerinde belirtildiği gibi - [WebSocket ve HTTP/2 protokolleri için yerel destek sağlar.](features.md#websocket-and-http2-traffic) Hem Uygulama Ağ Geçidi hem de Kubernetes Girişi için WebSocket desteğini seçerek etkinleştirmek veya devre dışı kalmak için kullanıcı tarafından yapılandırılabilir ayar olmadığını lütfen unutmayın.

Aşağıdaki Kubernetes dağıtım YAML normal bir web sunucusu dağıtma ile aynı olan bir WebSocket sunucusu dağıtmak için kullanılan minimum yapılandırmayı gösterir:
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

Tüm ön koşulların yerine getirildiği ve AKS'nizde bir Kubernetes Girişi tarafından kontrol edilen bir Uygulama Ağ Geçidi'ne sahip olduğunuz göz önüne alındığında, yukarıdaki dağıtım, Application Gateway'inizin ortak IP'sinin ve `ws.contoso.com` etki alanının 80 bağlantı noktasında açığa çıkarılan bir WebSockets sunucusuyla sonuçlanır.

Aşağıdaki cURL komutu WebSocket sunucu dağıtımını sınar:
```sh
curl -i -N -H "Connection: Upgrade" \
        -H "Upgrade: websocket" \
        -H "Origin: http://localhost" \
        -H "Host: ws.contoso.com" \
        -H "Sec-Websocket-Version: 13" \
        -H "Sec-WebSocket-Key: 123" \
        http://1.2.3.4:80/ws
```

## <a name="websocket-health-probes"></a>WebSocket Sağlık Probları

Dağıtımınız sistem durumu sondalarını açıkça tanımlamazsa, Application Gateway WebSocket sunucu bitiş noktanızda bir HTTP GET girişiminde bulundurur.
Sunucu uygulamasına bağlı olarak[(burada sevdiğimiz bir](https://github.com/gorilla/websocket/blob/master/examples/chat/main.go)tanesi) WebSocket'e özgü üstbilgi gerekebilir (örneğin).`Sec-Websocket-Version`
Application Gateway WebSocket üstbilgilerini eklemediğinden, WebSocket sunucunuzdan Uygulama Ağ Geçidi'nin `400 Bad Request`sistem durumu sondası yanıtı büyük olasılıkla .
Sonuç olarak Application Gateway, bölmelerinizi sağlıksız olarak işaretleyecek ve `502 Bad Gateway` bu da sonunda WebSocket sunucusunun tüketicileri için bir sonuç olacaktır.
Bunu önlemek için sunucunuza bir sağlık denetimi için bir HTTP`/health` GET işleyicisi eklemeniz gerekebilir (örneğin, bu döndürür). `200 OK`
