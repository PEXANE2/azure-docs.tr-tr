---
title: Azure Arc üzerinde Azure SQL yönetilen örneği oluşturma
description: Azure Arc üzerinde Azure SQL yönetilen örneği oluşturma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: vin-yu
ms.author: vinsonyu
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 0d1d6c7349adc65270e13d09d771963cb9cacea3
ms.sourcegitcommit: 58f12c358a1358aa363ec1792f97dae4ac96cc4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93280413"
---
# <a name="create-an-azure-sql-managed-instance-on-azure-arc"></a>Azure Arc üzerinde Azure SQL yönetilen örneği oluşturma

[!INCLUDE [azure-arc-common-prerequisites](../../../includes/azure-arc-common-prerequisites.md)]

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="login-to-the-azure-arc-data-controller"></a>Azure Arc veri denetleyicisinde oturum açma

Bir örnek oluşturabilmeniz için önce henüz oturum açmadıysanız Azure Arc veri denetleyicisinde oturum açın.

```console
azdata login
```

Ardından Kullanıcı adı, parola ve sistem ad alanı sorulur.  

```console
Username: arcadmin
Password:
Namespace: arc
Logged in successfully to `https://10.0.0.4:30080` in namespace `arc`. Setting active context to `arc`
```

## <a name="create-an-azure-sql-managed-instance"></a>Azure SQL Yönetilen Örnek oluşturma

Kullanılabilir oluşturma seçeneklerini görüntülemek için, aşağıdaki komutu kullanın:
```console
azdata arc sql mi create --help
```

SQL yönetilen örneği oluşturmak için aşağıdaki komutu kullanın:

```console
azdata arc sql mi create -n <instanceName> --storage-class-data <storage class> --storage-class-logs <storage class>
```

Örnek:

```console
azdata arc sql mi create -n sqldemo --storage-class-data managed-premium --storage-class-logs managed-premium
```
> [!NOTE]
>  Adların uzunluğu 13 karakterden az olmalı ve [DNS adlandırma kurallarına](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/#dns-label-names) uymalıdır
>
>  Bellek ayırmayı ve sanal çekirdek ayırmayı belirtirken, oluşturma işleminin başarılı olduğundan emin olmak için bu formülü kullanın-her 1 sanal çekirdek için, SQL yönetilen örneği Pod 'un çalışacağı Kubernetes düğümünde en az 4 GB kapasiteye ihtiyacınız vardır.
>
>  Azure'da sağlıyorsanız SQL örneği oluştururken adda büyük harf kullanmayın
>
>  Kubernetes küme çalıştırurdaki kullanılabilir depolama sınıflarını listelemek için `kubectl get storageclass` 


> [!NOTE]
> SQL örneklerinin oluşturulmasını otomatik hale getirmek ve yönetici parolası için etkileşimli istemden kaçınmak isterseniz, `AZDATA_USERNAME` `AZDATA_PASSWORD` komutunu çalıştırmadan önce, ve ortam değişkenlerini istenen Kullanıcı adı ve parola olarak ayarlayabilirsiniz `azdata arc sql mi create` .
> 
>  Veri denetleyicisini aynı Terminal oturumunda AZDATA_USERNAME ve AZDATA_PASSWORD kullanarak oluşturduysanız, SQL yönetilen örneği de oluşturmak için AZDATA_USERNAME ve AZDATA_PASSWORD değerleri kullanılacaktır.

> [!NOTE]
> Azure SQL yönetilen örneği oluşturma, kaynakları Azure 'a kaydetmez. Kaynağı kaydetme adımları aşağıdaki makalelerdir: 
> - [Kibana ve Grafana kullanarak günlükleri ve ölçümleri görüntüleme](monitor-grafana-kibana.md)
> - [Faturalama verilerini Azure 'a yükleyin ve Azure portal görüntüleyin](view-billing-data-in-azure.md) 


## <a name="view-instance-on-azure-arc"></a>Azure Arc 'da örneği görüntüle

Örneği görüntülemek için aşağıdaki komutu kullanın:

```console
azdata arc sql mi list
```

Çıktı şuna benzemelidir:

```console
Name    Replicas    ServerEndpoint    State
------  ----------  ----------------  -------
sqldemo 1/1         10.240.0.4:32023  Ready
```

AKS veya `kubeadm` veya OpenShift vb. kullanıyorsanız, buradan dış IP ve bağlantı noktası numarasını kopyalayabilir ve Azure Data Studio veya SQL Server Management Studio gibi BIR SQL Server/Azure SQL örneğine bağlanmak için en sevdiğiniz aracı kullanarak bağlanabilirsiniz. Ancak, hızlı başlangıç VM 'sini kullanıyorsanız özel yönergeler için [Azure Arc ETKIN SQL yönetilen örneği 'Ne bağlanma](connect-managed-instance.md) makalesine bakın.


## <a name="next-steps"></a>Sonraki adımlar
- [Azure Arc etkin SQL yönetilen örneğine bağlanma](connect-managed-instance.md)
- [Örneğinizi Azure ile kaydedin ve örneğiniz hakkında ölçümleri ve günlükleri karşıya yükleyin](upload-metrics-and-logs-to-azure-monitor.md)
- [Azure SQL yönetilen örneğini Azure Data Studio kullanarak dağıtma](create-sql-managed-instance-azure-data-studio.md)
