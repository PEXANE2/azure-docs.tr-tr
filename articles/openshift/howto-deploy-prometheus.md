---
title: Prometheus örneğini Azure Red Hat OpenShift kümesine dağıtın
description: Uygulamanızın ölçümlerini izlemek için Azure Red Hat OpenShift kümesinde bir Prometheus örneği oluşturun.
author: makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 06/17/2019
keywords: prometheus, aro, openshift, ölçümler, kırmızı şapka
ms.openlocfilehash: 7f22df587f51af735e0ea663e53f6eef14d60692
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886897"
---
# <a name="deploy-a-standalone-prometheus-instance-in-an-azure-red-hat-openshift-cluster"></a>Azure Red Hat OpenShift kümesinde tek başına bir Prometheus örneğini dağıtma

Bu makalede, bir Azure Red Hat OpenShift kümesinde hizmet bulma kullanan bağımsız bir Prometheus örneğinin nasıl yapılandırılacağı açıklanmaktadır.

> [!NOTE]
> Azure Red Hat OpenShift kümesine müşteri yöneticisi erişimi gerekmez.

Hedef kurulumu:

- Prometheus ve Alertmanager içeren bir proje (prometheus-project).
- İzlenecek uygulamaları içeren iki proje (app-project1 ve app-project2).

Bazı Prometheus config dosyalarını yerel olarak hazırlayayım. Bunları depolamak için yeni bir klasör oluşturun. Config dosyaları, gizli belirteçlerin daha sonra kümeye eklenmesi durumunda, sır olarak kümede saklanır.

## <a name="sign-in-to-the-cluster-by-using-the-oc-tool"></a>OC aracını kullanarak kümede oturum açma

1. Bir web tarayıcısı açın ve ardından kümenizinhttps://openshiftweb konsoluna gidin ( .* rastgele kimlik*. *bölge*.azmosa.io).
2. Azure kimlik bilgilerinizle oturum açın.
3. Sağ üst köşede kullanıcı adınızı seçin ve ardından **Giriş Komutunu Kopyala'yı**seçin.
4. Kullanıcı adınızı kullanacağınız terminale yapıştırın.

> [!NOTE]
> Doğru kümede oturum açıp imzalamadığınızı görmek `oc whoami -c` için komutu çalıştırın.

## <a name="prepare-the-projects"></a>Projeleri hazırlama

Projeleri oluşturmak için aşağıdaki komutları çalıştırın:
```
oc new-project prometheus-project
oc new-project app-project1
oc new-project app-project2
```


> [!NOTE]
> `-n` Ya parametre yi `--namespace` kullanabilir veya komutu `oc project` çalıştırarak etkin bir proje seçebilirsiniz.

## <a name="prepare-the-prometheus-configuration-file"></a>Prometheus yapılandırma dosyasını hazırlama
Aşağıdaki içeriği girerek bir prometheus.yml dosyası oluşturun:
```
global:
  scrape_interval: 30s
  evaluation_interval: 5s

scrape_configs:
    - job_name: prom-sd
      scrape_interval: 30s
      scrape_timeout: 10s
      metrics_path: /metrics
      scheme: http
      kubernetes_sd_configs:
      - api_server: null
        role: endpoints
        namespaces:
          names:
          - prometheus-project
          - app-project1
          - app-project2
```
Aşağıdaki yapılandırmayı girerek Prom adlı bir sır oluşturun:
```
oc create secret generic prom --from-file=prometheus.yml -n prometheus-project
```

Prometheus.yml dosyası temel bir Prometheus yapılandırma dosyasıdır. Aralıkları belirler ve otomatik keşfi üç projede (prometheus-project, app-project1, app-project2) yapılandırır. Önceki yapılandırma dosyasında, otomatik keşfedilen uç noktalar kimlik doğrulaması olmadan HTTP üzerinden kazındı.

Uç noktaları kazıma hakkında daha fazla bilgi için [Prometheus scape config'e](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config)bakın.


## <a name="prepare-the-alertmanager-config-file"></a>Alertmanager config dosyasını hazırlama
Aşağıdaki içeriği girerek alertmanager.yml dosyası oluşturun:
```
global:
  resolve_timeout: 5m
route:
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 12h
  receiver: default
  routes:
  - match:
      alertname: DeadMansSwitch
    repeat_interval: 5m
    receiver: deadmansswitch
receivers:
- name: default
- name: deadmansswitch
```
Aşağıdaki yapılandırmayı girerek Prom-Alerts adlı bir sır oluşturun:
```
oc create secret generic prom-alerts --from-file=alertmanager.yml -n prometheus-project
```

Alertmanager.yml, Alert Manager yapılandırma dosyasıdır.

> [!NOTE]
> Önceki iki adımı doğrulamak için `oc get secret -n prometheus-project` komutu çalıştırın.

## <a name="start-prometheus-and-alertmanager"></a>Prometheus ve Alertmanager'ı başlatın
[Openshift/origin deposuna](https://github.com/openshift/origin/tree/release-3.11/examples/prometheus) gidin ve [prometheus-standalone.yaml](
https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml) şablonuna indirin. Aşağıdaki yapılandırmayı girerek şablonu prometheus-project'e uygulayın:
```
oc process -f https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml | oc apply -f - -n prometheus-project
```
Prometheus-standalone.yaml dosyası bir OpenShift şablonudur. Önünde oauth-proxy ve bir Alertmanager örneği, aynı zamanda oauth-proxy ile güvenli bir Prometheus örnek oluşturacaktır. Bu şablonda, oauth-proxy prometheus-project ad alanını "alabilen" herhangi bir `-openshift-sar` kullanıcıya izin verecek şekilde yapılandırılır (bkz.

> [!NOTE]
> Prom StatefulSet eşit ISTENEN ve GÜNCEL sayı yinelemeleri `oc get statefulset -n prometheus-project` olup olmadığını doğrulamak için komutu çalıştırın. Projedeki tüm kaynakları denetlemek için `oc get all -n prometheus-project` komutu çalıştırın.

## <a name="add-permissions-to-allow-service-discovery"></a>Hizmet keşfine izin vermek için izin ekleme

Aşağıdaki içeriği girerek bir prometheus-sdrole.yml dosyası oluşturun:
```
apiVersion: template.openshift.io/v1
kind: Template
metadata:
  name: prometheus-sdrole
  annotations:
    "openshift.io/display-name": Prometheus service discovery role
    description: |
      Role and rolebinding added permissions required for service discovery in a given project.
    iconClass: fa fa-cogs
    tags: "monitoring,prometheus,alertmanager,time-series"
parameters:
- description: The project name, where a standalone Prometheus is deployed
  name: PROMETHEUS_PROJECT
  value: prometheus-project
objects:
- apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: prometheus-sd
  rules:
  - apiGroups:
    - ""
    resources:
    - services
    - endpoints
    - pods
    verbs:
    - list
    - get
    - watch
- apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: prometheus-sd
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: Role
    name: prometheus-sd
  subjects:
  - kind: ServiceAccount
    name: prom
    namespace: ${PROMETHEUS_PROJECT}
```
Şablonu hizmet keşfine izin vermek istediğiniz tüm projelere uygulamak için aşağıdaki komutları çalıştırın:
```
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project1
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project2
oc process -f prometheus-sdrole.yml | oc apply -f - -n prometheus-project
```

> [!NOTE]
> Role ve RoleBinding'in doğru oluşturulduğunu `oc get role` doğrulamak `oc get rolebinding` için, ve komutları çalıştırın.

## <a name="optional-deploy-example-application"></a>İsteğe bağlı: Örnek uygulamayı dağıt

Her şey çalışıyor, ama hiçbir ölçüm kaynakları vardır. Prometheus URL'sinehttps://prom-prometheus-project.appsgidin ( .* rastgele kimlik*. *bölge*.azmosa.io/). Aşağıdaki komutu kullanarak bulabilirsiniz:

```
oc get route prom -n prometheus-project
```
> [!IMPORTANT]
> Ana bilgisayar adının başına https:// öneki eklemeyi unutmayın.

**Durum > Hizmeti Bulma** sayfası 0/0 etkin hedefleri gösterir.

/metrics bitiş noktası altında temel Python ölçümlerini ortaya çıkaran bir örnek uygulama dağıtmak için aşağıdaki komutları çalıştırın:
```
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example1 -n app-project1

oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example2 -n app-project2
```
Yeni uygulamalar dağıtımdan sonraki 30 saniye içinde Hizmet Bulma sayfasında geçerli hedef olarak görünmelidir.

Daha fazla ayrıntı için **Durum** > **Hedefleri'ni**seçin.

> [!NOTE]
> Prometheus, başarıyla kazınmış her hedef için yukarı metrikte bir veri noktası ekler. Sol üst köşede **Prometheus'u** seçin, ifade olarak **yukarı** girin ve sonra **Yürüt'ü**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalarınıza özel Prometheus enstrümantasyonu ekleyebilirsiniz. Prometheus ölçümleri hazırlamayı kolaylaştıran Prometheus İstemci kitaplığı farklı programlama dilleri için hazırdır.

Daha fazla bilgi için aşağıdaki GitHub kitaplıklarını görün:

 - [Java](https://github.com/prometheus/client_java)
 - [Python](https://github.com/prometheus/client_python)
 - [Git](https://github.com/prometheus/client_golang)
 - [Ruby](https://github.com/prometheus/client_ruby)
