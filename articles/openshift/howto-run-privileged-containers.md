---
title: Ayrıcalıklı kapsayıcıları Azure Red Hat OpenShift kümesinde çalıştırma | Microsoft Docs
description: Güvenlik ve uyumluluğu izlemek için ayrıcalıklı kapsayıcıları çalıştırın.
author: makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 12/05/2019
keywords: Aro, OpenShift, aquasn, twistlock, Red Hat
ms.openlocfilehash: 5d28a19126c9b7ae4ef7afe2a6b69bd4a13e0c83
ms.sourcegitcommit: 390cfe85629171241e9e81869c926fc6768940a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78228235"
---
# <a name="run-privileged-containers-in-an-azure-red-hat-openshift-cluster"></a>Azure Red Hat OpenShift kümesinde ayrıcalıklı kapsayıcılar çalıştırma

Azure Red Hat OpenShift kümelerinde rastgele ayrıcalıklı kapsayıcılar çalıştıramazsınız.
İki güvenlik izleme ve uyumluluk çözümünün, ARO kümelerinde çalışmasına izin verilir.
Bu belgede, güvenlik ürün satıcılarının genel OpenShift dağıtım belgelerindeki farklar açıklanmaktadır.


Satıcının yönergelerini takip etmeden önce bu yönergeleri okuyun.
Aşağıdaki ürüne özgü adımlarda bulunan bölüm başlıkları, satıcıların belgelerindeki bölüm başlıklarına doğrudan başvurur.

## <a name="before-you-begin"></a>Başlamadan önce

Çoğu güvenlik ürününün belgeleri, Küme Yöneticisi ayrıcalıklarına sahip olduğunuzu varsayar.
Müşteri yöneticilerinin Azure Red Hat OpenShift 'de tüm ayrıcalıkları yoktur. Küme genelinde kaynakları değiştirmek için gereken izinler sınırlıdır.

İlk olarak, `oc get scc`çalıştırarak kullanıcının kümede müşteri yöneticisi olarak oturum açtığından emin olun. Müşteri Yöneticisi grubunun üyesi olan tüm kullanıcılar, kümedeki güvenlik bağlamı kısıtlamalarını (SCCs) görüntüleme iznine sahiptir.

Sonra `oc` ikili sürümünün `3.11.154`olduğundan emin olun.
```
oc version
oc v3.11.154
kubernetes v1.11.0+d4cacc0
features: Basic-Auth GSSAPI Kerberos SPNEGO

Server https://openshift.aqua-test.osadev.cloud:443
openshift v3.11.154
kubernetes v1.11.0+d4cacc0
```

## <a name="product-specific-steps-for-aqua-security"></a>Deniz mavisi güvenlik için ürüne özgü adımlar
Değiştirilecek temel yönergeler, [deniz mavisi güvenlik dağıtım belgelerinde](https://docs.aquasec.com/docs/openshift-red-hat)bulunabilir. Buradaki adımlar, deniz mavisi dağıtım belgelerine birlikte çalışacaktır.

İlk adım, güncelleştirilecektir gerekli olan SCCs 'ye açıklama eklemek için kullanılır. Bu ek açıklamalar, kümenin Sync Pod 'un bu SSCs 'teki değişiklikleri geri almasını engeller.

```
oc annotate scc hostaccess openshift.io/reconcile-protect=true
oc annotate scc privileged openshift.io/reconcile-protect=true
```

### <a name="step-1-prepare-prerequisites"></a>1\. Adım: önkoşulları hazırlama
Kümede, Küme Yöneticisi rolü yerine bir ARO müşteri yöneticisi olarak oturum açmanız gerektiğini unutmayın.

Projeyi ve hizmet hesabını oluşturun.
```
oc new-project aqua-security
oc create serviceaccount aqua-account -n aqua-security
```

Küme okuyucu rolünü atamak yerine, aşağıdaki komutla birlikte deniz mavisi hesabına müşteri-yönetici-küme rolünü atayın.
```
oc adm policy add-cluster-role-to-user customer-admin-cluster system:serviceaccount:aqua-security:aqua-account
oc adm policy add-scc-to-user privileged system:serviceaccount:aqua-security:aqua-account
oc adm policy add-scc-to-user hostaccess system:serviceaccount:aqua-security:aqua-account
```

Adım 1 ' de kalan yönergeleri izleyerek devam edin.  Bu yönergeler, deniz mavisi kayıt defteri için gizli dizi ayarlamayı anlatmaktadır.

### <a name="step-2-deploy-the-aqua-server-database-and-gateway"></a>2\. Adım: deniz mavisi sunucu, veritabanı ve ağ geçidini dağıtma
Deniz mavisi-Console. YAML 'yi yüklemek için, deniz mavisi belgelerde verilen adımları izleyin.

Belirtilen `aqua-console.yaml`değiştirin.  `kind: ClusterRole` ve `kind: ClusterRoleBinding`etiketli en üstteki iki nesneyi kaldırın.  Müşteri yöneticisinin `ClusterRole` ve `ClusterRoleBinding` nesneleri değiştirmek için şu anda izin olmadığından, bu kaynaklar oluşturulmayacak.

İkinci değişiklik `aqua-console.yaml``kind: Route` bölümüne olacaktır. `aqua-console.yaml` dosyasındaki `kind: Route` nesnesi için aşağıdaki YAML 'yi değiştirin.
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

Kalan yönergeleri izleyin.

### <a name="step-3-login-to-the-aqua-server"></a>3\. Adım: deniz mavisi sunucuda oturum açma
Bu bölüm hiçbir şekilde değiştirilmez.  Deniz mavisi belgeleri izleyin.

Deniz mavisi konsol adresini almak için aşağıdaki komutu kullanın.
```
oc get route aqua-web -n aqua-security
```

### <a name="step-4-deploy-aqua-enforcers"></a>4\. Adım: deniz mavisi Enforcers dağıtma
Enforcers dağıtımı sırasında aşağıdaki alanları ayarlayın:

| Alan          | Değer         |
| -------------- | ------------- |
| Orchestrator   | OpenShift     |
| Çalıştırıp | deniz mavisi-hesap  |
| Project        | deniz mavisi-güvenlik |

## <a name="product-specific-steps-for-prisma-cloud--twistlock"></a>Prma bulutu/twistlock için ürüne özgü adımlar

Değişiklik yaptığımız temel yönergeler [Prma bulutu dağıtım belgelerinde](https://docs.paloaltonetworks.com/prisma/prisma-cloud/19-11/prisma-cloud-compute-edition-admin/install/install_openshift.html) bulunabilir.

`twistcli` aracını "Przma bulutu yükleme" ve "Przma bulut yazılımını Indirme" bölümlerinde açıklandığı gibi yükleyerek başlayın.

Yeni bir OpenShift projesi oluştur
```
oc new-project twistlock
```

"Przma bulut görüntülerini özel bir kayıt defterine gönder" isteğe bağlı bölümünü atlayın. Azure Red Hat OpenShift üzerinde çalışmaz. Bunun yerine çevrimiçi kayıt defterini kullanın.

Aşağıda açıklanan düzeltmeleri uygularken resmi belgeleri takip edebilirsiniz.
"Konsolu yüklemeyi" bölümü ile başlayın.

### <a name="install-console"></a>Konsolu yükler

2\. adımdaki `oc create -f twistlock_console.yaml` sırasında, ad alanını oluştururken bir hata alırsınız.
Bu adı güvenle yoksayabilirsiniz, ad alanı daha önce `oc new-project` komutuyla oluşturulmuştur.

### <a name="create-an-external-route-to-console"></a>Konsola dış yol oluştur

Ya da OC komutunu tercih ediyorsanız, belgeleri veya aşağıdaki talimatları izleyebilirsiniz.
Aşağıdaki rota tanımını bilgisayarınızda twistlock_route. YAML adlı bir dosyaya kopyalayın
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
ardından şunu çalıştırın:
```
oc create -f twistlock_route.yaml
```

Twistlock konsoluna atanan URL 'YI şu komutla alabilirsiniz: `oc get route twistlock-console -n twistlock`

### <a name="configure-console"></a>Konsolu Yapılandır

Twistlock belgelerini izleyin.

### <a name="install-defender"></a>Defender 'ı yükler

2\. adımdaki `oc create -f defender.yaml` sırasında, küme rolünü ve küme rolü bağlamasını oluştururken hata alırsınız.
Bunları yoksayabilirsiniz.

Savunma işlemleri yalnızca işlem düğümlerinde dağıtılır. Bunları bir düğüm seçicisiyle sınırlandırmanıza gerek yoktur.
