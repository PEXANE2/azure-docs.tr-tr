---
title: include dosyası
description: include dosyası
services: service-bus-messaging, event-hubs
author: spelluru
ms.service: service-bus-messaging, event-hubs
ms.topic: include
ms.date: 12/12/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 9bc641d680d927c44814f6814ebf6a6dde958c9e
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98901258"
---
Azure Işlevleri önceden oluşturulmuş bir giriş noktasında yalın olan yalnızca yapılandırma çoğaltma görevlerinin oluşturulmasına olanak sağlar. [Azure işlevleri için yapılandırma tabanlı çoğaltma örnekleri](https://github.com/Azure-Samples/azure-messaging-replication-dotnet/tree/main/functions/config) , kendi kodunuzda [önceden oluşturulmuş yardımcıları](https://github.com/Azure-Samples/azure-messaging-replication-dotnet/tree/main/src/Azure.Messaging.Replication) nasıl kullanacağınızı veya kodun tamamen işlenmesini ve yalnızca yapılandırmayı kullanmayı gösterir.

## <a name="create-a-replication-task"></a>Çoğaltma görevi oluşturma
Böyle bir çoğaltma görevi oluşturmak için önce proje klasörünün altında yeni bir klasör oluşturun. Yeni klasörün adı, örneğin veya için işlevin adıdır `europeToAsia` `telemetry` . Adın, kullanılmakta olan mesajlaşma varlıklarıyla doğrudan bağıntısı yoktur ve yalnızca bunları tanımlamanızı sağlar. Aynı projede düzinelerce işlevleri oluşturabilirsiniz.

Sonra, klasörde bir `function.json` dosya oluşturun. Dosya, işlevini yapılandırır. Aşağıdaki içerikle başlayın:

``` JSON
{
    "configurationSource": "config",
    "bindings" : [
        {
            "direction": "in",
            "name": "input" 
        },
        {
            "direction": "out",
            "name": "output"
        }
    ],
    "retry": {
        "strategy": "exponentialBackoff",
        "maxRetryCount": -1,
        "minimumInterval": "00:00:05",
        "maximumInterval": "00:05:00"
    },
    "disabled": false,
    "scriptFile": "../bin/Azure.Messaging.Replication.dll",
    "entryPoint": "Azure.Messaging.Replication.*"
}
```

Bu dosyada, bağlanmak istediğiniz varlıklara bağlı olan üç yapılandırma adımını doldurmanız gerekir:

1. [Giriş yönünü yapılandırma](#configure-the-input-direction)
2. [Çıkış yönünü yapılandırma](#configure-the-output-direction)
3. [Giriş noktasını yapılandırma](#configure-the-entry-point)

### <a name="configure-the-input-direction"></a>Giriş yönünü yapılandırma

#### <a name="event-hub-input"></a>Olay Hub 'ı girişi

Bir olay hub 'ından olay almak istiyorsanız, yapılandırma bilgilerini, "Bindings" olarak ayarlayan

* **tür** -"eventHubTrigger" türü.
* **bağlantı** -Olay Hub 'ı bağlantı dizesi için uygulama ayarları değerinin adı. 
* **Eventhubname** -bağlantı dizesi tarafından tanımlanan ad alanı Içindeki Olay Hub 'ının adı.
* **consumergroup** -tüketici grubunun adı. Adın yüzde işaretleri içine alınmış olduğunu ve bu nedenle de bir uygulama ayarları değerine başvurduğunu unutmayın.

```JSON
    ...
    "bindings" : [
        {
            "direction": "in",
            "type": "eventHubTrigger",
            "connection": "functionname-source-connection",
            "eventHubName": "eventHubA",
            "consumerGroup" : "%functionname-source-consumergroup%",
            "name": "input" 
        }
    ...
```

#### <a name="service-bus-queue-input"></a>Service Bus kuyruğu girişi

Bir Service Bus sırasından olayları almak istiyorsanız, yapılandırma bilgilerini ayarlayan "Bindings" içindeki üst bölüme ekleyin

* **tür** -"serviceBusTrigger" türü.
* **bağlantı** -Service Bus bağlantı dizesi için uygulama ayarları değerinin adı.
* **SıraAdı** -bağlantı dizesi tarafından tanımlanan ad alanı Içindeki Service Bus kuyruğunun adı.

```JSON
    ...
    "bindings" : [
        {
            "direction": "in",
            "type": "serviceBusTrigger",
            "connection": "functionname-source-connection",
            "queueName": "queue-a",
            "name": "input" 
        }
    ...
```

#### <a name="service-bus-topic-input"></a>Service Bus konu girişi

Bir Service Bus konusunun olaylarını almak istiyorsanız, yapılandırma bilgilerini ayarlayan "Bindings" içindeki üst bölüme ekleyin

* **tür** -"serviceBusTrigger" türü.
* **bağlantı** -Service Bus bağlantı dizesi için uygulama ayarları değerinin adı. Bu değer `{FunctionName}-source-connection` , belirtilen betikleri kullanmak istiyorsanız olmalıdır.
* **topicName** -bağlantı dizesi tarafından tanımlanan ad alanı Içindeki Service Bus konusunun adı.
* **subscriptionName** -bağlantı dizesi tarafından tanımlanan ad alanı içindeki verilen konudaki Service Bus aboneliğinin adı.

```JSON
    ...
    "bindings" : [
        {
            "direction": "in",
            "type": "serviceBusTrigger",
            "connection": "functionname-source-connection",
            "topicName": "topic-x",
            "subscriptionName" : "sub-y",
            "name": "input" 
        }
    ...
```

### <a name="configure-the-output-direction"></a>Çıkış yönünü yapılandırma

#### <a name="event-hub-output"></a>Olay Hub 'ı çıkışı

Olayları bir olay hub 'ına iletmek istiyorsanız, yapılandırma bilgilerini ayarlayan "bağlamalar" içindeki alt bölüme ekleyin

* **tür** -"eventHub" türü.
* **bağlantı** -Olay Hub 'ı bağlantı dizesi için uygulama ayarları değerinin adı. 
* **Eventhubname** -bağlantı dizesi tarafından tanımlanan ad alanı Içindeki Olay Hub 'ının adı.

```JSON
    ...
    "bindings" : [
        {
            ...
        },
        {
            "direction": "out",
            "type": "eventHub",
            "connection": "functionname-target-connection",
            "eventHubName": "eventHubB",
            "name": "output" 
        }
    ...
```

#### <a name="service-bus-queue-output"></a>Service Bus kuyruğu çıkışı

Olayları bir Service Bus kuyruğuna iletmek istiyorsanız, yapılandırma bilgilerini ayarlayan "bağlamalar" içindeki alt bölüme ekleyin

* **tür** -"ServiceBus" türü.
* **bağlantı** -Service Bus bağlantı dizesi için uygulama ayarları değerinin adı. 
* **SıraAdı** -bağlantı dizesi tarafından tanımlanan ad alanı içindeki Service Bus kuyruğunun adı.

```JSON
    ...
    "bindings" : [
        {
            ...
        },
        {
            "direction": "out",
            "type": "serviceBus",
            "connection": "functionname-target-connection",
            "queueName": "queue-b",
            "name": "output" 
        }
    ...
```

#### <a name="service-bus-topic-output"></a>Service Bus konu çıkışı

Olayları Service Bus bir konuya iletmek istiyorsanız, yapılandırma bilgilerini ayarlayan "bağlamalar" içindeki alt bölüme ekleyin

* **tür** -"ServiceBus" türü.
* **bağlantı** -Service Bus bağlantı dizesi için uygulama ayarları değerinin adı. 
* **topicName** -bağlantı dizesi tarafından tanımlanan ad alanı içindeki Service Bus konusunun adı.

```JSON
    ...
    "bindings" : [
        {
            ...
        },
        {
            "direction": "out",
            "type": "serviceBus",
            "connection": "functionname-target-connection",
            "topicName": "topic-b",
            "name": "output" 
        }
    ...
```

### <a name="configure-the-entry-point"></a>Giriş noktasını yapılandırma

Giriş noktası yapılandırması, standart çoğaltma görevlerinden birini seçer. `Azure.Messaging.Replication`Projeyi değiştiriyorsanız, görevleri de ekleyebilir ve burada bunlara başvurabilirsiniz. Örneğin:

```JSON
    ...
    "scriptFile": "../dotnet/bin/Azure.Messaging.Replication.dll",
    "entryPoint": "Azure.Messaging.Replication.EventHubReplicationTasks.ForwardToEventHub"
    ...
```

Aşağıdaki tabloda kaynak ve hedeflerin birleşimleri için doğru değerler verilmiştir:

| Kaynak      | Hedef      | Girdi Noktası 
|-------------|-------------|------------------------------------------------------------------------
| Olay Hub'ı   | Olay Hub'ı   | `Azure.Messaging.Replication.EventHubReplicationTasks.ForwardToEventHub`
| Olay Hub'ı   | Service Bus | `Azure.Messaging.Replication.EventHubReplicationTasks.ForwardToServiceBus`
| Service Bus | Olay Hub'ı   | `Azure.Messaging.Replication.ServiceBusReplicationTasks.ForwardToEventHub`
| Service Bus | Service Bus | `Azure.Messaging.Replication.ServiceBusReplicationTasks.ForwardToServiceBus`

### <a name="retry-policy"></a>Yeniden Deneme ilkesi

Yeniden deneme ilkesini yapılandırmak için [yeniden denemeler hakkında Azure işlevleri belgelerine](../articles/azure-functions/functions-bindings-error-pages.md) bakın. Bu depodaki projeler genelinde seçilen ilke ayarları, yeniden deneme aralıklarıyla 5 saniyeden 5 dakikaya kadar olan ve veri kaybını önlemek için sonsuz yeniden denemeler içeren bir üstel geri alma stratejisi yapılandırır.

Service Bus için, tetikleyicilerin etkileşimini ve sıra için tanımlanan maksimum teslim sayısını anlamak için ["tetikleyici esnekliği üzerinde yeniden deneme desteği kullanma"](../articles/azure-functions/functions-bindings-error-pages.md#using-retry-support-on-top-of-trigger-resilience) bölümünü gözden geçirin.

### <a name="build-deploy-and-configure"></a>Oluşturma, dağıtma ve yapılandırma

Yapılandırmaya odaklandığınızda, görevler yine de dağıtılabilir bir uygulama oluşturmayı ve Azure Işlevleri ana bilgisayarlarını, belirtilen uç noktalara bağlanmak için gerekli tüm bilgilere sahip olacak şekilde yapılandırmayı gerektirir. 

[Azure işlevleri için yapılandırma tabanlı çoğaltma örneklerinde](https://github.com/Azure-Samples/azure-messaging-replication-dotnet/tree/main/functions/config)yeniden kullanılabilir betiklerle birlikte bu gösterilmektedir.