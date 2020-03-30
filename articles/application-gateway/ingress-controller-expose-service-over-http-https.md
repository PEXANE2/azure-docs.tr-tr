---
title: Uygulama Ağ Geçidi'ni kullanarak HTTP veya HTTPS üzerinden bir AKS hizmetini ortaya çıkarma
description: Bu makalede, Uygulama Ağ Geçidi'ni kullanarak HTTP veya HTTPS üzerinden bir AKS hizmetinin nasıl ortaya çıkarılacak hakkında bilgi verilmektedir.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: c664141a8c89ccbdf37bd3f9a19cfa659982a47d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73795581"
---
# <a name="expose-an-aks-service-over-http-or-https-using-application-gateway"></a>Uygulama Ağ Geçidi'ni kullanarak HTTP veya HTTPS üzerinden bir AKS hizmetini ortaya çıkarma 

Bu öğreticiler, Http veya HTTPS üzerinden [Azure Uygulama Ağ Geçidi](https://azure.microsoft.com/services/application-gateway/) üzerinden örnek bir Kubernetes hizmetini ortaya çıkarmak için [Kubernetes Ingress Kaynakları'nın](https://kubernetes.io/docs/concepts/services-networking/ingress/) kullanımını göstermeye yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar

- Yüklü `ingress-azure` dümen grafiği.
  - [**Greenfield Deployment**](ingress-controller-install-new.md): Sıfırdan başlıyorsanız, Uygulama Ağ Geçidi ile bir AKS kümesini dağıtmak ve AKS kümesine uygulama ağ geçidi denetleyicisini yüklemek için adımları özetleyen bu yükleme yönergelerine bakın.
  - [**Brownfield Deployment**](ingress-controller-install-existing.md): Varolan bir AKS kümeniz ve Uygulama Ağ Geçidiniz varsa, AKS kümesine uygulama ağ geçidi denetleyicisini yüklemek için bu talimatlara bakın.
- Bu uygulamada HTTPS kullanmak istiyorsanız, bir x509 sertifikası ve özel anahtarı gerekir.

## <a name="deploy-guestbook-application"></a>Uygulamayı `guestbook` dağıtma

Ziyaretçi defteri uygulaması, Web Kullanıcı Gönlör, arka uç ve Redis veritabanından oluşan kurallı bir Kubernetes uygulamasıdır. Varsayılan olarak, `guestbook` bağlantı noktası `frontend` `80`üzerinde adı olan bir hizmet aracılığıyla uygulama ortaya çıkarır. Kubernetes Giriş Kaynağı olmadan, hizmete AKS kümesi dışından erişilemez. Uygulamaya HTTP ve HTTPS üzerinden erişmek için uygulama ve kurulum Ingress Kaynakları kullanacağız.

Ziyaretçi defteri uygulamasını dağıtmak için aşağıdaki yönergeleri izleyin.

1. Buradan `guestbook-all-in-one.yaml` [here](https://raw.githubusercontent.com/kubernetes/examples/master/guestbook/all-in-one/guestbook-all-in-one.yaml) indirin
1. Çalıştırarak AKS kümenize dağıtın `guestbook-all-in-one.yaml`

  ```bash
  kubectl apply -f guestbook-all-in-one.yaml
  ```

Şimdi, `guestbook` uygulama dağıtıldı.

## <a name="expose-services-over-http"></a>HTTP üzerinden hizmetleri ortaya çıkarma

Ziyaretçi defteri uygulamasını ortaya çıkarmak için aşağıdaki giriş kaynağını kullanıyor olacağız:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: guestbook
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: frontend
          servicePort: 80
```

Bu giriş, `frontend` dağıtım hizmetini `guestbook-all-in-one` Uygulama Ağ Geçidi'nin varsayılan arka ucu olarak ortaya çıkarır.

Yukarıdaki giriş kaynağını `ing-guestbook.yaml`' olarak kaydedin.

1. Çalıştırarak dağıt: `ing-guestbook.yaml`

    ```bash
    kubectl apply -f ing-guestbook.yaml
    ```

1. Dağıtım durumu için giriş denetleyicisinin günlüğünü denetleyin.

Şimdi `guestbook` uygulama mevcut olmalıdır. Uygulama Ağ Geçidi'nin genel adresini ziyaret ederek bunu kontrol edebilirsiniz.

## <a name="expose-services-over-https"></a>HTTPS üzerinden hizmetleri ortaya çıkarma

### <a name="without-specified-hostname"></a>Belirtilen ana bilgisayar adı olmadan

Konuk defteri hizmeti, ana bilgisayar adı belirtmeden, uygulama ağ geçidini gösteren tüm ana bilgisayar adlarında kullanılabilir.

1. Girişi dağıtmadan önce, sertifikayı ve özel anahtarı barındırmak için bir kubernetes sırrı oluşturmanız gerekir. Çalıştırarak bir kubernetes gizli oluşturabilirsiniz

    ```bash
    kubectl create secret tls <guestbook-secret-name> --key <path-to-key> --cert <path-to-cert>
    ```

1. Aşağıdaki girişi tanımlayın. Girişte, bölümdeki sırrın adını `secretName` belirtin.

    ```yaml
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: guestbook
      annotations:
        kubernetes.io/ingress.class: azure/application-gateway
    spec:
      tls:
        - secretName: <guestbook-secret-name>
      rules:
      - http:
          paths:
          - backend:
              serviceName: frontend
              servicePort: 80
    ```

    > [!NOTE] 
    > Yukarıdaki `<guestbook-secret-name>` Ingress Resource'ı sırrınızın adı ile değiştirin. Yukarıdaki Giriş Kaynağını bir dosya `ing-guestbook-tls.yaml`adında depolar.

1. Çalıştırarak-ziyaretçi defteri-tls.yaml dağıtma

    ```bash
    kubectl apply -f ing-guestbook-tls.yaml
    ```

1. Dağıtım durumu için giriş denetleyicisinin günlüğünü denetleyin.

Şimdi `guestbook` uygulama hem HTTP hem de HTTPS'de kullanılabilir olacaktır.

### <a name="with-specified-hostname"></a>Belirtilen ana bilgisayar adı ile

TlS yapılandırmaları ve hizmetleri için girişte ana bilgisayar adını da belirtebilirsiniz.
Konuk defteri hizmeti, ana bilgisayar adı belirterek yalnızca belirtilen ana bilgisayarda kullanılabilir.

1. Aşağıdaki girişi tanımlayın.
    Girişte, `secretName` bölümdeki sırrın adını belirtin ve bölümdeki `hosts` ana bilgisayarı buna göre değiştirin.

    ```yaml
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: guestbook
      annotations:
        kubernetes.io/ingress.class: azure/application-gateway
    spec:
      tls:
        - hosts:
          - <guestbook.contoso.com>
          secretName: <guestbook-secret-name>
      rules:
      - host: <guestbook.contoso.com>
        http:
          paths:
          - backend:
              serviceName: frontend
              servicePort: 80
    ```

1. Çalıştırarak dağıt `ing-guestbook-tls-sni.yaml`

    ```bash
    kubectl apply -f ing-guestbook-tls-sni.yaml
    ```

1. Dağıtım durumu için giriş denetleyicisinin günlüğünü denetleyin.

Şimdi `guestbook` uygulama sadece belirtilen ana bilgisayarda (bu`<guestbook.contoso.com>` örnekte) hem HTTP hem de HTTPS'de kullanılabilir olacaktır.

## <a name="integrate-with-other-services"></a>Diğer hizmetlerle tümleştirme

Aşağıdaki giriş, bu girişe ek yollar eklemenize ve bu yolları diğer hizmetlere yönlendirmenize olanak sağlar:

    ```yaml
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: guestbook
      annotations:
        kubernetes.io/ingress.class: azure/application-gateway
    spec:
      rules:
      - http:
          paths:
          - path: </other/*>
            backend:
              serviceName: <other-service>
              servicePort: 80
          - backend:
              serviceName: frontend
              servicePort: 80
    ```
