---
title: KEDA ile Kubernetes'te Azure Fonksiyonları
description: Kubernetes tabanlı olay odaklı otomatik ölçekleme KEDA'yı kullanarak Bulut'ta veya şirket içinde Azure İşlerini Kubernetes'te nasıl çalıştırlayacağınızda anlayın.
author: jeffhollan
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: jehollan
ms.openlocfilehash: 2c06fdba8f60243acf4e0fabd23df8b832c210db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78301684"
---
# <a name="azure-functions-on-kubernetes-with-keda"></a>KEDA ile Kubernetes'te Azure Fonksiyonları

Azure İşlevler çalışma süresi, nerede ve nasıl istediğinizi barındırma da esneklik sağlar.  [KEDA](https://keda.sh) (Kubernetes tabanlı Event Driven Autoscaling), Kubernetes'te etkinlik odaklı ölçek sağlamak için Azure İşlevler çalışma zamanı ve araçla sorunsuz bir şekilde eşleşerek çalışır.

## <a name="how-kubernetes-based-functions-work"></a>Kubernetes tabanlı işlevler nasıl çalışır?

Azure İşlevler hizmeti iki temel bileşenden oluşur: çalışma zamanı ve ölçek denetleyicisi.  İşlevler çalışma süresi kodunuzu çalıştırıp yürütür.  Çalışma süresi, işlev yürütmelerini nasıl tetikleyip günlüğe kaydedin ve yönetecekmantık içerir.  Azure İşlevler çalışma zamanı *her yerde*çalıştırılabilir.  Diğer bileşen bir ölçek denetleyicisidir.  Ölçek denetleyicisi, işlevinizi hedefleyen olayların oranını izler ve uygulamanızı çalıştıran örnek sayısını proaktif olarak ölçeklendirin.  Daha fazla bilgi edinmek için Azure [İşlevlerini ölçeklendirme ve barındırma](functions-scale.md)'ya bakın.

Kubernetes tabanlı Fonksiyonlar, Bir [Docker kapsayıcısındaki](functions-create-function-linux-custom-image.md) işlevleri keda üzerinden olay odaklı ölçekleme ile çalıştırır.  KEDA 0 örneğine (olay oluşmadığında) ve *n* örneklerine ölçeklendirilebilir. Bunu, Kubernetes otomatik ölçekleyicisi (Yatay Pod Autoscaler) için özel ölçümleri ortaya çıkararak yapar.  KEDA ile Fonksiyonlar kapsayıcıları kullanarak herhangi bir Kubernetes kümesinde sunucusuz işlev yetenekleri çoğaltmak mümkün kılar.  Bu işlevler, sunucusuz altyapı için [Azure Kubernetes Hizmetleri (AKS) sanal düğümler](../aks/virtual-nodes-cli.md) özelliği kullanılarak da dağıtılabilir.

## <a name="managing-keda-and-functions-in-kubernetes"></a>KeDA ve işlevlerini Kubernetes'te yönetme

Kubernetes kümenizde İşlevler çalıştırmak için KEDA bileşenini yüklemeniz gerekir. Bu bileşeni Azure [İşlevler Temel Araçları'nı](functions-run-local.md)kullanarak yükleyebilirsiniz.

### <a name="installing-with-helm"></a>Helm ile yükleme

Helm dahil herhangi bir Kubernetes küme KEDA yüklemek için çeşitli yollar vardır.  Dağıtım seçenekleri [KEDA sitesinde](https://keda.sh/deploy/)belgelenmiştir.

## <a name="deploying-a-function-app-to-kubernetes"></a>Kubernetes'e bir işlev uygulaması dağıtma

Keda çalıştıran bir Kubernetes kümesine herhangi bir işlev uygulaması dağıtabilirsiniz.  Fonksiyonlarınız Docker kapsayıcısında çalıştığı için, `Dockerfile`projenizin bir .  Zaten yoksa, İşlevler projenizin kökünde aşağıdaki komutu çalıştırarak bir Dockerfile ekleyebilirsiniz:

```cli
func init --docker-only
```

Bir görüntü oluşturmak ve işlevlerinizi Kubernetes'e dağıtmak için aşağıdaki komutu çalıştırın:

> [!NOTE]
> Core Tools, görüntüyü oluşturmak ve yayınlamak için docker CLI'den yararlanır. Docker'ın zaten yüklü olduğundan ve hesabınıza `docker login`'' ile bağlı olduğundan emin olun.

```cli
func kubernetes deploy --name <name-of-function-deployment> --registry <container-registry-username>
```

> İşlev uygulamanızın adı ile değiştirin. `<name-of-function-deployment>`

`Deployment` Bu, dosyanızdan `ScaledObject` `local.settings.json` alınan ortam değişkenlerini içeren `Secrets`bir Kubernetes kaynağı, bir kaynak ve , oluşturur.

### <a name="deploying-a-function-app-from-a-private-registry"></a>Özel bir kayıt defterinden işlev uygulaması dağıtma

Yukarıdaki akış özel kayıtlar için de çalışır.  Kapsayıcı resminizi özel bir kayıt defterinden çekiyorsanız, çalışırken `--pull-secret` `func kubernetes deploy`özel kayıt defteri kimlik bilgilerini tutan Kubernetes gizli defterine başvuran bayrağı ekleyin.

## <a name="removing-a-function-app-from-kubernetes"></a>Bir işlev uygulamasını Kubernetes'ten kaldırma

Dağıttıktan sonra, `Deployment` `ScaledObject` `Secrets` oluşturulan bir ilişkili , , kaldırarak bir işlevi kaldırabilirsiniz.

```cli
kubectl delete deploy <name-of-function-deployment>
kubectl delete ScaledObject <name-of-function-deployment>
kubectl delete secret <name-of-function-deployment>
```

## <a name="uninstalling-keda-from-kubernetes"></a>KEDA'yı Kubernetes'ten Kaldırma

KEDA'yı kaldırmak için atılan adımlar [KEDA sitesinde](https://keda.sh/deploy/)belgelenmiştir.

## <a name="supported-triggers-in-keda"></a>KEDA'da desteklenen tetikleyiciler

KEDA aşağıdaki Azure İşi tetikleyicileri için destek vardır:

* [Azure Depolama Kuyrukları](functions-bindings-storage-queue.md)
* [Azure Servis Veri Hizmetleri Kuyrukları](functions-bindings-service-bus.md)
* [Azure Etkinliği / IoT Hub'ları](functions-bindings-event-hubs.md)
* [Apache Kafka](https://github.com/azure/azure-functions-kafka-extension)
* [RabbitMQ Sırası](https://github.com/azure/azure-functions-rabbitmq-extension)

### <a name="http-trigger-support"></a>HTTP Trigger desteği

HTTP tetikleyicilerini ortaya çıkaran Azure Işlevlerini kullanabilirsiniz, ancak KEDA bunları doğrudan yönetmez.  [HTTP Azure Fonksiyonlarını 1'den *n'ye* ölçeklendirmek](https://dev.to/anirudhgarg_99/scale-up-and-down-a-http-triggered-function-app-in-kubernetes-using-keda-4m42)için KEDA prometheus tetikleyicisinden yararlanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Özel bir görüntü kullanarak işlev oluşturma](functions-create-function-linux-custom-image.md)
* [Azure İşlevleri’ni yerel olarak kodlama ve test etme](functions-develop-local.md)
* [Azure İşlev Tüketimi planı nasıl çalışır?](functions-scale.md)