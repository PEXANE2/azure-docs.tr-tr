---
title: Uygulama Ağ Geçidi ile LetsEncrypt.org sertifikakullanma
description: Bu makalede, LetsEncrypt.org'dan sertifika alma ve AKS kümeleri için Uygulama Ağ Geçidi'nde nasıl kullanılacağı hakkında bilgi verilmektedir.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 92e9747865f1a0910c8bae4001cc597ae9ea3da6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73957979"
---
# <a name="use-certificates-with-letsencryptorg-on-application-gateway-for-aks-clusters"></a>AKS kümeleri için Uygulama Ağ Geçidi'nde LetsEncrypt.org olan sertifikaları kullanma

Bu bölüm, AKS'nizi [LetsEncrypt.org](https://letsencrypt.org/) kaldıraç yapacak ve etki alanınız için otomatik olarak bir TLS/SSL sertifikası alacak şekilde yapılandırır. Sertifika, AKS kümeniz için SSL/TLS sonlandırma gerçekleştirecek Olan Uygulama Ağ Geçidi'ne yüklenir. Burada açıklanan kurulum, sertifikaların oluşturulmasını ve yönetimini otomatikleştiren [sertifika yöneticisi](https://github.com/jetstack/cert-manager) Kubernetes eklentisini kullanır.

Varolan AKS kümenize [cert yöneticisi](https://docs.cert-manager.io) yüklemek için aşağıdaki adımları izleyin.

1. Dümen Grafiği

    Dümen grafiğini yüklemek `cert-manager` için aşağıdaki komut dosyasını çalıştırın. Bu olacaktır:

    - AKS'nizde yeni `cert-manager` bir ad alanı oluşturma
    - aşağıdaki CRD'leri oluşturun: Sertifika, Meydan Okuma, ClusterIssuer, Veren, Sipariş
    - cert-manager grafiğini yükleme [(docs.cert-manager.io itibaren)](https://docs.cert-manager.io/en/latest/getting-started/install/kubernetes.html#steps)

    ```bash
    #!/bin/bash

    # Install the CustomResourceDefinition resources separately
    kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.8/deploy/manifests/00-crds.yaml

    # Create the namespace for cert-manager
    kubectl create namespace cert-manager

    # Label the cert-manager namespace to disable resource validation
    kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true

    # Add the Jetstack Helm repository
    helm repo add jetstack https://charts.jetstack.io

    # Update your local Helm chart repository cache
    helm repo update

    # Install the cert-manager Helm chart
    helm install \
      --name cert-manager \
      --namespace cert-manager \
      --version v0.8.0 \
      jetstack/cert-manager
    ```

2. ClusterIssuer Kaynak

    Bir `ClusterIssuer` kaynak oluşturun. İmzalanan sertifikaların alınacağı sertifika yetkilisini `cert-manager` `Lets Encrypt` temsil etmek le yükümlüdür.

    Sertifika yöneticisi, ad alanı `ClusterIssuer` olmayan kaynağı kullanarak birden çok ad alanından tüketilebilen sertifikalar verir. `Let’s Encrypt`belirli bir etki alanı adını denetlediğinizi doğrulamak ve size bir sertifika vermek için ACME protokolünü kullanır. `ClusterIssuer` [Burada](https://docs.cert-manager.io/en/latest/tasks/issuers/index.html)özellikleri yapılandırma hakkında daha fazla bilgi . `ClusterIssuer`sınama için kullanılan `Lets Encrypt` evreleme ortamını kullanarak sertifika lar verme talimatı `cert-manager` verir (kök sertifika tarayıcı/istemci güven depolarında bulunmaz).

    Aşağıdaki YAML'deki varsayılan meydan `http01`okuma türü. Diğer zorluklar letsencrypt.org belgelenmiştir [- Challenge Türleri](https://letsencrypt.org/docs/challenge-types/)

    > [!IMPORTANT] 
    > Aşağıdaki `<YOUR.EMAIL@ADDRESS>` YAML güncelleme

    ```bash
    #!/bin/bash
    kubectl apply -f - <<EOF
    apiVersion: certmanager.k8s.io/v1alpha1
    kind: ClusterIssuer
    metadata:
    name: letsencrypt-staging
    spec:
    acme:
        # You must replace this email address with your own.
        # Let's Encrypt will use this to contact you about expiring
        # certificates, and issues related to your account.
        email: <YOUR.EMAIL@ADDRESS>
        # ACME server URL for Let’s Encrypt’s staging environment.
        # The staging environment will not issue trusted certificates but is
        # used to ensure that the verification process is working properly
        # before moving to production
        server: https://acme-staging-v02.api.letsencrypt.org/directory
        privateKeySecretRef:
        # Secret resource used to store the account's private key.
        name: example-issuer-account-key
        # Enable the HTTP-01 challenge provider
        # you prove ownership of a domain by ensuring that a particular
        # file is present at the domain
        http01: {}
    EOF
    ```

3. Uygulamayı Dağıt

    Lets Encrypt Certificate ile `guestbook` Uygulama Ağ Geçidi'ni kullanarak uygulamayı ortaya çıkarmak için bir Giriş kaynağı oluşturun.

    Uygulama Ağ Geçidi'nde DNS adı olan ortak bir Frontend `azure.com` IP yapılandırması `Azure DNS Zone` olduğundan emin olun (varsayılan etki alanını kullanarak veya bir hizmeti sağlama ve kendi özel etki alanınızı atama).
    Sertifika yöneticisine `certmanager.k8s.io/cluster-issuer: letsencrypt-staging`etiketlenmiş Giriş kaynağını işlemesini söyleyen ek açıklamaya dikkat edin.

    > [!IMPORTANT] 
    > Aşağıdaki `<PLACEHOLDERS.COM>` YAML'de kendi etki alanınızla (veya uygulama ağ geçidi yle, örneğin 'kh-aks-ingress.westeurope.cloudapp.azure.com' ile güncelleştirin)

    ```bash
    kubectl apply -f - <<EOF
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
    name: guestbook-letsencrypt-staging
    annotations:
        kubernetes.io/ingress.class: azure/application-gateway
        certmanager.k8s.io/cluster-issuer: letsencrypt-staging
    spec:
    tls:
    - hosts:
        - <PLACEHOLDERS.COM>
        secretName: guestbook-secret-name
    rules:
    - host: <PLACEHOLDERS.COM>
        http:
        paths:
        - backend:
            serviceName: frontend
            servicePort: 80
    EOF
    ```

    Birkaç saniye sonra, otomatik `guestbook` olarak verilen **evreleme** `Lets Encrypt` sertifikasını kullanarak Uygulama Ağ Geçidi HTTPS url'si üzerinden hizmete erişebilirsiniz.
    Tarayıcınız geçersiz bir sertifika yetkilisi hakkında sizi uyarabilir. Evreleme sertifikası `CN=Fake LE Intermediate X1`. Bu, sistemin beklendiği gibi çalıştığının ve üretim sertifikanız için hazır olduğunuzun bir göstergesidir.

4. Üretim Sertifikası

    Hazırlama sertifikanız başarıyla kurulduktan sonra bir üretim ACME sunucusuna geçebilirsiniz:
    1. Giriş kaynağınızdaki evreleme ek açıklamasını aşağıdakilerle değiştirin:`certmanager.k8s.io/cluster-issuer: letsencrypt-prod`
    1. Önceki adımda `ClusterIssuer` oluşturduğunuz mevcut evrelemeyi silin ve yukarıdaki ClusterIssuer YAML'deki ACME sunucusunu değiştirerek yeni bir tane oluşturun`https://acme-v02.api.letsencrypt.org/directory`

5. Sertifikanın Sona Ermesi ve Yenilenmesi

    Sertifikanın `Lets Encrypt` süresi dolmadan önce, `cert-manager` Sertifikaknetes gizli mağazasındaotomatik olarak güncellenir. Bu noktada, Uygulama Ağ Geçidi Giriş Denetleyicisi, Uygulama Ağ Geçidi'ni yapılandırmak için kullandığı giriş kaynaklarında başvurulan güncelleştirilmiş gizliyi uygular.
