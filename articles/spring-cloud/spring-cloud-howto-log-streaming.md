---
title: Azure yay bulutu uygulama günlüklerini gerçek zamanlı olarak akışla
description: Uygulama günlüklerini anında görüntülemek için günlük akışını kullanma
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/14/2019
ms.openlocfilehash: fa2e7af51ff681da0bfdac928cc08bf75126a3b8
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76156429"
---
# <a name="stream-azure-spring-cloud-app-logs-in-real-time"></a>Azure yay bulutu uygulama günlüklerini gerçek zamanlı olarak akışla
Azure yay bulutu, sorun giderme amacıyla gerçek zamanlı uygulama konsolu günlüklerini almak için Azure CLı 'de günlük akışı sağlar. Ayrıca, [günlükleri ve ölçümleri tanılama ayarlarıyla çözümleyebilirsiniz](./diagnostic-services.md).

## <a name="prerequisites"></a>Ön koşullar

* Yay bulutu için [Azure CLI uzantısı](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#install-the-azure-cli-extension) 'nı, en düşük sürüm 0.2.0 'yi yükler.
* Çalışan bir uygulamayla **Azure Spring Cloud** örneği, örneğin [Spring Cloud App](./spring-cloud-quickstart-launch-app-cli.md).

## <a name="use-cli-to-tail-logs"></a>Günlükleri kuyruk için CLı kullanma

Kaynak grubunuzu ve hizmet örneği adınızı tekrar tekrar belirtmeyi önlemek için varsayılan kaynak grubu adınızı ve küme adınızı ayarlayın.
```
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
```
Aşağıdaki örneklerde, komutlarda kaynak grubu ve hizmet adı atlanır.

### <a name="tail-log-for-app-with-single-instance"></a>Tek örnekli uygulama için kuyruk günlüğü
Auth-Service adlı bir uygulamanın yalnızca bir örneği varsa, uygulama örneğinin günlüğünü aşağıdaki komutla görüntüleyebilirsiniz:
```
az spring-cloud app log tail -n auth-service
```
Bu işlem günlükleri döndürür:
```
...
2020-01-15 01:54:40.481  INFO [auth-service,,,] 1 --- [main] o.apache.catalina.core.StandardService  : Starting service [Tomcat]
2020-01-15 01:54:40.482  INFO [auth-service,,,] 1 --- [main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.22]
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.a.c.c.C.[Tomcat].[localhost].[/uaa]  : Initializing Spring embedded WebApplicationContext
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.s.web.context.ContextLoader  : Root WebApplicationContext: initialization completed in 7203 ms

...
```

### <a name="tail-log-for-app-with-multiple-instances"></a>Birden çok örneğe sahip uygulama için kuyruk günlüğü
`auth-service`adlı uygulama için birden çok örnek varsa, `-i/--instance` seçeneğini kullanarak örnek günlüğünü görüntüleyebilirsiniz. Örneğin, uygulama adını ve örnek adını belirterek bir uygulamanın bir örneğinin günlüğünü akışla aktarabilirsiniz:

```
az spring-cloud app log tail -n auth-service -i auth-service-default-12-75cc4577fc-pw7hb
```
Uygulama örneklerini Azure portal da alabilirsiniz. 
1. Kaynak grubunuza gidin ve Azure Spring Cloud örneğinizi seçin.
1. Azure Spring Cloud Instance 'ta genel bakış sol gezinti bölmesindeki **uygulamalar** ' ı seçin.
1. Uygulamanızı seçin ve ardından sol gezinti bölmesindeki **uygulama örnekleri** ' ne tıklayın. 
1. Uygulama örnekleri görüntülenecektir.

### <a name="continuously-stream-new-logs"></a>Yeni günlükleri sürekli olarak akışla
Varsayılan olarak, `az spring-cloud ap log tail` yalnızca uygulama konsoluna akan mevcut günlükleri yazdırır ve sonra çıkar. Yeni Günlükler akışı yapmak istiyorsanız, Add-f (--izleyin):  

```
az spring-cloud app log tail -n auth-service -f
``` 
Desteklenen tüm günlük seçeneklerini denetlemek için:
``` 
az spring-cloud app log tail -h 
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tanılama ayarlarıyla günlükleri ve ölçümleri çözümleme](./diagnostic-services.md)

 





