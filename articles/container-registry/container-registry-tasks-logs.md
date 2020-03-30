---
title: Görev çalıştırma günlüklerini görüntüleme - Görevler
description: ACR Görevleri tarafından oluşturulan çalışma günlüklerini görüntüleme ve yönetme.
ms.topic: article
ms.date: 03/09/2020
ms.openlocfilehash: f7098f470a3f8a0cdac019f4bf8eb8fe14330337
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246974"
---
# <a name="view-and-manage-task-run-logs"></a>Görev çalıştırma günlüklerini görüntüleme ve yönetme

Azure Kapsayıcı [Kayıt Defteri görevlerinde](container-registry-tasks-overview.md) çalıştırılabilen her görev, görev adımlarının başarılı çalışıp çalışmadığını belirlemek için denetleyebileceğiniz günlük çıktısı oluşturur. 

Bu makalede, görev çalıştırma günlükleri nasıl görüntülenme ve yönetiştirilir.

## <a name="view-streamed-logs"></a>Akışlı günlükleri görüntüleme

Bir görevi el ile tetiklediğinde, günlük çıktısı doğrudan konsola aktarılır. Örneğin, [az acr build](/cli/azure/acr#az-acr-build), az [acr run](/cli/azure/acr#az-acr-run)veya az [acr task run](/cli/azure/acr/task#az-acr-task-run) komutunu kullanarak bir görevi el ile tetiklediğinizde, giriş çıkışının konsola aktedildiğini görürsünüz. 

Aşağıdaki örnek [az acr run](/cli/azure/acr#az-acr-run) komutu, aynı kayıt defterinden çıkarılan bir kapsayıcıyı çalıştıran bir görevi el ile tetikler:

```azurecli
az acr run --registry mycontainerregistry1220 \
  --cmd '$Registry/samples/hello-world:v1' /dev/null
```

Akış günlüğü:

```console
Queued a run with ID: cf4
Waiting for an agent...
2020/03/09 20:30:10 Alias support enabled for version >= 1.1.0, please see https://aka.ms/acr/tasks/task-aliases for more information.
2020/03/09 20:30:10 Creating Docker network: acb_default_network, driver: 'bridge'
2020/03/09 20:30:10 Successfully set up Docker network: acb_default_network
2020/03/09 20:30:10 Setting up Docker configuration...
2020/03/09 20:30:11 Successfully set up Docker configuration
2020/03/09 20:30:11 Logging in to registry: mycontainerregistry1220azurecr.io
2020/03/09 20:30:12 Successfully logged into mycontainerregistry1220azurecr.io
2020/03/09 20:30:12 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2020/03/09 20:30:12 Launching container with name: acb_step_0
Unable to find image 'mycontainerregistry1220azurecr.io/samples/hello-world:v1' locally
v1: Pulling from samples/hello-world
Digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e888a
Status: Downloaded newer image for mycontainerregistry1220azurecr.io/samples/hello-world:v1

Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]

2020/03/09 20:30:13 Successfully executed container: acb_step_0
2020/03/09 20:30:13 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.180081)

Run ID: cf4 was successful after 5s
```

## <a name="view-stored-logs"></a>Depolanan günlükleri görüntüleme 

Azure Konteyner Kayıt Defteri depoları tüm görevler için günlükleri çalıştırın. Azure portalında depolanan çalışma günlüklerini görüntüleyebilirsiniz. Veya, seçili bir günlüğü görüntülemek için [az acr görev günlükleri](/cli/azure/acr/task#az-acr-task-logs) komutunu kullanın. Varsayılan olarak, günlükleri 30 gün boyunca tutulur.

Bir görev otomatik olarak tetiklenirse, örneğin kaynak kodu güncelleştirmesi tarafından, depolanan günlüklere erişmek, çalıştırma günlüklerini görüntülemenin *tek* yoludur. Otomatik görev tetikleyicileri kaynak kodu taahhüt veya çekme istekleri, temel görüntü güncelleştirmeleri ve zamanlayıcı tetikleyicileri içerir.

Portaldaki çalıştır günlüklerini görüntülemek için:

1. Konteyner kayıt defterinize gidin.
1. **Hizmetlerde,** **Görevler** > **Çalışır'ı**seçin.
1. Çalışma durumunu görüntülemek ve günlükleri çalıştırmak için Bir **Çalıştır Kimliği** seçin. Günlük, biri oluşturulursa, akışlı günlükle aynı bilgileri içerir.

![Görev çalıştır giriş portalını görüntüleme](./media/container-registry-tasks-logs/portal-task-run-logs.png)

Azure CLI'yi kullanarak bir günlüğü görüntülemek için [az ad görev günlüklerini](/cli/azure/acr/task#az-acr-task-logs) çalıştırın ve bir çalışma kimliği, görev adı veya yapı görevi tarafından oluşturulan belirli bir resim belirtin. Bir görev adı belirtilirse, komut, oluşturulan son çalıştırmanın günlüğünü gösterir.

Aşağıdaki örnek id *cf4*ile çalıştırmak için günlük çıktıları:

```azurecli
az acr task logs --registry mycontainerregistry1220 \
  --run-id cf4
```

## <a name="alternative-log-storage"></a>Alternatif günlük depolama

Görev çalıştırma günlüklerini yerel bir dosya sisteminde depolamak veya Azure Depolama gibi alternatif bir arşivleme çözümü kullanmak isteyebilirsiniz.

Örneğin, yerel bir *görev günlüğü* oluşturun ve az [acr görev günlüklerinin çıktısını](/cli/azure/acr/task#az-acr-task-logs) yerel bir dosyaya yönlendirin:

```azurecli
mkdir ~/tasklogs

az acr task logs --registry mycontainerregistry1220 \
  --run-id cf4 > ~/tasklogs/cf4.log
```

Yerel günlük dosyalarını Azure Depolama'ya da kaydedebilirsiniz. Örneğin, bir depolama hesabına dosya yüklemek için [Azure CLI'yi,](../storage/blobs/storage-quickstart-blobs-cli.md) [Azure portalını](../storage/blobs/storage-quickstart-blobs-portal.md)veya diğer yöntemleri kullanın.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Kapsayıcı Kayıt Defteri Görevleri](container-registry-tasks-overview.md) hakkında daha fazla bilgi edinin

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-pack-build]: /cli/azure/acr/pack#az-acr-pack-build
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-update]: /cli/azure/acr/task#az-acr-task-update
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
