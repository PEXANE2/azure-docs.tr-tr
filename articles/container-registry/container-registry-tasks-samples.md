---
title: ACR görev örnekleri
description: Kapsayıcı görüntüleri oluşturmak, çalıştırmak ve yamalamak için Azure Kapsayıcı Kayıt Defteri Görevleri 'ni (ACR Görevleri) örnekleyin
ms.topic: article
ms.date: 11/14/2019
ms.openlocfilehash: 49df3bf565052a729ac3c587bd2ba11a299d05f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74456090"
---
# <a name="acr-tasks-samples"></a>ACR Görevleri örnekleri

Bu makalede, `task.yaml` birkaç Azure Kapsayıcı Kayıt Defteri Görevleri (ACR [Görevleri)](container-registry-tasks-overview.md) senaryoları için örnek dosyalara ve ilişkili Docker dosyalarına bağlantılar bulunur. 

Ek örnekler için [Azure örnekleri][task-examples] repo'ya bakın.

## <a name="scenarios"></a>Senaryolar

* **Yapı görüntüsü** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Yaml kabını çalıştırın** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml)

* **Oluşturma ve itme görüntü** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Oluşturma ve görüntü** - [YAML,](https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml) [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile) çalıştırın

* **Oluşturmak ve birden fazla görüntü** -  [YAML,](https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world-multi.yaml) [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile) itin

* **Paralel** -  [YAML,](https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml) [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile) görüntüleri oluşturma ve test

* **Oluşturma ve birden fazla kayıt** - [yaml,](https://github.com/Azure-Samples/acr-tasks/blob/master/multipleRegistries/testtask.yaml) [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/multipleRegistries/hello-world.dockerfile) görüntüleri itin


## <a name="next-steps"></a>Sonraki adımlar

ACR Görevleri hakkında daha fazla bilgi edinin:

* [Çok adımlı görevler](container-registry-tasks-multi-step.md) - Bulutta kapsayıcı görüntüleri oluşturmak, test etmek ve yamalama için ACR Görev tabanlı iş akışları.
* [Görev başvurusu](container-registry-tasks-reference-yaml.md) - Görev adımı türleri, özellikleri ve kullanımı.
* [Cmd repo](https://github.com/AzureCR/cmd) - ACR Görevleri için komut olarak kapsayıcıların bir koleksiyonu.


<!-- LINKS - External -->
[task-examples]: https://github.com/Azure-Samples/acr-tasks
