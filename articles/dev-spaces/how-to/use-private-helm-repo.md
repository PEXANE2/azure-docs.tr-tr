---
title: Azure Geliştirme Alanlarında özel Bir Miğfer deposu nasıl kullanılır?
services: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 08/22/2019
ms.topic: conceptual
description: Azure Geliştirme Alanında özel bir Miğfer deposu kullanın.
keywords: Docker, Kubernetes, Azure, AKS, Azure Konteyner Servisi, konteynerler, Dümen
manager: gwallace
ms.openlocfilehash: c8f0e463bc78d278d8162f8389664dbb46a83301
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240463"
---
# <a name="use-a-private-helm-repository-in-azure-dev-spaces"></a>Azure Geliştirme Alanlarında özel Bir Miğfer deposu kullanma

[Helm,][helm] Kubernetes'in paket yöneticisi. [Miğfer,][helm-chart] bağımlılıkları paketlemek için bir grafik biçimi kullanır. Miğfer grafikleri, herkese açık veya özel olabilecek bir depoda saklanır. Azure Dev Spaces, Miğfer grafiklerini yalnızca uygulamanızı çalıştırırken genel depolardan alır. Miğfer deposunun özel olduğu veya Azure Dev Spaces'in erişilemediği durumlarda, bu depodan doğrudan uygulamanıza bir grafik ekleyebilirsiniz. Grafiği doğrudan eklemek, Azure Dev Spaces'in özel Miğfer deposuna erişmek zorunda kalmadan uygulamanızı çalıştırmasını sağlar.

## <a name="add-the-private-helm-repository-to-your-local-machine"></a>Özel Miğfer deposunu yerel makinenize ekleyin

Yerel makinenizden özel Miğfer deposuna erişmek için [dümen repo ekleme][helm-repo-add] ve [dümen repo güncelleştirmesini][helm-repo-update] kullanın.

```cmd
helm repo add privateRepoName http://example.com/helm/v1/repo --username user --password 5tr0ng_P@ssw0rd!
helm repo update
```

## <a name="add-the-chart-to-your-application"></a>Grafiği uygulamanıza ekleme

Projenizin dizinine gidin ve `azds prep`çalıştırın.

```cmd
azds prep --enable-ingress
```

> [!TIP]
> Komut, `prep` projeniz için [bir Dockerfile ve Helm grafiği](../how-dev-spaces-works-prep.md#prepare-your-code) oluşturmaya çalışır. Azure Dev Spaces, kodunuzu oluşturmak ve çalıştırmak için bu dosyaları kullanır, ancak projenin oluşturulma ve çalışma şeklini değiştirmek istiyorsanız bu dosyaları değiştirebilirsiniz.

Uygulamanızın grafik dizininde grafiğin olduğu bir [requirements.yaml][helm-requirements] dosyası oluşturun. Örneğin, uygulamanızın adı *app1*ise, *grafikler/app1/requirements.yaml*oluşturursunuz.

```yaml
dependencies:
    - name: mychart
      version: 0.1.0
      repository:  http://example.com/helm/v1/repo
```

Uygulamanızın grafik dizinine gidin ve uygulamanızın Miğfer bağımlılıklarını güncelleştirmek ve grafiği özel depodan indirmek için [miğfer bağımlılığı güncelleştirmesini][helm-dependency-update] kullanın.

```cmd
helm dependency update
```

Uygulamanızın grafik dizinine bir *tgz* dosyası eklenmişbir *grafik* alt dizinini doğrulayın. Örneğin, *grafikler/app1/charts/mychart-0.1.0.tgz*.

Özel Helm deponuzdaki grafik indirildi ve projenize eklendi. Geliştirme Spaces'in bu bağımlılığı güncelleştirmeye çalışmaması için *requirements.yaml* dosyasını kaldırın.

## <a name="run-your-application"></a>Uygulamanızı çalıştırma

Projenizin kök dizinine gidin `azds up` ve uygulamanızın geliştirme alanınızda başarıyla çalıştığını doğrulamak için çalıştırın.

```cmd
$ azds up
Using dev space 'default' with target 'MyAKS'
Synchronizing files...2s
Installing Helm chart...2s
Waiting for container image build...2m 25s
Building container image...
...
Service 'app1' port 'http' is available at http://app1.1234567890abcdef1234.eus.azds.io/
Service 'app1' port 80 (http) is available at http://localhost:54256
...
```

## <a name="next-steps"></a>Sonraki adımlar

[Helm ve nasıl çalıştığı][helm]hakkında daha fazla bilgi edinin.

[helm]: https://docs.helm.sh
[helm-chart]: https://helm.sh/docs/topics/charts/
[helm-dependency-update]: https://helm.sh/docs/topics/charts/#managing-dependencies-with-the-dependencies-field
[helm-repo-add]: https://helm.sh/docs/intro/using_helm/#helm-repo-working-with-repositories
[helm-repo-update]: https://helm.sh/docs/intro/using_helm/#helm-repo-working-with-repositories
[helm-requirements]: https://helm.sh/docs/topics/charts/#chart-dependencies
