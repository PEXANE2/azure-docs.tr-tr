---
title: ACR görev örnekleri
description: Kapsayıcı görüntülerini derlemek, çalıştırmak ve düzeltme eki uygulamak için örnek Azure Container Registry görevleri (ACR görevleri)
ms.topic: article
ms.date: 11/14/2019
ms.openlocfilehash: 49df3bf565052a729ac3c587bd2ba11a299d05f1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74456090"
---
# <a name="acr-tasks-samples"></a>ACR görevleri örnekleri

Bu makale `task.yaml` , birkaç [Azure Container Registry görevi](container-registry-tasks-overview.md) (ACR görevi) senaryosu için örnek dosyalara ve Ilişkili dockerfiles 'a bağlantı sağlar. 

Daha fazla örnek için bkz. [Azure örnekleri][task-examples] deposu.

## <a name="scenarios"></a>Senaryolar

* **Görüntü oluştur**  -  [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Kapsayıcıyı Çalıştır**  -  [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml)

* Görüntü oluşturma ve **gönderme**  -  [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* Görüntü oluşturma ve **çalıştırma**  -  [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Birden çok görüntü**  -   oluşturma ve gönderme [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world-multi.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Görüntüleri paralel**  -   olarak oluşturma ve test etme [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Birden çok kayıt defterlerine**  -  görüntü oluşturma ve gönderme [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/multipleRegistries/testtask.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/multipleRegistries/hello-world.dockerfile)


## <a name="next-steps"></a>Sonraki adımlar

ACR görevleri hakkında daha fazla bilgi edinin:

* [Çok adımlı görevler](container-registry-tasks-multi-step.md) -bulutta kapsayıcı görüntüleri oluşturmaya, test etmeye ve düzeltme eki uygulamaya yönelik görev tabanlı iş akışlarıdır.
* [Görev başvurusu](container-registry-tasks-reference-yaml.md) -görev adımı türleri, özellikleri ve kullanımı.
* [Cmd deposu](https://github.com/AzureCR/cmd) -ACR görevleri için komut olarak kapsayıcı koleksiyonu.


<!-- LINKS - External -->
[task-examples]: https://github.com/Azure-Samples/acr-tasks
