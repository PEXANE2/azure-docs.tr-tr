---
title: Kibana ve Grafana kullanarak günlükleri ve ölçümleri görüntüleme
description: Kibana ve Grafana kullanarak günlükleri ve ölçümleri görüntüleme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: d876862d8f41ab8df646bef051629fd45c4d4601
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941758"
---
# <a name="view-logs-and-metrics-using-kibana-and-grafana"></a>Kibana ve Grafana kullanarak günlükleri ve ölçümleri görüntüleme

Kibana ve Grafana Web panoları, Azure Arc etkin veri Hizmetleri tarafından kullanılan Kubernetes ad alanlarına öngörü ve açıklık getirmek için sağlanır.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="retrieve-the-ip-address-of-your-cluster"></a>Kümenizin IP adresini alma

Panolara erişmek için, kümenizin IP adresini almanız gerekir. Doğru IP adresini alma yöntemi, Kubernetes 'i dağıtmayı nasıl seçtiğinize bağlı olarak değişir. Sizin için doğru olanı bulmak için aşağıdaki seçeneklerde adım adım ilerleyin.

### <a name="azure-virtual-machine"></a>Azure sanal makine

Genel IP adresini almak için aşağıdaki komutu kullanın:

```console
az network public-ip list -g azurearcvm-rg --query "[].{PublicIP:ipAddress}" -o table
```

### <a name="kubeadm-cluster"></a>Kubeadm kümesi

Kümenin IP adresini almak için aşağıdaki komutu kullanın:

```console
kubectl cluster-info
```


### <a name="aks-or-other-load-balanced-cluster"></a>AKS veya diğer yük dengeli küme

Ortamınızı AKS veya diğer yük dengeli bir kümede izlemek için yönetim Proxy hizmetinin IP adresini almanız gerekir. **Dış IP** adresini almak için bu komutu kullanın:

```console
kubectl get svc mgmtproxy-svc-external -n <namespace>

#Example:
#kubectl get svc mgmtproxy-svc-external -n arc
NAME                     TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)           AGE
mgmtproxy-svc-external   LoadBalancer   10.0.186.28   52.152.148.25   30777:30849/TCP   19h
```

## <a name="additional-firewall-configuration"></a>Ek güvenlik duvarı yapılandırması

Kibana ve Grafana uç noktalarına erişmek için güvenlik duvarınızdaki bağlantı noktalarını açmanız gerektiğini fark edebilirsiniz.

Bunun bir Azure VM için nasıl yapılacağını gösteren bir örnek aşağıda verilmiştir. Betiği kullanarak Kubernetes dağıttıysanız bunu yapmanız gerekir.

Aşağıdaki adımlarda, kibana ve Grafana uç noktaları için NSG kuralının nasıl oluşturulacağı vurgulanacak:

### <a name="find-the-name-of-the-nsg"></a>NSG 'nin adını bulma

```console
az network nsg list -g azurearcvm-rg --query "[].{NSGName:name}" -o table
```

### <a name="add-the-nsg-rule"></a>NSG kuralını ekleme

NSG adına sahip olduktan sonra aşağıdaki komutu kullanarak bir kural ekleyebilirsiniz:

```console
az network nsg rule create -n ports_30777 --nsg-name azurearcvmNSG --priority 600 -g azurearcvm-rg --access Allow --description 'Allow Kibana and Grafana ports' --destination-address-prefixes '*' --destination-port-ranges 30777 --direction Inbound --protocol Tcp --source-address-prefixes '*' --source-port-ranges '*'
```

## <a name="monitor-azure-sql-managed-instances-on-azure-arc"></a>Azure Arc 'da Azure SQL yönetilen örneklerini izleme

IP adresine sahip olduğunuza ve bağlantı noktalarını kullanıma açdığınıza göre Grafana ve kibana 'e erişmeniz gerekir.

> [!NOTE]
>  Bir Kullanıcı adı ve parola girmeniz istendiğinde, Azure Arc veri denetleyicisi 'ni oluşturduğunuz sırada verdiğiniz kullanıcı adını ve parolayı girin.

> [!NOTE]
>  Önizlemede kullanılan sertifikalar otomatik olarak imzalanan sertifikalar olduğundan, sizden bir sertifika uyarısıyla ilgili olarak uyarılırsınız.

Azure SQL yönetilen örneği için günlüğe kaydetme ve izleme panolarına erişmek üzere aşağıdaki URL modelini kullanın:

```html
https://<external-ip-from-above>:30777/grafana
https://<external-ip-from-above>:30777/kibana
```

İlgili panolar şunlardır:

* "Azure SQL yönetilen örnek ölçümleri"
* "Ana düğüm ölçümleri"
* "Konak pods ölçümleri"

## <a name="monitor-azure-database-for-postgresql-hyperscale---azure-arc"></a>PostgreSQL için Azure veritabanı hiper ölçeğini izleme-Azure Arc

PostgreSQL hiper ölçeğinde günlüğe kaydetme ve izleme panolarına erişmek için aşağıdaki URL düzenini kullanın:

```html
https://<external-ip-from-above>:30777/grafana
https://<external-ip-from-above>:30777/kibana
```

İlgili panolar şunlardır:

* "Postgres ölçümleri"
* "Postgres tablo ölçümleri"
* "Ana düğüm ölçümleri"
* "Konak pods ölçümleri"

## <a name="next-steps"></a>Sonraki adımlar
- [Ölçümleri ve günlükleri Azure izleyici 'ye yüklemeyi](upload-metrics-and-logs-to-azure-monitor.md) deneyin
- Grafana hakkında bilgi edinin:
   - [Başlarken](https://grafana.com/docs/grafana/latest/getting-started/getting-started)
   - [Grafana temelleri](https://grafana.com/tutorials/grafana-fundamentals/#1)
   - [Grafana öğreticileri](https://grafana.com/tutorials/grafana-fundamentals/#1)
- Kibana hakkında bilgi edinin
   - [Giriş](https://www.elastic.co/webinars/getting-started-kibana?baymax=default&elektra=docs&storm=top-video)
   - [Kibana Kılavuzu](https://www.elastic.co/guide/en/kibana/current/index.html)
   - [Kibana 'teki veri görselleştirmeleriyle Dashboard inmenize 'a giriş](https://www.elastic.co/webinars/dashboard-drilldowns-with-data-visualizations-in-kibana/)
   - [Kibana panoları oluşturma](https://www.elastic.co/webinars/how-to-build-kibana-dashboards/)

