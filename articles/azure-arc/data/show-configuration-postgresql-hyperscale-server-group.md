---
title: Bir yay etkin PostgreSQL hiper ölçek sunucu grubu yapılandırmasını göster
titleSuffix: Azure Arc enabled data services
description: Bir yay etkin PostgreSQL hiper ölçek sunucu grubu yapılandırmasını göster
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: ceab9af7e6556b2d957fafce8cd89d4a0daf9508
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90942139"
---
# <a name="show-the-configuration-of-an-arc-enabled-postgresql-hyperscale-server-group"></a>Bir yay etkin PostgreSQL hiper ölçek sunucu grubu yapılandırmasını göster

Bu makalede, sunucu gruplarınızın yapılandırmasının nasıl görüntüleneceği açıklanır. Bu, size sorabileceğiniz soruları benimsemeyi bekleme ve size yanıt verebiliriz. Her zaman birkaç geçerli yanıt olabilir. Bu makale, en yaygın veya faydalı olanları içerir. Bu soruları temaya göre gruplandırır:

- bir Kubernetes görünümünden
- Azure Arc etkin bir veri Hizmetleri görünümünden görüntüleme noktası

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="from-a-kubernetes-point-of-view"></a>Bir Kubernetes görünümünden

### <a name="how-many-pods-are-used-by-azure-arc-enabled-postgresql-hyperscale"></a>Azure Arc etkin PostgreSQL hiper ölçek tarafından kaç tane sayıda öğe kullanılıyor?

Postgres türündeki Kubernetes kaynaklarını listeleyin. Şu komutu çalıştırın:

```console
kubectl get postgresqls [-n <namespace name>]
```

Bu komutun çıktısı oluşturulan sunucu gruplarının listesini gösterir. Her biri için, pods sayısını belirtir. Örnek:

```output
NAME                                             STATE   READY-PODS   EXTERNAL-ENDPOINT   AGE
postgresql-12.arcdata.microsoft.com/postgres01   Ready   3/3          10.0.0.4:30499      6h34m
postgresql-12.arcdata.microsoft.com/postgres02   Ready   3/3          10.0.0.4:31066      6d7h
```

Bu örnek, 2 sunucu grubunun oluşturulduğunu ve her birinin 3 Pod 'de (1 düzenleyici + 2 çalışan) çalıştığını gösterir. Diğer bir deyişle, bu Azure Arc veri denetleyicisinde oluşturulan sunucu gruplarının 6 pods kullanması anlamına gelir.

### <a name="what-pods-are-used-by-azure-arc-enabled-postgresql-hyperscale-server-groups"></a>Azure Arc etkin PostgreSQL hiper ölçek sunucu grupları tarafından kullanılan Pod 'ler nelerdir?

Çalıştır:

```console
kubectl get pods [-n <namespace name>]
```

Bu, pods listesini döndürür. Bu sunucu gruplarına verdiğiniz adlara göre, sunucu gruplarınız tarafından kullanılan Pod 'leri görürsünüz. Örnek:

```console 
NAME                 READY   STATUS    RESTARTS   AGE
bootstrapper-vdltm   1/1     Running   0          6d8h
control-h6kc9        2/2     Running   0          6d8h
controldb-0          2/2     Running   0          6d8h
controlwd-96sbn      1/1     Running   0          6d8h
logsdb-0             1/1     Running   0          6d8h
logsui-7wkg2         1/1     Running   0          6d8h
metricsdb-0          1/1     Running   0          6d8h
metricsdc-28ffl      1/1     Running   0          6d8h
metricsui-k7qsh      1/1     Running   0          6d8h
mgmtproxy-gd84z      2/2     Running   0          6d8h
postgres01-0         3/3     Running   0          6h50m
postgres01-1         3/3     Running   0          6h50m
postgres01-2         3/3     Running   0          6h50m
postgres02-0         3/3     Running   0          22h
postgres02-1         3/3     Running   0          22h
postgres02-2         3/3     Running   0          22h
```

Bu örnekte, oluşturulan iki sunucu grubunu barındıran altı Pod şunlardır:
- `postgres01-0`
- `postgres01-1`
- `postgres01-2`
- `postgres02-0`
- `postgres02-1`
- `postgres02-2`  

### <a name="what-server-group-pod-is-used-for-what-role-the-server-group"></a>Sunucu grubu hangi rolü rol için kullanılır?

İle düzeltilen Pod adı, `-0` bir düzenleyici düğümünü temsil eder. `-x`1 veya daha büyük bir çalışan düğümü tarafından düzeltilen düğüm adı. Yukarıdaki örnekte:
- Koordinatörler: `postgres01-0` , `postgres02-0`
- Çalışanlar: `postgres01-2` , `postgres01-2` , `postgres02-1` , `postgres02-2`

### <a name="what-is-the-status-of-the-pods"></a>Pods 'nin durumu nedir?

Öğesini çalıştırın `kubectl get pods` ve sütununa bakın `STATUS`

### <a name="what-persistent-volume-claims-pvcs-are-being-used"></a>Hangi kalıcı birim talepleri (PVC) kullanılıyor? 

Hangi PVC 'Lerin kullanıldığını ve verilerin, günlüklerin ve yedeklemelerin ne şekilde kullanıldığını anlamak için şunu çalıştırın: 

```console
kubectl get pvc [-n <namespace name>]
```

Varsayılan olarak, bir PVC adının öneki kullanımını gösterir:

- `backups-`...: yedeklemeler için kullanılan bir PVC
- `data-`...: veri dosyaları için kullanılan PVC
- `logs-`...: işlem günlükleri/WAL dosyaları için kullanılan bir PVC

Örnek:

```output
NAME                   STATUS   VOLUME              CAPACITY   ACCESS MODES   STORAGECLASS    AGE
backups-postgres01-0   Bound    local-pv-485e37db   1938Gi     RWO            local-storage   6d6h
backups-postgres01-1   Bound    local-pv-9d3d4a15   1938Gi     RWO            local-storage   6d6h
backups-postgres01-2   Bound    local-pv-7b8dd819   1938Gi     RWO            local-storage   6d6h
...
data-postgres01-0      Bound    local-pv-3c1a8cc5   1938Gi     RWO            local-storage   6d6h
data-postgres01-1      Bound    local-pv-8303ab19   1938Gi     RWO            local-storage   6d6h
data-postgres01-2      Bound    local-pv-55572fe6   1938Gi     RWO            local-storage   6d6h
...
logs-postgres01-0      Bound    local-pv-5e852b76   1938Gi     RWO            local-storage   6d6h
logs-postgres01-1      Bound    local-pv-55d309a7   1938Gi     RWO            local-storage   6d6h
logs-postgres01-2      Bound    local-pv-5ccd02e6   1938Gi     RWO            local-storage   6d6h
...
```


## <a name="from-an-azure-arc-enabled-data-services-point-of-view"></a>Azure Arc etkin bir veri Hizmetleri görünümünden görüntüleme noktası:

* Bir yay veri denetleyicisinde kaç sunucu grubu oluşturulur?
* Adları nelerdir?
* Kaç çalışan düğümü kullanılıyor?
* Hangi Postgres sürümü çalıştırılır?

Aşağıdaki komutlardan birini kullanın.
- **Kubectl ile:**

   ```console
   kubectl get postgresqls [-n <namespace name>]
   ``` 

   Örneğin, her satırın bir olduğu aşağıdaki çıktıyı üretir `servergroup` . Aşağıdaki görüntü içindeki adın yapısı şöyle biçimlendirilir:

   `<Name-Of-Custom-Resource-Definition>`/`<Server-Group-Name>`

   ```output
   NAME                                             STATE   READY-PODS   EXTERNAL-ENDPOINT   AGE
   postgresql-12.arcdata.microsoft.com/postgres01   Ready   3/3          10.0.0.4:30499      7h15m
   postgresql-12.arcdata.microsoft.com/postgres02   Ready   3/3          10.0.0.4:31066      6d7h
   ```

   Yukarıdaki çıktıda, Postgres sürüm 12 ' nin 2 sunucu grubu gösterilmektedir. Sürüm Postgres 11 ise, bunun yerine CRD 'nin adı postgresql-11.arcdata.microsoft.com olur.

   Her biri 3 düğümde/pods 'de çalışır: 1 düzenleyici ve 2 çalışan.

- **Azdata ile:**

Aşağıdaki komutu çalıştırın. Çıktı, kubectl 'nin gösterdiği şuna benzer bilgiler gösterir:

   ```console
   azdata arc postgres server list

   `output
   Name        State    Workers
   ----------  -------  ---------
   postgres01  Ready    2
   postgres02  Ready    2
   ```


### <a name="how-much-memory-and-vcores-are-being-used-and-what-extensions-were-created-for-a-group"></a>Ne kadar bellek ve sanal çekirdek kullanılıyor, bir grup için hangi uzantılar oluşturuldu?

Aşağıdaki komutlardan birini çalıştırın

**Kubectl ile:**

Kubectl 'yi kullanarak bir Postgres kaynaklarını tanıtın. Bunu yapmak için, türü (yukarıda gösterildiği gibi karşılık gelen Postgres sürümü için Kubernetes kaynağı (CRD) adına ve sunucu grubunun adına ihtiyacınız vardır.
Bu komutun genel biçimi:

```console
kubectl describe <CRD name>/<server group name> [-n <namespace name>]
```

Örnek:

```console
kubectl describe postgresql-12/postgres02
```

Bu komutlar sunucu grubunun yapılandırmasını gösterir:

```output
Name:         postgres02
Namespace:    arc
Labels:       <none>
Annotations:  <none>
API Version:  arcdata.microsoft.com/v1alpha1
Kind:         postgresql-12
Metadata:
  Creation Timestamp:  2020-08-31T21:01:07Z
  Generation:          10
  Resource Version:    569516
  Self Link:           /apis/arcdata.microsoft.com/v1alpha1/namespaces/arc/postgresql-12s/postgres02
  UID:                 8a9cd118-361b-4a2e-8a9d-5f9257bf6abb
Spec:
  Backups:
    Delta Minutes:  3
    Full Minutes:   10
    Tiers:
      Retention:
        Maximums:
          6
          512MB
        Minimums:
          3
      Storage:
        Volume Size:  1Gi
  Engine:
    Extensions:
      Name:  citus
      Name:  pg_stat_statements
  Scale:
    Shards:  2
  Scheduling:
    Default:
      Resources:
        Limits:
          Cpu:     4
          Memory:  1024Mi
        Requests:
          Cpu:     1
          Memory:  512Mi
  Service:
    Type:  NodePort
  Storage:
    Data:
      Class Name:  local-storage
      Size:        5Gi
    Logs:
      Class Name:  local-storage
      Size:        5Gi
Status:
  External Endpoint:  10.0.0.4:31066
  Ready Pods:         3/3
  State:              Ready
Events:               <none>
```

Yukarıda gösterilen ' ın açıklamasında bazı belirli noktaları arayalım `servergroup` . Bu sunucu grubu hakkında bize ne söylüyorsunuz?

- Bu, Postgres 'nin 12. sürümüdür: 
   > Denetlenmesi         `postgresql-12`
- Ağustos 2020 ayı sırasında oluşturulmuştur:
   > Oluşturma zaman damgası:  `2020-08-31T21:01:07Z`
- Bu sunucu grubunda iki Postgres uzantısı oluşturuldu: `citus` ve `pg_stat_statements`
   > Motor: Uzantılar: ad:  `citus` ad:  `pg_stat_statements`
- İki çalışan düğümü kullanır
   > Ölçek: parçalar:  `2`
- Düğüm başına 1 CPU/sanal çekirdek ve 512MB RAM kullanılması garanti edilir. 4 ' ten fazla CPU/sanal çekirdek ve 10 24 MB bellek kullanır:
   > Zamanlama: varsayılan: kaynaklar: sınırlar: CPU: 4 bellek: 1024Mı Istekleri: CPU: 1 bellek: 512Mı
 - Sorgular için kullanılabilir ve herhangi bir sorun yoktur. Tüm düğümler çalışır duruma sahiptir:
   > Durum:... Ready pods: 3/3 durum: Ready

**Azdata ile:**

Komutun genel biçimi:

```console
azdata arc postgres server show -n <server group name>
```

Örnek:

```console
azdata arc postgres server show -n postgres02
```

Aşağıdaki çıktıyı, kubectl tarafından döndürülebilecek bir biçimde ve içeriğe benzer bir biçimde döndürür.

```output
{
  "apiVersion": "arcdata.microsoft.com/v1alpha1",
  "kind": "postgresql-12",
  "metadata": {
    "creationTimestamp": "2020-08-31T21:01:07Z",
    "generation": 10,
    "name": "postgres02",
    "namespace": "arc",
    "resourceVersion": "569516",
    "selfLink": "/apis/arcdata.microsoft.com/v1alpha1/namespaces/arc/postgresql-12s/postgres02",
    "uid": "8a9cd118-361b-4a2e-8a9d-5f9257bf6abb"
  },
  "spec": {
    "backups": {
      "deltaMinutes": 3,
      "fullMinutes": 10,
      "tiers": [
        {
          "retention": {
            "maximums": [
              "6",
              "512MB"
            ],
            "minimums": [
              "3"
            ]
          },
          "storage": {
            "volumeSize": "1Gi"
          }
        }
      ]
    },
    "engine": {
      "extensions": [
        {
          "name": "citus"
        },
        {
          "name": "pg_stat_statements"
        }
      ]
    },
    "scale": {
      "shards": 2
    },
    "scheduling": {
      "default": {
        "resources": {
          "limits": {
            "cpu": "4",
            "memory": "1024Mi"
          },
          "requests": {
            "cpu": "1",
            "memory": "512Mi"
          }
        }
      }
    },
    "service": {
      "type": "NodePort"
    },
    "storage": {
      "data": {
        "className": "local-storage",
        "size": "5Gi"
      },
      "logs": {
        "className": "local-storage",
        "size": "5Gi"
      }
    }
  },
  "status": {
    "externalEndpoint": "10.0.0.4:31066",
    "readyPods": "3/3",
    "state": "Ready"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Arc etkin PostgreSQL hiper ölçeğinde kavramlar hakkında bilgi edinin](concepts-distributed-postgres-hyperscale.md)
- [Bir sunucu grubunun ölçeğini genişletme (çalışan düğümleri ekleme) hakkında bilgi edinin](scale-out-postgresql-hyperscale-server-group.md)
- [Bir sunucu grubundaki ölçeği artırma/azaltma (belleği ve/veya sanal çekirdekleri artırma veya azaltma) hakkında bilgi edinin](scale-up-down-postgresql-hyperscale-server-group-using-cli.md)
- [Depolama yapılandırması hakkında bilgi edinin](storage-configuration.md)
- [Bir veritabanı örneğini nasıl izleyeceğinizi okuyun](monitor-grafana-kibana.md)
- [Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunda PostgreSQL uzantılarını kullanma](using-extensions-in-postgresql-hyperscale-server-group.md)
- [Azure Arc etkin PostgreSQL hiper ölçek sunucu grubu için güvenliği yapılandırma](configure-security-postgres-hyperscale.md)
