---
title: (AmortismanA Uğradı) Datadog ile Azure Kubernetes kümesini izleyin
description: Datadog'u kullanarak Azure Kapsayıcı Hizmeti'nde Kubernetes kümesini izleme
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 1f3f70c30ab397bd549a2f3305a738274ee4f64f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371180"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-datadog"></a>(AmortismanA Uğradı) DataDog ile Azure Kapsayıcı Hizmeti kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu izlenecek yol, [Azure Kapsayıcı Hizmeti'ni kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.

Ayrıca, Azure cli'sinin ve `az` `kubectl` araçlarının yüklü olduğunu varsayar.

Aracı çalıştırarak yüklü `az` yorurarak test edebilirsiniz:

```azurecli
az --version
```

`az` Aracı yüklü değilseniz, [burada](https://github.com/azure/azure-cli#installation)talimatlar vardır.

Aracı çalıştırarak yüklü `kubectl` yorurarak test edebilirsiniz:

```console
kubectl version
```

`kubectl` Yüklemediyseniz çalıştırabilirsiniz:

```azurecli
az acs kubernetes install-cli
```

## <a name="datadog"></a>Veri Köpeği
Datadog, Azure Kapsayıcı Hizmeti kümenizdeki kapsayıcılarınızdan izleme verilerini toplayan bir izleme hizmetidir. Datadog'da, kapsayıcılarınızda belirli ölçümleri görebileceğiniz bir Docker Entegrasyon Panosu vardır. Kapsayıcılarınızdan toplanan ölçümler CPU, Memory, Network ve I/O tarafından düzenlenir. Datadog ölçümleri kapsayıcılara ve görüntülere böler.

Öncelikle bir [hesap oluşturmanız](https://www.datadoghq.com/lpg/) gerekir

## <a name="installing-the-datadog-agent-with-a-daemonset"></a>Datadog Aracısını DaemonSet ile yükleme
DaemonSets kümedeki her ana bilgisayarda bir kapsayıcının tek bir örneğini çalıştırmak için Kubernetes tarafından kullanılır.
İzleme ajanlarını çalıştırmak için mükemmeller.

Datadog'a giriş yaptıktan sonra, DaemonSet kullanarak Cluster'ınıza Datadog aracılarını yüklemek için [Datadog yönergelerini](https://app.datadoghq.com/account/settings#agent/kubernetes) izleyebilirsiniz.

## <a name="conclusion"></a>Sonuç
İşte bu kadar! Aracılar çalışmaya başladıktan sonra birkaç dakika içinde konsoldaki verileri görmeniz gerekir. Kümenizin bir özetini görmek için tümleşik [kubernetes panosunu](https://app.datadoghq.com/screen/integration/kubernetes) ziyaret edebilirsiniz.
