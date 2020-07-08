---
title: Kullanım DıŞı Dataköpek ile Azure Kubernetes kümesini izleme
description: Dataköpek kullanarak Azure Container Service Kubernetes kümesini izleme
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 1f3f70c30ab397bd549a2f3305a738274ee4f64f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79371180"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-datadog"></a>Kullanım DıŞı Dataköpek ile Azure Container Service kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu izlenecek yol, [Azure Container Service kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.

Ayrıca, `az` Azure CLI ve `kubectl` araçları yüklü olduğunu varsayar.

' İ `az` çalıştırarak aracının yüklenip yüklenmediğini test edebilirsiniz:

```azurecli
az --version
```

`az`Araç yüklü değilse, [burada](https://github.com/azure/azure-cli#installation)yönergeler vardır.

' İ `kubectl` çalıştırarak aracının yüklenip yüklenmediğini test edebilirsiniz:

```console
kubectl version
```

`kubectl`Yüklü değilse şunu çalıştırabilirsiniz:

```azurecli
az acs kubernetes install-cli
```

## <a name="datadog"></a>DataDog
Dataköpek, Azure Container Service kümenizin içindeki kapsayıcılarınızdaki izleme verilerini toplayan bir izleme hizmetidir. Dataköpek, kapsayıcılarınızın içindeki belirli ölçümleri görebileceğiniz bir Docker tümleştirme panosuna sahiptir. Kapsayıcılardan toplanan ölçümler CPU, bellek, ağ ve g/ç tarafından düzenlenir. Dataköpek ölçümleri kapsayıcılara ve görüntülere böler.

Önce [bir hesap oluşturmanız](https://www.datadoghq.com/lpg/) gerekir

## <a name="installing-the-datadog-agent-with-a-daemonset"></a>DaemonSet ile Dataköpek aracısını yükleme
DaemonSets, Kubernetes tarafından kümedeki her konakta bir kapsayıcının tek bir örneğini çalıştırmak için kullanılır.
İzleme aracılarını çalıştırmak için idealdir.

Veri köbir kez oturum açtıktan sonra, DaemonSet kullanarak dataköpek aracılarını kümenize yüklemek için [dataköpek talimatlarını](https://app.datadoghq.com/account/settings#agent/kubernetes) izleyebilirsiniz.

## <a name="conclusion"></a>Sonuç
İşte bu kadar! Aracılar çalışır duruma getirildikten sonra, birkaç dakika içinde konsolundaki verileri görmeniz gerekir. Kümenizin özetini görmek için tümleşik [Kubernetes panosunu](https://app.datadoghq.com/screen/integration/kubernetes) ziyaret edebilirsiniz.
