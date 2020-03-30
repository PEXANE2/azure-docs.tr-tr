---
title: Azure Red Hat OpenShift kümesinde ayrıcalıklı kapsayıcıları çalıştırma | Microsoft Dokümanlar
description: Güvenlik ve uyumluluğu izlemek için ayrıcalıklı kapsayıcılar çalıştırın.
author: makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 12/05/2019
keywords: aro, openshift, aquasec, twistlock, kırmızı şapka
ms.openlocfilehash: e1c1dd9f27a207f78dd22e271f6b070c7f92f622
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78271371"
---
# <a name="run-privileged-containers-in-an-azure-red-hat-openshift-cluster"></a>Azure Red Hat OpenShift kümesinde ayrıcalıklı kapsayıcılar çalıştırma

Azure Red Hat OpenShift kümelerinde rasgele ayrıcalıklı kapsayıcılar çalıştıramazsınız.
ARO kümelerinde iki güvenlik izleme ve uyumluluk çözümünü çalıştırabilirsiniz.
Bu belge, güvenlik ürünü satıcılarının genel OpenShift dağıtım belgelerinden farklarını açıklar.


Satıcının yönergelerini takip etmeden önce bu yönergeleri okuyun.
Aşağıdaki ürüne özel adımlardaki bölüm başlıkları doğrudan satıcıların belgelerindeki bölüm başlıklarına başvurur.

## <a name="before-you-begin"></a>Başlamadan önce

Çoğu güvenlik ürününün belgeleri, küme yöneticisi ayrıcalıklarına sahip olduğunuzu varsayar.
Azure Red Hat OpenShift'te müşteri yöneticilerinin tüm ayrıcalıkları yoktur. Küme genelindeki kaynakları değiştirmek için gereken izinler sınırlıdır.

İlk olarak, kullanıcının çalıştırarak bir müşteri yöneticisi olarak `oc get scc`kümeye oturum açtığından emin olun. Müşteri yöneticisi grubunun üyesi olan tüm kullanıcıların kümedeki Güvenlik Bağlamı Kısıtlamalarını (SCC'ler) görüntüleme izinleri vardır.

Ardından, ikili `oc` sürümün `3.11.154`.
```
oc version
oc v3.11.154
kubernetes v1.11.0+d4cacc0
features: Basic-Auth GSSAPI Kerberos SPNEGO

Server https://openshift.aqua-test.osadev.cloud:443
openshift v3.11.154
kubernetes v1.11.0+d4cacc0
```

## <a name="product-specific-steps-for-aqua-security"></a>Aqua Security için ürüne özel adımlar
Değiştirilecek temel yönergeler [Aqua Security dağıtım belgelerinde](https://docs.aquasec.com/docs/openshift-red-hat)bulunabilir. Buradaki adımlar Aqua dağıtım belgeleriyle birlikte çalışacaktır.

İlk adım, güncellenecek gerekli SCC'lere açıklama vermektir. Bu ek açıklamalar kümenin Eşitleme Bölmesi'nin bu SSC'lerde yapılan değişiklikleri geri almamasını engeller.

```
oc annotate scc hostaccess openshift.io/reconcile-protect=true
oc annotate scc privileged openshift.io/reconcile-protect=true
```

### <a name="step-1-prepare-prerequisites"></a>Adım 1: Ön koşulları hazırlama
Küme yöneticisi rolü yerine ARO Müşteri Yöneticisi olarak kümede oturum açmayı unutmayın.

Projeyi ve hizmet hesabını oluşturun.
```
oc new-project aqua-security
oc create serviceaccount aqua-account -n aqua-security
```

Küme okuyucu rolünü atamak yerine, aşağıdaki komutla su hesabına müşteri-yönetici küme rolünü atayın.
```
oc adm policy add-cluster-role-to-user customer-admin-cluster system:serviceaccount:aqua-security:aqua-account
oc adm policy add-scc-to-user privileged system:serviceaccount:aqua-security:aqua-account
oc adm policy add-scc-to-user hostaccess system:serviceaccount:aqua-security:aqua-account
```

Adım 1'de kalan yönergeleri takip edin.  Bu talimatlar Aqua kayıt defterinin sırrını ortaya çıkartıyor.

### <a name="step-2-deploy-the-aqua-server-database-and-gateway"></a>Adım 2: Aqua Server, Veritabanı ve Ağ Geçidi'ni dağıtma
Aqua-console.yaml'ı yüklemek için Aqua belgelerinde belirtilen adımları izleyin.

Sağlanan `aqua-console.yaml`değiştirin.  Etiketli en üstteki iki `kind: ClusterRole` nesneyi kaldırın ve `kind: ClusterRoleBinding`.  Müşteri yöneticisinin şu anda değiştirme `ClusterRole` ve `ClusterRoleBinding` nesne yapma izni olmadığı için bu kaynaklar oluşturulmaz.

İkinci değişiklik `kind: Route` bölümüne `aqua-console.yaml`olacaktır. Dosyadaki nesne için `kind: Route` aşağıdaki yaml'yi değiştirin. `aqua-console.yaml`
```
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  labels:
    app: aqua-web
  name: aqua-web
  namespace: aqua-security
spec:
  port:
    targetPort: aqua-web
  tls:
    insecureEdgeTerminationPolicy: Redirect
    termination: edge
  to:
    kind: Service
    name: aqua-web
    weight: 100
  wildcardPolicy: None
```

Kalan talimatları izleyin.

### <a name="step-3-login-to-the-aqua-server"></a>Adım 3: Aqua Server'a giriş
Bu bölüm hiçbir şekilde değiştirilmez.  Aqua belgelerini takip edin.

Aqua Console adresini almak için aşağıdaki komutu kullanın.
```
oc get route aqua-web -n aqua-security
```

### <a name="step-4-deploy-aqua-enforcers"></a>Adım 4: Aqua Uygulayıcıları Dağıtın
Uygulayıcıları dağıtırken aşağıdaki alanları ayarlayın:

| Alan          | Değer         |
| -------------- | ------------- |
| Orchestrator   | OpenShift     |
| ServiceAccount | aqua hesabı  |
| Project        | su güvenliği |

## <a name="product-specific-steps-for-prisma-cloud--twistlock"></a>Prisma Cloud / Twistlock için ürüne özel adımlar

Değiştireceğimiz temel yönergeleri [Prisma Bulut dağıtım belgelerinde](https://docs.paloaltonetworks.com/prisma/prisma-cloud/19-11/prisma-cloud-compute-edition-admin/install/install_openshift.html) bulunabilir

`twistcli` "Prisma Cloud'u Yükle" ve "Prisma Bulutu yazılımını indirin" bölümlerinde açıklandığı gibi aracı yükleyerek başlayın.

Yeni bir OpenShift projesi oluşturun
```
oc new-project twistlock
```

İsteğe bağlı bölümü atlayın "Prisma Bulut görüntüleri özel bir kayıt defterine itin". Azure Red Hat Openshift'te çalışmaz. Bunun yerine çevrimiçi kayıt defterini kullanın.

Aşağıda açıklanan düzeltmeleri uygularken resmi belgeleri takip edebilirsiniz.
"Konsolu Yükle" bölümüyle başlayın.

### <a name="install-console"></a>Konsol Yükle

Adım `oc create -f twistlock_console.yaml` 2 sırasında, ad alanı oluştururken bir Hata alırsınız.
Güvenli bir şekilde yoksayabilirsiniz, ad alanı `oc new-project` komutu ile daha önce oluşturulmuştur.

Depolama `azure-disk` türü için kullanın.

### <a name="create-an-external-route-to-console"></a>Konsola harici bir rota oluşturma

Oc komutunu tercih ederseniz, belgeleri veya aşağıdaki talimatları takip edebilirsiniz.
Bilgisayarınızda twistlock_route.yaml adlı bir dosyaya aşağıdaki Rota tanımını kopyalama
```
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  labels:
    name: console
  name: twistlock-console
  namespace: twistlock
spec:
  port:
    targetPort: mgmt-http
  tls:
    insecureEdgeTerminationPolicy: Redirect
    termination: edge
  to:
    kind: Service
    name: twistlock-console
    weight: 100
  wildcardPolicy: None
```
sonra çalıştırın:
```
oc create -f twistlock_route.yaml
```

Bu komutla Twistlock konsoluna atanan URL'yi alabilirsiniz:`oc get route twistlock-console -n twistlock`

### <a name="configure-console"></a>Konsolu yapılandırma

Twistlock belgelerini takip edin.

### <a name="install-defender"></a>Defans Yükle

Adım `oc create -f defender.yaml` 2 sırasında, Küme Rolü ve Küme Rolü Bağlama oluştururken Hatalar alırsınız.
Bunları yoksayabilirsiniz.

Savunucular yalnızca işlem düğümlerinde dağıtılacaktır. Bir düğüm seçici ile sınırlamak zorunda değilsiniz.
