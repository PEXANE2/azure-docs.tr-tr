---
title: Azure Arc etkin veri denetleyicisi sorunlarını gidermek için günlükleri al
description: Azure Arc etkin veri denetleyicisinde sorun gidermek için hizmet günlüklerini alın.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 71c84b35c001be7fafdc2df53014050ae21dec63
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941559"
---
# <a name="get-azure-arc-enabled-data-services-logs"></a>Azure Arc etkin veri Hizmetleri günlüklerini al

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Önkoşullar

Azure Arc etkin veri Hizmetleri günlüklerini almak için Azure Data CLI aracına ihtiyacınız olacaktır. [Yükleme yönergeleri](./install-client-tools.md)

Azure Arc etkin veri Hizmetleri denetleyicisi hizmetinde yönetici olarak oturum açabiliyor olmanız gerekir.

## <a name="get-azure-arc-enabled-data-services-logs"></a>Azure Arc etkin veri Hizmetleri günlüklerini al

Sorun giderme amacıyla tüm yığınların veya belirli yığınların tamamında Azure Arc 'ın etkin olduğu veri Hizmetleri günlüklerini alabilirsiniz.  Bunu, komutu gibi standart Kubernetes araçlarını kullanarak yapabilirsiniz. bu `kubectl logs` makalede, tüm günlüklerin aynı anda daha kolay bir şekilde alınacağını sağlayan Azure Data CLI aracı kullanılır.

İlk olarak, veri denetleyicisinde oturum açtığınızdan emin olun.

```console
azdata login
```

Ardından günlüklerin dökümünü almak için aşağıdaki komutu çalıştırın:
```console
azdata arc dc debug copy-logs --namespace <namespace name> --exclude-dumps --skip-compress

#Example:
#azdata arc dc debug copy-logs --namespace arc --exclude-dumps --skip-compress
```

Günlük dosyaları, varsayılan olarak ' logs ' adlı bir alt dizinde geçerli çalışma dizininde oluşturulur.  Parametresini kullanarak günlük dosyalarını farklı bir dizine aktarabilirsiniz `--target-folder` .

Parametresini atlayarak dosyaları sıkıştırmayı seçebilirsiniz `--skip-compress` .

' I atlayarak bellek dökümlerini tetikleyip dahil edebilirsiniz `--exclude-dumps` , ancak Microsoft desteği bellek dökümlerini istemediği takdirde bu önerilmez.  Bellek dökümü almak için veri denetleyicisi ayarının `allowDumps` `true` veri denetleyicisi oluşturma zamanına ayarlanmış olması gerekir.

İsteğe bağlı olarak yalnızca belirli bir pod ( `--pod` ) veya Container () için günlüklere ait günlükleri toplamaya yönelik filtreyi seçebilirsiniz `--container` .

Ayrıca, `--resource-kind` ve parater ' ı geçirerek belirli bir özel kaynak için günlükleri toplamaya yönelik filtreyi seçebilirsiniz `--resource-name` .  `resource-kind`Parametre değeri, komutu tarafından alınabilecek özel kaynak tanımı adlarından biri olmalıdır `kubectl get customresourcedefinition` .

```console
azdata arc dc debug copy-logs --target-folder <desired folder> --exclude-dumps --skip-compress -resource-kind <custom resource definition name> --resource-name <resource name> --namespace <namespace name>

#Example
#azdata arc dc debug copy-logs --target-folder C:\temp\logs --exclude-dumps --skip-compress --resource-kind postgresql-12 --resource-name pg1 --namespace arc
```

Klasör hiyerarşisi örneği.  Klasör hiyerarşisinin Pod ad adına ve sonra kapsayıcıya göre ve sonra kapsayıcı içindeki dizin hiyerarşisine göre düzenlendiğini unutmayın.

```console
<export directory>
├───debuglogs-arc-20200827-180403
│   ├───bootstrapper-vl8j2
│   │   └───bootstrapper
│   │       ├───apt
│   │       └───fsck
│   ├───control-j2dm5
│   │   ├───controller
│   │   │   └───controller
│   │   │       ├───2020-08-27
│   │   │       └───2020-08-28
│   │   └───fluentbit
│   │       ├───agent
│   │       ├───fluentbit
│   │       └───supervisor
│   │           └───log
│   ├───controldb-0
│   │   ├───fluentbit
│   │   │   ├───agent
│   │   │   ├───fluentbit
│   │   │   └───supervisor
│   │   │       └───log
│   │   └───mssql-server
│   │       ├───agent
│   │       ├───mssql
│   │       ├───mssql-server
│   │       └───supervisor
│   │           └───log
│   ├───controlwd-ln6j8
│   │   └───controlwatchdog
│   │       └───controlwatchdog
│   ├───logsdb-0
│   │   └───elasticsearch
│   │       ├───agent
│   │       ├───elasticsearch
│   │       ├───provisioner
│   │       └───supervisor
│   │           └───log
│   ├───logsui-7gg2d
│   │   └───kibana
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───kibana
│   │       └───supervisor
│   │           └───log
│   ├───metricsdb-0
│   │   └───influxdb
│   │       ├───agent
│   │       ├───influxdb
│   │       └───supervisor
│   │           └───log
│   ├───metricsdc-2f62t
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsdc-jznd2
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsdc-n5vnx
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsui-h748h
│   │   └───grafana
│   │       ├───agent
│   │       ├───grafana
│   │       └───supervisor
│   │           └───log
│   └───mgmtproxy-r5zxs
│       ├───fluentbit
│       │   ├───agent
│       │   ├───fluentbit
│       │   └───supervisor
│       │       └───log
│       └───service-proxy
│           ├───agent
│           ├───nginx
│           └───supervisor
│               └───log
└───debuglogs-kube-system-20200827-180431
    ├───coredns-8bbb65c89-kklt7
    │   └───coredns
    ├───coredns-8bbb65c89-z2vvr
    │   └───coredns
    ├───coredns-autoscaler-5585bf8c9f-g52nt
    │   └───autoscaler
    ├───kube-proxy-5c9s2
    │   └───kube-proxy
    ├───kube-proxy-h6x56
    │   └───kube-proxy
    ├───kube-proxy-nd2b7
    │   └───kube-proxy
    ├───metrics-server-5f54b8994-vpm5r
    │   └───metrics-server
    └───tunnelfront-db87f4cd8-5xwxv
        ├───tunnel-front
        │   ├───apt
        │   └───journal
        └───tunnel-probe
            ├───apt
            ├───journal
            └───openvpn
```