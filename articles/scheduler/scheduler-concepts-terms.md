---
title: Kavramlar, terimler ve varlıklar
description: İşler ve iş koleksiyonları dahil Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi hakkında bilgi edinin
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: conceptual
ms.date: 08/18/2016
ms.openlocfilehash: 0a744c2de320ddad2e7959cae7b62d7990879953
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898583"
---
# <a name="concepts-terminology-and-entities-in-azure-scheduler"></a>Azure Scheduler kavramları, terminolojisi ve varlıkları

> [!IMPORTANT]
> [Azure Logic Apps,](../logic-apps/logic-apps-overview.md) [kullanımdan kaldırılan](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)Azure Zamanlayıcısı'nın yerini alıyor. Zamanlayıcı'da ayarladığınız işlerle çalışmaya devam etmek için lütfen mümkün olan en kısa sürede [Azure Logic Apps'a geçirin.](../scheduler/migrate-from-scheduler-to-logic-apps.md) 
>
> Zamanlayıcı artık Azure portalında kullanılamıyor, ancak işlerinizi ve iş koleksiyonlarınızı yönetebilmeniz için [ŞU anda REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlets'i](scheduler-powershell-reference.md) kullanılabilir durumda kalır.

## <a name="entity-hierarchy"></a>Varlık hiyerarşisi

Azure Scheduler REST API'si şu ana varlıkları veya kaynakları kullanıma sunar ve kullanır:

| Varlık | Açıklama |
|--------|-------------|
| **İş** | Yürütmek üzere basit veya karmaşık stratejilere sahip tek bir yinelenen eylemi tanımlar. Eylemler HTTP, Depolama kuyruğu, Service Bus kuyruğu ya da Service Bus konusu isteklerini içerebilir. | 
| **İş koleksiyonu** | Bir grup işi içerir ve koleksiyondaki işler tarafından paylaşılan ayarlar, kotalar ve kısıtlamaları tutar. Azure aboneliği sahibi olarak iş koleksiyonları oluşturabilir ve işleri kullanım veya uygulama sınırlarına göre gruplandırabilirsiniz. Bir iş koleksiyonu şu özniteliklere sahiptir: <p>- Tek bir bölge için kısıtlanmıştır. <br>- Koleksiyondaki tüm işlerin kullanımını kısıtlamak için kota uygulamanızı sağlar. <br>- Kotalar MaxJobs ve MaxRecurrence değerlerini içerir. | 
| **İş geçmişi** | Durum ve yanıt ayrıntıları gibi iş yürütme ayrıntılarını açıklar. |
||| 

## <a name="entity-management"></a>Varlık yönetimi

En geniş anlamıyla Scheduler REST API'si, varlıkların yönetilmesi için bu işlemleri kullanıma sunar.

### <a name="job-management"></a>İş yönetimi

İş oluşturma ve düzenleme işlemlerini destekler. Açık oluşturma olmaması için tüm işlerin var olan bir iş koleksiyonuna ait olması gerekir. Daha fazla bilgi için bkz. [Scheduler REST API - İşler](https://docs.microsoft.com/rest/api/scheduler/jobs). Bu işlemler için URI adresi aşağıda veda edebilirsiniz:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}
```

### <a name="job-collection-management"></a>İş koleksiyonu yönetimi

Kotalar ve paylaşılan ayarlarla eşleştirilen işlerin ve iş koleksiyonlarının oluşturulmasını ve düzenlenmesini destekler. Örneğin kotalar, maksimum iş sayısını ve en küçük yineleme aralığını belirtir. Daha fazla bilgi için bkz. [Scheduler REST API - İş Koleksiyonları](https://docs.microsoft.com/rest/api/scheduler/jobcollections). Bu işlemler için URI adresi aşağıda veda edebilirsiniz:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}
```

### <a name="job-history-management"></a>İş geçmişi yönetimi

Geçen iş süresi ve iş yürütme sonuçları gibi 60 günlük iş yürütme geçmişinin alınması için GET işleminin kullanılmasını destekler. Durum ve duruma göre filtreleme için sorgu dizesi parametresi desteği sunar. Daha fazla bilgi için bkz. [Scheduler REST API - İşler - İş Geçmişini Listeleme](https://docs.microsoft.com/rest/api/scheduler/jobs/listjobhistory). İşte bu işlem için URI adresi:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history
```

## <a name="job-types"></a>İş türleri

Azure Scheduler, birden fazla iş türünü destekler: 

* Var olan bir hizmetin veya iş yükünün uç noktasına sahip olduğunuzda SSL desteği sunan HTTPS işleri dahil olmak üzere HTTP işleri
* Depolama kuyruklarına ileti gönderme gibi Depolama kuyruklarını kullanan iş yükleri için Depolama kuyruğu işleri
* Service Bus kuyruklarını kullanan iş yükleri için Service Bus kuyruğu işleri
* Service Bus konularını kullanan iş yükleri için Service Bus konusu işleri

## <a name="job-definition"></a>İş tanımı

Bir Scheduler işi genel olarak şu temel bölümlerden oluşur:

* İş zamanlayıcı başlatıldığında çalıştırılan eylem
* İsteğe bağlı: İşin çalıştırma süresi
* İsteğe bağlı: İşin ne zaman ve ne sıklıkta tekrarlanacağı
* İsteğe bağlı: Birincil eylem başarısız olursa çalıştırılacak hata eylemi

İş aynı zamanda işin bir sonraki zamanlanmış çalışma zamanı gibi sistem tarafından sağlanan verileri de içerir. İşin kod tanımı, şu öğelere sahip olan bir JavaScript Nesne Gösterimi (JSON) biçimli nesnedir:

| Öğe | Gerekli | Açıklama | 
|---------|----------|-------------| 
| [**startTime**](#start-time) | Hayır | [ISO 8601 biçiminde](https://en.wikipedia.org/wiki/ISO_8601) saat dilimi farkına sahip olan işin başlangıç zamanı | 
| [**Eylem**](#action) | Evet | **errorAction** nesnesi de içerebilecek birincil eylem ayrıntıları | 
| [**errorAction**](#error-action) | Hayır | Birincil eylemin başarısız olması durumunda çalışan ikinci eylemin ayrıntıları |
| [**yineleme**](#recurrence) | Hayır | Yinelenen bir işin sıklık ve aralık gibi ayrıntıları | 
| [**retryPolicy**](#retry-policy) | Hayır | Bir eylemin yeniden deneme sıklığını belirten ayrıntılar | 
| [**durum**](#state) | Evet | İşin geçerli durumunun ayrıntıları |
| [**durum**](#status) | Evet | Hizmet tarafından denetlenen geçerli iş durumu ayrıntıları |
||||

Bu örnekte bir HTTP eyleminin kapsamlı iş tanımı gösterilmiştir ve öğeler sonraki bölümlerde ayrıntılı bir şekilde açıklanacaktır: 

```json
"properties": {
   "startTime": "2012-08-04T00:00Z",
   "action": {
      "type": "Http",
      "request": {
         "uri": "http://contoso.com/some-method", 
         "method": "PUT",          
         "body": "Posting from a timer",
         "headers": {
            "Content-Type": "application/json"
         },
         "retryPolicy": { 
             "retryType": "None" 
         },
      },
      "errorAction": {
         "type": "Http",
         "request": {
            "uri": "http://contoso.com/notifyError",
            "method": "POST"
         }
      }
   },
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "weekDays": ["Monday", "Wednesday", "Friday"],
         "hours": [10, 22]
      },
      "count": 10,
      "endTime": "2012-11-04"
   },
   "state": "Disabled",
   "status": {
      "lastExecutionTime": "2007-03-01T13:00:00Z",
      "nextExecutionTime": "2007-03-01T14:00:00Z ",
      "executionCount": 3,
      "failureCount": 0,
      "faultedCount": 0
   }
}
```

<a name="start-time"></a>

## <a name="starttime"></a>startTime

**startTime** nesnesinde başlangıç zamanını ve saat dilimi farkını [ISO 8601 biçiminde](https://en.wikipedia.org/wiki/ISO_8601) belirtebilirsiniz.

<a name="action"></a>

## <a name="action"></a>action

Scheduler işiniz belirtilen zamanlamayı kullanarak birincil **eylemi** çalıştırır. Scheduler HTTP, Depolama kuyruğu, Service Bus kuyruğu ve Service Bus konusu eylemlerini destekler. Birincil **action** nesnesi başarısız olursa Scheduler hatayı işleyen ikincil [**errorAction**](#erroraction) nesnesini çalıştırabilir. **action** nesnesi şu öğeleri tanımlar:

* Eylemin hizmet türü
* Eylemin ayrıntıları
* Alternatif bir **errorAction**

Yukarıdaki örnekte bir HTTP eylemi açıklanmıştır. Depolama kuyruğu eylemi örneği aşağıda verilmiştir:

```json
"action": {
   "type": "storageQueue",
   "queueMessage": {
      "storageAccount": "myStorageAccount",  
      "queueName": "myqueue",                
      "sasToken": "TOKEN",                   
      "message": "My message body"
    }
}
```

Service Bus kuyruğu eylemi örneği aşağıda verilmiştir:

```json
"action": {
   "type": "serviceBusQueue",
   "serviceBusQueueMessage": {
      "queueName": "q1",  
      "namespace": "mySBNamespace",
      "transportType": "netMessaging", // Either netMessaging or AMQP
      "authentication": {  
         "sasKeyName": "QPolicy",
         "type": "sharedAccessKey"
      },
      "message": "Some message",  
      "brokeredMessageProperties": {},
      "customMessageProperties": {
         "appname": "FromScheduler"
      }
   }
},
```

Service Bus konusu eylemi örneği aşağıda verilmiştir:

```json
"action": {
   "type": "serviceBusTopic",
   "serviceBusTopicMessage": {
      "topicPath": "t1",  
      "namespace": "mySBNamespace",
      "transportType": "netMessaging", // Either netMessaging or AMQP
      "authentication": {
         "sasKeyName": "QPolicy",
         "type": "sharedAccessKey"
      },
      "message": "Some message",
      "brokeredMessageProperties": {},
      "customMessageProperties": {
         "appname": "FromScheduler"
      }
   }
},
```

Paylaşılan Erişim İmzası (SAS) belirteçleri hakkında daha fazla bilgi için bkz. [Paylaşılan Erişim İmzasıyla Yetkilendirme](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

<a name="error-action"></a>

## <a name="erroraction"></a>errorAction

İşinizin birincil **action** nesnesi başarısız olursa Scheduler hatayı işleyen bir **errorAction** nesnesini çalıştırabilir. Birincil **action** nesnesinde bir **errorAction** nesnesi belirterek Scheduler uygulamasının uç nokta hatasını işlemesini veya kullanıcıya bildirim göndermesini sağlayabilirsiniz. 

Örneğin birincil uç noktada olağanüstü durum oluşursa **errorAction** kullanarak ikincil uç noktayı çağırabilir veya hata işleme uç noktasını bilgilendirebilirsiniz. 

Birincil **eylemde** olduğu gibi, hata eylemi diğer eylemler temelinde basit ya da birleşik mantıklı olabilir. 

<a name="recurrence"></a>

## <a name="recurrence"></a>yineleme

İşin JSON tanımında **recurrence** nesnesi varsa o iş yinelenir, örneğin:

```json
"recurrence": {
   "frequency": "Week",
   "interval": 1,
   "schedule": {
      "hours": [10, 22],
      "minutes": [0, 30],
      "weekDays": ["Monday", "Wednesday", "Friday"]
   },
   "count": 10,
   "endTime": "2012-11-04"
},
```

| Özellik | Gerekli | Değer | Açıklama | 
|----------|----------|-------|-------------| 
| **Frekans** | **recurrence** kullanıldığında evet | "Minute", "Hour", "Day", "Week", "Month", "Year" | Yinelemeler arası zaman birimi | 
| **Aralığı** | Hayır | 1-1000 arası, ikisi de dahil | **frequency** nesnesine göre yinelemeler arasındaki zaman birimi sayısını belirleyen pozitif tamsayı | 
| **Zamanlama** | Hayır | Değişir | Daha karmaşık ve gelişmiş zamanlamaların ayrıntıları. Bkz. **hours**, **minutes**, **weekDays**, **months** ve **monthDays** | 
| **Saat** | Hayır | 1-24 arası | İşin çalıştırılacağı saati belirten dizi | 
| **minutes** | Hayır | 0 ile 59 arasında | İşin çalıştırılacağı dakikayı belirten dizi | 
| **months** | Hayır | 1-12 arası | İşin çalıştırılacağı ayı belirten dizi | 
| **monthDays** | Hayır | Değişir | İşin çalıştırılacağı ayın gününü belirten dizi | 
| **weekDays** | Hayır | "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday" | İşin çalıştırılacağı haftanın gününü belirten dizi | 
| **count** | Hayır | <*Hiçbiri*> | Yineleme sayısı. Varsayılan değer sonsuzdur. **count** ve **endTime** nesnelerini birlikte kullanamazsınız ancak ilk tamamlanan kural uygulanır. | 
| **endTime** | Hayır | <*Hiçbiri*> | Yinelemenin durdurulacağı tarih ve saat. Varsayılan değer sonsuzdur. **count** ve **endTime** nesnelerini birlikte kullanamazsınız ancak ilk tamamlanan kural uygulanır. | 
||||

Bu öğeler hakkında daha fazla bilgi için bkz. [Karmaşık zamanlamalar ve gelişmiş yinelemeler oluşturma](../scheduler/scheduler-advanced-complexity.md).

<a name="retry-policy"></a>

## <a name="retrypolicy"></a>retryPolicy

Bir Scheduler işinin başarısız olması durumunda Scheduler uygulamasının eylemi yeniden deneme kararını ve şeklini belirleyen bir yenileme ilkesi ayarlayabilirsiniz. Scheduler varsayılan olarak işi 30 saniyelik aralıklarla dört kez daha dener. Bu ilkeyi daha katı veya gevşek haline getirebilirsiniz. Örneğin şu ilke eylemi günde iki kez dener:

```json
"retryPolicy": { 
   "retryType": "Fixed",
   "retryInterval": "PT1D",
   "retryCount": 2
},
```

| Özellik | Gerekli | Değer | Açıklama | 
|----------|----------|-------|-------------| 
| **retryType** | Evet | **Fixed**, **None** | Bir yenileme ilkesi belirtip (**fixed**) belirtmediğinizi (**none**) belirler. | 
| **retryInterval** | Hayır | PT30S | Yeniden deneme girişimleri arasındaki aralığı ve sıklığı [ISO 8601 biçiminde](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) belirtir. Minimum değer 15 saniye, maksimum değer ise 18 aydır. | 
| **retryCount** | Hayır | 4 | Yeniden deneme girişimlerinin sayısını belirtir. Maksimum değer 20'dir. | 
||||

Daha fazla bilgi için bkz. [Yüksek kullanılabilirlik ve güvenilirlik](../scheduler/scheduler-high-availability-reliability.md).

<a name="status"></a>

## <a name="state"></a>durum

Bir işin durumu **Etkin**, **Devre Dışı**, **Tamamlandı** veya **Hatalı** olacaktır, örneğin: 

`"state": "Disabled"`

İşleri **Etkin** veya **Devre Dışı** durumuna almak için işlerde PUT veya PATCH işlemlerini kullanabilirsiniz.
Ancak bir işin durumu **Tamamlandı** veya **Arızalı** şeklindeyse durumu güncelleştiremezsiniz ancak iş üzerinde DELETE işlemini gerçekleştirebilirsiniz. Scheduler, tamamlandı ve arızalı durumundaki işleri 60 gün sonra siler. 

<a name="status"></a>

## <a name="status"></a>durum

Bir iş başlatıldıktan sonra Scheduler, yalnızca Scheduler tarafından denetlenen **status** nesnesiyle iş durumu hakkında bilgi döndürür. Ancak **status** nesnesini **job** nesnesinin içinde bulabilirsiniz. İş durumunda bulunan bilgiler şunlardır:

* Varsa bir önceki yürütme işleminin zamanı
* Devam eden işler için bir sonraki zamanlanmış yürütmenin zamanı
* İş yürütme sayısı
* Varsa başarısız işlem sayısı
* Varsa hatalı işlem sayısı

Örnek:

```json
"status": {
   "lastExecutionTime": "2007-03-01T13:00:00Z",
   "nextExecutionTime": "2007-03-01T14:00:00Z ",
   "executionCount": 3,
   "failureCount": 0,
   "faultedCount": 0
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Karmaşık zamanlamalar ve gelişmiş yinelemeler oluşturma](scheduler-advanced-complexity.md)
* [Azure Scheduler REST API başvurusu](/rest/api/scheduler)
* [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)
* [Sınırlar, kotalar, varsayılan değerler ve hata kodları](scheduler-limits-defaults-errors.md)
