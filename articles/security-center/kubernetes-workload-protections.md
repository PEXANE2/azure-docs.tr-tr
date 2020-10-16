---
title: Kubernetes iş yükleriniz için iş yükü korumaları
description: Azure Güvenlik Merkezi 'nin bir Kubernetes iş yükü koruma güvenlik önerisi kümesini nasıl kullanacağınızı öğrenin
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 09/12/2020
ms.author: memildin
ms.openlocfilehash: 38c5df6a05d327e0b057501846e70d1f3c6c4896
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92091160"
---
# <a name="protect-your-kubernetes-workloads"></a>Kubernetes iş yüklerinizi koruma

Bu sayfada, Kubernetes iş yükü korumasına adanmış Azure Güvenlik Merkezi 'nin güvenlik önerileri kümesi 'nin nasıl kullanılacağı açıklanır.

Bu özellikler hakkında daha fazla bilgi için, [Kubernetes giriş denetimini kullanan Iş yükü koruma en iyi uygulamaları](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control)

Güvenlik Merkezi, Azure Defender 'ı etkinleştirirseniz daha fazla kapsayıcı güvenlik özelliği sunar. Özellikle:

- [Kapsayıcı kayıt defterleri Için Azure Defender](defender-for-container-registries-introduction.md) ile kapsayıcı kayıt defterlerinden güvenlik açıklarını tarayın
- [Kubernetes Için Azure Defender](defender-for-kubernetes-introduction.md) K8s kümeleriniz için gerçek zamanlı tehdit algılama uyarıları alın

> [!TIP]
> Kubernetes kümeleri ve düğümleri için görünebilen *Tüm* güvenlik önerilerinin listesi için, öneriler başvuru tablosunun [kapsayıcı bölümüne](recommendations-reference.md#recs-containers) bakın.



## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Önizleme|
|Fiyat|Ücretsiz|
|Gerekli roller ve izinler:|Bir atamayı düzenlemek için **sahip** veya **Güvenlik Yöneticisi**<br>Önerileri görüntülemek için **okuyucu**|
|Desteklenen kümeler:|Kubernetes v 1.14 (veya üzeri) gereklidir<br>Kümelerde Pod SecurityPolicy kaynağı (eski PSP modeli) yok<br>Windows düğümleri desteklenmez|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![No](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||


## <a name="set-up-your-workload-protection"></a>İş yükü korumanızı ayarlama

Azure Güvenlik Merkezi, **Kubernetes Için Azure ilke eklentisini**yüklediğinizde kullanabileceğiniz bir öneriler paketi içerir.

1. Önerileri yapılandırmak için öncelikle eklentiyi yüklemeniz gerekir:

    1. Öneriler sayfasından, **Kubernetes Için Azure ilke eklentisi adlı öneriyi aramak, kümelerinizde yüklü ve etkinleştirilmiş olmalıdır**.

        :::image type="content" source="./media/defender-for-kubernetes-usage/recommendation-to-install-policy-add-on-for-kubernetes.png" alt-text="Öneri * * Kubernetes için Azure Ilke eklentisi 'nin kümelerinizde yüklü ve etkin olması gerekir * *":::

        > [!TIP]
        > Öneri beş farklı güvenlik denetimine dahildir ve bu, bir sonraki adımda nerede seçim yapmanız gerektiğini fark etmez.

    1. Güvenlik denetimlerinden herhangi birinden, üzerinde eklentiyi yükleyebileceğiniz kaynakları görmek için öneriyi seçin ve **Düzelt**' i seçin. 

        :::image type="content" source="./media/defender-for-kubernetes-usage/recommendation-to-install-policy-add-on-for-kubernetes-details.png" alt-text="Öneri * * Kubernetes için Azure Ilke eklentisi 'nin kümelerinizde yüklü ve etkin olması gerekir * *" öğesini seçin.
    1. **Parametreler** sekmesini açın ve değerleri gerektiği gibi değiştirin.
    1. **Gözden geçir + kaydet**' i seçin.
    1. **Kaydet**’i seçin.


1. Önerilerden herhangi birini zorlamak için, 

    1. Öneri ayrıntıları sayfasını açın ve **Reddet**' i seçin:

        :::image type="content" source="./media/defender-for-kubernetes-usage/enforce-workload-protection-example.png" alt-text="Öneri * * Kubernetes için Azure Ilke eklentisi 'nin kümelerinizde yüklü ve etkin olması gerekir * *":::

        Bu işlem, kapsamı ayarladığınız bölmeyi açar. 

    1. Kapsamı ayarladığınızda, **Reddet olarak değiştir**' i seçin.

1. Kümelerinize hangi önerilerin uygulanacağını görmek için:

    1. Güvenlik Merkezi 'nin [varlık envanteri](asset-inventory.md) sayfasını açın ve **Kubernetes Hizmetleri**için kaynak türü filtresini kullanın.

    1. Araştırılması için bir küme seçin ve kullanılabilir önerileri gözden geçirin. 

1. İş yükü koruma kümesinden bir öneri görüntülenirken, küme ile birlikte listelenen etkilenen düğüm sayısını ("Kubernetes bileşenleri") görürsünüz. Belirli FID 'lerin bir listesi için kümeyi seçin ve ardından **eylem al**' ı seçin.

    :::image type="content" source="./media/defender-for-kubernetes-usage/view-affected-pods-for-recommendation.gif" alt-text="Öneri * * Kubernetes için Azure Ilke eklentisi 'nin kümelerinizde yüklü ve etkin olması gerekir * *"::: 

1. Zorlamayı test etmek için aşağıdaki iki Kubernetes dağıtımını kullanın:

    - Biri sağlıklı dağıtım için iş yükü koruma önerileri paketiyle uyumludur.
    - Diğeri, *hiçbir* öneriyle uyumlu olmayan sağlıksız bir dağıtım içindir.

    Örnek. YAML dosyalarını olduğu gibi dağıtın veya kendi iş yükünüzü düzeltmek için bunları bir başvuru olarak kullanın (adım VIıı)  


## <a name="healthy-deployment-example-yaml-file"></a>Sağlıklı dağıtım örneği. YAML dosyası

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-healthy-deployment
  labels:
    app: redis
spec:
  replicas: 3
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
      annotations:
        apparmor.security.beta.kubernetes.io/pod: runtime/default
        container.apparmor.security.beta.kubernetes.io/redis: runtime/default
    spec:
      containers:
      - name: redis
        image: healthyClusterRegistry.azurecr.io/redis:latest
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: 100m
            memory: 250Mi
        securityContext:
          privileged: false
          readOnlyRootFilesystem: true
          allowPrivilegeEscalation: false
          runAsNonRoot: true
          runAsUser: 1000
---
apiVersion: v1
kind: Service
metadata:
  name: redis-healthy-service
spec:
  type: LoadBalancer
  selector:
    app: redis
  ports:
  - port: 80
    targetPort: 80
```

## <a name="unhealthy-deployment-example-yaml-file"></a>Sağlıksız dağıtım örneği. YAML dosyası

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-unhealthy-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:      
      labels:
        app: nginx
    spec:
      hostNetwork: true
      hostPID: true 
      hostIPC: true
      containers:
      - name: nginx
        image: nginx:1.15.2
        ports:
        - containerPort: 9001
          hostPort: 9001
        securityContext:
          privileged: true
          readOnlyRootFilesystem: false
          allowPrivilegeEscalation: true
          runAsUser: 0
          capabilities:
            add:
              - NET_ADMIN
        volumeMounts:
        - mountPath: /test-pd
          name: test-volume
          readOnly: true
      volumes:
      - name: test-volume
        hostPath:
          # directory location on host
          path: /tmp
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-unhealthy-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - port: 6001
    targetPort: 9001
```



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Kubernetes iş yükü korumasını nasıl yapılandıracağınızı öğrendiniz. 

Diğer ilgili malzemeler için aşağıdaki sayfalara bakın: 

- [Kapsayıcılar için Güvenlik Merkezi önerileri](recommendations-reference.md#recs-containers)
- [AKS kümesi düzeyi için uyarılar](alerts-reference.md#alerts-akscluster)
- [Kapsayıcı ana bilgisayar düzeyi uyarıları](alerts-reference.md#alerts-containerhost)