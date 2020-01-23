---
title: KEDA ile Kubernetes üzerinde Azure Işlevleri
description: Kubernetes 'te Azure Işlevlerinin nasıl çalıştırılacağını, KEDA, Kubernetes tabanlı olay temelli otomatik ölçeklendirmeyi kullanarak bulutta veya şirket içinde nasıl çalıştıracağınızı anlayın.
author: jeffhollan
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: jehollan
ms.openlocfilehash: 83c57b27c1cd1d524805a92381a1ba9eb2e1fbd6
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76549045"
---
# <a name="azure-functions-on-kubernetes-with-keda"></a>KEDA ile Kubernetes üzerinde Azure Işlevleri

Azure Işlevleri çalışma zamanı, nerede ve nasıl istediğinizi barındırmak için esneklik sağlar.  [Keda](https://keda.sh) (Kubernetes tabanlı olay temelli otomatik ölçeklendirme), Kubernetes 'te olay odaklı ölçek sağlamak Için Azure işlevleri çalışma zamanı ve araçları ile sorunsuz bir şekilde çiftler sağlar.

## <a name="how-kubernetes-based-functions-work"></a>Kubernetes tabanlı işlevler nasıl çalışır?

Azure Işlevleri hizmeti iki temel bileşenden oluşur: çalışma zamanı ve ölçek denetleyicisi.  Işlevler çalışma zamanı, kodunuzu çalıştırır ve yürütür.  Çalışma zamanı, işlev yürütmelerinin tetiklenmesi, günlüğe kaydı ve yönetilmesi ile ilgili mantığı içerir.  Azure Işlevleri çalışma zamanı *her yerde*çalıştırılabilir.  Diğer bileşen bir ölçek denetleyicisidir.  Ölçek denetleyicisi, işlevinizi hedefleyen olayların oranını izler ve uygulamanızı çalıştıran örnek sayısını etkin bir şekilde ölçeklendirir.  Daha fazla bilgi için bkz. [Azure işlevleri ölçeklendirme ve barındırma](functions-scale.md).

Kubernetes tabanlı Işlevler, bir [Docker kapsayıcısında](functions-create-function-linux-custom-image.md) , Keda ile olay odaklı ölçeklendirmeyle işlevleri çalışma zamanı sağlar.  KEDA, 0 örneğe (hiçbir olay gerçekleşmesiz) ve en fazla *n* örneğe kadar ölçeklendirebilir. Bu, Kubernetes otomatik (yatay Pod otomatik Scaler) için özel ölçümler ortaya çıkaran bunu yapar.  Bir Kubernetes kümesinde, Işlev kapsayıcılarını KEDA kullanarak, sunucusuz işlev yeteneklerini çoğaltabilirsiniz.  Bu işlevler, sunucusuz altyapı için [Azure Kubernetes Hizmetleri (AKS) sanal düğümleri](../aks/virtual-nodes-cli.md) özelliği kullanılarak da dağıtılabilir.

## <a name="managing-keda-and-functions-in-kubernetes"></a>Kubernetes 'te KEDA ve işlevleri yönetme

Kubernetes kümenizdeki Işlevleri çalıştırmak için KEDA bileşenini yüklemelisiniz. Bu bileşeni, [Azure Functions Core Tools](functions-run-local.md)kullanarak yükleyebilirsiniz.

### <a name="installing-with-helm"></a>Held ile yükleme

Helm dahil olmak üzere herhangi bir Kubernetes kümesine KEDA yüklemenin çeşitli yolları vardır.  Dağıtım seçenekleri [Keda sitesinde](https://keda.sh/deploy/)belgelenmiştir.

## <a name="deploying-a-function-app-to-kubernetes"></a>Kubernetes 'e işlev uygulaması dağıtma

Herhangi bir işlev uygulamasını KEDA çalıştıran bir Kubernetes kümesine dağıtabilirsiniz.  İşlevleriniz bir Docker kapsayıcısında çalıştığından, projenizin bir `Dockerfile`olması gerekir.  Henüz bir tane yoksa, Işlevler projenizin kökünde aşağıdaki komutu çalıştırarak bir Dockerfile ekleyebilirsiniz:

```cli
func init --docker-only
```

Bir görüntü oluşturmak ve işlevlerinizi Kubernetes 'e dağıtmak için aşağıdaki komutu çalıştırın:

> [!NOTE]
> Temel araçlar, görüntüyü derlemek ve yayımlamak için Docker CLı özelliğinden yararlanır. Docker 'ın zaten yüklü olduğundan ve `docker login`hesabınıza bağlı olduğundan emin olun.

```cli
func kubernetes deploy --name <name-of-function-deployment> --registry <container-registry-username>
```

> `<name-of-function-deployment>`, işlev uygulamanızın adıyla değiştirin.

Bu bir Kubernetes `Deployment` kaynağı, bir `ScaledObject` kaynağı ve `Secrets`, `local.settings.json` dosyasından içeri aktarılan ortam değişkenlerini içeren bir şekilde oluşturur.

### <a name="deploying-a-function-app-from-a-private-registry"></a>Özel bir kayıt defterinden işlev uygulaması dağıtma

Yukarıdaki akış, özel kayıt defterleri için de kullanılır.  Kapsayıcı görüntünüzü özel bir kayıt defterinden çekeediyorsanız, `func kubernetes deploy`çalıştırırken özel kayıt defteri kimlik bilgilerini tutan Kubernetes gizliliğine başvuran `--pull-secret` bayrağını ekleyin.

## <a name="removing-a-function-app-from-kubernetes"></a>Kubernetes 'ten bir işlev uygulamasını kaldırma

Dağıttıktan sonra, ilişkili `Deployment``ScaledObject`, oluşturulan `Secrets` kaldırarak bir işlevi kaldırabilirsiniz.

```cli
kubectl delete deploy <name-of-function-deployment>
kubectl delete ScaledObject <name-of-function-deployment>
kubectl delete secret <name-of-function-deployment>
```

## <a name="uninstalling-keda-from-kubernetes"></a>Kubernetes 'ten KEDA kaldırma

KEDA 'yı kaldırma adımları [Keda sitesinde](https://keda.sh/deploy/)belgelenmiştir.

## <a name="supported-triggers-in-keda"></a>KEDA 'da desteklenen Tetikleyiciler

KEDA, aşağıdaki Azure Işlev Tetikleyicileri için destek içerir:

* [Azure Depolama Kuyrukları](functions-bindings-storage-queue.md)
* [Azure Service Bus kuyrukları](functions-bindings-service-bus.md)
* [Azure olay/IoT Hub 'Ları](functions-bindings-event-hubs.md)
* [Apache Kafka](https://github.com/azure/azure-functions-kafka-extension)
* [Oybbitmq kuyruğu](https://github.com/azure/azure-functions-rabbitmq-extension)

### <a name="http-trigger-support"></a>HTTP tetikleyicisi desteği

HTTP Tetikleyicileri sunan Azure Işlevlerini kullanabilirsiniz, ancak KEDA bunları doğrudan yönetemez.  [Http Azure işlevlerini 1 ' den *n* örneğe ölçeklendirmek](https://dev.to/anirudhgarg_99/scale-up-and-down-a-http-triggered-function-app-in-kubernetes-using-keda-4m42)için Keda Prometheus tetikleyicisinden yararlanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Özel görüntü kullanarak bir işlev oluşturma](functions-create-function-linux-custom-image.md)
* [Azure İşlevleri’ni yerel olarak kodlama ve test etme](functions-develop-local.md)
* [Azure Işlevi tüketim planı nasıl çalışır?](functions-scale.md)