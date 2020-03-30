---
title: Uygulama Ağ Geçidi Denetleyicisi ek açıklamaları
description: Bu makalede, Uygulama Ağ Geçidi Giriş Denetleyicisi'ne özgü ek açıklamalar la ilgili belgeler sağlanmaktadır.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: f54381ddcd11a2e4a24d30d812468da85b5403de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335817"
---
# <a name="annotations-for-application-gateway-ingress-controller"></a>Uygulama Ağ Geçidi Denetleyicisi için ek açıklamalar 

## <a name="introductions"></a>Tanıtım

Kubernetes Ingress kaynağı rasgele anahtar/değer çiftleri ile açıklamalı olabilir. AGIC, Giriş YAML üzerinden yapılandırılabilen Uygulama Ağ Geçidi özelliklerini programlamak için ek açıklamalara dayanır. Giriş ek açıklamaları, giriş kaynağından türetilen tüm HTTP ayarına, arka uç havuzlarına ve dinleyicilere uygulanır.

## <a name="list-of-supported-annotations"></a>Desteklenen ek açıklamalar listesi

Bir Giriş kaynağının AGIC tarafından gözlemlemesi için açıklamalı `kubernetes.io/ingress.class: azure/application-gateway`olması **gerekir.** Ancak o zaman AGIC söz konusu Ingress kaynağı ile çalışacaktır.

| Ek Açıklama Anahtarı | Değer Türü | Varsayılan Değer | İzin Verilen Değerler
| -- | -- | -- | -- |
| [appgw.ingress.kubernetes.io/backend-path-prefix](#backend-path-prefix) | `string` | `nil` | |
| [appgw.ingress.kubernetes.io/ssl-redirect](#tls-redirect) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/connection-draining](#connection-draining) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/connection-draining-timeout](#connection-draining) | `int32`(saniye) | `30` | |
| [appgw.ingress.kubernetes.io/cookie-based-affinity](#cookie-based-affinity) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/request-timeout](#request-timeout) | `int32`(saniye) | `30` | |
| [appgw.ingress.kubernetes.io/use-private-ip](#use-private-ip) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/backend-protocol](#backend-protocol) | `string` | `http` | `http`, `https` |

## <a name="backend-path-prefix"></a>Arka Uç Yolu Öneki

Bu ek açıklama, giriş kaynağında belirtilen arka uç yolunun bu ek açıklamada belirtilen önek ile yeniden yazılmasını sağlar. Bu, kullanıcıların bir hizmeti giriş kaynağında ortaya çıkarmak için kullanılan uç nokta adlarından uç noktaları farklı olan hizmetleri ortaya çıkarmalarına olanak tanır.

### <a name="usage"></a>Kullanım

```yaml
appgw.ingress.kubernetes.io/backend-path-prefix: <path prefix>
```

### <a name="example"></a>Örnek

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-bkprefix
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/backend-path-prefix: "/test/"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```
Yukarıdaki örnekte, bir ek açıklama `go-server-ingress-bkprefix` ile adlı bir `appgw.ingress.kubernetes.io/backend-path-prefix: "/test/"` giriş kaynağı tanımladık. Ek açıklama, bir YOL öneki geçersiz kılınacak bir HTTP ayarı oluşturmak `/hello` `/test/`için uygulama ağ geçidi söyler.

> [!NOTE] 
> Yukarıdaki örnekte tanımlanan tek bir kural var. Ancak, ek açıklamalar tüm giriş kaynağı için geçerlidir, bu nedenle bir kullanıcı birden çok kural tanımlasaydı, arka uç yolu öneki belirtilen yolların her biri için ayarlanır. Bu nedenle, bir kullanıcı farklı yol önekleri (aynı hizmet için bile) farklı kurallar istiyorsa, farklı giriş kaynakları tanımlamak gerekir.

## <a name="tls-redirect"></a>TLS Yönlendirme

Uygulama Ağ Geçidi, HTTP URL'lerini HTTPS'deki karşılıklarına otomatik olarak yönlendirecek şekilde [yapılandırılabilir.](https://docs.microsoft.com/azure/application-gateway/application-gateway-redirect-overview) Bu ek açıklama mevcut olduğunda ve TLS düzgün yapılandırılmışolduğunda, Kubernetes Ingress denetleyicisi [yeniden yönlendirme yapılandırması içeren bir yönlendirme kuralı](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-portal#add-a-routing-rule-with-a-redirection-configuration) oluşturur ve değişiklikleri Uygulama Ağ Geçidinize uygular. Oluşturulan yönlendirme HTTP `301 Moved Permanently`olacaktır.

### <a name="usage"></a>Kullanım

```yaml
appgw.ingress.kubernetes.io/ssl-redirect: "true"
```

### <a name="example"></a>Örnek

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-redirect
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
   - hosts:
     - www.contoso.com
     secretName: testsecret-tls
  rules:
  - host: www.contoso.com
    http:
      paths:
      - backend:
          serviceName: websocket-repeater
          servicePort: 80
```

## <a name="connection-draining"></a>Bağlantı Boşaltma

`connection-draining`: Bu ek açıklama, kullanıcıların bağlantı boşaltmayı etkinleştirip etkinleştirmeyeceklerini belirtmelerine olanak tanır.
`connection-draining-timeout`: Bu ek açıklama, kullanıcıların Uygulama Ağ Geçidi'nin istekleri boşalan arka uç bitiş noktasına sonlandıracağı bir zaman alacaktır.

### <a name="usage"></a>Kullanım

```yaml
appgw.ingress.kubernetes.io/connection-draining: "true"
appgw.ingress.kubernetes.io/connection-draining-timeout: "60"
```

### <a name="example"></a>Örnek

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-drain
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/connection-draining: "true"
    appgw.ingress.kubernetes.io/connection-draining-timeout: "60"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="cookie-based-affinity"></a>Çerez Bazlı Affinity

Bu ek açıklama, çerez tabanlı yakınlığı etkinleştirmek için olup olmadığını belirtmenize olanak sağlar.

### <a name="usage"></a>Kullanım

```yaml
appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
```

### <a name="example"></a>Örnek

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-affinity
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="request-timeout"></a>İstek Zaman Aşımı

Bu ek açıklama, yanıt alınmazsa Uygulama Ağ Geçidi'nin isteği başarısız edeceği saniyeler içinde istek zaman anına belirtmeye olanak tanır.

### <a name="usage"></a>Kullanım

```yaml
appgw.ingress.kubernetes.io/request-timeout: "20"
```

### <a name="example"></a>Örnek

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-timeout
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/request-timeout: "20"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="use-private-ip"></a>Özel IP kullanın

Bu ek açıklama, bu bitiş noktasını Uygulama Ağ Geçidi'nin Özel IP'si üzerinde ortaya çıkarılıp açıklamayacağımızı belirtmemizi sağlar.

> [!NOTE]
> * Uygulama Ağ Geçidi aynı bağlantı noktasında birden çok IP desteklemez (örnek: 80/443). Ek açıklama `appgw.ingress.kubernetes.io/use-private-ip: "false"` ve başka `appgw.ingress.kubernetes.io/use-private-ip: "true"` bir `HTTP` giriş agic Uygulama Ağ Geçidi güncelleştirmede başarısız neden olur.
> * Özel BIR IP'si olmayan Uygulama Ağ `appgw.ingress.kubernetes.io/use-private-ip: "true"` Geçidi için Ingresses ile birlikte yoksayılır. Bu uyarı ile `NoPrivateIP` bu girişler için denetleyici günlükleri ve giriş olayları yansıtılır.


### <a name="usage"></a>Kullanım
```yaml
appgw.ingress.kubernetes.io/use-private-ip: "true"
```

### <a name="example"></a>Örnek
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-timeout
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/use-private-ip: "true"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="backend-protocol"></a>Arka Uç Protokolü

Bu ek açıklama, Uygulama Ağ Geçidi'nin Podlarla konuşurken kullanması gereken protokolü belirtmemizi sağlar. Desteklenen Protokoller: `http`,`https`

> [!NOTE]
> * Şu anda Uygulama Ağ Geçidi'nde kendi imzalı sertifikalar `https` desteklenirken, AGIC yalnızca Pods tanınmış bir CA tarafından imzalanmış sertifikayı kullandığında desteklenir.
> * HTTPS ile 80 portu ve Pod'larda HTTP ile 443 bağlantı noktasını kullanmadığından emin olun.

### <a name="usage"></a>Kullanım
```yaml
appgw.ingress.kubernetes.io/backend-protocol: "https"
```

### <a name="example"></a>Örnek
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-timeout
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/backend-protocol: "https"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 443
```