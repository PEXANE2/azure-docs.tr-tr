---
title: KEDA ile Kubernetes üzerinde Azure Işlevleri
description: Kubernetes 'te Azure Işlevlerinin nasıl çalıştırılacağını, KEDA, Kubernetes tabanlı olay temelli otomatik ölçeklendirmeyi kullanarak bulutta veya şirket içinde nasıl çalıştıracağınızı anlayın.
services: functions
documentationcenter: na
author: jeffhollan
manager: jeconnoc
keywords: Azure işlevleri, işlevler, olay işleme, dinamik işlem, sunucusuz mimari, Kubernetes
ms.service: azure-functions
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: jehollan
ms.openlocfilehash: b581d7c9b5876813e36ebbf41be713b44dd97735
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70096090"
---
# <a name="azure-functions-on-kubernetes-with-keda"></a>KEDA ile Kubernetes üzerinde Azure Işlevleri

Azure Işlevleri çalışma zamanı, nerede ve nasıl istediğinizi barındırmak için esneklik sağlar.  [Keda](https://github.com/kedacore/kore) dili (Kubernetes tabanlı otomatik ölçeklendirme) çiftleri, Kubernetes 'te olay odaklı ölçek sağlamak için Azure Işlevleri çalışma zamanı ve araçları ile sorunsuz bir şekilde çalışır.

## <a name="how-kubernetes-based-functions-work"></a>Kubernetes tabanlı işlevler nasıl çalışır?

Azure Işlevleri hizmeti iki temel bileşenden oluşur: çalışma zamanı ve ölçek denetleyicisi.  Işlevler çalışma zamanı, kodunuzu çalıştırır ve yürütür.  Çalışma zamanı, işlev yürütmelerinin tetiklenmesi, günlüğe kaydı ve yönetilmesi ile ilgili mantığı içerir.  Diğer bileşen bir ölçek denetleyicisidir.  Ölçek denetleyicisi, işlevinizi hedefleyen olayların oranını izler ve uygulamanızı çalıştıran örnek sayısını etkin bir şekilde ölçeklendirir.  Daha fazla bilgi için bkz. [Azure işlevleri ölçeklendirme ve barındırma](functions-scale.md).

Kubernetes tabanlı Işlevler, bir [Docker kapsayıcısında](functions-create-function-linux-custom-image.md) , Keda ile olay odaklı ölçeklendirmeyle işlevleri çalışma zamanı sağlar.  KEDA, 0 örneğe (hiçbir olay gerçekleşmesiz) ve en fazla *n* örneğe kadar ölçeklendirebilir. Bu, Kubernetes otomatik (yatay Pod otomatik Scaler) için özel ölçümler ortaya çıkaran bunu yapar.  Bir Kubernetes kümesinde, Işlev kapsayıcılarını KEDA kullanarak, sunucusuz işlev yeteneklerini çoğaltabilirsiniz.  Bu işlevler, sunucusuz altyapı için [Azure Kubernetes Hizmetleri (AKS) sanal düğümleri](../aks/virtual-nodes-cli.md) özelliği kullanılarak da dağıtılabilir.

## <a name="managing-keda-and-functions-in-kubernetes"></a>Kubernetes 'te KEDA ve işlevleri yönetme

Kubernetes kümenizdeki Işlevleri çalıştırmak için KEDA bileşenini yüklemelisiniz. Bu bileşeni, [Azure Functions Core Tools](functions-run-local.md)kullanarak yükleyebilirsiniz.

### <a name="installing-with-the-azure-functions-core-tools"></a>Azure Functions Core Tools ile yükleme

Varsayılan olarak, temel araçlar, sırasıyla olay odaklı ve HTTP ölçeklendirmesini destekleyen KEDA ve Osıris bileşenlerini de yüklerse.  Yükleme geçerli bağlamda `kubectl` çalışan kullanır.

Aşağıdaki Install komutunu çalıştırarak KEDA kümenize ' i de yüklersiniz:

```cli
func kubernetes install --namespace keda
```

## <a name="deploying-a-function-app-to-kubernetes"></a>Kubernetes 'e işlev uygulaması dağıtma

Herhangi bir işlev uygulamasını KEDA çalıştıran bir Kubernetes kümesine dağıtabilirsiniz.  İşlevleriniz bir Docker kapsayıcısında çalıştığından, projeniz bir `Dockerfile`gerektirir.  Henüz bir tane yoksa, Işlevler projenizin kökünde aşağıdaki komutu çalıştırarak bir Dockerfile ekleyebilirsiniz:

```cli
func init --docker-only
```

Bir görüntü oluşturmak ve işlevlerinizi Kubernetes 'e dağıtmak için aşağıdaki komutu çalıştırın:

```cli
func kubernetes deploy --name <name-of-function-deployment> --registry <container-registry-username>
```

> İşlev `<name-of-function-deployment>` uygulamanızın adıyla değiştirin.

Bu, bir Kubernetes `Deployment` kaynağı, bir `ScaledObject` kaynak ve `Secrets` `local.settings.json` , dosyanızda içeri aktarılan ortam değişkenlerini içeren bir kaynak oluşturur.

## <a name="removing-a-function-app-from-kubernetes"></a>Kubernetes 'ten bir işlev uygulamasını kaldırma

Dağıttıktan sonra, ilişkili `Deployment`, oluşturuldu olan `Secrets` öğesini `ScaledObject`kaldırarak bir işlevi kaldırabilirsiniz.

```cli
kubectl delete deploy <name-of-function-deployment>
kubectl delete ScaledObject <name-of-function-deployment>
kubectl delete secret <name-of-function-deployment>
```

## <a name="uninstalling-keda-from-kubernetes"></a>Kubernetes 'ten KEDA kaldırma

Bir Kubernetes kümesinden KEDA kaldırmak için aşağıdaki çekirdek araçları komutunu çalıştırabilirsiniz:

```cli
func kubernetes remove --namespace keda
```

## <a name="supported-triggers-in-keda"></a>KEDA 'da desteklenen Tetikleyiciler

KEDA Şu anda beta sürümünde aşağıdaki Azure Işlev Tetikleyicileri desteğiyle birlikte çalışıyor:

* [Azure depolama kuyrukları](functions-bindings-storage-queue.md)
* [Azure Service Bus kuyrukları](functions-bindings-service-bus.md)
* [HTTP](functions-bindings-http-webhook.md)
* [Apache Kafka](https://github.com/azure/azure-functions-kafka-extension)

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Özel görüntü kullanarak bir işlev oluşturma](functions-create-function-linux-custom-image.md)
* [Azure İşlevleri’ni yerel olarak kodlama ve test etme](functions-develop-local.md)
* [Azure Işlevi tüketim planı nasıl çalışır?](functions-scale.md)