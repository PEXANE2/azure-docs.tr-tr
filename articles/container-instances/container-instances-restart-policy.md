---
title: Çalıştırma sonrası görevler için ilkeyi yeniden başlatma
description: Yapı, test veya görüntü oluşturma işleri gibi tamamlanmak üzere çalışan görevleri yürütmek için Azure Kapsayıcı Örnekleri'ni nasıl kullanacağınızı öğrenin.
ms.topic: article
ms.date: 04/15/2019
ms.openlocfilehash: 8ef4ef228038242f53abc8041470f7f596ab1157
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131503"
---
# <a name="run-containerized-tasks-with-restart-policies"></a>Kapsayıcıgörevleri yeniden başlatma ilkeleriyle çalıştırma

Azure Container Instances’da hızla ve kolayca kapsayıcı dağıtma olanağı, bir kapsayıcı örneğinde derleme, test ve görüntü işleme gibi tek kez çalıştırılan görevlerin yürütülmesi için cazip bir platform sağlar.

Yapılandırılabilen bir yeniden başlatma ilkesiyle kapsayıcıların işlemleri tamamlandıktan sonra durdurulmasını belirtebilirsiniz. Kapsayıcı örnekleri saniyelik olarak faturalandığından, yalnızca görevinizi çalıştıran kapsayıcının çalıştığı süre boyunca kullanılan işlem kaynakları için ücret ödersiniz.

Bu makalede sunulan örneklerde Azure CLI kullanılır. Azure CLI sürümü 2.0.21 veya daha büyük [yerel olarak yüklü][azure-cli-install]olmalı veya Azure Bulut [BulutU'nda](../cloud-shell/overview.md)CLI'yi kullanmalısınız.

## <a name="container-restart-policy"></a>Kapsayıcı yeniden başlatma ilkesi

Azure Kapsayıcı Örnekleri'nde bir [kapsayıcı grubu](container-instances-container-groups.md) oluşturduğunuzda, üç yeniden başlatma ilkesi ayarlarından birini belirtebilirsiniz.

| Yeniden başlatma ilkesi   | Açıklama |
| ---------------- | :---------- |
| `Always` | Kapsayıcı grubundaki kapsayıcılar her zaman yeniden başlatılır. Bu, kapsayıcı oluşturulurken yeniden başlatma ilkesi belirtilmezse uygulanan **varsayılan** ayardır. |
| `Never` | Kapsayıcı grubundaki kapsayıcılar hiçbir zaman yeniden başlatılmaz. Kapsayıcılar en fazla bir kez çalışır. |
| `OnFailure` | Kapsayıcı grubundaki kapsayıcılar yalnızca kapsayıcıda yürütülen işlem başarısız olduğunda (sıfır olmayan çıkış kodu ile sonlandırıldığında) yeniden başlatılır. Kapsayıcılar en az bir kez çalıştırılır. |

## <a name="specify-a-restart-policy"></a>Yeniden başlatma ilkesi ni belirtin

Yeniden başlatma ilkesini nasıl belirtdiğiniz, Azure CLI, Azure PowerShell cmdlets veya Azure portalı gibi kapsayıcı örneklerinizi nasıl oluşturduğunuza bağlıdır. Azure CLI'de az `--restart-policy` kapsayıcı oluştur'u çağırdığınızda parametreyi [belirtin.][az-container-create]

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

## <a name="run-to-completion-example"></a>Tamamlama örneğine çalıştırın

Yeniden başlatma ilkesini iş başında görmek için, Microsoft [aci-wordcount][aci-wordcount-image] görüntüsünden `OnFailure` bir kapsayıcı örneği oluşturun ve yeniden başlatma ilkesini belirtin. Bu örnek kapsayıcı, varsayılan olarak, Shakespeare's [Hamlet](http://shakespeare.mit.edu/hamlet/full.html)metnini analiz eden bir Python komut dosyası çalışır, STDOUT için en yaygın 10 kelime yazar ve sonra çıkar.

Örnek kapsayıcıyı aşağıdaki [az kapsayıcı oluşturma][az-container-create] komutuyla çalıştırın:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure Container Instances kapsayıcıyı başlatır ve kapsayıcıdaki uygulama (veya bu durumda betik) çıkış yaptığında durdurur. Azure Kapsayıcı Örnekleri, yeniden başlatma ilkesi `Never` olan `OnFailure`bir kapsayıcıyı durdurduğunda veya kapsayıcının durumu **Sonlandırıldı**olarak ayarlandığında. [Az kapsayıcı göster][az-container-show] komutu yla bir kapsayıcının durumunu kontrol edebilirsiniz:

```azurecli-interactive
az container show \
    --resource-group myResourceGroup \
    --name mycontainer \
    --query containers[0].instanceView.currentState.state
```

Örnek çıktı:

```bash
"Terminated"
```

Örnek kapsayıcının durumunda *Sonlandırıldı* ifadesi göründüğünde kapsayıcı günlüklerini görüntüleyerek görev çıktısını görebilirsiniz. Komut dosyasının çıktısını görüntülemek için [az kapsayıcı günlükleri][az-container-logs] komutunu çalıştırın:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Çıktı:

```bash
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```

Bu örnek, komut dosyasının STDOUT'a gönderdiği çıktıyı gösterir. Ancak kapsayıcı görevleriniz, bunun yerine çıktılarını daha sonra almak üzere kalıcı depolamaalanına yazabilir. Örneğin, bir [Azure dosya paylaşımına.](container-instances-mounting-azure-files-volume.md)

## <a name="next-steps"></a>Sonraki adımlar

Birkaç kapsayıcıyla büyük bir veri kümesini toplu işleme gibi görev tabanlı [senaryolar,](container-instances-environment-variables.md) çalışma zamanında özel ortam değişkenlerinden veya [komut satırlarından](container-instances-start-command.md) yararlanabilir.

Tamamlanınacak şekilde çalışan kapsayıcılarınızın çıktısını nasıl devam ettirileceğiyle ilgili ayrıntılar için Azure [Kapsayıcı Örnekleri ile Azure dosya paylaşımı nı montaj'a](container-instances-mounting-azure-files-volume.md)bakın.

<!-- LINKS - External -->
[aci-wordcount-image]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az-container-logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az-container-show
[azure-cli-install]: /cli/azure/install-azure-cli
