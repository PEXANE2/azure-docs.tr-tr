---
title: ACR görev örnekleri
description: Kapsayıcı görüntülerini derlemek, çalıştırmak ve düzeltme eki uygulamak için örnek Azure Container Registry görevleri (ACR görevleri)
ms.topic: article
ms.date: 11/14/2019
ms.openlocfilehash: 49df3bf565052a729ac3c587bd2ba11a299d05f1
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74456090"
---
# <a name="acr-tasks-samples"></a>ACR görevleri örnekleri

Bu makalede birkaç [Azure Container Registry görevi](container-registry-tasks-overview.md) (ACR görevi) senaryosu için örnek `task.yaml` dosyaları ve Ilişkili Dockerfiles bağlantısı yer aldığı bir bağlantı vardır. 

Daha fazla örnek için bkz. [Azure örnekleri][task-examples] deposu.

## <a name="scenarios"></a>Senaryolar

* **Derleme görüntüsü** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml) - **kapsayıcıyı Çalıştır**

* [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile) - **derleme ve gönderme resmi**

*  - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile) **görüntüsünü derleyin ve çalıştırın**

* [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world-multi.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile) -  **birden çok görüntü oluşturun ve gönderin**

* Paralel -  [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile) **'da görüntü oluşturma ve test** etme

* [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/multipleRegistries/testtask.yaml), [dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/multipleRegistries/hello-world.dockerfile) - **birden çok kayıt defterlerine görüntü oluşturun ve gönderin**


## <a name="next-steps"></a>Sonraki adımlar

ACR görevleri hakkında daha fazla bilgi edinin:

* [Çok adımlı görevler](container-registry-tasks-multi-step.md) -bulutta kapsayıcı görüntüleri oluşturmaya, test etmeye ve düzeltme eki uygulamaya yönelik görev tabanlı iş akışlarıdır.
* [Görev başvurusu](container-registry-tasks-reference-yaml.md) -görev adımı türleri, özellikleri ve kullanımı.
* [Cmd deposu](https://github.com/AzureCR/cmd) -ACR görevleri için komut olarak kapsayıcı koleksiyonu.


<!-- LINKS - External -->
[task-examples]: https://github.com/Azure-Samples/acr-tasks
