---
title: Kullanım DıŞı Dataköpek ile Azure Kubernetes kümesini izleme
description: Dataköpek kullanarak Azure Container Service Kubernetes kümesini izleme
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: c8ed146a224ec4225a7a0e85c76227fb1dc71b0b
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76271044"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-datadog"></a>Kullanım DıŞı Dataköpek ile Azure Container Service kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu izlenecek yol, [Azure Container Service kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.

Ayrıca, Azure CLI `az` ve `kubectl` araçlarının yüklü olduğunu varsaymaktadır.

Şunu çalıştırarak `az` aracı yüklüyse test edebilirsiniz:

```console
$ az --version
```

`az` aracı yüklü değilse, [burada](https://github.com/azure/azure-cli#installation)yönergeler vardır.

Şunu çalıştırarak `kubectl` aracı yüklüyse test edebilirsiniz:

```console
$ kubectl version
```

Yüklü `kubectl` yoksa şu şekilde çalıştırabilirsiniz:

```console
$ az acs kubernetes install-cli
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
