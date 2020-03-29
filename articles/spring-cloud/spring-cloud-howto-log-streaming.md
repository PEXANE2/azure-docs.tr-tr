---
title: Azure Spring Cloud uygulama günlüklerini gerçek zamanlı olarak akışla aktarma
description: Uygulama günlüklerini anında görüntülemek için günlük akışı nasıl kullanılır?
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/14/2019
ms.openlocfilehash: fc208a3542528fb4554a365a02e13c2da3055cf2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78192209"
---
# <a name="stream-azure-spring-cloud-app-logs-in-real-time"></a>Azure Spring Cloud uygulama günlüklerini gerçek zamanlı olarak akışla aktarma
Azure Bahar Bulutu, sorun giderme için gerçek zamanlı uygulama konsolgünlükleri almak için Azure CLI'de günlük akışısağlar. Ayrıca [tanılama ayarları ile günlükleri ve ölçümleri analiz](./diagnostic-services.md)edebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Bahar Bulutu için [Azure CLI uzantısını,](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#install-the-azure-cli-extension) minimum sürüm 0.2.0'ı yükleyin.
* Çalışan bir uygulama ile **Azure Bahar Bulut** örneği, örneğin Bahar Bulut [uygulaması](./spring-cloud-quickstart-launch-app-cli.md).

> [!NOTE]
>  ASC CLI uzantısı sürüm 0.2.0'dan 0.2.1'e güncelleştirilir. Bu değişiklik günlük akışı için komutun `az spring-cloud app log tail`sözdizimini etkiler: `az spring-cloud app logs`, hangi tarafından değiştirilir: . Komut: `az spring-cloud app log tail` gelecekteki bir sürümde amortismana alınacaktır. 0.2.0 sürümünü kullanıyorsanız, 0.2.1'e yükseltebilirsiniz. İlk olarak, komutu ile `az extension remove -n spring-cloud`eski sürümü kaldırın: .  Sonra, komutu ile 0.2.1 yükleyin: `az extension add -n spring-cloud`.

## <a name="use-cli-to-tail-logs"></a>Kuyruk günlükleri için CLI'yi kullanma

Kaynak grubuve hizmet örneği adınızı tekrar tekrar belirtmekten kaçınmak için varsayılan kaynak grup adınızı ve küme adınızı ayarlayın.
```
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
```
Aşağıdaki örneklerde, kaynak grubu ve hizmet adı komutları atlanır.

### <a name="tail-log-for-app-with-single-instance"></a>Tek örnekli uygulama için kuyruk günlüğü
Auth-service adlı bir uygulamanın yalnızca bir örneği varsa, uygulama örneğinin günlüğünü aşağıdaki komutla görüntüleyebilirsiniz:
```
az spring-cloud app logs -n auth-service
```
Bu günlükleri döndürecek:
```
...
2020-01-15 01:54:40.481  INFO [auth-service,,,] 1 --- [main] o.apache.catalina.core.StandardService  : Starting service [Tomcat]
2020-01-15 01:54:40.482  INFO [auth-service,,,] 1 --- [main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.22]
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.a.c.c.C.[Tomcat].[localhost].[/uaa]  : Initializing Spring embedded WebApplicationContext
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.s.web.context.ContextLoader  : Root WebApplicationContext: initialization completed in 7203 ms

...
```

### <a name="tail-log-for-app-with-multiple-instances"></a>Birden çok örnekli uygulama için kuyruk günlüğü
Adlı `auth-service`uygulama için birden çok örnek varsa, `-i/--instance` seçeneği kullanarak örnek günlüğünü görüntüleyebilirsiniz. 

İlk olarak, aşağıdaki komutu ile uygulama örnek adlarını alabilirsiniz.

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
Ardından, bir uygulama örneğinin günlüklerini seçenek `-i/--instance` seçeneğiyle akışlayabilirsiniz:

```
az spring-cloud app logs -n auth-service -i auth-service-default-12-75cc4577fc-pw7hb
```

Azure portalından uygulama örneklerinin ayrıntılarını da alabilirsiniz.  Azure Bahar Bulutu hizmetinizin sol gezinti bölmesinde **Uygulamalar'ı** seçtikten sonra **Uygulama Örnekleri'ni**seçin.

### <a name="continuously-stream-new-logs"></a>Sürekli olarak yeni günlükleri akış
Varsayılan olarak, `az spring-cloud ap log tail` yalnızca uygulama konsoluna aktarılan varolan günlükleri yazdırır ve sonra çıkar. Yeni günlükleri akış istiyorsanız, ekle -f (--takip):  

```
az spring-cloud app logs -n auth-service -f
``` 
Desteklenen tüm günlük seçeneklerini denetlemek için:
``` 
az spring-cloud app logs -h 
```

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri ve ölçümleri tanılama ayarlarıyla analiz edin](./diagnostic-services.md)

 





