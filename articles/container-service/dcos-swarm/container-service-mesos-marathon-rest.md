---
title: Kullanım DıŞı Azure DC/OS kümesini Marathon ile yönetme REST API
description: Marathon REST API kullanarak kapsayıcıları Azure Container Service DC/OS kümesine dağıtın.
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 3492f35d54dd3ee61ab8d29a3af06e4998bbd477
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76277776"
---
# <a name="deprecated-dcos-container-management-through-the-marathon-rest-api"></a>Kullanım DıŞı Marathon REST API aracılığıyla DC/OS kapsayıcı yönetimi

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

DC/OS, temel donanımı özetlerken, kümelenmiş iş yüklerini dağıtmak ve ölçeklendirmek için ortam sağlar. DC/OS’nin en üstünde, hesaplama iş yüklerini zamanlamayı ve yürütmeyi yöneten bir çerçeve vardır. Çerçeveler birçok popüler iş yükü için kullanılabilir olsa da, bu belge Marathon REST API kullanarak kapsayıcı dağıtımları oluşturmaya ve ölçeklendirmeye başlamanızı de ister. 

## <a name="prerequisites"></a>Ön koşullar

Bu örneklerin üzerinden geçmeden önce, Azure Kapsayıcı Hizmeti’nde yapılandırılan bir DC/OS kümeniz olması gerekir. Bu kümeye uzaktan bağlantınız olması da gerekir. Bu öğeler hakkında daha fazla bilgi için, aşağıdaki makalelere bakın:

* [Azure Container Service kümesini dağıtma](container-service-deployment.md)
* [Azure Container Service kümesine bağlanma](../container-service-connect.md)

## <a name="access-the-dcos-apis"></a>DC/OS API 'Lerine erişin
Azure Container Service kümesine bağlandıktan sonra, DC/OS ve ilgili REST API 'Lerine http:\//localhost: local-port aracılığıyla erişebilirsiniz. Bu belgedeki örneklerde, bağlantı noktası 80 üzerinde tünel oluşturulmaktadır. Örneğin, Marathon bitiş noktalarına http ile başlayan URI 'Ler ile ulaşılabilir:\//localhost/Marathon/v2/. 

Çeşitli API'ler hakkında daha fazla bilgi için, [Marathon API’si](https://mesosphere.github.io/marathon/docs/rest-api.html) ve [Chronos API’si](https://mesos.github.io/chronos/docs/api.html) için Mesosphere belgelerine ve [Mesos Scheduler API’si](https://mesos.apache.org/documentation/latest/scheduler-http-api/) için Apache belgelerine bakın.

## <a name="gather-information-from-dcos-and-marathon"></a>DC/OS’den ve Marathon’dan bilgi toplama
DC/OS kümesine kapsayıcılar dağıtmadan önce DC/OS kümesi hakkında, DC/OS aracılarının adları ve durumu gibi bazı bilgiler toplayın. Bunu yapmak için, DC/OS REST API’sinin `master/slaves` uç noktasını sorgulayın. Her şey yolunda giderse, sorgu DC/OS aracıları ve her biri için çeşitli özellikler listesini döndürür.

```bash
curl http://localhost/mesos/master/slaves
```

Şimdi, DC/OS kümesine geçerli uygulama dağıtımlarını denetlemek için Marathon `/apps` uç noktasını kullanın. Bu yeni bir küme ise, uygulamalar için boş bir dizi görürsünüz.

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Docker biçimli kapsayıcı dağıtma
Marathon REST API aracılığıyla, istenen dağıtımı açıklayan bir JSON dosyası kullanarak Docker biçimli kapsayıcılar dağıtırsınız. Aşağıdaki örnek, bir NGINX kapsayıcısını kümedeki özel bir aracıya dağıtır. 

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

Docker biçimli bir kapsayıcı dağıtmak için JSON dosyasını erişilebilir bir konuma depolayın. Ardından, kapsayıcıyı dağıtmak için aşağıdaki komutu çalıştırın. JSON dosyasının adını belirtin (Bu örnekte`marathon.json`).

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

## <a name="reach-the-container"></a>Kapsayıcıya ulaşın

NGINX 'in kümedeki özel aracılardan birindeki kapsayıcıda çalıştığını doğrulayabilirsiniz. Kapsayıcının çalıştığı Konağı ve bağlantı noktasını bulmak için çalışan görevler için Marathon sorgulayın: 

```bash
curl localhost/marathon/v2/tasks
```

Çıktıda `host` değerini (`10.32.0.x`benzer bir IP adresi) ve `ports`değerini bulun.


Şimdi kümenin yönetim FQDN 'sine bir SSH terminal bağlantısı (tünel dışı bağlantı değil) yapın. Bağlandıktan sonra, `host` ve `ports`doğru değerlerini değiştirerek aşağıdaki isteği yapın:

```bash
curl http://host:ports
```

NGINX sunucu çıkışı aşağıdakine benzer:

![Kapsayıcıdan NGINX](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a>Kapsayıcılarınızı ölçeklendirme
Uygulama dağıtımlarında ölçeği genişletmek veya ölçeklendirmek için Marathon API 'sini kullanabilirsiniz. Önceki örnekte, uygulamanın bir örneğini dağıttınız. Şimdi bunun ölçeğini uygulamanın üç örneğine genişletelim. Bunu yapmak için, aşağıdaki JSON metnini kullanarak bir JSON dosyası oluşturun ve erişilebilir bir konumda depolayın.

```json
{ "instances": 3 }
```

Tünel bağlantınızdan, uygulamanın ölçeğini genişletmek için aşağıdaki komutu çalıştırın.

> [!NOTE]
> URI: http:\//localhost/Marathon/v2/Apps/ve ardından Ölçeklendirilecek uygulamanın KIMLIĞI. Burada belirtilen NGINX örneğini kullanıyorsanız, URI http:\//localhost/Marathon/v2/Apps/nginx.

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Son olarak, uygulamalar için Marathon uç noktasını sorgulayın. Artık üç adet Nginx kapsayıcısı olduğunu görürsünüz.

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a>Eşdeğer PowerShell komutları
Bir Windows sisteminde PowerShell komutlarını kullanarak aynı eylemleri gerçekleştirebilirsiniz.

Aracı adları ve Aracı durumu gibi DC/OS kümesi hakkında bilgi toplamak için aşağıdaki komutu çalıştırın:

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

Docker biçimli bir kapsayıcı dağıtmak için JSON dosyasını erişilebilir bir konuma depolayın. Ardından, kapsayıcıyı dağıtmak için aşağıdaki komutu çalıştırın. JSON dosyasının yolunu belirtin (Bu örnekte`marathon.json`).

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Marathon API’sini uygulama dağıtımlarının ölçeğini genişletmek ve ölçeğini daraltmak için de kullanabilirsiniz. Önceki örnekte, uygulamanın bir örneğini dağıttınız. Şimdi bunun ölçeğini uygulamanın üç örneğine genişletelim. Bunu yapmak için, aşağıdaki JSON metnini kullanarak bir JSON dosyası oluşturun ve erişilebilir bir konumda depolayın.

```json
{ "instances": 3 }
```

Uygulamanın ölçeğini genişletmek için aşağıdaki komutu çalıştırın:

> [!NOTE]
> URI: http:\//localhost/Marathon/v2/Apps/ve ardından Ölçeklendirilecek uygulamanın KIMLIĞI. Burada belirtilen NGINX örneğini kullanıyorsanız, URI http:\//localhost/Marathon/v2/Apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Sonraki adımlar
* [Mesos HTTP uç noktaları hakkında daha fazla bilgi edinin](https://mesos.apache.org/documentation/latest/endpoints/)
* [Marathon REST API hakkında daha fazla bilgi edinin](https://mesosphere.github.io/marathon/docs/rest-api.html)

