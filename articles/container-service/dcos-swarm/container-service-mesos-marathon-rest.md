---
title: (AmortismanA Uğradı) Marathon REST API ile Azure DC/OS kümesini yönetme
description: Marathon REST API'sini kullanarak kapsayıcıları Azure Kapsayıcı Hizmeti DC/OS kümesine dağıtın.
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 3492f35d54dd3ee61ab8d29a3af06e4998bbd477
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277776"
---
# <a name="deprecated-dcos-container-management-through-the-marathon-rest-api"></a>(AmortismanA Uğradı) Marathon REST API ile DC/OS konteyner yönetimi

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

DC/OS, temel donanımı özetlerken, kümelenmiş iş yüklerini dağıtmak ve ölçeklendirmek için ortam sağlar. DC/OS’nin en üstünde, hesaplama iş yüklerini zamanlamayı ve yürütmeyi yöneten bir çerçeve vardır. Birçok popüler iş yükü için çerçeveler mevcut olsa da, bu belge Marathon REST API'sini kullanarak kapsayıcı dağıtımlarını oluşturmaya ve ölçeklendirmeye başlamanızı sağlar. 

## <a name="prerequisites"></a>Ön koşullar

Bu örneklerin üzerinden geçmeden önce, Azure Kapsayıcı Hizmeti’nde yapılandırılan bir DC/OS kümeniz olması gerekir. Bu kümeye uzaktan bağlantınız olması da gerekir. Bu öğeler hakkında daha fazla bilgi için, aşağıdaki makalelere bakın:

* [Azure Container Service kümesini dağıtma](container-service-deployment.md)
* [Azure Container Service kümesine bağlanma](../container-service-connect.md)

## <a name="access-the-dcos-apis"></a>DC/OS API'lara erişin
Azure Kapsayıcı Hizmeti kümesine bağlandıktan sonra, DC/OS ve ilgili REST API'lerine http:\//localhost:local-port üzerinden erişebilirsiniz. Bu belgedeki örneklerde, bağlantı noktası 80 üzerinde tünel oluşturulmaktadır. Örneğin, Maraton bitiş noktalarına http:\//localhost/marathon/v2/ ile başlayan URI'lerden ulaşılabilir. 

Çeşitli API'ler hakkında daha fazla bilgi için, [Marathon API’si](https://mesosphere.github.io/marathon/docs/rest-api.html) ve [Chronos API’si](https://mesos.github.io/chronos/docs/api.html) için Mesosphere belgelerine ve [Mesos Scheduler API’si](https://mesos.apache.org/documentation/latest/scheduler-http-api/) için Apache belgelerine bakın.

## <a name="gather-information-from-dcos-and-marathon"></a>DC/OS’den ve Marathon’dan bilgi toplama
Kapsayıcıları DC/OS kümesine dağıtmadan önce, DC/OS aracılarının adları ve durumu gibi DC/OS kümesi hakkında bazı bilgiler toplayın. Bunu yapmak için, DC/OS REST API’sinin `master/slaves` uç noktasını sorgulayın.  Her şey yolunda giderse, sorgu DC/OS aracıları ve her biri için çeşitli özellikler listesini döndürür.

```bash
curl http://localhost/mesos/master/slaves
```

Şimdi, DC/OS kümesine geçerli uygulama dağıtımlarını denetlemek için Marathon `/apps` uç noktasını kullanın. Bu yeni bir küme ise, uygulamalar için boş bir dizi görürsünüz.

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Docker biçimli kapsayıcı dağıtma
Docker biçimli kapsayıcıları Marathon REST API'si aracılığıyla, amaçlanan dağıtımı açıklayan bir JSON dosyasını kullanarak dağıtırsınız. Aşağıdaki örnek kümedeki özel bir aracıya bir Nginx kapsayıcısı dağıtLar. 

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Docker biçimli bir kapsayıcı dağıtmak için JSON dosyasını erişilebilir bir konumda saklayın. Ardından, kapsayıcıyı dağıtmak için aşağıdaki komutu çalıştırın. JSON dosyasının adını (bu`marathon.json` örnekte) belirtin.

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Çıkış aşağıdakine benzer:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Şimdi, uygulamalar için Marathon’u sorguladığınızda, bu yeni uygulama çıktıda görünür.

```bash
curl localhost/marathon/v2/apps
```

## <a name="reach-the-container"></a>Konteynere ulaşın

Nginx kümedeki özel aracılardan birinde bir kapsayıcıda çalıştığını doğrulayabilirsiniz. Kapsayıcının çalıştığı ana bilgisayarı ve bağlantı noktasını bulmak için koşu görevleri için Maraton'u sorgulayın: 

```bash
curl localhost/marathon/v2/tasks
```

Çıktının `host` değerini (benzer bir IP `10.32.0.x`adresi) ve .'ın `ports`değerini bulun


Şimdi kümenin yönetim FQDN'sine bir SSH terminal bağlantısı (tünelli bağlantı değil) yapın. Bağlandıktan sonra, aşağıdaki isteği yapın, doğru değerleri `host` `ports`yerine ve:

```bash
curl http://host:ports
```

Nginx sunucu çıktısı aşağıdakilere benzer:

![Konteynerden Nginx](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a>Kapsayıcılarınızı ölçeklendirme
Uygulama dağıtımlarında ölçeklendirmek veya ölçeklendirmek için Maraton API'sini kullanabilirsiniz. Önceki örnekte, uygulamanın bir örneğini dağıttınız. Şimdi bunun ölçeğini uygulamanın üç örneğine genişletelim. Bunu yapmak için, aşağıdaki JSON metnini kullanarak bir JSON dosyası oluşturun ve erişilebilir bir konumda depolayın.

```json
{ "instances": 3 }
```

Tünelli bağlantınızdan uygulamayı ölçeklendirmek için aşağıdaki komutu çalıştırın.

> [!NOTE]
> URI http:\//localhost/marathon/v2/apps/ ardından uygulamanın kimliğini ölçeklendirin. Burada sağlanan Nginx örnek kullanıyorsanız, URI http olacaktır:\//localhost/marathon/v2/apps/nginx.

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Son olarak, uygulamalar için Marathon uç noktasını sorgulayın. Artık üç adet Nginx kapsayıcısı olduğunu görürsünüz.

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a>Eşdeğer PowerShell komutları
Bir Windows sisteminde PowerShell komutlarını kullanarak aynı eylemleri gerçekleştirebilirsiniz.

Temsilci adları ve aracı durumu gibi DC/OS kümesi hakkında bilgi toplamak için aşağıdaki komutu çalıştırın:

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Docker biçimli kapsayıcıları Marathon aracılığıyla, hedeflenen dağıtımı açıklayan bir JSON dosyası kullanarak dağıtın. Aşağıdaki örnek, Nginx kapsayıcısı DC/OS aracısı bağlama bağlantı noktası 80'i kapsayıcının 80 numaralı bağlantı noktasına dağıtır.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Docker biçimli bir kapsayıcı dağıtmak için JSON dosyasını erişilebilir bir konumda saklayın. Ardından, kapsayıcıyı dağıtmak için aşağıdaki komutu çalıştırın. JSON dosyasına giden yolu`marathon.json` belirtin (bu örnekte).

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Marathon API’sini uygulama dağıtımlarının ölçeğini genişletmek ve ölçeğini daraltmak için de kullanabilirsiniz. Önceki örnekte, uygulamanın bir örneğini dağıttınız. Şimdi bunun ölçeğini uygulamanın üç örneğine genişletelim. Bunu yapmak için, aşağıdaki JSON metnini kullanarak bir JSON dosyası oluşturun ve erişilebilir bir konumda depolayın.

```json
{ "instances": 3 }
```

Uygulamayı ölçeklendirmek için aşağıdaki komutu çalıştırın:

> [!NOTE]
> URI http:\//localhost/marathon/v2/apps/ ardından uygulamanın kimliğini ölçeklendirin. Burada sağlanan Nginx örnek kullanıyorsanız, URI http\/olacaktır: /localhost/marathon/v2/apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Sonraki adımlar
* [Mesos HTTP uç noktaları hakkında daha fazla bilgi edinin](https://mesos.apache.org/documentation/latest/endpoints/)
* [Marathon REST API hakkında daha fazla bilgi edinin](https://mesosphere.github.io/marathon/docs/rest-api.html)

