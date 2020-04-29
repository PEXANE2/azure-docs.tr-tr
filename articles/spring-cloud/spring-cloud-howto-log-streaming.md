---
title: Azure Spring Cloud uygulama günlüklerini gerçek zamanlı olarak akışla aktarma
description: Uygulama günlüklerini anında görüntülemek için günlük akışını kullanma
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/14/2019
ms.openlocfilehash: fc208a3542528fb4554a365a02e13c2da3055cf2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78192209"
---
# <a name="stream-azure-spring-cloud-app-logs-in-real-time"></a>Azure Spring Cloud uygulama günlüklerini gerçek zamanlı olarak akışla aktarma
Azure yay bulutu, sorun giderme amacıyla gerçek zamanlı uygulama konsolu günlüklerini almak için Azure CLı 'de günlük akışı sağlar. Ayrıca, [günlükleri ve ölçümleri tanılama ayarlarıyla çözümleyebilirsiniz](./diagnostic-services.md).

## <a name="prerequisites"></a>Ön koşullar

* Yay bulutu için [Azure CLI uzantısı](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#install-the-azure-cli-extension) 'nı, en düşük sürüm 0.2.0 'yi yükler.
* Çalışan bir uygulamayla **Azure Spring Cloud** örneği, örneğin [Spring Cloud App](./spring-cloud-quickstart-launch-app-cli.md).

> [!NOTE]
>  ASC CLı uzantısı, 0.2.0 sürümünden 0.2.1 sürümüne güncelleştirilir. Bu değişiklik `az spring-cloud app log tail`, şu şekilde değiştirilecek olan günlük akışı komutunun sözdizimini etkiler: `az spring-cloud app logs`. Komut: `az spring-cloud app log tail` gelecek bir sürümde kullanım dışı olacaktır. Sürüm 0.2.0 kullanıyorsanız, 0.2.1 sürümüne yükseltebilirsiniz. İlk olarak, eski sürümü şu komutla kaldırın: `az extension remove -n spring-cloud`.  Sonra, 0.2.1 komutunu şu komutla yüklersiniz: `az extension add -n spring-cloud`.

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
az spring-cloud app logs -n auth-service
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
Adlı `auth-service`uygulama için birden çok örnek varsa, `-i/--instance` seçeneğini kullanarak örnek günlüğünü görüntüleyebilirsiniz. 

İlk olarak, aşağıdaki komutla uygulama örneği adlarını alabilirsiniz.

```
az spring-cloud app show -n auth-service --query properties.activeDeployment.properties.instances -o table
```
Sonuçlarla:

```
Name                                         Status    DiscoveryStatus
-------------------------------------------  --------  -----------------
auth-service-default-12-75cc4577fc-pw7hb  Running   UP
auth-service-default-12-75cc4577fc-8nt4m  Running   UP
auth-service-default-12-75cc4577fc-n25mh  Running   UP
``` 
Ardından, bir uygulama örneğinin günlüklerini seçenek `-i/--instance` seçeneğiyle akışla aktarabilirsiniz:

```
az spring-cloud app logs -n auth-service -i auth-service-default-12-75cc4577fc-pw7hb
```

Ayrıca Azure portal uygulama örneklerinin ayrıntılarını alabilirsiniz.  Azure Spring Cloud Service 'in sol gezinti bölmesinde **uygulamalar** ' ı seçtikten sonra **uygulama örnekleri**' ni seçin.

### <a name="continuously-stream-new-logs"></a>Yeni günlükleri sürekli olarak akışla
Varsayılan olarak, `az spring-cloud ap log tail` yalnızca uygulama konsoluna akan mevcut günlükleri yazdırır ve sonra çıkar. Yeni Günlükler akışı yapmak istiyorsanız, Add-f (--izleyin):  

```
az spring-cloud app logs -n auth-service -f
``` 
Desteklenen tüm günlük seçeneklerini denetlemek için:
``` 
az spring-cloud app logs -h 
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tanılama ayarlarıyla günlükleri ve ölçümleri çözümleme](./diagnostic-services.md)

 





