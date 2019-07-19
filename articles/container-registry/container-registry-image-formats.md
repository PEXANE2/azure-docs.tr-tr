---
title: Azure Container Registry içerik biçimleri
description: Azure Container Registry desteklenen içerik biçimleri hakkında bilgi edinin.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 04/18/2019
ms.author: danlep
ms.openlocfilehash: c804dd7b73ea4a51c02c8b342a4ac60d992ec7c5
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310686"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Azure Container Registry desteklenen içerik biçimleri

Aşağıdaki içerik biçimlerinden birini yönetmek için Azure Container Registry bir özel depo kullanın. 

## <a name="docker-compatible-container-images"></a>Docker ile uyumlu kapsayıcı görüntüleri

Aşağıdaki Docker kapsayıcı görüntü biçimleri desteklenir:

* [Docker görüntü bildirimi v2, şema 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker Image manifest v2, şema 2](https://docs.docker.com/registry/spec/manifest-v2-2/) -kayıt defterlerinin çoklu platform görüntülerini tek bir "Image: Tag" başvurusu altında depolamasına Izin veren bildirim listeleri içerir

## <a name="oci-images"></a>OCı görüntüleri

Azure Container Registry, [Açık kapsayıcı girişimi (OCI) görüntü biçimi belirtimini](https://github.com/opencontainers/image-spec/blob/master/spec.md)karşılayan görüntüleri de destekler. Paketleme biçimleri, [Singularımage Format (SIF)](https://www.sylabs.io/2018/03/sif-containing-your-containers/)içerir.

## <a name="helm-charts"></a>Helb grafikleri

Azure Container Registry, Kubernetes için uygulamaları hızlı bir şekilde yönetmek ve dağıtmak üzere kullanılan bir paketleme biçimi olan [helk grafikleri](https://helm.sh/)için depolar barındırabilir. [Held Client](https://docs.helm.sh/using_helm/#installing-helm) Version 2.11.0 veya üzeri desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Container Registry görüntüleri [gönderme ve çekme](container-registry-get-started-docker-cli.md) hakkında bilgi edinme.

* Kapsayıcı görüntülerini derlemek ve test etmek için [ACR görevlerini](container-registry-tasks-overview.md) kullanın. 

* Kapsayıcıları OCı biçiminde derlemek ve paketlemek için [Moby BuildKit](https://github.com/moby/buildkit) ' i kullanın.

* Azure Container Registry barındırılan bir [Held deposu](container-registry-helm-repos.md) ayarlayın. 


