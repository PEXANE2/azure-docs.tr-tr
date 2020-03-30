---
title: Desteklenen içerik biçimleri
description: Docker uyumlu kapsayıcı resimleri, Miğfer grafikleri, OCI görüntüleri ve OCI yapıları da dahil olmak üzere Azure Kapsayıcı Kayıt Defteri tarafından desteklenen içerik biçimleri hakkında bilgi edinin.
ms.topic: article
ms.date: 08/30/2019
ms.openlocfilehash: ab915385f46f83c7b655acd1a48d66df84b50653
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247013"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Azure Kapsayıcı Kayıt Defteri'nde desteklenen içerik biçimleri

Aşağıdaki içerik biçimlerinden birini yönetmek için Azure Kapsayıcı Kayıt Defteri'nde özel bir depo kullanın. 

## <a name="docker-compatible-container-images"></a>Docker uyumlu konteyner görüntüleri

Aşağıdaki Docker kapsayıcı görüntü biçimleri desteklenir:

* [Docker Görüntü Manifestosu V2, Şema 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker Image Manifest V2, Schema 2](https://docs.docker.com/registry/spec/manifest-v2-2/) - kayıt defterlerinin tek bir "resim:etiket" referansı altında çok platformlu görüntüleri depolamasına olanak tanıyan Manifesto Listeleri içerir

## <a name="oci-images"></a>OCI görüntüleri

Azure Kapsayıcı Kayıt [Defteri, Açık Kapsayıcı Girişimi (OCI) Görüntü Biçimi Belirtimini](https://github.com/opencontainers/image-spec/blob/master/spec.md)karşılayan görüntüleri destekler. Ambalaj biçimleri [Tekillik Görüntü Biçimi (SIF)](https://github.com/sylabs/sif)içerir.

## <a name="oci-artifacts"></a>OCI eserler

Azure Kapsayıcı Kayıt Defteri, kapsayıcı görüntülerini ve diğer içerik türlerini (yapıtları) depolamak, paylaşmak, güvenli hale getirmek ve dağıtmak için satıcıdan bağımsız, bulut agnostik bir speküle olan [OCI Dağıtım Belirtimini](https://github.com/opencontainers/distribution-spec)destekler. Belirtim, bir kayıt defterinin kapsayıcı görüntülerine ek olarak çok çeşitli yapıları depolamasına olanak tanır. Yapıyı itmek ve çekmek için yapıya uygun araç kullanırsınız. Örneğin, Azure [kapsayıcı kayıt defterini kullanarak Bir OCI artifakı it ve çekin'](container-registry-oci-artifacts.md)e bakın.

OCI yapıları hakkında daha fazla bilgi edinmek için, [OCI Kayıt Defteri'ni Depolama (ORAS) reposu olarak](https://github.com/deislabs/oras) ve GitHub'daki [OCI Artefakt](https://github.com/opencontainers/artifacts) reposu'na bakın.

## <a name="helm-charts"></a>Miğfer grafikleri

Azure Konteyner Kayıt Defteri, Kubernetes için uygulamaları hızla yönetmek ve dağıtmak için kullanılan bir ambalaj biçimi olan [Miğfer grafikleri](https://helm.sh/)için depoları barındırabilir. [Helm istemci](https://docs.helm.sh/using_helm/#installing-helm) sürümü 2 (2.11.0 veya sonrası) desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Kapsayıcı Kayıt Defteri ile görüntüleri nasıl [itip çekeceksiniz.](container-registry-get-started-docker-cli.md)

* Kapsayıcı görüntüleri oluşturmak ve test etmek için [ACR görevlerini](container-registry-tasks-overview.md) kullanın. 

* Moby [BuildKit'i](https://github.com/moby/buildkit) kullanarak oci formatında kapsayıcılar oluşturun ve paketlendi.

* Azure Kapsayıcı Kayıt Defteri'nde barındırılan bir [Miğfer deposu](container-registry-helm-repos.md) ayarlayın. 


