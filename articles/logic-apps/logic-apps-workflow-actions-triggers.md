---
title: Iş akışı Tanım dilindeki tetikleyici ve eylem türleri için başvuru-Azure Logic Apps
description: Azure Logic Apps için Iş akışı Tanım dilindeki tetikleyici ve eylem türleri için başvuru kılavuzu
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: reference
ms.date: 06/19/2019
ms.openlocfilehash: df1b03d5fbb5b8ef8cda9407e4a595bc2de8ce54
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70918951"
---
# <a name="reference-for-trigger-and-action-types-in-workflow-definition-language-for-azure-logic-apps"></a>Azure Logic Apps için Iş akışı Tanım dilindeki tetikleyici ve eylem türleri için başvuru

Bu başvuru, mantıksal uygulamanızın [Iş akışı Tanım Dili](../logic-apps/logic-apps-workflow-definition-language.md)tarafından tanımlanan ve doğrulanan temel alınan iş akışı tanımındaki Tetikleyicileri ve eylemleri tanımlamak için kullanılan genel türleri açıklar.
Mantıksal uygulamalarınızda kullanabileceğiniz belirli bağlayıcı tetikleyicilerini ve eylemlerini bulmak için, [Bağlayıcılar genel bakış](https://docs.microsoft.com/connectors/)altındaki listeye bakın.

<a name="triggers-overview"></a>

## <a name="triggers-overview"></a>Tetikleyicilere genel bakış

Her iş akışı, iş akışını örnekleyip Başlatan çağrıları tanımlayan bir tetikleyici içerir. Genel tetikleyici kategorileri şunlardır:

* Düzenli aralıklarla bir hizmetin uç noktasını denetleyen bir *yoklama* tetikleyicisi

* Uç nokta için bir abonelik oluşturan ve bir geri çağırma URL 'si sağlayan bir *gönderme* tetikleyicisi, belirtilen olay gerçekleştiğinde veya veriler kullanılabilir olduğunda bitiş noktasının tetikleyiciyi bildirebilmesi için bir *geri arama URL 'si* sağlar. Tetikleyici daha sonra, başlamadan önce uç noktanın yanıtını bekler. 

Tetikleyiciler, bazı isteğe bağlı olsa da, bu en üst düzey öğelere sahiptir:  
  
```json
"<trigger-name>": {
   "type": "<trigger-type>",
   "inputs": { "<trigger-inputs>" },
   "recurrence": { 
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>
   },
   "conditions": [ "<array-with-conditions>" ],
   "runtimeConfiguration": { "<runtime-config-options>" },
   "splitOn": "<splitOn-expression>",
   "operationOptions": "<operation-option>"
},
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*trigger-name*> | String | Tetikleyicinin adı | 
| <*trigger-type*> | String | "Http" veya "ApiConnection" gibi tetikleyici türü | 
| <*trigger-inputs*> | JSON nesnesi | Tetikleyicinin davranışını tanımlayan girişler | 
| <*time-unit*> | String | Tetikleyicinin ne sıklıkta çalıştığını açıklayan zaman birimi: "Saniye", "dakika", "saat", "gün", "hafta", "ay" | 
| <*number-of-time-units*> | Integer | Tetikleyicinin yeniden tetiklenmesi için bekleyeceği zaman birimi sayısı olan sıklık temelinde tetikleyicinin ne sıklıkta tetikleyeceğini belirten bir değer <p>En düşük ve en büyük aralıklar aşağıda verilmiştir: <p>Başından 1-16 ay </br>Günündeki 1-500 gün </br>Saate 1-12000 saat </br>Dakikaya 1-72000 dakika </br>İkincisi 1-9999999 saniye<p>Örneğin, Aralık 6, sıklık ise "month" ise, yineleme 6 aydır. | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*array-with-conditions*> | Array | İş akışının çalıştırılıp çalıştırılmayacağını tespit eden bir veya daha fazla [koşul](#trigger-conditions) içeren bir dizi. Yalnızca Tetikleyiciler için kullanılabilir. | 
| <*runtime-config-options*> | JSON nesnesi | Özellikleri ayarlayarak `runtimeConfiguration` tetikleyici çalışma zamanı davranışını değiştirebilirsiniz. Daha fazla bilgi için bkz. [çalışma zamanı yapılandırma ayarları](#runtime-config-options). | 
| <*Spton-ifadesi*> | String | Dizi döndüren Tetikleyiciler için, dizi öğelerini işlemek üzere birden çok iş akışı örneğine [ayıran veya](#split-on-debatch) içermeyen bir ifade belirtebilirsiniz. | 
| <*operation-option*> | String | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için bkz. [işlem seçenekleri](#operation-options). | 
|||| 

## <a name="trigger-types-list"></a>Tetikleyici türleri listesi

Her tetikleyici türünün, tetikleyicisinin davranışını tanımlayan farklı bir arabirimi ve girişleri vardır. 

### <a name="built-in-triggers"></a>Yerleşik Tetikleyiciler

| Tetikleyici türü | Açıklama | 
|--------------|-------------| 
| [**HTTP**](#http-trigger) | Tüm uç noktaları denetler veya *yoklar* . Bu uç noktanın, bir "202" zaman uyumsuz düzeni kullanılarak veya bir dizi döndürerek belirli bir tetikleyici sözleşmesine uyması gerekir. | 
| [**HTTPWebhook**](#http-webhook-trigger) | Mantıksal uygulamanız için çağrılabilir bir uç nokta oluşturur, ancak kayıt veya kayıt silme için belirtilen URL 'YI çağırır. |
| [**Recurrence**](#recurrence-trigger) | Tanımlı bir zamanlamaya göre ateşlenir. Bu tetikleyiciyi tetikleyebilmeniz için gelecek bir tarih ve saat ayarlayabilirsiniz. Sıklık temelinde, iş akışınızı çalıştırmak için saatler ve günler de belirtebilirsiniz. | 
| [**Request**](#request-trigger)  | Mantıksal uygulamanız için çağrılabilir bir uç nokta oluşturur ve "el ile" tetikleyicisi olarak da bilinir. Örneğin, bkz. [http uç noktaları Ile çağrı, tetikleyici veya iç içe iş akışları](../logic-apps/logic-apps-http-endpoint.md). | 
||| 

### <a name="managed-api-triggers"></a>Yönetilen API Tetikleyicileri

| Tetikleyici türü | Açıklama | 
|--------------|-------------| 
| [**ApiConnection**](#apiconnection-trigger) | Bir uç noktayı [Microsoft tarafından yönetilen API 'ler](../connectors/apis-list.md)kullanarak denetler veya *yoklar* . | 
| [**ApiConnectionWebhook**](#apiconnectionwebhook-trigger) | Abone ol ve abonelik kaldırma için [Microsoft tarafından yönetilen API 'ler](../connectors/apis-list.md) çağırarak mantıksal uygulamanız için çağrılabilir bir uç nokta oluşturur. | 
||| 

## <a name="triggers---detailed-reference"></a>Tetikleyiciler-ayrıntılı başvuru

<a name="apiconnection-trigger"></a>

### <a name="apiconnection-trigger"></a>APIConnection tetikleyicisi  

Bu tetikleyici, [Microsoft tarafından yönetilen API 'ler](../connectors/apis-list.md) kullanarak bir uç noktayı denetler veya *yoklar* ve bu tetikleyicinin parametreleri uç noktaya göre farklılık gösterebilir. Bu tetikleyici tanımındaki birçok bölüm isteğe bağlıdır. Tetikleyicinin davranışı bölümlerin dahil edilip edilmeyeceğini gösterir.

```json
"<APIConnection_trigger_name>": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<connection-name>']['connectionId']"
         }
      },
      "method": "<method-type>",
      "path": "/<api-operation>",
      "retryPolicy": { "<retry-behavior>" },
      "queries": { "<query-parameters>" }
   },
   "recurrence": { 
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "splitOn": "<splitOn-expression>",
   "operationOptions": "<operation-option>"
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*APIConnection_trigger_name*> | String | Tetikleyicinin adı | 
| <*bağlantı adı*> | String | İş akışının kullandığı yönetilen API bağlantısı için ad | 
| <*Yöntem-tür*> | String | Yönetilen API ile iletişim kurmak için HTTP yöntemi: "AL", "PUT", "POST", "PATCH", "DELETE" | 
| <*api-işlem*> | String | Çağrılacak API işlemi | 
| <*time-unit*> | String | Tetikleyicinin ne sıklıkta çalıştığını açıklayan zaman birimi: "Saniye", "dakika", "saat", "gün", "hafta", "ay" | 
| <*number-of-time-units*> | Integer | Tetikleyicinin yeniden tetiklenmesi için bekleyeceği zaman birimi sayısı olan sıklık temelinde tetikleyicinin ne sıklıkta tetikleyeceğini belirten bir değer <p>En düşük ve en büyük aralıklar aşağıda verilmiştir: <p>Başından 1-16 ay </br>Günündeki 1-500 gün </br>Saate 1-12000 saat </br>Dakikaya 1-72000 dakika </br>İkincisi 1-9999999 saniye<p>Örneğin, Aralık 6, sıklık ise "month" ise, yineleme 6 aydır. | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum koduna ve tüm bağlantı özel durumlarına sahip aralıklı olmayan hatalara yönelik yeniden deneme davranışını özelleştirir. Daha fazla bilgi için bkz. [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*sorgu-Parametreler*> | JSON nesnesi | API çağrısıyla birlikte içerilecek herhangi bir sorgu parametresi. Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi çağrıya ekler `?api-version=2018-01-01` . | 
| <*en fazla çalıştırma*> | Integer | Varsayılan olarak, iş akışı örnekleri aynı anda veya [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)paralel olarak çalışır. Yeni bir <*sayı*> değeri ayarlayarak bu sınırı değiştirmek için bkz. [tetikleyici eşzamanlılık değiştirme](#change-trigger-concurrency). | 
| <*en fazla çalıştırma-kuyruk*> | Integer | İş akışınız, `runtimeConfiguration.concurrency.runs` özelliği temel alarak değiştirebileceğiniz en fazla örnek sayısını zaten çalıştırıyorsa, tüm yeni çalıştırmalar bu kuyruğa [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar yerleştirilir. Varsayılan sınırı değiştirmek için bkz. [değişiklik bekleyen çalışma sınırı](#change-waiting-runs). | 
| <*Spton-ifadesi*> | String | Bu ifade, diziler döndüren Tetikleyiciler için, "for each" döngüsünü kullanmak yerine her bir dizi öğesi için bir iş akışı örneği oluşturup çalıştırabilmeniz için kullanılacak diziye başvurur. <p>Örneğin, bu ifade, tetikleyicinin gövde içeriği içinde döndürülen dizideki bir öğeyi temsil eder:`@triggerbody()?['value']` |
| <*operation-option*> | String | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için bkz. [işlem seçenekleri](#operation-options). |
||||

*Çıkışlar*
 
| Öğe | Type | Açıklama |
|---------|------|-------------|
| Bilgisinde | JSON nesnesi | Yanıttaki üstbilgiler |
| body | JSON nesnesi | Yanıttaki gövde |
| Durum kodu | Integer | Yanıttaki durum kodu |
|||| 

*Örnek*

Bu tetikleyici tanımı, bir Office 365 Outlook hesabı için gelen kutusu içinde her gün e-postayı denetler: 

```json
"When_a_new_email_arrives": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "method": "get",
      "path": "/Mail/OnNewEmail",
      "queries": {
          "fetchOnlyWithAttachment": false,
          "folderPath": "Inbox",
          "importance": "Any",
          "includeAttachments": false
      }
   },
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

<a name="apiconnectionwebhook-trigger"></a>

### <a name="apiconnectionwebhook-trigger"></a>Apiconnectionweb kancası tetikleyicisi

Bu tetikleyici, [Microsoft tarafından yönetilen BIR API](../connectors/apis-list.md)kullanarak bir uç noktaya bir abonelik isteği gönderir, uç noktanın yanıt gönderebildiği ve bitiş noktasının yanıt vermesini beklediği bir *geri çağırma URL 'si* sağlar. Daha fazla bilgi için bkz. [Endpoint abonelikleri](#subscribe-unsubscribe).

```json
"<ApiConnectionWebhook_trigger_name>": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "body": {
          "NotificationUrl": "@{listCallbackUrl()}"
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<connection-name>']['connectionId']"
         }
      },
      "retryPolicy": { "<retry-behavior>" },
      "queries": "<query-parameters>"
   },
   "runTimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-run-queue>
      }
   },
   "splitOn": "<splitOn-expression>",
   "operationOptions": "<operation-option>"
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*bağlantı adı*> | String | İş akışının kullandığı yönetilen API bağlantısı için ad | 
| <*gövde-içerik*> | JSON nesnesi | Yönetilen API 'ye yük olarak göndermek için herhangi bir ileti içeriği | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum koduna ve tüm bağlantı özel durumlarına sahip aralıklı olmayan hatalara yönelik yeniden deneme davranışını özelleştirir. Daha fazla bilgi için bkz. [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*sorgu-Parametreler*> | JSON nesnesi | API çağrısıyla birlikte içerilecek sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi çağrıya ekler `?api-version=2018-01-01` . | 
| <*en fazla çalıştırma*> | Integer | Varsayılan olarak, iş akışı örnekleri aynı anda veya [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)paralel olarak çalışır. Yeni bir <*sayı*> değeri ayarlayarak bu sınırı değiştirmek için bkz. [tetikleyici eşzamanlılık değiştirme](#change-trigger-concurrency). | 
| <*en fazla çalıştırma-kuyruk*> | Integer | İş akışınız, `runtimeConfiguration.concurrency.runs` özelliği temel alarak değiştirebileceğiniz en fazla örnek sayısını zaten çalıştırıyorsa, tüm yeni çalıştırmalar bu kuyruğa [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar yerleştirilir. Varsayılan sınırı değiştirmek için bkz. [değişiklik bekleyen çalışma sınırı](#change-waiting-runs). | 
| <*Spton-ifadesi*> | String | Bu ifade, diziler döndüren Tetikleyiciler için, "for each" döngüsünü kullanmak yerine her bir dizi öğesi için bir iş akışı örneği oluşturup çalıştırabilmeniz için kullanılacak diziye başvurur. <p>Örneğin, bu ifade, tetikleyicinin gövde içeriği içinde döndürülen dizideki bir öğeyi temsil eder:`@triggerbody()?['value']` |
| <*operation-option*> | String | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için bkz. [işlem seçenekleri](#operation-options). | 
|||| 

*Örnek*

Bu tetikleyici tanımı Office 365 Outlook API 'sine abone olur, API uç noktası için bir geri çağırma URL 'SI sağlar ve yeni bir e-posta geldiğinde bitiş noktasının yanıt vermesini bekler.

```json
"When_a_new_email_arrives_(webhook)": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "body": {
         "NotificationUrl": "@{listCallbackUrl()}" 
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "path": "/MailSubscription/$subscriptions",
      "queries": {
          "folderPath": "Inbox",
          "hasAttachment": "Any",
          "importance": "Any"
      }
   },
   "splitOn": "@triggerBody()?['value']"
}
```

<a name="http-trigger"></a>

### <a name="http-trigger"></a>HTTP tetikleyicisi

Bu tetikleyici belirtilen bitiş noktasını belirtilen yinelenme zamanlaması temelinde denetler veya yoklar. Uç noktanın yanıtı, iş akışının çalışıp çalışmadığını belirler.

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "<method-type>",
      "uri": "<endpoint-URL>",
      "headers": { "<header-content>" },
      "body": "<body-content>",
      "authentication": { "<authentication-method>" },
      "retryPolicy": { "<retry-behavior>" },
      "queries": "<query-parameters>"
   },
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "operationOptions": "<operation-option>"
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*Yöntem-tür*> | String | Belirtilen uç noktayı yoklamak için kullanılacak HTTP yöntemi: "AL", "PUT", "POST", "PATCH", "DELETE" | 
| <*uç nokta-URL*> | String | Yoklamaya bitiş noktası için HTTP veya HTTPS URL 'SI <p>En büyük dize boyutu: 2 KB | 
| <*time-unit*> | String | Tetikleyicinin ne sıklıkta çalıştığını açıklayan zaman birimi: "Saniye", "dakika", "saat", "gün", "hafta", "ay" | 
| <*number-of-time-units*> | Integer | Tetikleyicinin yeniden tetiklenmesi için bekleyeceği zaman birimi sayısı olan sıklık temelinde tetikleyicinin ne sıklıkta tetikleyeceğini belirten bir değer <p>En düşük ve en büyük aralıklar aşağıda verilmiştir: <p>Başından 1-16 ay </br>Günündeki 1-500 gün </br>Saate 1-12000 saat </br>Dakikaya 1-72000 dakika </br>İkincisi 1-9999999 saniye<p>Örneğin, Aralık 6, sıklık ise "month" ise, yineleme 6 aydır. | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*üst bilgi-içerik*> | JSON nesnesi | İstekle birlikte gönderilen üstbilgiler <p>Örneğin, bir isteğin dilini ve türünü ayarlamak için: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*gövde-içerik*> | String | İstekle birlikte yük olarak gönderilmek üzere ileti içeriği | 
| <*Authentication-yöntemi*> | JSON nesnesi | İsteğin kimlik doğrulaması için kullandığı yöntem. Daha fazla bilgi için bkz. [Scheduler giden kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). Zamanlayıcı ötesinde, `authority` Özellik desteklenir. Belirtilmediğinde, varsayılan değer `https://login.windows.net`, ancak gibi farklı bir değer`https://login.windows\-ppe.net`de kullanabilirsiniz. |
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum koduna ve tüm bağlantı özel durumlarına sahip aralıklı olmayan hatalara yönelik yeniden deneme davranışını özelleştirir. Daha fazla bilgi için bkz. [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). |  
 <*sorgu-Parametreler*> | JSON nesnesi | İsteğe dahil edilecek herhangi bir sorgu parametresi <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesne isteğe ekler `?api-version=2018-01-01` . | 
| <*en fazla çalıştırma*> | Integer | Varsayılan olarak, iş akışı örnekleri aynı anda veya [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)paralel olarak çalışır. Yeni bir <*sayı*> değeri ayarlayarak bu sınırı değiştirmek için bkz. [tetikleyici eşzamanlılık değiştirme](#change-trigger-concurrency). | 
| <*en fazla çalıştırma-kuyruk*> | Integer | İş akışınız, `runtimeConfiguration.concurrency.runs` özelliği temel alarak değiştirebileceğiniz en fazla örnek sayısını zaten çalıştırıyorsa, tüm yeni çalıştırmalar bu kuyruğa [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar yerleştirilir. Varsayılan sınırı değiştirmek için bkz. [değişiklik bekleyen çalışma sınırı](#change-waiting-runs). | 
| <*operation-option*> | String | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için bkz. [işlem seçenekleri](#operation-options). | 
|||| 

*Çıkışlar*

| Öğe | Type | Açıklama |
|---------|------|-------------| 
| Bilgisinde | JSON nesnesi | Yanıttaki üstbilgiler | 
| body | JSON nesnesi | Yanıttaki gövde | 
| Durum kodu | Integer | Yanıttaki durum kodu | 
|||| 

*Gelen istekler için gereksinimler*

Mantıksal uygulamanızla iyi çalışmak için uç noktanın belirli bir tetikleyici düzenine veya sözleşmesine uyması ve bu özellikleri tanıması gerekir:  
  
| Yanıt | Gerekli | Açıklama | 
|----------|----------|-------------| 
| Durum kodu | Evet | "200 OK" durum kodu bir çalıştırma başlatır. Başka herhangi bir durum kodu çalıştırma başlamaz. | 
| Retry-After üst bilgisi | Hayır | Mantıksal uygulamanız bitiş noktasını yeniden yokladığı saniye sayısı | 
| Konum üst bilgisi | Hayır | Sonraki yoklama aralığında çağrılacak URL. Belirtilmemişse, özgün URL kullanılır. | 
|||| 

*Farklı istekler için örnek davranışlar*

| Durum kodu | Sonra yeniden dene | Davranış | 
|-------------|-------------|----------|
| 200 | seçim | İş akışını çalıştırın, sonra tanımlanan tekrardan sonra daha fazla veri için yeniden denetleyin. | 
| 200 | 10 saniye | İş akışını çalıştırın, sonra 10 saniye sonra daha fazla veri için yeniden kontrol edin. |  
| 202 | 60 saniye | İş akışını tetiklemez. Sonraki deneme bir dakika içinde gerçekleşir ve tanımlanan tekrarya tabidir. Tanımlanan yinelenme bir dakikadan azsa, yeniden deneme üst bilgisi önceliklidir. Aksi takdirde, tanımlanan yinelenme kullanılır. | 
| 400 | seçim | Hatalı istek, iş akışını çalıştırmayın. `retryPolicy` Eğer tanımlanmamışsa, varsayılan ilke kullanılır. Yeniden deneme sayısına ulaşıldığında, tetikleyici tanımlanan tekrardan sonra verileri yeniden denetler. | 
| 500 | seçim| Sunucu hatası, iş akışını çalıştırmayın. `retryPolicy` Eğer tanımlanmamışsa, varsayılan ilke kullanılır. Yeniden deneme sayısına ulaşıldığında, tetikleyici tanımlanan tekrardan sonra verileri yeniden denetler. | 
|||| 

<a name="http-webhook-trigger"></a>

### <a name="httpwebhook-trigger"></a>Httpweb kancası tetikleyicisi  

Bu tetikleyici, belirtilen uç nokta URL 'sini çağırarak bir aboneliği kaydedebilmesi için bir uç nokta oluşturarak mantıksal uygulamanızı çağrılabilir hale getirir. Bu tetikleyiciyi iş akışınızda oluşturduğunuzda, giden bir istek aboneliği kaydetme çağrısını yapar. Bu şekilde, tetikleyici olayları dinlemeye başlayabilir. Bir işlem bu tetikleyiciyi geçersiz yaptığında, bir giden istek otomatik olarak aboneliğini iptal etme çağrısını yapar. Daha fazla bilgi için bkz. [Endpoint abonelikleri](#subscribe-unsubscribe).

Ayrıca, bir **Httpweb kancası** tetikleyicisi üzerinde [zaman uyumsuz sınırlar](#asynchronous-limits) belirtebilirsiniz.
Tetikleyicinin davranışı, kullandığınız veya atladığınızda bölümlere bağlıdır. 

```json
"HTTP_Webhook": {
   "type": "HttpWebhook",
   "inputs": {
      "subscribe": {
         "method": "<method-type>",
         "uri": "<endpoint-subscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" },
         "retryPolicy": { "<retry-behavior>" }
         },
      },
      "unsubscribe": {
         "method": "<method-type>",
         "url": "<endpoint-unsubscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" }
      }
   },
   "runTimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "operationOptions": "<operation-option>"
}
```

<*Method-Type*> gibi bazı değerler hem hem de `"subscribe"` `"unsubscribe"` nesneleri için kullanılabilir.

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*Yöntem-tür*> | String | Abonelik isteği için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "PATCH" veya "DELETE" | 
| <*uç nokta-abone ol-URL*> | String | Abonelik isteğinin gönderileceği uç nokta URL 'SI | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*Yöntem-tür*> | String | İptal isteği için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "PATCH" veya "DELETE" | 
| <*uç nokta-abonelik-URL*> | String | İptal isteğinin gönderileceği uç nokta URL 'SI | 
| <*gövde-içerik*> | String | Abonelik veya iptal isteğinde göndermek için herhangi bir ileti içeriği | 
| <*Authentication-yöntemi*> | JSON nesnesi | İsteğin kimlik doğrulaması için kullandığı yöntem. Daha fazla bilgi için bkz. [Scheduler giden kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). |
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum koduna ve tüm bağlantı özel durumlarına sahip aralıklı olmayan hatalara yönelik yeniden deneme davranışını özelleştirir. Daha fazla bilgi için bkz. [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*en fazla çalıştırma*> | Integer | Varsayılan olarak, iş akışı örneklerinin hepsi aynı anda çalışır veya [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar paralel olarak yapılır. Yeni bir <*sayı*> değeri ayarlayarak bu sınırı değiştirmek için bkz. [tetikleyici eşzamanlılık değiştirme](#change-trigger-concurrency). | 
| <*en fazla çalıştırma-kuyruk*> | Integer | İş akışınız, `runtimeConfiguration.concurrency.runs` özelliği temel alarak değiştirebileceğiniz en fazla örnek sayısını zaten çalıştırıyorsa, tüm yeni çalıştırmalar bu kuyruğa [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar yerleştirilir. Varsayılan sınırı değiştirmek için bkz. [değişiklik bekleyen çalışma sınırı](#change-waiting-runs). | 
| <*operation-option*> | String | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için bkz. [işlem seçenekleri](#operation-options). | 
|||| 

*Çıkışlar* 

| Öğe | Type | Açıklama |
|---------|------|-------------| 
| Bilgisinde | JSON nesnesi | Yanıttaki üstbilgiler | 
| body | JSON nesnesi | Yanıttaki gövde | 
| Durum kodu | Integer | Yanıttaki durum kodu | 
|||| 

*Örnek*

Bu tetikleyici, belirtilen uç noktaya bir abonelik oluşturur, benzersiz bir geri çağırma URL 'SI sağlar ve yeni yayımlanmış teknoloji makalelerini bekler.

```json
"HTTP_Webhook": {
   "type": "HttpWebhook",
   "inputs": {
      "subscribe": {
         "method": "POST",
         "uri": "https://pubsubhubbub.appspot.com/subscribe",
         "body": {
            "hub.callback": "@{listCallbackUrl()}",
            "hub.mode": "subscribe",
            "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
         },
      },
      "unsubscribe": {
         "method": "POST",
         "url": "https://pubsubhubbub.appspot.com/subscribe",
         "body": {
            "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
            "hub.mode": "unsubscribe",
            "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
         }
      }
   }
}
```

<a name="recurrence-trigger"></a>

### <a name="recurrence-trigger"></a>Yinelenme tetikleyicisi  

Bu tetikleyici, belirtilen yinelenme zamanlaması temelinde çalışır ve düzenli olarak çalışan bir iş akışı oluşturmak için kolay bir yol sağlar. 

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
      "startTime": "<start-date-time-with-format-YYYY-MM-DDThh:mm:ss>",
      "timeZone": "<time-zone>",
      "schedule": {
         // Applies only when frequency is Day or Week. Separate values with commas.
         "hours": [ <one-or-more-hour-marks> ], 
         // Applies only when frequency is Day or Week. Separate values with commas.
         "minutes": [ <one-or-more-minute-marks> ], 
         // Applies only when frequency is Week. Separate values with commas.
         "weekDays": [ "Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" ] 
      }
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "operationOptions": "<operation-option>"
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*time-unit*> | String | Tetikleyicinin ne sıklıkta çalıştığını açıklayan zaman birimi: "Saniye", "dakika", "saat", "gün", "hafta", "ay" | 
| <*number-of-time-units*> | Integer | Tetikleyicinin yeniden tetiklenmesi için bekleyeceği zaman birimi sayısı olan sıklık temelinde tetikleyicinin ne sıklıkta tetikleyeceğini belirten bir değer <p>En düşük ve en büyük aralıklar aşağıda verilmiştir: <p>Başından 1-16 ay </br>Günündeki 1-500 gün </br>Saate 1-12000 saat </br>Dakikaya 1-72000 dakika </br>İkincisi 1-9999999 saniye<p>Örneğin, Aralık 6, sıklık ise "month" ise, yineleme 6 aydır. | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*başlangıç-tarih-saat--biçimlendirme-YYYY-MM-DDThh: mm: ss*> | String | Bu biçimdeki başlangıç tarihi ve saati: <p>YYYY-MM-DDThh: mm: ss saat dilimi belirtirseniz <p>-veya- <p>YYYY-MM-DDThh: mm: ssZ saat dilimi belirtmezseniz <p>Örneğin, 18 Eylül 2017, 2:00 PM üzerinde istiyorsanız, "2017-09-18T14:00:00" belirtin ve "Pasifik Standart Saati" gibi bir saat dilimi belirtin ya da saat dilimi olmadan "2017-09-18T14:00:00Z" belirtin. <p>**Not:** Bu başlangıç saati, gelecekte en fazla 49 yıla sahiptir ve UTC [8601 tarih saat belirtimini](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) UTC [Tarih saat biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time)izlemelidir, ancak [UTC bir fark](https://en.wikipedia.org/wiki/UTC_offset)olmadan gelmelidir. Saat dilimi belirtmezseniz, sonunda boşluk olmadan "Z" harfini eklemeniz gerekir. Bu "Z", eşdeğer [nadeniz saati](https://en.wikipedia.org/wiki/Nautical_time)anlamına gelir. <p>Basit zamanlamalar için başlangıç zamanı ilk oluşumdır, ancak karmaşık zamanlamalar için tetikleyici başlangıç zamanından daha önce harekete geçmez. Başlangıç tarihleri ve zamanları hakkında daha fazla bilgi için bkz. [düzenli olarak çalışan görevler oluşturma ve zamanlama](../connectors/connectors-native-recurrence.md). | 
| <*Saat dilimi*> | String | Yalnızca bir başlangıç saati belirttiğinizde geçerlidir çünkü bu tetikleyici [UTC sapmasını](https://en.wikipedia.org/wiki/UTC_offset)kabul etmez. Uygulamak istediğiniz saat dilimini belirtin. | 
| <*bir veya daha çok saat-işareti*> | Tamsayı veya tamsayı dizisi | İçin `frequency`"gün" veya "hafta" belirtirseniz, iş akışını çalıştırmak istediğiniz günün saatleri olarak virgülle ayırarak 0 ile 23 arasında bir veya daha fazla tamsayı belirtebilirsiniz. <p>Örneğin, "10", "12" ve "14" belirtirseniz, saat işaretleri olarak 10 har, 12 PM ve 2 PM alırsınız. | 
| <*bir veya daha fazla-dakika-işareti*> | Tamsayı veya tamsayı dizisi | İçin `frequency`"gün" veya "hafta" belirtirseniz, bir veya daha fazla tamsayı belirterek, iş akışını çalıştırmak istediğiniz saatin dakikası olarak virgülle ayırarak 0 ile 59 arasında bir veya daha fazla tamsayı belirtebilirsiniz. <p>Örneğin, "30" öğesini dakika işareti olarak belirtebilir ve günün saati için önceki örneği kullanarak 10:30, 12:30 PM ve 2:30 PM kazanın. | 
| weekDays | Dize veya dize dizisi | İçin `frequency`"hafta" belirtirseniz, iş akışını çalıştırmak istediğinizde, virgülle ayırarak bir veya daha fazla gün belirtebilirsiniz: "Pazartesi", "Salı", "Çarşamba", "Perşembe", "Cuma", "Cumartesi" ve "Pazar" | 
| <*en fazla çalıştırma*> | Integer | Varsayılan olarak, iş akışı örneklerinin hepsi aynı anda çalışır veya [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar paralel olarak yapılır. Yeni bir <*sayı*> değeri ayarlayarak bu sınırı değiştirmek için bkz. [tetikleyici eşzamanlılık değiştirme](#change-trigger-concurrency). | 
| <*en fazla çalıştırma-kuyruk*> | Integer | İş akışınız, `runtimeConfiguration.concurrency.runs` özelliği temel alarak değiştirebileceğiniz en fazla örnek sayısını zaten çalıştırıyorsa, tüm yeni çalıştırmalar bu kuyruğa [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar yerleştirilir. Varsayılan sınırı değiştirmek için bkz. [değişiklik bekleyen çalışma sınırı](#change-waiting-runs). | 
| <*operation-option*> | String | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için bkz. [işlem seçenekleri](#operation-options). | 
|||| 

*Örnek 1*

Bu temel yinelenme tetikleyicisi her gün çalışır:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

*Örnek 2*

Tetikleyicinin tetiklenmesi için bir başlangıç tarihi ve saati belirtebilirsiniz. Bu yineleme tetikleyicisi belirtilen tarihte başlar ve sonra günlük olarak başlatılır:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1,
      "startTime": "2017-09-18T00:00:00Z"
   }
}
```

*Örnek 3*

Bu yineleme tetikleyicisi, 9 Eylül 2017 ' de 2:00 PM tarihinde başlar ve her Pazartesi, 10:30, 12:30 ve 2:30 Pasifik standart saati ile haftada bir saatte bir ateşlenir:

``` json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "hours": [ 10, 12, 14 ],
         "minutes": [ 30 ],
         "weekDays": [ "Monday" ]
      },
      "startTime": "2017-09-07T14:00:00",
      "timeZone": "Pacific Standard Time"
   }
}
```

Daha fazla bilgi ve bu tetikleyicinin örnekleri için bkz. [düzenli olarak çalışan görevler oluşturma ve zamanlama](../connectors/connectors-native-recurrence.md).

<a name="request-trigger"></a>

### <a name="request-trigger"></a>İstek tetikleyicisi

Bu tetikleyici, gelen istekleri kabul edebilecek bir uç nokta oluşturarak mantıksal uygulamanızı çağrılabilir hale getirir. Bu tetikleyici için, tetikleyicinin gelen istekten aldığı yük veya girişleri açıklayan ve doğrulayan bir JSON şeması sağlayın. Şema ayrıca tetikleyici özelliklerini iş akışındaki sonraki eylemlerden daha kolay başvuruya de olanak sağlar. 

Bu tetikleyiciyi çağırmak için, `listCallbackUrl` [iş akışı hizmeti REST API](https://docs.microsoft.com/rest/api/logic/workflows)açıklanan API 'yi kullanmanız gerekir. Bu tetikleyiciyi bir HTTP uç noktası olarak kullanmayı öğrenmek için bkz. [http uç noktaları Ile çağrı, tetikleyici veya iç içe iş akışları](../logic-apps/logic-apps-http-endpoint.md).

```json
"manual": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "<method-type>",
      "relativePath": "<relative-path-for-accepted-parameter>",
      "schema": {
         "type": "object",
         "properties": { 
            "<property-name>": {
               "type": "<property-type>"
            }
         },
         "required": [ "<required-properties>" ]
      }
   },
   "runTimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-run-queue>
      },
   },
   "operationOptions": "<operation-option>"
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*Özellik adı*> | String | Yükü açıklayan JSON şemasında bir özelliğin adı | 
| <*Özellik türü*> | String | Özelliğin türü | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*Yöntem-tür*> | String | Gelen isteklerin mantıksal uygulamanızı çağırmak için kullanması gereken Yöntem: "AL", "PUT", "POST", "PATCH", "DELETE" |
| <*göreli-yol-for-kabul parametresi*> | String | Endpoint URL 'nizin kabul edebileceği parametrenin göreli yolu | 
| <*gerekli-özellikler*> | Array | Değer gerektiren bir veya daha fazla özellik | 
| <*en fazla çalıştırma*> | Integer | Varsayılan olarak, iş akışı örneklerinin hepsi aynı anda çalışır veya [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar paralel olarak yapılır. Yeni bir <*sayı*> değeri ayarlayarak bu sınırı değiştirmek için bkz. [tetikleyici eşzamanlılık değiştirme](#change-trigger-concurrency). | 
| <*en fazla çalıştırma-kuyruk*> | Integer | İş akışınız, `runtimeConfiguration.concurrency.runs` özelliği temel alarak değiştirebileceğiniz en fazla örnek sayısını zaten çalıştırıyorsa, tüm yeni çalıştırmalar bu kuyruğa [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar yerleştirilir. Varsayılan sınırı değiştirmek için bkz. [değişiklik bekleyen çalışma sınırı](#change-waiting-runs). | 
| <*operation-option*> | String | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için bkz. [işlem seçenekleri](#operation-options). | 
|||| 

*Örnek*

Bu tetikleyici, gelen bir isteğin tetikleyiciyi çağırmak için HTTP POST metodunu kullanması gerektiğini ve gelen istekten gelen girişi doğrulayan bir şema içerdiğini belirtir: 

```json
"manual": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "POST",
      "schema": {
         "type": "object",
         "properties": {
            "customerName": {
               "type": "String"
            },
            "customerAddress": { 
               "type": "Object",
               "properties": {
                  "streetAddress": {
                     "type": "string"
                  },
                  "city": {
                     "type": "string"
                  }
               }
            }
         }
      }
   }
}
```

<a name="trigger-conditions"></a>

## <a name="trigger-conditions"></a>Tetikleme koşulları

Herhangi bir tetikleyici ve yalnızca Tetikleyiciler için, iş akışının çalıştırılıp çalıştırılmayacağını tespit eden koşullar için bir veya daha fazla ifade içeren bir dizi ekleyebilirsiniz. `conditions` Özelliği iş akışınızda bir tetikleyiciye eklemek için, kod görünümü düzenleyicisinde mantıksal uygulamanızı açın.

Örneğin, bir tetikleyicinin yalnızca bir Web sitesi bir iç sunucu hatası döndürdüğünde, `conditions` tetikleyicisinin özelliğindeki durum koduna başvurarak bunu tetikleyebelirtebilirsiniz:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Hour",
      "interval": 1
   },
   "conditions": [ {
      "expression": "@equals(triggers().code, 'InternalServerError')"
   } ]
}
```

Varsayılan olarak, bir tetikleyici yalnızca bir "200 Tamam" yanıtı alındıktan sonra ateşlenir. Bir ifade bir tetikleyicinin durum koduna başvurduğunda, tetikleyicinin varsayılan davranışı değiştirilmiştir. Bu nedenle, tetikleyicinin "200" ve "201" durum kodu gibi birden fazla durum kodu için tetiklemeyi istiyorsanız, bu ifadeyi koşulunuz olarak dahil etmeniz gerekir: 

`@or(equals(triggers().code, 200),equals(triggers().code, 201))` 

<a name="split-on-debatch"></a>

## <a name="trigger-multiple-runs"></a>Birden çok çalıştırma Tetikle

Tetikleyiciniz mantıksal uygulamanızın işlemesi için bir dizi döndürürse, bazen "for each" döngüsünün her bir dizi öğesini işlemesi çok uzun sürebilir. Bunun yerine, diziyi *toplu* olarak bırakmak için Tetikleyicinizdeki **spton** özelliğini kullanabilirsiniz. Toplu işleme, dizi öğelerini böler ve her dizi öğesi için çalışan yeni bir iş akışı örneği başlatır. Bu yaklaşım, örneğin yoklama aralıkları arasında birden çok yeni öğe döndürebilecek bir uç noktayı yoklamak istediğinizde yararlıdır.
Tek bir mantıksal uygulama çalıştırmasında, **Spton** 'un işleyebilmesine izin verilen en fazla dizi öğesi sayısı için bkz. [sınırlar ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). 

> [!NOTE]
> Zaman uyumlu bir yanıt düzeniyle **Spton** kullanamazsınız. Tüm iş **akışları ve bir** yanıt eylemi, zaman uyumsuz olarak çalışır ve anında `202 ACCEPTED` yanıt gönderir.

Tetikleyicinizin Swagger dosyasında bir dizi olan bir yük varsa, tetikleyicisine otomatik olarak **Spton** özelliği eklenir. Aksi takdirde, bu özelliği, toplu işlem yapmak istediğiniz diziyi içeren yanıt yükünün içine ekleyin. 

*Örnek*

Bu yanıtı döndüren bir API olduğunu varsayalım: 
  
```json
{
   "Status": "Succeeded",
   "Rows": [ 
      { 
         "id": 938109380,
         "name": "customer-name-one"
      },
      {
         "id": 938109381,
         "name": "customer-name-two"
      }
   ]
}
```
 
Mantıksal uygulamanız yalnızca içindeki `Rows`dizideki içeriğe ihtiyaç duyuyor, bu nedenle aşağıdaki örnekte olduğu gibi bir tetikleyici oluşturabilirsiniz:

``` json
"HTTP_Debatch": {
   "type": "Http",
    "inputs": {
        "uri": "https://mydomain.com/myAPI",
        "method": "GET"
    },
   "recurrence": {
      "frequency": "Second",
      "interval": 1
    },
    "splitOn": "@triggerBody()?.Rows"
}
```

> [!NOTE]
> `SplitOn` Komutunu kullanırsanız, dizi dışındaki özellikleri alamazsınız. Bu nedenle bu örnek için, API 'den döndürülen `status` yanıtta özelliği alamazsınız.
> 
> `Rows` Özelliği yoksa, bir hatadan kaçınmak için, bu örnek `?` işlecini kullanır.

İş akışı tanımınız, ilk `@triggerBody().name` çalıştırandan ve `name` `"customer-name-two"` ikinci çalıştırandan itibaren `"customer-name-one"` olan değerleri almak için artık kullanılabilir. Bu nedenle, tetikleyicinizin çıkışları şu örneklere benzer şekilde görünür:

```json
{
   "body": {
      "id": 938109380,
      "name": "customer-name-one"
   }
}
```

```json
{
   "body": {
      "id": 938109381,
      "name": "customer-name-two"
   }
}
```

<a name="actions-overview"></a>

## <a name="actions-overview"></a>Eylemlere genel bakış

Azure Logic Apps, her biri bir eylemin benzersiz davranışını tanımlayan farklı girişlerle birlikte çeşitli eylem türleri sağlar. 

Eylemler bu üst düzey öğelere sahiptir, ancak bazıları isteğe bağlıdır:

```json
"<action-name>": {
   "type": "<action-type>",
   "inputs": { 
      "<input-name>": { "<input-value>" },
      "retryPolicy": "<retry-behavior>" 
   },
   "runAfter": { "<previous-trigger-or-action-status>" },
   "runtimeConfiguration": { "<runtime-config-options>" },
   "operationOptions": "<operation-option>"
},
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------|
| <*eylem-ad*> | String | Eylemin adı | 
| <*eylem-tür*> | String | Eylem türü, örneğin, "http" veya "ApiConnection"| 
| <*giriş adı*> | String | Eylemin davranışını tanımlayan bir girdinin adı | 
| <*giriş değeri*> | Türlerini | Dize, tamsayı, JSON nesnesi vb. olabilen giriş değeri | 
| <*önceki-tetikleyici-veya-eylem-durum*> | JSON nesnesi | Bu geçerli eylem çalıştırılmadan hemen önce çalışması gereken tetikleyici veya eylemin adı ve sonuç durumu | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------|
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum koduna ve tüm bağlantı özel durumlarına sahip aralıklı olmayan hatalara yönelik yeniden deneme davranışını özelleştirir. Daha fazla bilgi için bkz. yeniden deneme ilkeleri. | 
| <*runtime-config-options*> | JSON nesnesi | Bazı eylemler için, özellikleri ayarlayarak `runtimeConfiguration` eylemin çalışma zamanında davranışını değiştirebilirsiniz. Daha fazla bilgi için bkz. [çalışma zamanı yapılandırma ayarları](#runtime-config-options). | 
| <*operation-option*> | String | Bazı eylemler için, `operationOptions` özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için bkz. [işlem seçenekleri](#operation-options). | 
|||| 

## <a name="action-types-list"></a>Eylem türleri listesi

Yaygın olarak kullanılan bazı eylem türleri şunlardır: 

* Bu örnekler ve daha fazlası gibi [yerleşik eylem türleri](#built-in-actions) : 

  * HTTP veya HTTPS üzerinden uç noktaları çağırmak için [**http**](#http-action)

  * İsteklere yanıt verme [**yanıtı**](#response-action)

  * JavaScript kod parçacıklarını çalıştırmak için [**JavaScript kodunu yürütme**](#run-javascript-code)

  * Azure Işlevleri çağırma [**işlevi**](#function-action)

  * Farklı girdilerden veri oluşturan veya dönüştüren, [**JOIN**](#join-action), [**Compose**](#compose-action), [**Table**](#table-action), [**Select**](#select-action)ve diğerleri gibi veri işleme eylemleri

  * Başka bir mantıksal uygulama iş akışını çağırmak için [**Iş akışı**](#workflow-action)

* Microsoft tarafından yönetilen çeşitli bağlayıcıları ve API 'Leri çağıran [**Apiconnection**](#apiconnection-action) ve [**Apiconnectionweb KANCASı**](#apiconnectionwebhook-action) gibi [yönetilen API eylem türleri](#managed-api-actions) ; Örneğin, Azure Service Bus, Office 365 Outlook, Power BI, Azure Blob depolama, OneDrive, GitHub ve daha fazlası

* Diğer eylemleri içeren ve iş akışı yürütmeyi düzenlemenize yardımcı olan [**IF**](#if-action), [**foreach**](#foreach-action), [**Switch**](#switch-action), [**scope**](#scope-action)ve [**until**](#until-action)gibi [iş akışı eylem türlerini denetleyin](#control-workflow-actions)

<a name="built-in-actions"></a>

### <a name="built-in-actions"></a>Yerleşik eylemler

| Eylem türü | Açıklama | 
|-------------|-------------| 
| [**İletinizi**](#compose-action) | Girdilerden farklı türlere sahip olabilen tek bir çıktı oluşturur. | 
| [**JavaScript kodunu Yürüt**](#run-javascript-code) | Belirli ölçütlere uyan JavaScript kod parçacıklarını çalıştırın. Kod gereksinimleri ve daha fazla bilgi için bkz. [satır içi kod ile kod parçacıkları ekleme ve çalıştırma](../logic-apps/logic-apps-add-run-inline-code.md). |
| [**Çalışmayacaktır**](#function-action) | Bir Azure Işlevi çağırır. | 
| [**HTTP**](#http-action) | Bir HTTP uç noktası çağırır. | 
| [**Ayrılma**](#join-action) | Dizideki tüm öğelerden bir dize oluşturur ve bu öğeleri belirli bir sınırlayıcı karakterle ayırır. | 
| [**JSON Ayrıştır**](#parse-json-action) | JSON içeriğindeki özelliklerden Kullanıcı dostu belirteçler oluşturur. Daha sonra, mantıksal uygulamanıza belirteçleri ekleyerek bu özelliklere başvurabilirsiniz. | 
| [**Sorgulayamadı**](#query-action) | Bir koşula veya filtreye bağlı olarak başka bir dizideki öğelerden bir dizi oluşturur. | 
| [**Yanıtıyla**](#response-action) | Gelen çağrıya veya isteğe yanıt oluşturur. | 
| [**Seçin**](#select-action) | Belirtilen haritaya göre başka bir diziden öğeleri dönüştürerek JSON nesneleriyle bir dizi oluşturur. | 
| [**Tablosundan**](#table-action) | Bir diziden CSV veya HTML tablosu oluşturur. | 
| [**Sonlandırmayı**](#terminate-action) | Etkin şekilde çalışan bir iş akışını sonlandırır. | 
| [**Bekleneceğini**](#wait-action) | İş akışınızı belirtilen süre veya belirtilen tarih ve saate kadar duraklatır. | 
| [**Akışıyla**](#workflow-action) | Bir iş akışını başka bir iş akışı içinde bir şekilde alır. | 
||| 

<a name="managed-api-actions"></a>

### <a name="managed-api-actions"></a>Yönetilen API eylemleri

| Eylem türü | Açıklama | 
|-------------|-------------|  
| [**ApiConnection**](#apiconnection-action) | [Microsoft tarafından yönetilen BIR API](../connectors/apis-list.md)kullanarak bir HTTP uç noktası çağırır. | 
| [**ApiConnectionWebhook**](#apiconnectionwebhook-action) | HTTP Web kancası gibi çalışarak [Microsoft tarafından yönetilen BIR API](../connectors/apis-list.md)kullanır. | 
||| 

<a name="control-workflow-actions"></a>

### <a name="control-workflow-actions"></a>Denetim iş akışı eylemleri

Bu eylemler, iş akışı yürütmeyi denetlemenize ve diğer eylemleri dahil etmenize yardımcı olur. Denetim iş akışı eyleminin dışından, bu denetim iş akışı eyleminin içindeki eylemlere doğrudan başvurabilirsiniz. Örneğin, bir kapsam içinde bir `Http` eyleminiz varsa, `@body('Http')` ifadeye iş akışındaki herhangi bir yerden başvurabilirsiniz. Ancak, bir denetim iş akışı eyleminde bulunan eylemler yalnızca aynı denetim iş akışı yapısında yer alan diğer eylemlerden "daha sonra çalıştırılabilir".

| Eylem türü | Açıklama | 
|-------------|-------------| 
| [**ForEach**](#foreach-action) | Bir dizideki her öğe için aynı eylemleri döngüde çalıştırın. | 
| [**Kullandıysanız**](#if-action) | Belirtilen koşulun doğru veya yanlış olduğunu temel alarak eylemleri çalıştırın. | 
| [**Kapsam**](#scope-action) | Eylemler kümesinden grup durumuna göre eylemleri çalıştırın. | 
| [**Değiştirebilirsiniz**](#switch-action) | Deyimlerden, nesnelerden veya belirteçlerdeki değerlerin her bir durum tarafından belirtilen değerlerle eşleşmesi durumunda durumları düzenlenmiş eylemler çalıştırın. | 
| [**Until**](#until-action) | Belirtilen koşul true olana kadar eylemleri bir döngüde çalıştırın. | 
|||  

## <a name="actions---detailed-reference"></a>Eylemler-ayrıntılı başvuru

<a name="apiconnection-action"></a>

### <a name="apiconnection-action"></a>APIConnection eylemi

Bu eylem, [Microsoft tarafından yönetilen BIR API](../connectors/apis-list.md) 'ye bir http isteği GÖNDERIR ve API ve parametrelerle ilgili bilgilerin yanı sıra geçerli bir bağlantıya yönelik bir başvuru gerektirir. 

``` json
"<action-name>": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<api-name>']['connectionId']"
         },
         "<other-action-specific-input-properties>"        
      },
      "method": "<method-type>",
      "path": "/<api-operation>",
      "retryPolicy": "<retry-behavior>",
      "queries": { "<query-parameters>" },
      "<other-action-specific-properties>"
    },
    "runAfter": {}
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*eylem-ad*> | String | Bağlayıcı tarafından belirtilen eylemin adı | 
| <*api adı*> | String | Bağlantı için kullanılan Microsoft tarafından yönetilen API 'nin adı | 
| <*Yöntem-tür*> | String | API 'YI çağırmak için HTTP yöntemi: "GET", "PUT", "POST", "PATCH" veya "DELETE" | 
| <*api-işlem*> | String | Çağrılacak API işlemi | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*diğer eyleme özgü-giriş-Özellikler*> | JSON nesnesi | Bu özel eylem için uygulanan diğer tüm giriş özellikleri | 
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum koduna ve tüm bağlantı özel durumlarına sahip aralıklı olmayan hatalara yönelik yeniden deneme davranışını özelleştirir. Daha fazla bilgi için bkz. [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*sorgu-Parametreler*> | JSON nesnesi | API çağrısıyla birlikte içerilecek herhangi bir sorgu parametresi. <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi çağrıya ekler `?api-version=2018-01-01` . | 
| <*diğer eyleme özgü özellikler*> | JSON nesnesi | Bu özel eylem için uygulanan diğer özellikler | 
|||| 

*Örnek*

Bu tanım, Microsoft tarafından yönetilen bir API olan Office 365 Outlook Bağlayıcısı için **e-posta gönder** eylemini açıklar: 

```json
"Send_an_email": {
   "type": "ApiConnection",
   "inputs": {
      "body": {
         "Body": "Thank you for your membership!",
         "Subject": "Hello and welcome!",
         "To": "Sophie.Owen@contoso.com"
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "method": "POST",
      "path": "/Mail"
    },
    "runAfter": {}
}
```

<a name="apiconnection-webhook-action"></a>

### <a name="apiconnectionwebhook-action"></a>Apiconnectionweb kancası eylemi

Bu eylem, [Microsoft tarafından yönetilen BIR API](../connectors/apis-list.md)kullanarak bir uç noktaya http üzerinden bir abonelik isteği gönderir, uç noktanın yanıt gönderebildiği konuma bir *geri çağırma URL 'si* sağlar ve uç noktanın yanıt vermesini bekler. Daha fazla bilgi için bkz. [Endpoint abonelikleri](#subscribe-unsubscribe).

```json
"<action-name>": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "subscribe": {
         "method": "<method-type>",
         "uri": "<api-subscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" },
         "retryPolicy": "<retry-behavior>",
         "queries": { "<query-parameters>" },
         "<other-action-specific-input-properties>"
      },
      "unsubscribe": {
         "method": "<method-type>",
         "uri": "<api-unsubscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" },
         "<other-action-specific-properties>"
      },
   },
   "runAfter": {}
}
```

<*Method-Type*> gibi bazı değerler hem hem de `"subscribe"` `"unsubscribe"` nesneleri için kullanılabilir.

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*eylem-ad*> | String | Bağlayıcı tarafından belirtilen eylemin adı | 
| <*Yöntem-tür*> | String | Bir uç noktadan abone olmak veya aboneliği kaldırmak için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "PATCH" veya "DELETE" | 
| <*api-Subscribe-URL*> | String | API 'ye abone olmak için kullanılacak URI | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*API-abonelikten çıkma-URL*> | String | API 'den aboneliği kaldırma için kullanılacak URI | 
| <*üst bilgi-içerik*> | JSON nesnesi | İstekte gönderilen tüm üstbilgiler <p>Örneğin, bir istek için dili ve türü ayarlamak için: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*gövde-içerik*> | JSON nesnesi | İstekte göndermek için herhangi bir ileti içeriği | 
| <*Authentication-yöntemi*> | JSON nesnesi | İsteğin kimlik doğrulaması için kullandığı yöntem. Daha fazla bilgi için bkz. [Scheduler giden kimlik doğrulaması](../scheduler/scheduler-outbound-authentication.md). |
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum koduna ve tüm bağlantı özel durumlarına sahip aralıklı olmayan hatalara yönelik yeniden deneme davranışını özelleştirir. Daha fazla bilgi için bkz. [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*sorgu-Parametreler*> | JSON nesnesi | API çağrısıyla birlikte içerilecek sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi çağrıya ekler `?api-version=2018-01-01` . | 
| <*diğer eyleme özgü-giriş-Özellikler*> | JSON nesnesi | Bu özel eylem için uygulanan diğer tüm giriş özellikleri | 
| <*diğer eyleme özgü özellikler*> | JSON nesnesi | Bu özel eylem için uygulanan diğer özellikler | 
|||| 

Ayrıca, bir **Apiconnectionweb kancası** eyleminde, [http zaman uyumsuz limitleriyle](#asynchronous-limits)aynı şekilde sınırlar belirtebilirsiniz.

<a name="compose-action"></a>

### <a name="compose-action"></a>Oluşturma eylemi

Bu eylem, ifadeler de dahil olmak üzere birden çok girişin tek bir çıkışını oluşturur. Hem çıktı hem de girişler, Azure Logic Apps yerel olarak desteklenen, diziler, JSON nesneleri, XML ve ikili gibi herhangi bir türde olabilir.
Daha sonra eylemin çıkışını diğer eylemlerdeki bir şekilde kullanabilirsiniz. 

```json
"Compose": {
   "type": "Compose",
   "inputs": "<inputs-to-compose>",
   "runAfter": {}
},
```

*Gerekli* 

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*oluşturma girişleri*> | Any | Tek bir çıkış oluşturmak için girişler | 
|||| 

*Örnek 1*

<!-- markdownlint-disable MD038 -->
Bu eylem tanımı bir `abcdefg ` sonundaki boşluk ve değer `1234`ile birleştirir:
<!-- markdownlint-enable MD038 -->

```json
"Compose": {
   "type": "Compose",
   "inputs": "abcdefg 1234",
   "runAfter": {}
},
```

Bu eylemin oluşturduğu çıktı aşağıda verilmiştir:

`abcdefg 1234`

*Örnek 2*

Bu eylem tanımı `abcdefg` , içeren bir dize değişkenini ve içeren bir tamsayı değişkenini `1234`birleştirir:

```json
"Compose": {
   "type": "Compose",
   "inputs": "@{variables('myString')}@{variables('myInteger')}",
   "runAfter": {}
},
```

Bu eylemin oluşturduğu çıktı aşağıda verilmiştir:

`"abcdefg1234"`

<a name="run-javascript-code"></a>

### <a name="execute-javascript-code-action"></a>JavaScript kodu yürütme eylemi

Bu eylem bir JavaScript kod parçacığı çalıştırır ve sonuçları daha sonra başvurabilebir `Result` belirteç aracılığıyla döndürür.

```json
"Execute_JavaScript_Code": {
   "type": "JavaScriptCode",
   "inputs": {
      "code": "<JavaScript-code-snippet>",
      "explicitDependencies": {
         "actions": [ <previous-actions> ],
         "includeTrigger": true
      }
   },
   "runAfter": {}
}
```

*Gerekli*

| Value | Type | Açıklama |
|-------|------|-------------|
| <*JavaScript-Code-kod parçacığı*> | Varies | Çalıştırmak istediğiniz JavaScript kodu. Kod gereksinimleri ve daha fazla bilgi için bkz. [satır içi kod ile kod parçacıkları ekleme ve çalıştırma](../logic-apps/logic-apps-add-run-inline-code.md). <p>Özniteliğinde, kod parçacığınızı giriş olarak salt okunurdur `workflowContext`. `code` Bu nesne, kodunuzu tetikleyiciden gelen sonuçlara ve iş akışınızdan önceki eylemlere erişim sağlayan alt özellikler içerir. `workflowContext` Nesnesi hakkında daha fazla bilgi için bkz. [kodunuzda başvuru tetikleyicisi ve eylem sonuçları](../logic-apps/logic-apps-add-run-inline-code.md#workflowcontext). |
||||

*Bazı durumlarda gereklidir*

`explicitDependencies` Özniteliği tetikleyiciden, önceki eylemlerden veya her ikisinin de kod parçacığınızı bağımlılıkları olarak açıkça dahil etmek istediğinizi belirtir. Bu bağımlılıkları ekleme hakkında daha fazla bilgi için bkz. [satır içi kod için parametre ekleme](../logic-apps/logic-apps-add-run-inline-code.md#add-parameters). 

Özniteliği için, veya `true` `false` değerlerini belirtebilirsiniz. `includeTrigger`

| Value | Type | Açıklama |
|-------|------|-------------|
| <*önceki eylemler*> | Dize dizisi | Belirtilen eylem isimlerinizi içeren bir dizi. İş akışı tanımınızda görünen eylem adlarını, eylem adlarında boşluk ("") değil, alt çizgi (_) kullanıldığı durumlarda kullanın. |
||||

*Örnek 1*

Bu eylem, mantıksal uygulamanızın adını alan ve sonuç olarak " \<Logic-app-name >" metnini döndüren kodu çalıştırır. Bu örnekte, kod salt okunurdur `workflowContext.workflow.name` `workflowContext` nesnesi aracılığıyla özelliğe erişerek iş akışının adına başvurur. `workflowContext` Nesnesini kullanma hakkında daha fazla bilgi için bkz. [kodunuzda başvuru tetikleyicisi ve eylem sonuçları](../logic-apps/logic-apps-add-run-inline-code.md#workflowcontext).

```json
"Execute_JavaScript_Code": {
   "type": "JavaScriptCode",
   "inputs": {
      "code": "var text = \"Hello world from \" + workflowContext.workflow.name;\r\n\r\nreturn text;"
   },
   "runAfter": {}
}
```

*Örnek 2*

Bu eylem, Office 365 Outlook hesabına yeni bir e-posta geldiğinde tetiklenen bir mantıksal uygulamadaki kodu çalıştırır. Mantıksal uygulama Ayrıca, alınan e-postadaki içeriği onay isteğiyle birlikte ileten onay e-postası gönder eylemini kullanır. 

Kod, tetikleyici `Body` özelliğinden e-posta adreslerini ayıklar ve bu e-posta adreslerini onay eyleminden `SelectedOption` özellik değeriyle birlikte döndürür. Eylem, `explicitDependencies` özniteliğe`actions` bağımlılık  >  olarak onay e-postası gönder eylemini açıkça içerir.

```json
"Execute_JavaScript_Code": {
   "type": "JavaScriptCode",
   "inputs": {
      "code": "var re = /(([^<>()\\[\\]\\\\.,;:\\s@\"]+(\\.[^<>()\\[\\]\\\\.,;:\\s@\"]+)*)|(\".+\"))@((\\[[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}])|(([a-zA-Z\\-0-9]+\\.)+[a-zA-Z]{2,}))/g;\r\n\r\nvar email = workflowContext.trigger.outputs.body.Body;\r\n\r\nvar reply = workflowContext.actions.Send_approval_email_.outputs.body.SelectedOption;\r\n\r\nreturn email.match(re) + \" - \" + reply;\r\n;",
      "explicitDependencies": {
         "actions": [
            "Send_approval_email_"
         ]
      }
   },
   "runAfter": {}
}
```



<a name="function-action"></a>

### <a name="function-action"></a>İşlev eylemi

Bu eylem, önceden oluşturulmuş bir [Azure işlevini](../azure-functions/functions-create-first-azure-function.md)çağırır.

```json
"<Azure-function-name>": {
   "type": "Function",
   "inputs": {
     "function": {
        "id": "<Azure-function-ID>"
      },
      "method": "<method-type>",
      "headers": { "<header-content>" },
      "body": { "<body-content>" },
      "queries": { "<query-parameters>" } 
   },
   "runAfter": {}
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------|  
| <*Azure-işlev KIMLIĞI*> | String | Çağırmak istediğiniz Azure işlevinin kaynak KIMLIĞI. Bu değerin biçimi aşağıda verilmiştir:<p>"/Subscriptions/<*Azure-Subscription-ıd*>/resourceGroups/<*Azure-resource-group*>/Providers/Microsoft.Web/Sites/<*Azure-function-app-name*>/Functions/<*Azure-Function-Name*> " | 
| <*Yöntem-tür*> | String | İşlevi çağırmak için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "PATCH" veya "DELETE" <p>Belirtilmemişse, varsayılan "POST" yöntemidir. | 
||||

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------|  
| <*üst bilgi-içerik*> | JSON nesnesi | Çağrıya gönderecek tüm üstbilgiler <p>Örneğin, bir istek için dili ve türü ayarlamak için: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*gövde-içerik*> | JSON nesnesi | İstekte göndermek için herhangi bir ileti içeriği | 
| <*sorgu-Parametreler*> | JSON nesnesi | API çağrısıyla birlikte içerilecek sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi çağrıya ekler `?api-version=2018-01-01` . | 
| <*diğer eyleme özgü-giriş-Özellikler*> | JSON nesnesi | Bu özel eylem için uygulanan diğer tüm giriş özellikleri | 
| <*diğer eyleme özgü özellikler*> | JSON nesnesi | Bu özel eylem için uygulanan diğer özellikler | 
||||

Mantıksal uygulamanızı kaydettiğinizde, Logic Apps altyapısı başvurulan işlev üzerinde bu denetimleri gerçekleştirir:

* İş akışınızın işleve erişimi olmalıdır.

* İş akışınız yalnızca standart bir HTTP tetikleyicisi veya genel JSON Web kancası tetikleyicisi kullanabilir. 

  Logic Apps altyapısı, çalışma zamanında kullanılan tetikleyicisinin URL 'sini alır ve önbelleğe alır. Ancak, herhangi bir işlem önbelleğe alınmış URL 'YI geçersiz kılar, **işlev** eylemi çalışma zamanında başarısız olur. Bu sorunu düzeltemedi, mantıksal uygulamanın tetikleyici URL 'sini yeniden alması ve önbelleğe almak için mantıksal uygulamayı yeniden kaydedin.

* İşlevin herhangi bir yolu tanımlı olamaz.

* Yalnızca "function" ve "Anonymous" yetkilendirme düzeylerine izin verilir. 

*Örnek*

Bu eylem tanımı, önceden oluşturulan "Getproductıd" işlevini çağırır:

```json
"GetProductID": {
   "type": "Function",
   "inputs": {
     "function": {
        "id": "/subscriptions/<XXXXXXXXXXXXXXXXXXXX>/resourceGroups/myLogicAppResourceGroup/providers/Microsoft.Web/sites/InventoryChecker/functions/GetProductID"
      },
      "method": "POST",
      "headers": { 
          "x-ms-date": "@utcnow()"
       },
      "body": { 
          "Product_ID": "@variables('ProductID')"
      }
   },
   "runAfter": {}
}
```

<a name="http-action"></a>

### <a name="http-action"></a>HTTP eylemi

Bu eylem, belirtilen uç noktaya bir istek gönderir ve iş akışının çalıştırılıp çalıştırılmayacağını belirleme yanıtını denetler. 

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "<method-type>",
      "uri": "<HTTP-or-HTTPS-endpoint-URL>"
   },
   "runAfter": {}
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*Yöntem-tür*> | String | İsteği göndermek için kullanılacak yöntem: "GET", "PUT", "POST", "PATCH" veya "DELETE" | 
| <*HTTP-veya-HTTPS uç noktası-URL*> | String | Çağrılacak HTTP veya HTTPS uç noktası. En büyük dize boyutu: 2 KB | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*üst bilgi-içerik*> | JSON nesnesi | İstekle birlikte göndermek için herhangi bir üst bilgi <p>Örneğin, dili ve türünü ayarlamak için: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*gövde-içerik*> | JSON nesnesi | İstekte göndermek için herhangi bir ileti içeriği | 
| <*yeniden deneme davranışı*> | JSON nesnesi | 408, 429 ve 5XX durum koduna ve tüm bağlantı özel durumlarına sahip aralıklı olmayan hatalara yönelik yeniden deneme davranışını özelleştirir. Daha fazla bilgi için bkz. [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*sorgu-Parametreler*> | JSON nesnesi | İsteğe dahil edilecek herhangi bir sorgu parametresi <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesnesi çağrıya ekler `?api-version=2018-01-01` . | 
| <*diğer eyleme özgü-giriş-Özellikler*> | JSON nesnesi | Bu özel eylem için uygulanan diğer tüm giriş özellikleri | 
| <*diğer eyleme özgü özellikler*> | JSON nesnesi | Bu özel eylem için uygulanan diğer özellikler | 
|||| 

*Örnek*

Bu eylem tanımı, belirtilen uç noktaya bir istek göndererek en son haberleri alır:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://mynews.example.com/latest"
   }
}
```

<a name="join-action"></a>

### <a name="join-action"></a>JOIN eylemi

Bu eylem bir dizideki tüm öğelerden bir dize oluşturur ve bu öğeleri belirtilen sınırlayıcı karakterle ayırır. 

```json
"Join": {
   "type": "Join",
   "inputs": {
      "from": <array>,
      "joinWith": "<delimiter>"
   },
   "runAfter": {}
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*dizide*> | Array | Kaynak öğeleri sağlayan dizi veya ifade. Bir ifade belirtirseniz, bu ifadeyi çift tırnak içine alın. | 
| <*ayırıcı*> | Tek karakter dizesi | Dizedeki her öğeyi ayıran karakter | 
|||| 

*Örnek*

Bu tamsayı dizisini içeren daha önce oluşturulmuş bir "myIntegerArray" değişkenine sahip olduğunuzu varsayalım: 

`[1,2,3,4]` 

Bu eylem tanımı, bir ifadede `variables()` işlevini kullanarak değişkenden değerleri alır ve bu dizeyi, virgülle ayrılmış şekilde bu değerlerle oluşturur:`"1,2,3,4"`

```json
"Join": {
   "type": "Join",
   "inputs": {
      "from": "@variables('myIntegerArray')",
      "joinWith": ","
   },
   "runAfter": {}
}
```

<a name="parse-json-action"></a>

### <a name="parse-json-action"></a>JSON eylemini Ayrıştır

Bu eylem, JSON içeriğindeki özelliklerden Kullanıcı dostu alanları veya *belirteçleri* oluşturur. Bunun yerine belirteçleri kullanarak mantıksal uygulamanızdaki bu özelliklere erişebilirsiniz. Örneğin, Azure Service Bus ve Azure Cosmos DB gibi hizmetlerden JSON çıktısı kullanmak istiyorsanız bu eylemi mantıksal uygulamanıza ekleyebilirsiniz, böylece söz konusu çıktıdaki verilere daha kolay başvurabilirsiniz. 

```json
"Parse_JSON": {
   "type": "ParseJson",
   "inputs": {
      "content": "<JSON-source>",
         "schema": { "<JSON-schema>" }
      },
      "runAfter": {}
},
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*JSON kaynağı*> | JSON nesnesi | Ayrıştırmak istediğiniz JSON içeriği | 
| <*JSON şeması*> | JSON nesnesi | İşlemin, kaynak JSON içeriğini ayrıştırmak için kullandığı JSON içeriğini tanımlayan JSON şeması. <p>**İpucu**: Logic Apps tasarımcısında, şemayı sağlayabilir veya işlemin şemayı oluşturabilmesi için bir örnek yük sağlayabilirsiniz. | 
|||| 

*Örnek*

Bu eylem tanımı, iş akışınızda kullanabileceğiniz, ancak yalnızca **JSON ayrıştırması** eylemini izleyen eylemlerde çalıştırılan bu belirteçleri oluşturur: 

`FirstName`, `LastName`ve`Email`

```json
"Parse_JSON": {
   "type": "ParseJson",
   "inputs": {
      "content": {
         "Member": {
            "Email": "Sophie.Owen@contoso.com",
            "FirstName": "Sophie",
            "LastName": "Owen"
         }
      },
      "schema": {
         "type": "object",
         "properties": {
            "Member": {
               "type": "object",
               "properties": {
                  "Email": {
                     "type": "string"
                  },
                  "FirstName": {
                     "type": "string"
                  },
                  "LastName": {
                     "type": "string"
                  }
               }
            }
         }
      }
   },
   "runAfter": { }
},
```

Bu örnekte, "content" özelliği ayrıştırılacak eylemin JSON içeriğini belirtir. Şemayı oluşturmak için örnek yük olarak bu JSON içeriğini de sağlayabilirsiniz.

```json
"content": {
   "Member": { 
      "FirstName": "Sophie",
      "LastName": "Owen",
      "Email": "Sophie.Owen@contoso.com"
   }
},
```

"Schema" özelliği, JSON içeriğini açıklamak için kullanılan JSON şemasını belirtir:

```json
"schema": {
   "type": "object",
   "properties": {
      "Member": {
         "type": "object",
         "properties": {
            "FirstName": {
               "type": "string"
            },
            "LastName": {
               "type": "string"
            },
            "Email": {
               "type": "string"
            }
         }
      }
   }
}
```

<a name="query-action"></a>

### <a name="query-action"></a>Sorgu eylemi

Bu eylem, belirtilen koşula veya filtreye bağlı olarak başka bir dizideki öğelerden bir dizi oluşturur.

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": <array>,
      "where": "<condition-or-filter>"
   },
   "runAfter": {}
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*dizide*> | Array | Kaynak öğeleri sağlayan dizi veya ifade. Bir ifade belirtirseniz, bu ifadeyi çift tırnak içine alın. |
| <*koşul-or-filtre*> | String | Kaynak dizideki öğeleri filtrelemek için kullanılan koşul <p>**Not**: Koşulu karşılayamadığı bir değer yoksa, eylem boş bir dizi oluşturur. |
|||| 

*Örnek*

Bu eylem tanımı, belirtilen değerden daha büyük değerlere sahip bir dizi oluşturur, bu iki:

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": [ 1, 3, 0, 5, 4, 2 ],
      "where": "@greater(item(), 2)"
   }
}
```

<a name="response-action"></a>

### <a name="response-action"></a>Yanıt eylemi  

Bu eylem bir HTTP isteğine yanıt için yük oluşturur. 

```json
"Response" {
    "type": "Response",
    "kind": "http",
    "inputs": {
        "statusCode": 200,
        "headers": { <response-headers> },
        "body": { <response-body> }
    },
    "runAfter": {}
},
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*Yanıt-durum kodu*> | Integer | Gelen isteğe gönderilen HTTP durum kodu. Varsayılan kod "200 Tamam" dır, ancak kod, 2xx, 4xx veya 5xx ile başlayan ancak 3xxx ile birlikte olmayan geçerli bir durum kodu olabilir. | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*Yanıt-üst bilgiler*> | JSON nesnesi | Yanıtla birlikte içerilecek bir veya daha fazla üst bilgi | 
| <*yanıt gövdesi*> | Türlerini | Önceki bir eylemden dize, JSON nesnesi, hatta ikili içerik olabilen yanıt gövdesi | 
|||| 

*Örnek*

Bu eylem tanımı, belirtilen durum kodu, ileti gövdesi ve ileti üst bilgilerine sahip bir HTTP isteğine yanıt oluşturur:

```json
"Response": {
   "type": "Response",
   "inputs": {
      "statusCode": 200,
      "body": {
         "ProductID": 0,
         "Description": "Organic Apples"
      },
      "headers": {
         "x-ms-date": "@utcnow()",
         "content-type": "application/json"
      }
   },
   "runAfter": {}
}
```

*Larından*

Diğer eylemlerin aksine, **Yanıt** eyleminde özel kısıtlamalar vardır: 

* İş akışınız, **Yanıt** eylemini yalnızca iş akışı bir http isteği tetikleyicisi ile başladığında kullanabilir, yani iş AKıŞıNıZıN bir http isteği tarafından tetiklenmesi gerekir.

* İş akışınız, sıralı döngüler ve paralel dallar dahil olmak üzere döngülerin **içinde** *olan her yerde* **Yanıt** eylemini kullanabilir. 

* Özgün HTTP isteği yalnızca **Yanıt** eylemi için gereken tüm eylemler [http istek zaman aşımı sınırı](../logic-apps/logic-apps-limits-and-config.md#request-limits)içinde bittiğinde iş akışınızın yanıtını alır.

  Ancak, iş akışınız, iç içe geçmiş bir iş akışı olarak başka bir mantıksal uygulamayı çağırırsa, iç iş akışı, iç içe geçmiş iş akışı tamamlanmadan önce ne kadar zaman geçene olursa olsun, iç içe geçmiş iş

* İş akışınız **Yanıt** eylemini ve zaman uyumlu bir yanıt modelini kullandığında, bu komut birden çok çalıştırma oluşturduğundan, iş akışı tetikleyici tanımında de **spton** komutunu kullanamaz. PUT yöntemi kullanıldığında bu durumu denetleyin ve true ise "Hatalı istek" yanıtı döndürür.

  Aksi takdirde, iş akışınız **Spton** komutunu ve bir **Yanıt** eylemini kullanıyorsa, iş akışı zaman uyumsuz olarak çalışır ve anında "202 kabul edildi" yanıtını döndürür.

* İş akışınızın yürütmesi **Yanıt** eylemine ulaştığında, ancak gelen istek zaten bir yanıt aldıysa, çakışma nedeniyle **Yanıt** eylemi "başarısız" olarak işaretlenir. Sonuç olarak, mantıksal uygulamanızın çalışması da "başarısız" durumu ile işaretlenir.

<a name="select-action"></a>

### <a name="select-action"></a>Eylem Seç

Bu eylem, belirtilen haritaya göre başka bir diziden öğeleri dönüştürerek JSON nesnelerine sahip bir dizi oluşturur. Çıkış dizisi ve kaynak dizisi her zaman aynı sayıda öğe içermelidir. Çıktı dizisindeki nesne sayısını değiştiremeseniz de, bu nesneler arasında özellikleri ve değerlerini ekleyebilir veya kaldırabilirsiniz. `select` Özelliği, kaynak dizideki öğeleri dönüştürmek için Haritayı tanımlayan en az bir anahtar-değer çifti belirtir. Anahtar-değer çifti, çıkış dizisindeki tüm nesneler genelinde bir özelliği ve değerini temsil eder. 

```json
"Select": {
   "type": "Select",
   "inputs": {
      "from": <array>,
      "select": { 
          "<key-name>": "<expression>",
          "<key-name>": "<expression>"        
      }
   },
   "runAfter": {}
},
```

*Gerekli* 

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*dizide*> | Array | Kaynak öğeleri sağlayan dizi veya ifade. Bir ifadeyi çift tırnak işareti içine aldığınızdan emin olun. <p>**Not**: Kaynak dizisi boşsa, eylem boş bir dizi oluşturur. | 
| <*key-name*> | String | <*İfadeden* sonuca atanan özellik adı> <p>Çıkış dizisindeki tüm nesneler arasında yeni bir özellik eklemek için, bu özellik için bir <*anahtar adı*> ve özellik değeri için > <*ifadesi*sağlayın. <p>Dizideki tüm nesnelerden bir özelliği kaldırmak için, bu özellik için <*anahtar adı*> atlayın. | 
| <*ifadesini*> | String | Kaynak dizideki öğeyi dönüştüren ve sonucu <*anahtar adına* atayan ifade> | 
|||| 

**Select** eylemi bir diziyi çıkış olarak oluşturur, bu nedenle bu çıktıyı kullanmak isteyen herhangi bir eylemin bir diziyi kabul etmesi ya da diziyi, tüketici eyleminin kabul ettiği türe dönüştürmeniz gerekir. Örneğin, çıkış dizisini bir dizeye dönüştürmek için, bu diziyi **oluşturma** eylemine geçirebilir ve sonra diğer eylemlerdeki **oluşturma** eyleminin çıktısına başvurabilirsiniz.

*Örnek*

Bu eylem tanımı bir tamsayı dizisinden JSON nesnesi dizisi oluşturur. Eylem, kaynak dizi boyunca yinelenir, `@item()` ifadeyi kullanarak her bir tamsayı değeri alır ve her bir değeri her bir JSON nesnesinde "`number`" özelliğine atar: 

```json
"Select": {
   "type": "Select",
   "inputs": {
      "from": [ 1, 2, 3 ],
      "select": { 
         "number": "@item()" 
      }
   },
   "runAfter": {}
},
```

Bu eylemin oluşturduğu dizi aşağıdadır:

`[ { "number": 1 }, { "number": 2 }, { "number": 3 } ]`

Bu dizi çıkışını başka eylemlerde kullanmak için, bu çıktıyı bir **oluşturma** eylemine geçirin:

```json
"Compose": {
   "type": "Compose",
   "inputs": "@body('Select')",
   "runAfter": {
      "Select": [ "Succeeded" ]
   }
},
```

Daha sonra, diğer eylemlerdeki **oluşturma** eyleminden çıktıyı kullanabilirsiniz; Örneğin, **Office 365 Outlook-e-posta gönder** eylemi:

```json
"Send_an_email": {
   "type": "ApiConnection",
   "inputs": {
      "body": {
         "Body": "@{outputs('Compose')}",
         "Subject": "Output array from Select and Compose actions",
         "To": "<your-email@domain>"
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "method": "post",
      "path": "/Mail"
   },
   "runAfter": {
      "Compose": [ "Succeeded" ]
   }
},
```

<a name="table-action"></a>

### <a name="table-action"></a>Tablo eylemi

Bu eylem bir diziden CSV veya HTML tablosu oluşturur. JSON nesnelerine sahip diziler için bu eylem, nesnelerin özellik adlarından sütun üstbilgilerini otomatik olarak oluşturur. Diğer veri türlerine sahip diziler için, sütun üst bilgilerini ve değerlerini belirtmeniz gerekir. Örneğin bu dizi, bu eylemin sütun başlığı adları için kullanabileceği "ID" ve "Ürün_adı" özelliklerini içerir:

`[ {"ID": 0, "Product_Name": "Apples"}, {"ID": 1, "Product_Name": "Oranges"} ]` 

```json
"Create_<CSV | HTML>_table": {
   "type": "Table",
   "inputs": {
      "format": "<CSV | HTML>",
      "from": <array>,
      "columns": [ 
         {
            "header": "<column-name>",
            "value": "<column-value>"
         },
         {
            "header": "<column-name>",
            "value": "<column-value>"
         } 
      ]
   },
   "runAfter": {}
}
```

*Gerekli* 

| Value | Type | Açıklama | 
|-------|------|-------------| 
| \<CSV *veya* HTML >| String | Oluşturmak istediğiniz tablo için biçim | 
| <*dizide*> | Array | Tablo için kaynak öğeleri sağlayan dizi veya ifade <p>**Not**: Kaynak dizisi boşsa, eylem boş bir tablo oluşturur. | 
|||| 

*Seçim*

Sütun üst bilgilerini ve değerlerini belirtmek veya özelleştirmek için, `columns` diziyi kullanın. Çiftler `header-value` aynı üstbilgi adına sahip olduğunda, değerleri bu üst bilgi adı altındaki aynı sütunda görünür. Aksi takdirde, benzersiz üst bilgi benzersiz bir sütun tanımlar.

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*sütun-adı*> | String | Bir sütunun üst bilgi adı | 
| <*sütun-değer*> | Any | Bu sütundaki değer | 
|||| 

*Örnek 1*

Şu anda bu diziyi içeren daha önce oluşturulmuş bir "myItemArray" değişkenine sahip olduğunuzu varsayalım: 

`[ {"ID": 0, "Product_Name": "Apples"}, {"ID": 1, "Product_Name": "Oranges"} ]`

Bu eylem tanımı, "myItemArray" değişkeninden bir CSV tablosu oluşturur. `from` Özelliği tarafından kullanılan ifade, `variables()` işlevi kullanılarak "myitemarray" öğesinden diziyi alır: 

```json
"Create_CSV_table": {
   "type": "Table",
   "inputs": {
      "format": "CSV",
      "from": "@variables('myItemArray')"
   },
   "runAfter": {}
}
```

Bu eylemin oluşturduğu CSV tablosu aşağıda verilmiştir: 

```
ID,Product_Name 
0,Apples 
1,Oranges 
```

*Örnek 2*

Bu eylem tanımı, "myItemArray" değişkeninden bir HTML tablosu oluşturur. `from` Özelliği tarafından kullanılan ifade, `variables()` işlevi kullanılarak "myitemarray" öğesinden diziyi alır: 

```json
"Create_HTML_table": {
   "type": "Table",
   "inputs": {
      "format": "HTML",
      "from": "@variables('myItemArray')"
   },
   "runAfter": {}
}
```

Bu eylemin oluşturduğu HTML tablosu aşağıda verilmiştir: 

<table><thead><tr><th>ID</th><th>Ürün</th></tr></thead><tbody><tr><td>0</td><td>Elma</td></tr><tr><td>1\.</td><td>Portakallar</td></tr></tbody></table>

*Örnek 3*

Bu eylem tanımı, "myItemArray" değişkeninden bir HTML tablosu oluşturur. Ancak bu örnek, "Stock_ID" ve "Description" adlı varsayılan sütun üstbilgi adlarını geçersiz kılar ve "Organic" sözcüğünü "Description" sütunundaki değerlere ekler.

```json
"Create_HTML_table": {
   "type": "Table",
   "inputs": {
      "format": "HTML",
      "from": "@variables('myItemArray')",
      "columns": [ 
         {
            "header": "Stock_ID",
            "value": "@item().ID"
         },
         {
            "header": "Description",
            "value": "@concat('Organic ', item().Product_Name)"
         }
      ]
    },
   "runAfter": {}
},
```

Bu eylemin oluşturduğu HTML tablosu aşağıda verilmiştir: 

<table><thead><tr><th>Stock_ID</th><th>Açıklama</th></tr></thead><tbody><tr><td>0</td><td>Organik elmalar</td></tr><tr><td>1\.</td><td>Organik Portages</td></tr></tbody></table>

<a name="terminate-action"></a>

### <a name="terminate-action"></a>Sonlandırma eylemi

Bu eylem bir iş akışı örneği için çalışmayı durduruyor, devam eden tüm işlemleri iptal eder, kalan eylemleri atlar ve belirtilen durumu döndürür. Örneğin, mantıksal uygulamanız bir hata durumundan tamamen çıkış yapması gerektiğinde **Sonlandır** eylemini kullanabilirsiniz. Bu eylem, zaten tamamlanmış eylemleri etkilemez ve sıralı döngüler dahil olmak üzere **ForEach** ve **until** döngüleri içinde yer alamaz. 

```json
"Terminate": {
   "type": "Terminate",
   "inputs": {
       "runStatus": "<status>",
       "runError": {
            "code": "<error-code-or-name>",
            "message": "<error-message>"
       }
   },
   "runAfter": {}
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*status*> | String | Çalıştırma için döndürülecek durum: "Başarısız", "Iptal edildi" veya "başarılı" |
|||| 

*Seçim*

"RunStatus" nesnesinin özellikleri yalnızca "runStatus" özelliği "başarısız" durumuna ayarlandığında geçerlidir.

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*hata-kod veya-ad*> | String | Hata için kod veya ad |
| <*hata-ileti*> | String | Hatayı açıklayan ileti veya metin ve uygulama kullanıcısının gerçekleştirebileceği eylemler | 
|||| 

*Örnek*

Bu eylem tanımı bir iş akışı çalıştırmasını durduruyor, çalışma durumunu "başarısız" olarak ayarlar ve durum, hata kodu ve bir hata iletisi döndürür:

```json
"Terminate": {
    "type": "Terminate",
    "inputs": {
        "runStatus": "Failed",
        "runError": {
            "code": "Unexpected response",
            "message": "The service received an unexpected response. Please try again."
        }
   },
   "runAfter": {}
}
```

<a name="wait-action"></a>

### <a name="wait-action"></a>Bekleme eylemi  

Bu eylem, belirtilen zaman aralığı veya belirtilen saate kadar iş akışı yürütmeyi duraklatır, ancak her ikisine birden değil. 

*Belirtilen Aralık*

```json
"Delay": {
   "type": "Wait",
   "inputs": {
      "interval": {
         "count": <number-of-units>,
         "unit": "<interval>"
      }
   },
   "runAfter": {}
},
```

*Belirtilen zaman*

```json
"Delay_until": {
   "type": "Wait",
   "inputs": {
      "until": {
         "timestamp": "<date-time-stamp>"
      }
   },
   "runAfter": {}
},
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*birim sayısı*> | Integer | **Gecikme** eylemi için beklenecek birim sayısı | 
| <*aralığında*> | String | **Gecikme** eylemi için beklenecek Aralık: "Saniye", "dakika", "saat", "gün", "hafta", "ay" | 
| <*Tarih-saat damgası*> | String | **Tamamlanana kadar geciktir** , yürütmenin sürdürüleceği tarih ve saat. Bu değer [UTC Tarih saat biçimini](https://en.wikipedia.org/wiki/Coordinated_Universal_Time)kullanmalıdır. | 
|||| 

*Örnek 1*

Bu eylem tanımı, 15 dakika boyunca iş akışını duraklatır:

```json
"Delay": {
   "type": "Wait",
   "inputs": {
      "interval": {
         "count": 15,
         "unit": "Minute"
      }
   },
   "runAfter": {}
},
```

*Örnek 2*

Bu eylem tanımı, iş akışını belirtilen saate kadar duraklatır:

```json
"Delay_until": {
   "type": "Wait",
   "inputs": {
      "until": {
         "timestamp": "2017-10-01T00:00:00Z"
      }
   },
   "runAfter": {}
},
```

<a name="workflow-action"></a>

### <a name="workflow-action"></a>İş akışı eylemi

Bu eylem, daha önce oluşturulmuş başka bir mantıksal uygulamayı çağırır, bu da diğer mantıksal uygulama iş akışlarını dahil edebilir ve yeniden kullanabilirsiniz. Alt mantıksal uygulamanın bir yanıt döndürmesi kaydıyla, iç içe mantıksal uygulamayı izleyen Eylemler bölümünde alt veya *iç içe* mantıksal uygulamadaki çıkışları da kullanabilirsiniz.

Logic Apps motor çağırmak istediğiniz tetikleyiciye erişimi denetler, bu nedenle bu tetikleyiciye erişebildiğinizden emin olun. Ayrıca, iç içe mantıksal uygulamanın şu ölçütlere uyması gerekir:

* Tetikleyici, iç içe geçmiş mantıksal uygulamayı bir [istek](#request-trigger) veya [http](#http-trigger) tetikleyicisi gibi çağrılabilir hale getirir

* Ana mantıksal uygulamanız ile aynı Azure aboneliği

* Ana mantıksal uygulamanızdaki iç içe mantıksal uygulamadaki çıktıları kullanmak için, iç içe mantıksal uygulamanın bir [Yanıt](#response-action) eylemi olması gerekir 

```json
"<nested-logic-app-name>": {
   "type": "Workflow",
   "inputs": {
      "body": { "<body-content" },
      "headers": { "<header-content>" },
      "host": {
         "triggerName": "<trigger-name>",
         "workflow": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group>/providers/Microsoft.Logic/<nested-logic-app-name>"
         }
      }
   },
   "runAfter": {}
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*iç içe mantık-uygulama adı*> | String | Çağırmak istediğiniz mantıksal uygulamanın adı | 
| <*trigger-name*> | String | Çağırmak istediğiniz iç içe mantıksal uygulamadaki tetikleyicinin adı | 
| <*Azure-abonelik-KIMLIĞI*> | String | İç içe mantıksal uygulama için Azure abonelik KIMLIĞI |
| <*Azure-Resource-Group*> | String | İç içe mantıksal uygulama için Azure Kaynak grubu adı |
| <*iç içe mantık-uygulama adı*> | String | Çağırmak istediğiniz mantıksal uygulamanın adı |
||||

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------|  
| <*üst bilgi-içerik*> | JSON nesnesi | Çağrıya gönderecek tüm üstbilgiler | 
| <*gövde-içerik*> | JSON nesnesi | Çağrıyla gönderecek ileti içeriği | 
||||

*Çıkışlar*

Bu eylemin çıkışları, iç içe mantıksal uygulamanın yanıt eylemine göre farklılık gösterir. İç içe mantıksal uygulama bir yanıt eylemi içermiyorsa, çıktılar boştur.

*Örnek*

"Start_search" eylemi başarıyla tamamlandıktan sonra, bu iş akışı eylemi tanımı, belirtilen girdileri geçen "Get_product_information" adlı başka bir mantıksal uygulamayı çağırır: 

```json
"actions": {
   "Start_search": { <action-definition> },
   "Get_product_information": {
      "type": "Workflow",
      "inputs": {
         "body": {
            "ProductID": "24601",
         },
         "host": {
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/InventoryManager-RG/providers/Microsoft.Logic/Get_product_information",
            "triggerName": "Find_product"
         },
         "headers": {
            "content-type": "application/json"
         }
      },
      "runAfter": { 
         "Start_search": [ "Succeeded" ]
      }
   }
},
```

## <a name="control-workflow-action-details"></a>Denetim iş akışı eylem ayrıntıları

<a name="foreach-action"></a>

### <a name="foreach-action"></a>Foreach eylemi

Bu döngü eylemi bir dizi boyunca yinelenir ve her dizi öğesinde eylemler gerçekleştirir. Varsayılan olarak, "for each" döngüsü, en fazla döngü sayısına paralel olarak çalışır. Bu en büyük için bkz. [Limit ve config](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). ["For each" döngülerini oluşturmayı](../logic-apps/logic-apps-control-flow-loops.md#foreach-loop)öğrenin.

```json
"For_each": {
   "type": "Foreach",
   "actions": { 
      "<action-1>": { "<action-definition-1>" },
      "<action-2>": { "<action-definition-2>" }
   },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "runtimeConfiguration": {
      "concurrency": {
         "repetitions": <count>
      }
    },
    "operationOptions": "<operation-option>"
}
```

*Gerekli* 

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*eylem-1... No*> | String | Her dizi öğesinde çalışan eylemlerin adları | 
| <*eylem-tanım-1... No*> | JSON nesnesi | Çalışan eylemlerin tanımları | 
| < *-for-ifadesi*> | String | Belirtilen dizideki her öğeye başvuran ifade | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*biriktirme*> | Integer | Varsayılan olarak, "for each" döngüsü yinelemeleri aynı anda veya [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)paralel olarak çalışır. Yeni bir <*sayı*> değeri ayarlayarak bu sınırı değiştirmek için, bkz. ["for each" döngüsü eşzamanlılık](#change-for-each-concurrency). | 
| <*operation-option*> | String | Paralel yerine "for each" döngüsünü sırayla çalıştırmak için, <*Operation-Option*> `Sequential` olarak ayarlayın veya `1`<*Count*>, her ikisini birden değil. Daha fazla bilgi için bkz. ["for each" döngülerini sırayla çalıştırma](#sequential-for-each). | 
|||| 

*Örnek*

Bu "for each" döngüsü dizideki her öğe için bir e-posta göndererek gelen bir e-postanın eklerini içerir. Döngü, Eki inceleyen bir kişiye ek dahil bir e-posta gönderir.

```json
"For_each": {
   "type": "Foreach",
   "actions": {
      "Send_an_email": {
         "type": "ApiConnection",
         "inputs": {
            "body": {
               "Body": "@base64ToString(items('For_each')?['Content'])",
               "Subject": "Review attachment",
               "To": "Sophie.Owen@contoso.com"
                },
            "host": {
               "connection": {
                  "id": "@parameters('$connections')['office365']['connectionId']"
               }
            },
            "method": "post",
            "path": "/Mail"
         },
         "runAfter": {}
      }
   },
   "foreach": "@triggerBody()?['Attachments']",
   "runAfter": {}
}
```

Yalnızca tetikleyiciden çıkış olarak geçirilen bir diziyi belirtmek için, bu ifade tetikleyici gövdesinden <*dizi adı*> dizisini alır. Dizi yoksa, bir hatadan kaçınmak için, ifadesi `?` işlecini kullanır:

`@triggerBody()?['<array-name>']` 

<a name="if-action"></a>

### <a name="if-action"></a>Eğer eylem

*Koşullu bir deyim*olan bu eylem, bir koşulu temsil eden bir ifadeyi değerlendirir ve koşulun doğru ya da yanlış olduğunu temel alarak farklı bir dal çalıştırır. Koşul doğru ise, koşul "başarılı" durumla işaretlenir. [Koşullu deyimler oluşturmayı](../logic-apps/logic-apps-control-flow-conditional-statement.md)öğrenin.

``` json
"Condition": {
   "type": "If",
   "expression": { "<condition>" },
   "actions": {
      "<action-1>": { "<action-definition>" }
   },
   "else": {
      "actions": {
        "<action-2>": { "<action-definition" }
      }
   },
   "runAfter": {}
}
```

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*koşul*> | JSON nesnesi | Değerlendirmek için bir ifade olabilen koşul | 
| <*eylem-1*> | JSON nesnesi | <*Koşul*> doğru olarak değerlendirildiğinde çalıştırılacak eylem | 
| <*eylem-tanım*> | JSON nesnesi | Eylemin tanımı | 
| <*eylem-2*> | JSON nesnesi | <*Koşul*> yanlış olarak değerlendirildiğinde çalıştırılacak eylem | 
|||| 

`actions` Veya`else` nesnelerindeki eylemler şu durumları alır:

* Çalıştırıldığında ve başarılı olduğunda "başarılı oldu"
* Çalıştırıldığında ve başarısız olduğunda "başarısız oldu"
* İlgili dal çalıştırılmazsa "atlandı"

*Örnek*

Bu koşul, tamsayı değişkeni sıfırdan büyük bir değere sahip olduğunda, iş akışının bir Web sitesini denetlediğini belirtir. Değişken sıfır veya daha azsa, iş akışı farklı bir Web sitesini denetler.

```json
"Condition": {
   "type": "If",
   "expression": {
      "and": [ {
         "greater": [ "@variables('myIntegerVariable')", 0 ] 
      } ]
   },
   "actions": { 
      "HTTP - Check this website": {
         "type": "Http",
         "inputs": {
         "method": "GET",
            "uri": "http://this-url"
         },
         "runAfter": {}
      }
   },
   "else": {
      "actions": {
         "HTTP - Check this other website": {
            "type": "Http",
            "inputs": {
               "method": "GET",
               "uri": "http://this-other-url"
            },
            "runAfter": {}
         }
      }
   },
   "runAfter": {}
}
```  

#### <a name="how-conditions-use-expressions"></a>Koşulların ifadeleri nasıl kullanır

İfadeleri koşullarda nasıl kullanabileceğinizi gösteren bazı örnekler şunlardır:
  
| JSON | Sonuç | 
|------|--------| 
| "Expression": "@parameters(' <*hasSpecialAction*> ')" | Yalnızca Boole ifadeleri için, koşul doğru değerlendirilen herhangi bir değer için geçirilir. <p>Diğer türleri Boolean 'a dönüştürmek için şu işlevleri kullanın: `empty()` veya. `equals()` | 
| "Expression": "@greater(Eylemler (' <*eylem*> '). Output. Value, Parameters (' <*Threshold*> '))" | Karşılaştırma işlevleri için, eylem yalnızca <*eylem*> çıktısı <*eşik*> değerinden daha büyükse çalışır. | 
| "Expression": "@or(daha fazla (Eylemler (' <*eylem*> '). çıktı. değer, parametreler (' <*eşiği*> ')), daha az (Eylemler (' <*aynı eylem*> '). çıkış. değer, 100))" | Mantıksal işlevler ve iç içe geçmiş Boole ifadeleri oluşturmak için eylem, <*eylem*> çıktısı <*eşik*> değerinden veya 100 ' den fazla olduğunda çalışır. | 
| "Expression": "@equals(length (Eylemler (' <*Action*> '). çıkışlar. Errors), 0))" | Dizide herhangi bir öğe olup olmadığını denetlemek için dizi işlevlerini kullanabilirsiniz. İşlem, `errors` dizi boş olduğunda çalışır. | 
||| 

<a name="scope-action"></a>

### <a name="scope-action"></a>Kapsam eylemi

Bu eylem, bu kapsamdaki eylemlerin çalışması bittikten sonra kendi durumlarını alan, işlemleri mantıksal olarak *kapsamlar*halinde gruplandırır. Daha sonra, diğer eylemlerin çalıştırılıp çalıştırılmadığını anlamak için kapsamın durumunu kullanabilirsiniz. [Kapsamları oluşturmayı](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)öğrenin.

```json
"Scope": {
   "type": "Scope",
   "actions": {
      "<inner-action-1>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      },
      "<inner-action-2>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      }
   }
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------|  
| <*iç eylem-1... No*> | JSON nesnesi | Kapsam içinde çalışan bir veya daha fazla eylem |
| <*eylem-girişler*> | JSON nesnesi | Her eyleme ait girişler |
|||| 

<a name="switch-action"></a>

### <a name="switch-action"></a>Değiştirme eylemi

Bu eylem, *Switch deyimleri*olarak da bilinen, diğer eylemleri *durumlar*halinde düzenler ve bir varsa, varsayılan durum dışında her bir durum için bir değer atar. İş akışınız çalıştığında, **anahtar** eylemi bir ifade, nesne veya belirteçten değeri her bir durum için belirtilen değerlere göre karşılaştırır. **Switch** eylemi eşleşen bir durum bulursa, iş akışınız yalnızca söz konusu servis talebiyle ilgili eylemleri çalıştırır. **Anahtar** eylemi her çalıştığında yalnızca bir eşleşen durum var veya eşleşme yok. Hiçbir eşleşme yoksa, **anahtar** eylemi varsayılan eylemleri çalıştırır. [Switch deyimlerini oluşturmayı](../logic-apps/logic-apps-control-flow-switch-statement.md)öğrenin.

``` json
"Switch": {
   "type": "Switch",
   "expression": "<expression-object-or-token>",
   "cases": {
      "Case": {
         "actions": {
           "<action-name>": { "<action-definition>" }
         },
         "case": "<matching-value>"
      },
      "Case_2": {
         "actions": {
           "<action-name>": { "<action-definition>" }
         },
         "case": "<matching-value>"
      }
   },
   "default": {
      "actions": {
         "<default-action-name>": { "<default-action-definition>" }
      }
   },
   "runAfter": {}
}
```

*Gerekli*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*ifade-veya-belirteç*> | Varies | Değerlendirilecek ifade, JSON nesnesi veya belirteç | 
| <*eylem-ad*> | String | Eşleşen durum için çalıştırılacak eylemin adı | 
| <*eylem-tanım*> | JSON nesnesi | Eşleşen durum için çalıştırılacak eylemin tanımı | 
| <*eşleşen değer*> | Varies | Değerlendirilen sonuçla karşılaştırılacak değer | 
|||| 

*Seçim*

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*varsayılan eylem-adı*> | String | Eşleşen bir durum yoksa çalıştırılacak varsayılan eylemin adı | 
| <*varsayılan eylem-tanım*> | JSON nesnesi | Eşleşen bir durum mevcut olmadığında çalıştırılacak eylemin tanımı | 
|||| 

*Örnek*

Bu eylem tanımı, kişinin onay isteği e-postasına yanıt verip vermediğini "Onayla" seçeneğini veya "Reddet" seçeneğini seçili olarak değerlendirir. Bu seçime bağlı olarak, **Switch** eylemi, farklı bir e-posta göndermek, ancak her durumda farklı bir e-posta göndermek için, eşleşen durum için eylemleri çalıştırır. 

``` json
"Switch": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions": {
            "Send_an_email": { 
               "type": "ApiConnection",
               "inputs": {
                  "Body": "Thank you for your approval.",
                  "Subject": "Response received", 
                  "To": "Sophie.Owen@contoso.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            },
            "runAfter": {}
         },
         "case": "Approve"
      },
      "Case_2": {
         "actions": {
            "Send_an_email_2": { 
               "type": "ApiConnection",
               "inputs": {
                  "Body": "Thank you for your response.",
                  "Subject": "Response received", 
                  "To": "Sophie.Owen@contoso.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            },
            "runAfter": {}     
         },
         "case": "Reject"
      }
   },
   "default": {
      "actions": { 
         "Send_an_email_3": { 
            "type": "ApiConnection",
            "inputs": {
               "Body": "Please respond with either 'Approve' or 'Reject'.",
               "Subject": "Please respond", 
               "To": "Sophie.Owen@contoso.com"
            },
            "host": {
               "connection": {
                  "name": "@parameters('$connections')['office365']['connectionId']"
               }
            },
            "method": "post",
            "path": "/Mail"
         },
         "runAfter": {} 
      }
   },
   "runAfter": {
      "Send_approval_email": [ 
         "Succeeded"
      ]
   }
}
```

<a name="until-action"></a>

### <a name="until-action"></a>Until eylemi

Bu döngü eylemi, belirtilen koşul doğru olana kadar çalışan eylemleri içerir. Döngü, tüm diğer eylemler çalıştırıldıktan sonra koşulu son adım olarak denetler. `"actions"` Nesnesine birden fazla eylem ekleyebilirsiniz ve eylem en az bir sınır tanımlamalıdır. ["Until" döngülerini oluşturmayı](../logic-apps/logic-apps-control-flow-loops.md#until-loop)öğrenin. 

```json
 "Until": {
   "type": "Until",
   "actions": {
      "<action-name>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      },
      "<action-name>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      }
   },
   "expression": "<condition>",
   "limit": {
      "count": <loop-count>,
      "timeout": "<loop-timeout>"
   },
   "runAfter": {}
}
```

| Value | Type | Açıklama | 
|-------|------|-------------| 
| <*eylem-ad*> | String | Döngü içinde çalıştırmak istediğiniz eylemin adı | 
| <*eylem-tür*> | String | Çalıştırmak istediğiniz eylem türü | 
| <*eylem-girişler*> | Türlerini | Çalıştırılacak eylemin girişleri | 
| <*koşul*> | String | Döngüdeki tüm eylemlerin çalışmasını bitirdikten sonra değerlendirilecek koşul veya ifade | 
| <*döngü sayısı*> | Integer | Eylemin çalışacağı en çok döngü sayısı sınırı. Varsayılan `count` değer 60 ' dir. | 
| <*döngü zaman aşımı*> | String | Döngünün en uzun sürede çalışacağı sınır. `timeout` Varsayılan`PT1H`değer, gereken [ISO 8601 biçimidir](https://en.wikipedia.org/wiki/ISO_8601). |
|||| 

*Örnek*

Bu döngü eylemi tanımı, bu koşullardan biri karşılanana kadar belirtilen URL 'ye bir HTTP isteği gönderir: 

* İstek, "200 OK" durum kodu ile bir yanıt alır.
* Döngü 60 kez çalışır.
* Döngü bir saat için çalıştırıldı.

```json
 "Run_until_loop_succeeds_or_expires": {
    "type": "Until",
    "actions": {
        "HTTP": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {}
        }
    },
    "expression": "@equals(outputs('HTTP')['statusCode'], 200)",
    "limit": {
        "count": 60,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

<a name="subscribe-unsubscribe"></a>

## <a name="webhooks-and-subscriptions"></a>Web kancaları ve abonelikler

Web kancası tabanlı tetikleyiciler ve eylemler uç noktaları düzenli olarak denetlemez, ancak bunun yerine bu uç noktalarda belirli olayları veya verileri bekler. Bu Tetikleyiciler ve Eylemler, uç noktaların yanıt gönderebileceği bir *geri çağırma URL 'si* sağlayarak uç noktalara *abone* olur.

Bu `subscribe` çağrı, iş akışı herhangi bir şekilde değiştirildiğinde (örneğin, kimlik bilgileri yenilendiğinde veya bir tetikleyici veya eylem için giriş parametreleri değiştiğinde) gerçekleşir. Bu çağrı, standart HTTP eylemleri ile aynı parametreleri kullanır. 

Bir işlem tetikleyiciyi veya eylemi geçersiz yaptığında çağrıotomatikolarakgerçekleşir,örneğin:`unsubscribe`

* Tetikleyiciyi silme veya devre dışı bırakma. 
* İş akışını silme veya devre dışı bırakma. 
* Aboneliği silme veya devre dışı bırakma. 

Bu çağrıları desteklemek için, `@listCallbackUrl()` ifade tetikleyici veya eylem için benzersiz bir "geri arama URL 'si" döndürür. Bu URL, hizmetin REST API kullanan uç noktalar için benzersiz tanımlayıcıyı temsil eder. Bu işlevin parametreleri Web kancası tetikleyicisi veya eylemiyle aynıdır.

<a name="asynchronous-limits"></a>

## <a name="change-asynchronous-duration"></a>Zaman uyumsuz süreyi değiştirme

Her iki tetikleyici ve eylem için, `limit.timeout` özelliği ekleyerek zaman uyumsuz düzenin süresini belirli bir zaman aralığına sınırlayabilirsiniz. Bu şekilde, zaman aralığı `Cancelled` `ActionTimedOut` üzerinde işlem bitmemişse, eylemin durumu kodla olarak işaretlenir. Özelliği ISO [8601 biçimini](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)kullanır. `timeout` 

``` json
"<trigger-or-action-name>": {
   "type": "Workflow | Webhook | Http | ApiConnectionWebhook | ApiConnection",
   "inputs": {},
   "limit": {
      "timeout": "PT10S"
   },
   "runAfter": {}
}
```

<a name="runtime-config-options"></a>

## <a name="runtime-configuration-settings"></a>Çalışma zamanı yapılandırma ayarları

Tetikleyiciler ve eylemler için varsayılan çalışma zamanı davranışını tetikleyici veya eylem tanımında bu `runtimeConfiguration` özelliklerle değiştirebilirsiniz.

| Özellik | Tür | Açıklama | Tetikleyici veya eylem | 
|----------|------|-------------|-------------------| 
| `runtimeConfiguration.concurrency.runs` | Integer | Aynı anda veya paralel olarak çalışabilen iş akışı örneklerinin sayısı için [*varsayılan sınırı*](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) değiştirin. Bu değer, arka uç sistemlerinin aldığı isteklerin sayısını sınırlamanıza yardımcı olabilir. <p>Özelliği özelliği olarak ayarlama ile aynışekildeçalışacakşekildeayarlamak.`SingleInstance` `1` `runs` `operationOptions` Her iki özelliği de ayarlayabilirsiniz, ancak ikisini birden kullanamazsınız. <p>Varsayılan sınırı değiştirmek için bkz. [tetikleyici eşzamanlılık](#change-trigger-concurrency) veya [tetikleyici örneklerini sırayla](#sequential-trigger)değiştirme. | Tüm tetikleyiciler | 
| `runtimeConfiguration.concurrency.maximumWaitingRuns` | Integer | İş akışınız zaten maksimum eşzamanlı örnekleri çalıştırıyorsa çalışmayı beklemeleri gereken iş akışı örneklerinin sayısı için [*varsayılan sınırı*](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) değiştirin. `concurrency.runs` Özelliğindeki eşzamanlılık limitini değiştirebilirsiniz. <p>Varsayılan sınırı değiştirmek için bkz. [değişiklik bekleyen çalışma sınırı](#change-waiting-runs). | Tüm tetikleyiciler | 
| `runtimeConfiguration.concurrency.repetitions` | Integer | Aynı anda veya paralel olarak çalışabilen "for each" döngüsü yinelemelerinin sayısı için [*varsayılan sınırı*](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) değiştirin. <p>Özelliği özelliği olarak ayarlama ile aynışekildeçalışacakşekildeayarlamak.`SingleInstance` `1` `repetitions` `operationOptions` Her iki özelliği de ayarlayabilirsiniz, ancak ikisini birden kullanamazsınız. <p>Varsayılan sınırı değiştirmek için, " [her bir eşzamanlılık için" değiştirme](#change-for-each-concurrency) veya "her bir" [for each "döngüsü sırayla](#sequential-for-each)' ne bakın. | Eylem: <p>[Foreach](#foreach-action) | 
| `runtimeConfiguration.paginationPolicy.minimumItemCount` | Integer | Tarafından desteklenen ve sayfalama özelliği açık olan belirli eylemler için, bu değer alınacak *en az* sonuç sayısını belirtir. <p>Sayfalandırmayı açmak için bkz. [sayfalama kullanarak toplu veri, öğe veya sonuç edinme](../logic-apps/logic-apps-exceed-default-page-size-with-pagination.md) | Eylem: Değiştirilecek |
| `runtimeConfiguration.secureData.properties` | Array | Birçok tetikleyici ve eylem üzerinde, bu ayarlar mantıksal uygulamanın çalıştırma geçmişinden girdileri, çıkışları veya her ikisini de gizler. <p>Bu verilerin güvenliğini sağlamak için bkz. [çalıştırma geçmişinden girişleri ve çıkışları gizleme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-data-code-view). | Birçok tetikleyici ve eylem |
| `runtimeConfiguration.staticResult` | JSON nesnesi | ' Yi destekleyen ve [statik sonuç](../logic-apps/test-logic-apps-mock-data-static-results.md) ayarı açık olan eylemler için, `staticResult` nesne şu özniteliklere sahiptir: <p>- `name`, mantıksal uygulama `staticResults` `definition` iş akışınızın özniteliğinde özniteliği içinde görüntülenen geçerli eylemin statik sonuç tanımı adına başvuran. Daha fazla bilgi için bkz. [statik sonuçlar-Iş akışı tanımlama dili Için şema başvurusu](../logic-apps/logic-apps-workflow-definition-language.md#static-results). <p> - `staticResultOptions`Bu, statik sonuçların `Enabled` geçerli eylem için olup olmadığını belirtir. <p>Statik sonuçları açmak için, bkz. [statik sonuçlar ayarlayarak, veri ve sahte verilerle test mantığı uygulamaları](../logic-apps/test-logic-apps-mock-data-static-results.md) | Eylem: Değiştirilecek |
||||| 

<a name="operation-options"></a>

## <a name="operation-options"></a>İşlem seçenekleri

Tetikleyiciler ve eylemler için varsayılan davranışı tetikleyici veya eylem tanımındaki `operationOptions` özelliği ile değiştirebilirsiniz.

| İşlem seçeneği | Type | Açıklama | Tetikleyici veya eylem | 
|------------------|------|-------------|-------------------| 
| `DisableAsyncPattern` | String | HTTP tabanlı eylemleri zaman uyumsuz yerine eşzamanlı olarak çalıştırın. <p><p>Bu seçeneği ayarlamak için bkz. [eylemleri zaman uyumlu olarak çalıştırma](#asynchronous-patterns). | Eylem <p>[Apiconnection](#apiconnection-action), <br>[HTTP](#http-action), <br>[Yanıt](#response-action) | 
| `OptimizedForHighThroughput` | String | 5 dakika [](../logic-apps/logic-apps-limits-and-config.md#throughput-limits) başına eylem yürütmelerinin sayısı [üst sınırını en fazla](../logic-apps/logic-apps-limits-and-config.md#throughput-limits)olacak şekilde değiştirin. <p><p>Bu seçeneği ayarlamak için bkz. [yüksek verimlilik modunda çalıştırma](#run-high-throughput-mode). | Tüm eylemler | 
| `Sequential` | String | "Her" for each "döngüsü yinelemelerini, paralel olarak aynı anda değil, tek bir kez çalıştırın. <p>Bu seçenek `runtimeConfiguration.concurrency.repetitions` `1`özelliği olarak ayarlamayla aynı şekilde çalışmaktadır. Her iki özelliği de ayarlayabilirsiniz, ancak ikisini birden kullanamazsınız. <p><p>Bu seçeneği ayarlamak için, bkz. ["for each" döngülerini sırayla çalıştır](#sequential-for-each).| Eylem: <p>[Foreach](#foreach-action) | 
| `SingleInstance` | String | Her mantıksal uygulama örneği için tetikleyiciyi sırayla çalıştırın ve sonraki mantıksal uygulama örneğini tetiklemeden önce daha önce etkin çalıştırmanın bitmesini bekleyin. <p><p>Bu seçenek `runtimeConfiguration.concurrency.runs` `1`özelliği olarak ayarlamayla aynı şekilde çalışmaktadır. Her iki özelliği de ayarlayabilirsiniz, ancak ikisini birden kullanamazsınız. <p>Bu seçeneği ayarlamak için bkz. [tetikleyici örnekleri ardışık](#sequential-trigger)olarak. | Tüm tetikleyiciler | 
||||

<a name="change-trigger-concurrency"></a>

### <a name="change-trigger-concurrency"></a>Tetikleyici eşzamanlılık değiştirme

Varsayılan olarak, mantıksal uygulama örnekleri aynı anda, eşzamanlı olarak veya [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)paralel olarak çalışır. Bu nedenle, her tetikleyici örneği, önceki iş akışı örneği çalışmaya başlamadan önce tetiklenir. Bu sınır, arka uç sistemlerinin aldığı istek sayısını denetlemeye yardımcı olur. 

Varsayılan sınırı değiştirmek için, kod görünümü Düzenleyicisi 'ni veya Logic Apps tasarımcısını kullanarak eşzamanlılık ayarını tasarımcı aracılığıyla değiştirmek, temel tetikleyici tanımında `runtimeConfiguration.concurrency.runs` özelliği ekler veya günceller, ya da tam tersi. Bu özellik paralel olarak çalışabilecek en fazla iş akışı örneği sayısını denetler. 

> [!NOTE] 
> Tetikleyiciyi tasarımcı veya kod görünümü Düzenleyicisi kullanarak sırayla çalışacak şekilde ayarlarsanız, tetikleyicisinin `operationOptions` özelliğini kod görünümü düzenleyicisinde olarak `SingleInstance` ayarlamayın. Aksi takdirde, doğrulama hatası alırsınız. Daha fazla bilgi için bkz. [örnekleri sırayla tetikleme](#sequential-trigger).

#### <a name="edit-in-code-view"></a>Kod görünümünde Düzenle 

Temel tetikleyici tanımında `runtimeConfiguration.concurrency.runs` özelliği, `1` ve `50` değerleri dahil olmak üzere bir değere ekleyin veya güncelleştirin.

Eşzamanlı çalıştırmaları 10 örneğe sınırlayan bir örnek aşağıda verilmiştir:

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": 10
      }
   }
}
```

#### <a name="edit-in-logic-apps-designer"></a>Logic Apps tasarımcısında Düzenle

1. Tetikleyicinin sağ üst köşesinde üç nokta (...) düğmesini ve ardından **Ayarlar**' ı seçin.

2. **Eşzamanlılık denetimi**altında **sınırı** **Açık**olarak ayarlayın. 

3. **Paralellik** sürgüsünün derecesini istediğiniz değere sürükleyin. Mantıksal uygulamanızı sırayla çalıştırmak için kaydırıcı değerini **1**olarak sürükleyin.

<a name="change-for-each-concurrency"></a>

### <a name="change-for-each-concurrency"></a>Eşzamanlılık "for each"

Varsayılan olarak, "her" for döngüsü yinelemesi aynı anda veya paralel, [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar çalışır. Varsayılan sınırı değiştirmek için, kod görünümü Düzenleyicisi 'ni veya Logic Apps tasarımcısını kullanarak eşzamanlılık ayarını tasarımcı aracılığıyla değiştirmeniz, temel alınan "her" eylem tanımında `runtimeConfiguration.concurrency.repetitions` özelliği ekler veya günceller, ya da tam tersi. Bu özellik paralel çalışabilecek en fazla yineleme sayısını denetler.

> [!NOTE] 
> "For each" eylemini tasarımcı veya kod görünümü Düzenleyicisi kullanarak sırayla çalışacak şekilde ayarlarsanız, eylemin `operationOptions` özelliğini kod görünümü düzenleyicisinde olarak `Sequential` ayarlamayın. Aksi takdirde, doğrulama hatası alırsınız. Daha fazla bilgi için bkz. ["for each" döngülerini sırayla çalıştırma](#sequential-for-each).

#### <a name="edit-in-code-view"></a>Kod görünümünde Düzenle 

Temel alınan "for each" tanımında `runtimeConfiguration.concurrency.repetitions` özelliği, `1` ve `50` değerleri dahil olmak üzere bir değere ekleyin veya güncelleştirin. 

Eşzamanlı çalıştırmaları 10 yineleme ile sınırlayan bir örnek aşağıda verilmiştir:

```json
"For_each" {
   "type": "Foreach",
   "actions": { "<actions-to-run>" },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "runtimeConfiguration": {
      "concurrency": {
         "repetitions": 10
      }
   }
}
```

#### <a name="edit-in-logic-apps-designer"></a>Logic Apps tasarımcısında Düzenle

1. Her eylem **için** sağ üst köşedeki üç nokta (...) düğmesini ve ardından **Ayarlar**' ı seçin.

2. **Eşzamanlılık denetimi**altında **eşzamanlılık denetimini** **Açık**olarak ayarlayın. 

3. **Paralellik** sürgüsünün derecesini istediğiniz değere sürükleyin. Mantıksal uygulamanızı sırayla çalıştırmak için kaydırıcı değerini **1**olarak sürükleyin.

<a name="change-waiting-runs"></a>

### <a name="change-waiting-runs-limit"></a>Değişiklik bekleyen çalışma sınırı

Varsayılan olarak, mantıksal uygulama iş akışı örneklerinin hepsi aynı anda, aynı anda veya [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)paralel olarak çalışır. Her tetikleyici örneği, daha önce etkin iş akışı örneğinin çalışması bitmeden önce ateşlenir. [Bu varsayılan sınırı değiştirebilseniz](#change-trigger-concurrency)de, iş akışı örneklerinin sayısı yeni eşzamanlılık sınırına ulaştığında, diğer tüm yeni örneklerin çalıştırılması beklenmelidir. 

Beklemeleri gereken çalıştırmanın sayısı, [varsayılan bir sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)sahiptir ve bu da değiştirebilirsiniz. Ancak, mantıksal uygulamanız bekleme çalıştırmalarının sınırına ulaştıktan sonra, Logic Apps altyapısı artık yeni çalıştırmalar kabul etmez. İstek ve Web kancası Tetikleyicileri 429 hata döndürüyor ve yinelenen Tetikleyiciler yoklama girişimlerini atmaya başladı.

Bekleyen çalışmalarla ilgili varsayılan sınırı değiştirmek için, temel tetikleyici tanımında `runtimeConfiguration.concurency.maximumWaitingRuns` özelliği ile `100`arasında `0` bir değer ekleyin. 

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "runtimeConfiguration": {
      "concurrency": {
         "maximumWaitingRuns": 50
      }
   }
}
```

<a name="sequential-trigger"></a>

### <a name="trigger-instances-sequentially"></a>Örnekleri sıralı olarak Tetikle

Her mantıksal uygulama iş akışı örneğini yalnızca önceki örnek çalışmayı tamamladığında çalıştırmak için tetikleyiciyi sıralı olarak çalışacak şekilde ayarlayın. Aynı zamanda tasarımcı aracılığıyla eşzamanlılık ayarını değiştirmek, temel tetikleyici tanımındaki `runtimeConfiguration.concurrency.runs` özelliği de ekler veya günceller, ya da tam tersi de geçerlidir. Logic Apps 

> [!NOTE] 
> Tasarımcıyı veya kod görünümü düzenleyicisini kullanarak sırayla çalışacak bir tetikleyici ayarladığınızda, tetikleyicisinin `operationOptions` özelliğini kod görünümü düzenleyicisinde olarak `Sequential` ayarlamayın. Aksi takdirde, doğrulama hatası alırsınız. 

#### <a name="edit-in-code-view"></a>Kod görünümünde Düzenle

Tetikleyici tanımında, bu özelliklerden birini ayarlayın, ancak ikisini birden kullanmayın. 

Özelliği şu şekilde `1`ayarlayın: `runtimeConfiguration.concurrency.runs`

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": 1
      }
   }
}
```

*veya*

Özelliği şu şekilde `SingleInstance`ayarlayın: `operationOptions`

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "operationOptions": "SingleInstance"
}
```

#### <a name="edit-in-logic-apps-designer"></a>Logic Apps tasarımcısında Düzenle

1. Tetikleyicinin sağ üst köşesinde üç nokta (...) düğmesini ve ardından **Ayarlar**' ı seçin.

2. **Eşzamanlılık denetimi**altında **sınırı** **Açık**olarak ayarlayın. 

3. **Paralellik** sürgüsünün derecesini sayıya `1`sürükleyin. 

<a name="sequential-for-each"></a>

### <a name="run-for-each-loops-sequentially"></a>"Her" for each "döngülerini sırayla çalıştır

Yalnızca önceki yineleme çalıştıktan sonra "for each" döngüsü yinelemesi çalıştırmak için, "for each" eylemini sıralı olarak çalışacak şekilde ayarlayın. Eylemin eşzamanlılık ' i tasarımcı aracılığıyla değiştirmek Ayrıca temel eylem tanımındaki `runtimeConfiguration.concurrency.repetitions` özelliği de ekler veya güncelleştirir, ya da tam tersi de geçerlidir. Logic Apps 

> [!NOTE] 
> Tasarımcı veya kod görünümü Düzenleyicisi 'ni kullanarak sırayla çalışacak bir "for each" eylemini ayarladığınızda, kod görünümü düzenleyicisinde eylemin `operationOptions` özelliğini olarak `Sequential` ayarlamayın. Aksi takdirde, doğrulama hatası alırsınız. 

#### <a name="edit-in-code-view"></a>Kod görünümünde Düzenle

Eylem tanımında, bu özelliklerden birini ayarlayın, ancak ikisini birden kullanmayın. 

Özelliği şu şekilde `1`ayarlayın: `runtimeConfiguration.concurrency.repetitions`

```json
"For_each" {
   "type": "Foreach",
   "actions": { "<actions-to-run>" },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "runtimeConfiguration": {
      "concurrency": {
         "repetitions": 1
      }
   }
}
```

*veya*

Özelliği şu şekilde `Sequential`ayarlayın: `operationOptions`

```json
"For_each" {
   "type": "Foreach",
   "actions": { "<actions-to-run>" },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "operationOptions": "Sequential"
}
```

#### <a name="edit-in-logic-apps-designer"></a>Logic Apps tasarımcısında Düzenle

1. **Her** bir eylemin sağ üst köşesinde, üç nokta (...) düğmesini ve ardından **Ayarlar**' ı seçin.

2. **Eşzamanlılık denetimi**altında **eşzamanlılık denetimini** **Açık**olarak ayarlayın. 

3. **Paralellik** sürgüsünün derecesini sayıya `1`sürükleyin. 

<a name="asynchronous-patterns"></a>

### <a name="run-actions-synchronously"></a>Eylemleri zaman uyumlu olarak çalıştır

Varsayılan olarak, tüm HTTP tabanlı eylemler standart zaman uyumsuz işlem modelini izler. Bu model, HTTP tabanlı bir eylem belirtilen uç noktaya bir istek gönderdiğinde, uzak sunucunun "202 kabul EDILDI" yanıtını geri göndereceğini belirtir. Bu yanıt, sunucunun işleme isteğini kabul ettiği anlamına gelir. Logic Apps altyapısı, 202 olmayan bir yanıt olan işlem duraklarına kadar yanıtın konum üst bilgisi tarafından belirtilen URL 'YI denetlemeye devam eder.

Ancak, isteklerin bir zaman aşımı sınırı vardır. bu nedenle uzun süre çalışan eylemler için `operationOptions` `DisableAsyncPattern` özelliği eylem girişlerinin altına ekleyerek ve ayarlayarak zaman uyumsuz davranışı devre dışı bırakabilirsiniz.
  
```json
"<some-long-running-action>": {
   "type": "Http",
   "inputs": { "<action-inputs>" },
   "operationOptions": "DisableAsyncPattern",
   "runAfter": {}
}
```

<a name="run-high-throughput-mode"></a>

### <a name="run-in-high-throughput-mode"></a>Yüksek aktarım hızı modunda çalıştır

Tek bir mantıksal uygulama tanımı için, 5 dakikada bir yürütülen eylemlerin sayısı [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#throughput-limits)sahiptir. Bu sınırı mümkün olan [en yüksek](../logic-apps/logic-apps-limits-and-config.md#throughput-limits) sınıra yükseltmek için `operationOptions` özelliğini olarak `OptimizedForHighThroughput`ayarlayın. Bu ayar mantıksal uygulamanızı "yüksek aktarım hızı" moduna geçirir. 

> [!NOTE]
> Yüksek verimlilik modu önizlemededir. Ayrıca, gerektiğinde bir iş yükünü birden fazla mantıksal uygulama üzerinde dağıtabilirsiniz.

```json
"<action-name>": {
   "type": "<action-type>",
   "inputs": { "<action-inputs>" },
   "operationOptions": "OptimizedForHighThroughput",
   "runAfter": {}
}
```

<a name="connector-authentication"></a>

## <a name="authenticate-http-triggers-and-actions"></a>HTTP Tetikleyicileri ve eylemlerinin kimliklerini doğrulama

HTTP uç noktaları farklı kimlik doğrulama türlerini destekler. Bu HTTP Tetikleyicileri ve eylemleri için kimlik doğrulaması ayarlayabilirsiniz:

* [HTTP](../connectors/connectors-native-http.md)
* [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)
* [HTTP Web kancası](../connectors/connectors-native-webhook.md)

Ayarlayabileceğiniz kimlik doğrulama türleri şunlardır:

* [Temel kimlik doğrulaması](#basic-authentication)
* [İstemci sertifikası kimlik doğrulaması](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth kimlik doğrulaması](#azure-active-directory-oauth-authentication)

> [!IMPORTANT]
> Mantıksal uygulama iş akışı tanımınızın işleyeceği tüm hassas bilgileri koruduğunuzdan emin olun. Güvenli parametreleri kullanın ve verileri gerektiği şekilde kodlayın. Parametreleri kullanma ve güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters)getirme.

<a name="basic-authentication"></a>

### <a name="basic-authentication"></a>Temel kimlik doğrulaması

Azure Active Directory kullanarak [temel kimlik doğrulaması](../active-directory-b2c/active-directory-b2c-custom-rest-api-netfw-secure-basic.md) için, Tetikleyiciniz veya eylem tanımınız, aşağıdaki `authentication` tabloda belirtilen özellikleri içeren bir JSON nesnesi içerebilir. Çalışma zamanında parametre değerlerine erişmek için, `@parameters('parameterName')` [iş akışı Tanım Dili](https://aka.ms/logicappsdocs)tarafından belirtilen ifadesini kullanabilirsiniz. 

| Özellik | Gerekli | Value | Açıklama | 
|----------|----------|-------|-------------| 
| **type** | Evet | Basit | Burada "temel" olan, kullanılacak kimlik doğrulama türü | 
| **Kullanıcı adı** | Evet | "@parameters(' usernameparam ')" | Hedef hizmet uç noktasına erişim doğrulaması için Kullanıcı adı |
| **Parola** | Evet | "@parameters(' passwordparam ')" | Hedef hizmet uç noktasına erişim doğrulaması için parola |
||||| 

Bu örnek http eylem tanımında, `authentication` bölümünde kimlik doğrulaması belirtilir. `Basic` Parametreleri kullanma ve güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters)getirme.

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://www.microsoft.com",
      "authentication": {
         "type": "Basic",
         "username": "@parameters('userNameParam')",
         "password": "@parameters('passwordParam')"
      }
  },
  "runAfter": {}
}
```

> [!IMPORTANT]
> Mantıksal uygulama iş akışı tanımınızın işleyeceği tüm hassas bilgileri koruduğunuzdan emin olun. Güvenli parametreleri kullanın ve verileri gerektiği şekilde kodlayın. Parametrelerin güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters)getirme.

<a name="client-certificate-authentication"></a>

### <a name="client-certificate-authentication"></a>İstemci sertifikası kimlik doğrulaması

Azure Active Directory kullanarak [sertifika tabanlı kimlik doğrulaması](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) için, Tetikleyiciniz veya eylem tanımınız, aşağıdaki `authentication` tabloda belirtilen özellikleri içeren bir JSON nesnesi içerebilir. Çalışma zamanında parametre değerlerine erişmek için, `@parameters('parameterName')` [iş akışı Tanım Dili](https://aka.ms/logicappsdocs)tarafından belirtilen ifadesini kullanabilirsiniz. Kullanabileceğiniz istemci sertifikası sayısına yönelik sınırlar için bkz. [Azure Logic Apps Için sınırlar ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md).

| Özellik | Gerekli | Value | Açıklama |
|----------|----------|-------|-------------|
| **type** | Evet | ClientCertificate | Güvenli Yuva Katmanı (SSL) istemci sertifikaları için kullanılacak kimlik doğrulaması türü. Otomatik olarak imzalanan sertifikalar desteklenirken, SSL için otomatik olarak imzalanan sertifikalar desteklenmez. |
| **Türk** | Evet | "@parameters(' pfxparam ') | Kişisel bilgi değişimi (PFX) dosyasından gelen Base64 kodlamalı içerik |
| **Parola** | Evet | "@parameters(' passwordparam ')" | PFX dosyasına erişim parolası |
||||| 

Bu örnek http eylem tanımında, `authentication` bölümünde kimlik doğrulaması belirtilir. `ClientCertificate` Parametreleri kullanma ve güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters)getirme.

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://www.microsoft.com",
      "authentication": {
         "type": "ClientCertificate",
         "pfx": "@parameters('pfxParam')",
         "password": "@parameters('passwordParam')"
      }
   },
   "runAfter": {}
}
```

> [!IMPORTANT]
> Mantıksal uygulama iş akışı tanımınızın işleyeceği tüm hassas bilgileri koruduğunuzdan emin olun. Güvenli parametreleri kullanın ve verileri gerektiği şekilde kodlayın. Parametrelerin güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters)getirme.

<a name="azure-active-directory-oauth-authentication"></a>

### <a name="azure-active-directory-ad-oauth-authentication"></a>Azure Active Directory (AD) OAuth kimlik doğrulaması

[Azure AD OAuth kimlik doğrulaması](../active-directory/develop/authentication-scenarios.md)için, Tetikleyiciniz veya eylem tanımınız, `authentication` aşağıdaki tabloda belirtilen özellikleri içeren bir JSON nesnesi içerebilir. Çalışma zamanında parametre değerlerine erişmek için, `@parameters('parameterName')` [iş akışı Tanım Dili](https://aka.ms/logicappsdocs)tarafından belirtilen ifadesini kullanabilirsiniz.

| Özellik | Gerekli | Value | Açıklama |
|----------|----------|-------|-------------|
| **type** | Evet | `ActiveDirectoryOAuth` | Azure AD OAuth için "ActiveDirectoryOAuth" olan kullanılacak kimlik doğrulaması türü |
| **iniz** | Hayır | <*URL-for-Authority-Token-Issuer*> | Kimlik doğrulama belirtecini sağlayan yetkilinin URL 'SI |
| **tenant** | Evet | <*Kiracı KIMLIĞI*> | Azure AD kiracısı için kiracı KIMLIĞI |
| **grubu** | Evet | <*kaynaktan yetkilendirme*> | Yetkilendirme için kullanmak istediğiniz kaynak (örneğin,`https://management.core.windows.net/` |
| **ClientID** | Evet | <*istemci KIMLIĞI*> | Yetkilendirme isteyen uygulamanın istemci KIMLIĞI |
| **credentialType** | Evet | "Sertifika" veya "gizli" | İstemcinin yetkilendirme istemek için kullandığı kimlik bilgisi türü. Bu özellik ve değer temel tanımda görünmez, ancak kimlik bilgisi türü için gerekli parametreleri belirler. |
| **Türk** | Evet, yalnızca "sertifika" kimlik bilgisi türü | "@parameters(' pfxparam ') | Kişisel bilgi değişimi (PFX) dosyasından gelen Base64 kodlamalı içerik |
| **Parola** | Evet, yalnızca "sertifika" kimlik bilgisi türü | "@parameters(' passwordparam ')" | PFX dosyasına erişim parolası |
| **gizlilikle** | Evet, yalnızca "gizli" kimlik bilgisi türü | "@parameters(' secretparam ')" | Yetkilendirme isteğinde bulunan istemci parolası |
|||||

Bu örnek http eylem tanımında, `authentication` Bölüm kimlik doğrulamasını ve "gizli" kimlik bilgisi türünü belirtir. `ActiveDirectoryOAuth` Parametreleri kullanma ve güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters)getirme.

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://www.microsoft.com",
      "authentication": {
         "type": "ActiveDirectoryOAuth",
         "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
         "audience": "https://management.core.windows.net/",
         "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
         "secret": "@parameters('secretParam')"
     }
   },
   "runAfter": {}
}
```

> [!IMPORTANT]
> Mantıksal uygulama iş akışı tanımınızın işleyeceği tüm hassas bilgileri koruduğunuzdan emin olun. Güvenli parametreleri kullanın ve verileri gerektiği şekilde kodlayın. Parametrelerin güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters)getirme.

## <a name="next-steps"></a>Sonraki adımlar

* [Iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md) hakkında daha fazla bilgi edinin
