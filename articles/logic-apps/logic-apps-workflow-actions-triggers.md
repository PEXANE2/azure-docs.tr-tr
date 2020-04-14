---
title: Tetikleme ve eylem türleri için şema başvurusu
description: Azure Logic Apps'ta İş Akışı Tanımı Dili tetikleyicisi ve eylem türleri için Şema başvuru kılavuzu
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 01/19/2020
ms.openlocfilehash: 7e14cc00d1bd716b3e4880e585b05447d2e55e2b
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81257445"
---
# <a name="schema-reference-guide-for-trigger-and-action-types-in-azure-logic-apps"></a>Azure Logic Apps'ta tetikleme ve eylem türleri için Şema başvuru kılavuzu

Bu başvuru, mantık uygulamanızın İş [Akışı Tanımı Dili](../logic-apps/logic-apps-workflow-definition-language.md)tarafından açıklanan ve doğrulanan temel iş akışı tanımındaki tetikleyicileri ve eylemleri tanımlamak için kullanılan genel türleri açıklar. Mantık uygulamalarınızda kullanabileceğiniz belirli bağlayıcı tetikleyicileri ve eylemleri bulmak için [Bağlayıcılar genel bakış](https://docs.microsoft.com/connectors/)altındaki listeye bakın.

<a name="triggers-overview"></a>

## <a name="triggers-overview"></a>Tetikleyicilere genel bakış

Her iş akışı, anlık çağrılar tanımlayan ve iş akışını başlatan bir tetikleyici içerir. Genel tetikleyici kategoriler şunlardır:

* Bir hizmetin bitiş noktasını düzenli aralıklarla kontrol eden bir *yoklama tetikleyicisi*

* Uç noktaya abonelik oluşturan ve bitiş noktasının belirtilen olay gerçekleştiğinde veya veriler kullanılabilir olduğunda tetikleyiciyi bildirebilmeleri için *geri arama URL'si* sağlayan bir *itme* tetikleyicisi. Tetikleyici daha sonra ateş etmeden önce bitiş noktasının yanıtını bekler.

Tetikleyiciler bu üst düzey öğelere sahiptir, ancak bazıları isteğe bağlıdır:  
  
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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*tetikleyici adı*> | Dize | Tetikleyicinin adı | 
| <*tetikleme türü*> | Dize | "Http" veya "ApiConnection" gibi tetikleyici türü | 
| <*tetikleme girişleri*> | JSON Nesnesi | Tetikleyicinin davranışını tanımlayan girişler | 
| <*zaman birimi*> | Dize | Tetikleyicinin ne sıklıkta ateşlediğini açıklayan zaman birimi: "İkinci", "Dakika", "Saat", "Gün", "Hafta", "Ay" | 
| <*zaman birimi sayısı*> | Tamsayı | Tetikleyicinin frekansa göre ne sıklıkta ateşlediğini belirten bir değer, yani tetikleyici yeniden çalışana kadar bekleyecek zaman birimlerinin sayısıdır <p>Minimum ve maksimum aralıklar şunlardır: <p>- Ay: 1-16 ay </br>- Gün: 1-500 gün </br>- Saat: 1-12.000 saat </br>- Dakika: 1-72.000 dakika </br>- İkinci: 1-9,999,999 saniye<p>Örneğin, aralık 6 ise ve sıklık "Ay" ise, yineleme her 6 ayda bir olur. | 
|||| 

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*dizi-koşullar ile*> | Dizi | İş akışını çalıştırıp çalıştırmayacağını belirleyen bir veya daha fazla [koşul](#trigger-conditions) içeren bir dizi. Yalnızca tetikleyiciler için kullanılabilir. | 
| <*çalışma zamanı-config-seçenekleri*> | JSON Nesnesi | Özellikleri ayarlayarak `runtimeConfiguration` tetikleme davranışını değiştirebilirsiniz. Daha fazla bilgi için [Runtime yapılandırma ayarlarına](#runtime-config-options)bakın. | 
| <*splitOn-ifade*> | Dize | Bir dizi döndüren tetikleyiciler için, dizi öğelerini işlemek için birden çok iş akışı örneğine [bölen veya *debatchle* ](#split-on-debatch) eden bir ifade belirtebilirsiniz. | 
| <*işlem seçeneği*> | Dize | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için [İşlem seçeneklerine](#operation-options)bakın. | 
|||| 

## <a name="trigger-types-list"></a>Tetikleyici türleri listesi

Her tetikleyici türü, tetikleyicinin davranışını tanımlayan farklı bir arabirime ve girişe sahiptir. 

### <a name="built-in-triggers"></a>Dahili tetikleyiciler

| Tetikleyici türü | Açıklama | 
|--------------|-------------| 
| [**HTTP**](#http-trigger) | Herhangi bir bitiş noktasını denetler veya *anketler.* Bu uç nokta, "202" asynchronous deseni kullanarak veya bir dizi döndürerek belirli bir tetikleyici sözleşmeye uygun olmalıdır. | 
| [**HTTPWebhook**](#http-webhook-trigger) | Mantık uygulamanız için çağrılabilir bir bitiş noktası oluşturur, ancak belirtilen URL'yi kaydolmak veya kaydını iptal etmek için çağırır. |
| [**Yineleme**](#recurrence-trigger) | Yangınlar tanımlı bir zamanlamaya göre. Bu tetikleyiciyi ateşlemek için gelecekteki bir tarih ve saat ayarlayabilirsiniz. Sıklığa bağlı olarak, iş akışınızı çalıştırmak için saatleri ve günleri de belirtebilirsiniz. | 
| [**İstek**](#request-trigger)  | Mantık uygulamanız için çağrılabilir bir uç nokta oluşturur ve "manuel" tetikleyici olarak da bilinir. Örneğin, [bkz: ARAMA, tetikleyici veya http uç noktaları ile yuva iş akışları.](../logic-apps/logic-apps-http-endpoint.md) | 
||| 

### <a name="managed-api-triggers"></a>Yönetilen API tetikleyicileri

| Tetikleyici türü | Açıklama | 
|--------------|-------------| 
| [**ApiConnection**](#apiconnection-trigger) | [Microsoft tarafından yönetilen API'leri](../connectors/apis-list.md)kullanarak bir bitiş noktasını denetler veya *yoklar.* | 
| [**ApiConnectionWebhook**](#apiconnectionwebhook-trigger) | [Microsoft tarafından yönetilen API'leri](../connectors/apis-list.md) abone olmak ve aboneliği iptal etmek için arayarak mantık uygulamanız için çağrılabilir bir bitiş noktası oluşturur. | 
||| 

## <a name="triggers---detailed-reference"></a>Tetikleyiciler - Ayrıntılı başvuru

<a name="apiconnection-trigger"></a>

### <a name="apiconnection-trigger"></a>APIConnection tetikleyicisi  

Bu tetikleyici, [Microsoft tarafından yönetilen API'leri](../connectors/apis-list.md) kullanarak bir bitiş noktasını denetler veya *yoklar,* böylece bu tetikleyicinin parametreleri bitiş noktasına göre farklılık gösterebilir. Bu tetikleyici tanımındaki birçok bölüm isteğe bağlıdır. Tetikleyicinin davranışı bölümlerin dahil edilip edilmediğine bağlıdır.

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

| Değer | Tür | Açıklama |
|-------|------|-------------|
| <*APIConnection_trigger_name*> | Dize | Tetikleyicinin adı |
| <*bağlantı adı*> | Dize | İş akışının kullandığı yönetilen API bağlantısının adı |
| <*yöntem tipi*> | Dize | Yönetilen API ile iletişim kurmak için HTTP yöntemi: "GET", "PUT", "POST", "PATCH", "DELETE" |
| <*api-operasyon*> | Dize | Aramak için API işlemi |
| <*zaman birimi*> | Dize | Tetikleyicinin ne sıklıkta ateşlediğini açıklayan zaman birimi: "İkinci", "Dakika", "Saat", "Gün", "Hafta", "Ay" |
| <*zaman birimi sayısı*> | Tamsayı | Tetikleyicinin frekansa göre ne sıklıkta ateşlediğini belirten bir değer, yani tetikleyici yeniden çalışana kadar bekleyecek zaman birimlerinin sayısıdır <p>Minimum ve maksimum aralıklar şunlardır: <p>- Ay: 1-16 ay </br>- Gün: 1-500 gün </br>- Saat: 1-12.000 saat </br>- Dakika: 1-72.000 dakika </br>- İkinci: 1-9,999,999 saniye<p>Örneğin, aralık 6 ise ve sıklık "Ay" ise, yineleme her 6 ayda bir olur. |
||||

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yeniden deneme davranışı*> | JSON Nesnesi | 408, 429 ve 5XX durum kodu ve herhangi bir bağlantı özel durumu olan aralıklı hatalar için yeniden deneme davranışını özelleştirer. Daha fazla bilgi için yeniden [deneme ilkelerine](../logic-apps/logic-apps-exception-handling.md#retry-policies)bakın. | 
| <*sorgu parametreleri*> | JSON Nesnesi | API çağrısına dahil edilecek tüm sorgu parametreleri. Örneğin, `"queries": { "api-version": "2018-01-01" }` nesne çağrıya ekler. `?api-version=2018-01-01` | 
| <*maksimum çalışır*> | Tamsayı | Varsayılan olarak, iş akışı örnekleri [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar aynı anda (aynı anda veya paralel olarak) çalışır. Yeni bir <*sayısı*> değer ayarlayarak bu sınırı değiştirmek için [bkz.](#change-trigger-concurrency) | 
| <*max-runs-kuyruk*> | Tamsayı | İş akışınız zaten özelliğine göre değiştirebileceğiniz en fazla örnek `runtimeConfiguration.concurrency.runs` sayısını çalıştırıyorsa, tüm yeni çalıştırmalar [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar bu kuyruğa konur. Varsayılan sınırı değiştirmek için [bkz.](#change-waiting-runs) | 
| <*splitOn-ifade*> | Dize | Dizileri döndüren tetikleyiciler için, bu ifade, "her biri için" döngü kullanmak yerine her dizi öğesi için bir iş akışı örneği oluşturup çalıştırabilmeniz için kullanılacak diziye başvurur. <p>Örneğin, bu ifade tetikleyicinin gövde içeriği içinde döndürülen dizideki bir öğeyi temsil eder:`@triggerbody()?['value']` |
| <*işlem seçeneği*> | Dize | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için [İşlem seçeneklerine](#operation-options)bakın. |
||||

*Çıkışlar*
 
| Öğe | Tür | Açıklama |
|---------|------|-------------|
| Üstbilgi | JSON Nesnesi | Yanıttaki üstbilgi |
| body | JSON Nesnesi | Yanıttan vücut |
| durum kodu | Tamsayı | Yanıttaki durum kodu |
|||| 

*Örnek*

Bu tetikleyici tanımı, Office 365 Outlook hesabı için gelen kutusunun içinde her gün e-postayı denetler:

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

### <a name="apiconnectionwebhook-trigger"></a>ApiConnectionWebhook tetikleyici

Bu tetikleyici, Microsoft tarafından yönetilen bir [API](../connectors/apis-list.md)kullanarak bir bitiş noktasına abonelik isteği gönderir, bitiş noktasının yanıt gönderebileceği bir *geri arama URL'si* sağlar ve bitiş noktasının yanıt gelmesini bekler. Daha fazla bilgi için [Endpoint aboneliklerine](#subscribe-unsubscribe)bakın.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*bağlantı adı*> | Dize | İş akışının kullandığı yönetilen API bağlantısının adı | 
| <*gövde içeriği*> | JSON Nesnesi | Yönetilen API'ye yük olarak gönderilecek ileti içeriği | 
|||| 

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yeniden deneme davranışı*> | JSON Nesnesi | 408, 429 ve 5XX durum kodu ve herhangi bir bağlantı özel durumu olan aralıklı hatalar için yeniden deneme davranışını özelleştirer. Daha fazla bilgi için yeniden [deneme ilkelerine](../logic-apps/logic-apps-exception-handling.md#retry-policies)bakın. | 
| <*sorgu parametreleri*> | JSON Nesnesi | API çağrısına dahil edilecek tüm sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesne çağrıya ekler. `?api-version=2018-01-01` | 
| <*maksimum çalışır*> | Tamsayı | Varsayılan olarak, iş akışı örnekleri [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar aynı anda (aynı anda veya paralel olarak) çalışır. Yeni bir <*sayısı*> değer ayarlayarak bu sınırı değiştirmek için [bkz.](#change-trigger-concurrency) | 
| <*max-runs-kuyruk*> | Tamsayı | İş akışınız zaten özelliğine göre değiştirebileceğiniz en fazla örnek `runtimeConfiguration.concurrency.runs` sayısını çalıştırıyorsa, tüm yeni çalıştırmalar [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar bu kuyruğa konur. Varsayılan sınırı değiştirmek için [bkz.](#change-waiting-runs) | 
| <*splitOn-ifade*> | Dize | Dizileri döndüren tetikleyiciler için, bu ifade, "her biri için" döngü kullanmak yerine her dizi öğesi için bir iş akışı örneği oluşturup çalıştırabilmeniz için kullanılacak diziye başvurur. <p>Örneğin, bu ifade tetikleyicinin gövde içeriği içinde döndürülen dizideki bir öğeyi temsil eder:`@triggerbody()?['value']` |
| <*işlem seçeneği*> | Dize | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için [İşlem seçeneklerine](#operation-options)bakın. | 
|||| 

*Örnek*

Bu tetikleyici tanım Office 365 Outlook API'sine abone olur, API bitiş noktasına bir geri arama URL'si sağlar ve yeni bir e-posta geldiğinde bitiş noktasının yanıt vermesi için bitiş noktasını bekler.

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

Bu tetikleyici, belirtilen yineleme zamanlamasına dayalı olarak belirtilen HTTP veya HTTPS bitiş noktasına bir istek gönderir. Tetikleyici daha sonra iş akışının çalışıp çalışmadığını belirlemek için yanıtı denetler.

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "<method-type>",
      "uri": "<HTTP-or-HTTPS-endpoint-URL>",
      "headers": { "<header-content>" },
      "queries": "<query-parameters>",
      "body": "<body-content>",
      "authentication": { "<authentication-type-and-property-values>" },
      "retryPolicy": {
         "type": "<retry-behavior>"
      }
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

| Özellik | Değer | Tür | Açıklama |
|----------|-------|------|-------------|
| `method` | <*yöntem tipi*> | Dize | Giden isteği göndermek için kullanılacak yöntem: "GET", "PUT", "POST", "PATCH" veya "DELETE" |
| `uri` | <*HTTP-veya-HTTPS-endpoint-URL*> | Dize | Giden isteği göndermek istediğiniz HTTP veya HTTPS bitiş noktası URL'si. Maksimum dize boyutu: 2 KB <p>Bir Azure hizmeti veya kaynağı için bu URI sözdizimi kaynak kimliğini ve erişmek istediğiniz kaynağa giden yolu içerir. |
| `frequency` | <*zaman birimi*> | Dize | Tetikleyicinin ne sıklıkta ateşlediğini açıklayan zaman birimi: "İkinci", "Dakika", "Saat", "Gün", "Hafta", "Ay" |
| `interval` | <*zaman birimi sayısı*> | Tamsayı | Tetikleyicinin frekansa göre ne sıklıkta ateşlediğini belirten bir değer, yani tetikleyici yeniden çalışana kadar bekleyecek zaman birimlerinin sayısıdır <p>Minimum ve maksimum aralıklar şunlardır: <p>- Ay: 1-16 ay </br>- Gün: 1-500 gün </br>- Saat: 1-12.000 saat </br>- Dakika: 1-72.000 dakika </br>- İkinci: 1-9,999,999 saniye<p>Örneğin, aralık 6 ise ve sıklık "Ay" ise, yineleme her 6 ayda bir olur. |
|||||

*Isteğe bağlı*

| Özellik | Değer | Tür | Açıklama |
|----------|-------|------|-------------|
| `headers` | <*üstbilgi içeriği*> | JSON Nesnesi | İsteğe eklemeniz gereken üstbilgi <p>Örneğin, dili ve türü ayarlamak için: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| `queries` | <*sorgu parametreleri*> | JSON Nesnesi | İstekte kullanmanız gereken sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesne isteğe ekler. `?api-version=2018-01-01` |
| `body` | <*gövde içeriği*> | JSON Nesnesi | İstekle birlikte yük olarak gönderilecek ileti içeriği |
| `authentication` | <*kimlik doğrulama-türü ve özellik değerleri*> | JSON Nesnesi | İsteğin giden istekleri doğrulamak için kullandığı kimlik doğrulama modeli. Daha fazla bilgi için bkz: [Giden aramalara kimlik doğrulama ekle.](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound) Zamanlayıcının `authority` ötesinde, özellik desteklenir. Belirtilmediğinde, varsayılan değer `https://management.azure.com/`, ancak farklı bir değer kullanabilirsiniz. |
| `retryPolicy` > `type` | <*yeniden deneme davranışı*> | JSON Nesnesi | 408, 429 ve 5XX durum kodu ve herhangi bir bağlantı özel durumu olan aralıklı hatalar için yeniden deneme davranışını özelleştirer. Daha fazla bilgi için yeniden [deneme ilkelerine](../logic-apps/logic-apps-exception-handling.md#retry-policies)bakın. |
| `runs` | <*maksimum çalışır*> | Tamsayı | Varsayılan olarak, iş akışı örnekleri [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar aynı anda (aynı anda veya paralel olarak) çalışır. Yeni bir <*sayısı*> değer ayarlayarak bu sınırı değiştirmek için [bkz.](#change-trigger-concurrency) |
| `maximumWaitingRuns` | <*max-runs-kuyruk*> | Tamsayı | İş akışınız zaten özelliğine göre değiştirebileceğiniz en fazla örnek `runtimeConfiguration.concurrency.runs` sayısını çalıştırıyorsa, tüm yeni çalıştırmalar [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar bu kuyruğa konur. Varsayılan sınırı değiştirmek için [bkz.](#change-waiting-runs) |
| `operationOptions` | <*işlem seçeneği*> | Dize | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için [İşlem seçeneklerine](#operation-options)bakın. |
|||||

*Çıkışlar*

| Öğe | Tür | Açıklama |
|---------|------|-------------| 
| Üstbilgi | JSON Nesnesi | Yanıttaki üstbilgi | 
| body | JSON Nesnesi | Yanıttan vücut | 
| durum kodu | Tamsayı | Yanıttaki durum kodu | 
|||| 

*Gelen istekler için gereksinimler*

Mantık uygulamanızla iyi çalışmak için bitiş noktasının belirli bir tetikleyici desenine veya sözleşmeye uyması ve bu özellikleri tanıması gerekir:  
  
| Yanıt | Gerekli | Açıklama | 
|----------|----------|-------------| 
| Durum kodu | Evet | "200 Tamam" durum kodu çalıştırmayı başlatır. Diğer durum kodu çalıştırmayı başlatmaz. | 
| Yeniden deneme dene-sonra üstbilgi | Hayır | Mantık uygulamanızın bitiş noktasını yeniden yoklayana saniye sayısı | 
| Konum üstbilgi | Hayır | Bir sonraki yoklama aralığında arayacak URL. Belirtilmemişse, orijinal URL kullanılır. | 
|||| 

*Farklı istekler için örnek davranışlar*

| Durum kodu | Sonra yeniden deneyin | Davranış | 
|-------------|-------------|----------|
| 200 | {yok} | İş akışını çalıştırın ve tanımlanan yinelemeden sonra daha fazla veri için yeniden denetleyin. | 
| 200 | 10 saniye | İş akışını çalıştırın ve 10 saniye sonra daha fazla veri için yeniden denetleyin. |  
| 202 | 60 saniye | İş akışını tetiklemeyin. Bir sonraki deneme, tanımlanan yinelemeye bağlı olarak bir dakika içinde gerçekleşir. Tanımlanan yineleme bir dakikadan azsa, yeniden denemeden sonraki üstbilgi önceliklidir. Aksi takdirde, tanımlanan yineleme kullanılır. | 
| 400 | {yok} | Kötü istek, iş akışını çalıştırma. Hayır `retryPolicy` tanımlanırsa, varsayılan ilke kullanılır. Yeniden deneme sayısına ulaşıldıktan sonra, tetikleyici tanımlanan yinelemeden sonra verileri yeniden denetler. | 
| 500 | {yok}| Sunucu hatası, iş akışını çalıştırmayın. Hayır `retryPolicy` tanımlanırsa, varsayılan ilke kullanılır. Yeniden deneme sayısına ulaşıldıktan sonra, tetikleyici tanımlanan yinelemeden sonra verileri yeniden denetler. | 
|||| 

<a name="http-webhook-trigger"></a>

### <a name="httpwebhook-trigger"></a>HTTPWebhook tetikleme  

Bu tetikleyici, belirtilen bitiş noktası URL'sini arayarak aboneliği kaydedebilecek bir uç nokta oluşturarak mantık uygulamanızı çağrılabilir hale getirir. İş akışınızda bu tetikleyiciyi oluşturduğunuzda, giden bir istek aboneliği kaydetmek için arama yapar. Bu şekilde, tetikleyici olayları dinlemeye başlayabilir. Bir işlem bu tetikleyiciyi geçersiz kıldığında, giden bir istek aboneliği iptal etmek için otomatik olarak arama yapar. Daha fazla bilgi için [Endpoint aboneliklerine](#subscribe-unsubscribe)bakın.

**Bir HTTPWebhook** tetikleyicisi üzerinde [eşzamanlı sınırlar](#asynchronous-limits) da belirtebilirsiniz. Tetikleyicinin davranışı kullandığınız veya atladığınız bölümlere bağlıdır.

```json
"HTTP_Webhook": {
   "type": "HttpWebhook",
   "inputs": {
      "subscribe": {
         "method": "<method-type>",
         "uri": "<endpoint-subscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-type>" },
         "retryPolicy": { "<retry-behavior>" }
         },
      },
      "unsubscribe": {
         "method": "<method-type>",
         "url": "<endpoint-unsubscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-type>" }
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

Yöntem *türü*> <gibi bazı değerler hem `"subscribe"` nesneler `"unsubscribe"` hem de nesneler için kullanılabilir.

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yöntem tipi*> | Dize | Abonelik isteği için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "PATCH" veya "DELETE" | 
| <*uç nokta-abone-URL*> | Dize | Abonelik isteğinin gönderilen uç nokta URL'si | 
|||| 

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yöntem tipi*> | Dize | İptal isteği için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "PATCH" veya "DELETE" | 
| <*bitiş noktası-abonelikten çıkma-URL*> | Dize | İptal isteğinin gönderilen uç nokta URL'si | 
| <*gövde içeriği*> | Dize | Abonelik veya iptal isteğinde gönderilecek tüm ileti içeriği | 
| <*kimlik doğrulama türü*> | JSON Nesnesi | İsteğin giden istekleri doğrulamak için kullandığı kimlik doğrulama modeli. Daha fazla bilgi için bkz: [Giden aramalara kimlik doğrulama ekle.](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound) |
| <*yeniden deneme davranışı*> | JSON Nesnesi | 408, 429 ve 5XX durum kodu ve herhangi bir bağlantı özel durumu olan aralıklı hatalar için yeniden deneme davranışını özelleştirer. Daha fazla bilgi için yeniden [deneme ilkelerine](../logic-apps/logic-apps-exception-handling.md#retry-policies)bakın. | 
| <*maksimum çalışır*> | Tamsayı | Varsayılan olarak, iş akışı örneklerinin tümü varsayılan [sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar aynı anda (eşzamanlı veya paralel olarak) çalışır. Yeni bir <*sayısı*> değer ayarlayarak bu sınırı değiştirmek için [bkz.](#change-trigger-concurrency) | 
| <*max-runs-kuyruk*> | Tamsayı | İş akışınız zaten özelliğine göre değiştirebileceğiniz en fazla örnek `runtimeConfiguration.concurrency.runs` sayısını çalıştırıyorsa, tüm yeni çalıştırmalar [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar bu kuyruğa konur. Varsayılan sınırı değiştirmek için [bkz.](#change-waiting-runs) | 
| <*işlem seçeneği*> | Dize | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için [İşlem seçeneklerine](#operation-options)bakın. | 
|||| 

*Çıkışlar* 

| Öğe | Tür | Açıklama |
|---------|------|-------------| 
| Üstbilgi | JSON Nesnesi | Yanıttaki üstbilgi | 
| body | JSON Nesnesi | Yanıttan vücut | 
| durum kodu | Tamsayı | Yanıttaki durum kodu | 
|||| 

*Örnek*

Bu tetikleyici belirtilen bitiş noktasına abonelik oluşturur, benzersiz bir geri arama URL'si sağlar ve yeni yayımlanan teknoloji makalelerini bekler.

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

### <a name="recurrence-trigger"></a>Yineleme tetikleyicisi  

Bu tetikleyici, belirtilen yineleme zamanlamasına göre çalışır ve düzenli olarak çalışan bir iş akışı oluşturmak için kolay bir yol sağlar.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*zaman birimi*> | Dize | Tetikleyicinin ne sıklıkta ateşlediğini açıklayan zaman birimi: "İkinci", "Dakika", "Saat", "Gün", "Hafta", "Ay" | 
| <*zaman birimi sayısı*> | Tamsayı | Tetikleyicinin frekansa göre ne sıklıkta ateşlediğini belirten bir değer, yani tetikleyici yeniden çalışana kadar bekleyecek zaman birimlerinin sayısıdır <p>Minimum ve maksimum aralıklar şunlardır: <p>- Ay: 1-16 ay </br>- Gün: 1-500 gün </br>- Saat: 1-12.000 saat </br>- Dakika: 1-72.000 dakika </br>- İkinci: 1-9,999,999 saniye<p>Örneğin, aralık 6 ise ve sıklık "Ay" ise, yineleme her 6 ayda bir olur. | 
|||| 

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*başlangıç-tarih-tarih-format-YYYY-MM-DDThh:mm:ss*> | Dize | Bu formattaki başlangıç tarihi ve saati: <p>Saat dilimi belirtirseniz YYYY-MM-DDThh:mm:ss <p>-veya- <p>Saat dilimi belirtmezseniz YYYY-MM-DDThh:mm:ssZ <p>Örneğin, 18 Eylül 2017 saat 14:00'te istiyorsanız, "2017-09-18T14:00:00" adresini belirtin ve "Pasifik Standart Saati" gibi bir saat dilimi belirtin veya saat dilimi olmadan "2017-09-18T14:00:00Z" belirtin. <p>**Not:** Bu başlangıç süresi gelecekte en fazla 49 yıl vardır ve [UTC tarih saati biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) [ISO 8601 tarih saati belirtimini](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) takip etmelidir, ancak [UTC mahsup](https://en.wikipedia.org/wiki/UTC_offset)olmadan. Bir saat dilimi belirtmezseniz, herhangi bir boşluk olmadan sonuna "Z" harfini eklemeniz gerekir. Bu "Z" eşdeğer [denizcilik zamanı](https://en.wikipedia.org/wiki/Nautical_time)anlamına gelir. <p>Basit zamanlamalar için başlangıç zamanı ilk olaydır, karmaşık zamanlamalar için tetikleyici başlangıç saatinden daha erken ateş etmez. Başlangıç tarihleri ve saatleri hakkında daha fazla bilgi için [bkz.](../connectors/connectors-native-recurrence.md) | 
| <*saat dilimi*> | Dize | Bu tetikleyici [UTC ofset](https://en.wikipedia.org/wiki/UTC_offset)kabul etmediğinden, yalnızca bir başlangıç saati belirttiğiniz zaman geçerlidir. Uygulamak istediğiniz saat dilimini belirtin. | 
| <*bir veya daha fazla saat işaretleri*> | İnteger veya sonda dizisi | "Gün" veya "Hafta" için `frequency`belirtirseniz, iş akışını çalıştırmak istediğiniz günün saatleri olarak virgülle ayrılmış 0'dan 23'e kadar bir veya daha fazla arayı belirtebilirsiniz. <p>Örneğin, 10, 12 ve 14 belirtirseniz, saat işaretleri olarak 10, 12 PM ve 14 alırsınız. | 
| <*bir veya daha fazla dakika işaretleri*> | İnteger veya sonda dizisi | "Gün" veya "Hafta" için `frequency`belirtirseniz, iş akışını çalıştırmak istediğiniz saatin dakikası olarak 0'dan 59'a virgülle ayrılmış bir veya daha fazla arayı belirtebilirsiniz. <p>Örneğin, dakika işareti olarak "30" belirtebilirsiniz ve günün saatleri için önceki örneği kullanarak, 10:30, 12:30 ve 14:30 olsun. | 
| weekDays | Dize veya dize dizisi | "Hafta" için `frequency`"Hafta" belirtirseniz, iş akışını çalıştırmak istediğinizde virgülle ayrılmış bir veya daha fazla gün belirtebilirsiniz: "Pazartesi", "Salı", "Çarşamba", "Perşembe", "Cuma", "Cumartesi" ve "Pazar" | 
| <*maksimum çalışır*> | Tamsayı | Varsayılan olarak, iş akışı örneklerinin tümü varsayılan [sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar aynı anda (eşzamanlı veya paralel olarak) çalışır. Yeni bir <*sayısı*> değer ayarlayarak bu sınırı değiştirmek için [bkz.](#change-trigger-concurrency) | 
| <*max-runs-kuyruk*> | Tamsayı | İş akışınız zaten özelliğine göre değiştirebileceğiniz en fazla örnek `runtimeConfiguration.concurrency.runs` sayısını çalıştırıyorsa, tüm yeni çalıştırmalar [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar bu kuyruğa konur. Varsayılan sınırı değiştirmek için [bkz.](#change-waiting-runs) | 
| <*işlem seçeneği*> | Dize | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için [İşlem seçeneklerine](#operation-options)bakın. | 
|||| 

*Örnek 1*

Bu temel yineleme tetikleyicisi her gün çalışır:

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

Tetikleyiciyi ateşlemek için bir başlangıç tarihi ve saati belirtebilirsiniz. Bu yineleme tetikleyicisi belirtilen tarihte başlar ve her gün yangınlar:

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

Bu yineleme tetikleyicisi 9 Eylül 2017 tarihinde saat 14:00'te başlar ve her Pazartesi 22:30, 12:30 ve Pasifik Standart Saati ile her Pazartesi saat 14:30'da yayınyapmaktadır:

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

Daha fazla bilgi ve bu tetikleyici için örnekler için [bkz.](../connectors/connectors-native-recurrence.md)

<a name="request-trigger"></a>

### <a name="request-trigger"></a>İstek tetikleyicisi

Bu tetikleyici, gelen istekleri kabul edebilecek bir uç nokta oluşturarak mantık uygulamanızı çağrılabilir hale getirir. Bu tetikleyici için, tetikleyicinin gelen istekten aldığı yükü veya girişleri açıklayan ve doğrulayan bir JSON şeması sağlayın. Şema, iş akışındaki sonraki eylemlerden tetikleyici özelliklerin başvurulmasını da kolaylaştırır.

Bu tetikleyiciyi çağırmak için, `listCallbackUrl` [İş Akışı Hizmeti REST API'sinde](https://docs.microsoft.com/rest/api/logic/workflows)açıklanan API'yi kullanmanız gerekir. Bu tetikleyiciyi HTTP bitiş noktası olarak nasıl kullanacağınızı öğrenmek için, [HTTP uç noktalarıyla Arama, tetikleme veya yuva iş akışlarına](../logic-apps/logic-apps-http-endpoint.md)bakın.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*özellik adı*> | Dize | Yükü açıklayan JSON şemasındaki bir mülkün adı | 
| <*özellik türü*> | Dize | Özelliğin türü | 
|||| 

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yöntem tipi*> | Dize | Gelen isteklerin mantık uygulamanızı aramak için kullanması gereken yöntem: "GET", "PUT", "POST", "PATCH", "DELETE" |
| <*göreli yol-kabul edilen parametre için*> | Dize | Bitiş noktanızın URL'sinin kabul edebileceği parametrenin göreli yolu | 
| <*gerekli özellikler*> | Dizi | Değer gerektiren bir veya daha fazla özellik | 
| <*maksimum çalışır*> | Tamsayı | Varsayılan olarak, iş akışı örneklerinin tümü varsayılan [sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar aynı anda (eşzamanlı veya paralel olarak) çalışır. Yeni bir <*sayısı*> değer ayarlayarak bu sınırı değiştirmek için [bkz.](#change-trigger-concurrency) | 
| <*max-runs-kuyruk*> | Tamsayı | İş akışınız zaten özelliğine göre değiştirebileceğiniz en fazla örnek `runtimeConfiguration.concurrency.runs` sayısını çalıştırıyorsa, tüm yeni çalıştırmalar [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar bu kuyruğa konur. Varsayılan sınırı değiştirmek için [bkz.](#change-waiting-runs) | 
| <*işlem seçeneği*> | Dize | `operationOptions` Özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için [İşlem seçeneklerine](#operation-options)bakın. | 
|||| 

*Örnek*

Bu tetikleyici, gelen bir isteğin tetikleyiciyi çağırmak için HTTP POST yöntemini kullanması gerektiğini belirtir ve gelen istekten girişi doğrulayan bir şema içerir:

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

## <a name="trigger-conditions"></a>Tetikleyici koşulları

Herhangi bir tetikleyici ve yalnızca tetikleyiciler için, iş akışının çalışıp çalışmayacağını belirleyen koşullar için bir veya daha fazla ifade içeren bir dizi ekleyebilirsiniz. İş akışınızdaki bir tetikleyiciye `conditions` özelliği eklemek için kod görünümü düzenleyicisinde mantık uygulamanızı açın.

Örneğin, tetikleyicinin yalnızca bir web sitesi bir iç sunucu hatasını döndürdüğünde, tetikleyicinin `conditions` özellikteki durum koduna başvurarak tetikleyicinin devreye girebileceğini belirtebilirsiniz:

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

Varsayılan olarak, bir tetikleyici yalnızca "200 Tamam" yanıtı aldıktan sonra devreye girerek devreye girer. Bir ifade tetikleyicinin durum koduna başvurursa, tetikleyicinin varsayılan davranışı değiştirilir. Bu nedenle, tetikleyicinin "200" ve "201" durum kodu gibi birden fazla durum kodu için ateş etmesini istiyorsanız, bu ifadeyi durumunuz olarak eklemeniz gerekir:

`@or(equals(triggers().code, 200),equals(triggers().code, 201))` 

<a name="split-on-debatch"></a>

## <a name="trigger-multiple-runs"></a>Birden çok çalıştırmayı tetikleme

Tetikleyiciniz mantık uygulamanızın işlemesi için bir dizi döndürürse, bazen her dizi öğesini işlemek için "her biri için" bir döngü çok uzun sürebilir. Bunun yerine, diziyi *ayırmak* için tetikleyicinizdeki **SplitOn** özelliğini kullanabilirsiniz. Debatching dizi öğelerini böler ve her dizi öğesi için çalışan yeni bir iş akışı örneği başlatır. Bu yaklaşım, örneğin, yoklama aralıkları arasında birden çok yeni öğe döndürebilecek bir bitiş noktası nı yoklamak istediğinizde yararlıdır. **SplitOn'un** tek bir mantık uygulamasında işleyebilir dizi öğelerinin maksimum sayısı için [Sınırlar ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)ya göre. 

> [!NOTE]
> Senkron yanıt deseniyle **SplitOn'u** kullanamazsınız. **SplitOn** kullanan ve yanıt eylemi içeren herhangi bir iş akışı `202 ACCEPTED` eşzamanlı olarak çalışır ve hemen bir yanıt gönderir.
>
> Tetikleyici eşzamanlılık etkinleştirildiğinde, [SplitOn sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) önemli ölçüde azalır. Madde sayısı bu sınırı aşarsa, SplitOn özelliği devre dışı bırakılır.
 
Tetikleyicinizin Swagger dosyasında dizi olan bir yük açıklanıyorsa, **SplitOn** özelliği tetikleyicinize otomatik olarak eklenir. Aksi takdirde, bu özelliği, boşaltmak istediğiniz diziye sahip yanıt yüküne ekleyin.

*Örnek*

Bu yanıtı döndüren bir API'niz olduğunu varsayalım: 
  
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

Mantık uygulamanızın yalnızca dizideki içeriğe ihtiyacı `Rows`vardır, böylece aşağıdaki gibi bir tetikleyici oluşturabilirsiniz:

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
> Komutu `SplitOn` kullanırsanız, dizinin dışındaki özellikleri alamazsınız. Bu örnekte, API'den `status` döndürülen yanıttaki özelliği alamazsınız.
> 
> `Rows` Özellik yoksa bir hata önlemek için, bu örnek `?` işleci kullanır.

İş akışı tanımınız `@triggerBody().name` artık ilk `name` `"customer-name-one"` çalıştırmadan ve `"customer-name-two"` ikinci çalıştırmadan gelen değerleri almak için kullanılabilir. Tetik çıktılarınız aşağıdaki örneklere benzer:

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

Azure Logic Apps, her biri bir eylemin benzersiz davranışını tanımlayan farklı girdileriçeren çeşitli eylem türleri sağlar. Eylemler bu üst düzey öğelere sahiptir, ancak bazıları isteğe bağlıdır:

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

| Değer | Tür | Açıklama | 
|-------|------|-------------|
| <*eylem adı*> | Dize | Eylemin adı | 
| <*eylem türü*> | Dize | Eylem türü, örneğin, "Http" veya "ApiConnection"| 
| <*giriş adı*> | Dize | Eylemin davranışını tanımlayan bir girdinin adı | 
| <*giriş değeri*> | Çeşitli | Bir dize, tamsayı, JSON nesnesi ve benzeri olabilir giriş değeri, | 
| <*önceki tetikleyici veya eylem durumu*> | JSON Nesnesi | Bu geçerli eylem çalıştırılamadan hemen önce çalışması gereken tetikleyici veya eylemin adı ve sonucu durumu | 
|||| 

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------|
| <*yeniden deneme davranışı*> | JSON Nesnesi | 408, 429 ve 5XX durum kodu ve herhangi bir bağlantı özel durumu olan aralıklı hatalar için yeniden deneme davranışını özelleştirer. Daha fazla bilgi için bkz. | 
| <*çalışma zamanı-config-seçenekleri*> | JSON Nesnesi | Bazı eylemler için, özellikleri ayarlayarak `runtimeConfiguration` çalışma zamanında eylemin davranışını değiştirebilirsiniz. Daha fazla bilgi için [Runtime yapılandırma ayarlarına](#runtime-config-options)bakın. | 
| <*işlem seçeneği*> | Dize | Bazı eylemler `operationOptions` için, özelliği ayarlayarak varsayılan davranışı değiştirebilirsiniz. Daha fazla bilgi için [İşlem seçeneklerine](#operation-options)bakın. | 
|||| 

## <a name="action-types-list"></a>Eylem türleri listesi

Sık kullanılan bazı eylem türleri şunlardır: 

* Bu örnekler ve daha fazlası gibi [yerleşik eylem türleri:](#built-in-actions) 

  * [**HTTP**](#http-action) veya HTTPS üzerinden uç noktaları aramak için HTTP

  * İsteklere yanıt vermek için [**yanıt**](#response-action)

  * JavaScript kod parçacıklarını çalıştırmak için [**JavaScript Kodunu çalıştırın**](#run-javascript-code)

  * Azure Fonksiyonlarını arama [**işlevi**](#function-action)

  * [**Birleştirme,**](#join-action) [**Oluşturma,**](#compose-action) [**Tablo**](#table-action)oluşturma , [**Seç**](#select-action)ve çeşitli girişlerden veri oluşturan veya dönüştüren diğerleri gibi veri işlem eylemleri

  * Başka bir mantık uygulaması iş akışını aramak için [**iş**](#workflow-action) akışı

* Microsoft tarafından yönetilen çeşitli bağlayıcıları ve API'leri (örneğin Azure Hizmet Veri Yolu, Office 365 Outlook, Power BI, Azure Blob Depolama, OneDrive, GitHub ve daha fazlası) arayan [**ApiConnection**](#apiconnection-action) ve [**ApiConnectionWebHook**](#apiconnectionwebhook-action) gibi [yönetilen API eylem türleri](#managed-api-actions)

* [**Diğer**](#if-action)eylemleri içeren ve iş akışı yürütmesini düzenlemenize yardımcı olan If , [**Foreach**](#foreach-action), [**Switch**](#switch-action), [**Scope**](#scope-action)ve [**Until**](#until-action)gibi iş akışı [eylem türlerini denetleme](#control-workflow-actions)

<a name="built-in-actions"></a>

### <a name="built-in-actions"></a>Yerleşik eylemler

| Eylem türü | Açıklama | 
|-------------|-------------| 
| [**Oluşturmak**](#compose-action) | Çeşitli türleri olabilir girişlerden tek bir çıktı oluşturur. | 
| [**JavaScript Kodunu Çalıştır**](#run-javascript-code) | Belirli ölçütlere uyan JavaScript kod parçacıklarını çalıştırın. Kod gereksinimleri ve daha fazla bilgi için [bkz.](../logic-apps/logic-apps-add-run-inline-code.md) |
| [**İşlev**](#function-action) | Azure İşi çağırır. | 
| [**HTTP**](#http-action) | HTTP bitiş noktasını çağırır. | 
| [**Katılın**](#join-action) | Bir dizideki tüm öğelerden bir dize oluşturur ve bu öğeleri belirtilen bir sınırlayıcı karakterle ayırır. | 
| [**Ayrışdırık JSON**](#parse-json-action) | JSON içeriğindeki özelliklerden kullanıcı dostu jetonlar oluşturur. Daha sonra, mantık uygulamanıza belirteçleri ekleyerek bu özelliklere başvuruda bulunabilirsiniz. | 
| [**Sorgu**](#query-action) | Bir koşula veya filtreye dayalı olarak başka bir dizideki öğelerden bir dizi oluşturur. | 
| [**Yanıt**](#response-action) | Gelen bir çağrı veya isteğe yanıt oluşturur. | 
| [**Seçin**](#select-action) | Belirtilen haritayı temel alarak öğeleri başka bir diziden dönüştürerek JSON nesneleri ile bir dizi oluşturur. | 
| [**Tablo**](#table-action) | Bir diziden csv veya HTML tablosu oluşturur. | 
| [**Sonlandır**](#terminate-action) | Etkin olarak çalışan bir iş akışını durdurur. | 
| [**Wait**](#wait-action) | İş akışınızı belirli bir süre veya belirtilen tarih ve saate kadar duraklatabilir. | 
| [**Iş akışı**](#workflow-action) | İş akışını başka bir iş akışının içine yerleştirir. | 
||| 

<a name="managed-api-actions"></a>

### <a name="managed-api-actions"></a>Yönetilen API eylemleri

| Eylem türü | Açıklama | 
|-------------|-------------|  
| [**ApiConnection**](#apiconnection-action) | Microsoft tarafından yönetilen bir [API](../connectors/apis-list.md)kullanarak bir HTTP bitiş noktası çağırır. | 
| [**ApiConnectionWebhook**](#apiconnectionwebhook-action) | HTTP Webhook gibi çalışır ancak Microsoft tarafından yönetilen bir [API](../connectors/apis-list.md)kullanır. | 
||| 

<a name="control-workflow-actions"></a>

### <a name="control-workflow-actions"></a>İş akışı eylemlerini denetleme

Bu eylemler, iş akışı yürütmeyi denetlemenize ve diğer eylemleri eklemenize yardımcı olur. Bir denetim iş akışı eylemi dışından, iş akışı eylemini kontrol eden eylemlere doğrudan başvuruyapabilirsiniz. Örneğin, bir kapsam `Http` içinde bir eyleminiz varsa, `@body('Http')` ifadeyi iş akışının herhangi bir yerinden başvurulayabilirsiniz. Ancak, bir denetim iş akışı eylemi içinde var olan eylemler yalnızca aynı denetim iş akışı yapısında olan diğer eylemlerden sonra "çalıştırılabilir".

| Eylem türü | Açıklama | 
|-------------|-------------| 
| [**Foreach**](#foreach-action) | Dizideki her öğe için aynı eylemleri bir döngüde çalıştırın. | 
| [**Eğer**](#if-action) | Belirtilen koşulun doğru veya yanlış olup olmadığına bağlı olarak eylemleri çalıştırın. | 
| [**Kapsam**](#scope-action) | Bir dizi eylemden grup durumuna göre eylemleri çalıştırın. | 
| [**Anahtarı**](#switch-action) | İfadelerden, nesnelerden veya belirteçlerden gelen değerlerin her servis talebi tarafından belirtilen değerlerle eşleştirdiği durumlara yönelik olarak düzenlenen eylemleri çalıştırın. | 
| [**Kadar**](#until-action) | Belirtilen koşul doğru olana kadar eylemleri bir döngüde çalıştırın. | 
|||  

## <a name="actions---detailed-reference"></a>Eylemler - Ayrıntılı başvuru

<a name="apiconnection-action"></a>

### <a name="apiconnection-action"></a>APIConnection eylemi

Bu [eylem, Microsoft tarafından yönetilen](../connectors/apis-list.md) bir API'ye bir HTTP isteği gönderir ve API ve parametreler hakkında bilgi nin yanı sıra geçerli bir bağlantıya başvuru gerektirir. 

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*eylem adı*> | Dize | Bağlayıcı tarafından sağlanan eylemin adı | 
| <*api adı*> | Dize | Bağlantı için kullanılan Microsoft tarafından yönetilen API'nin adı | 
| <*yöntem tipi*> | Dize | API'yi aramak için HTTP yöntemi: "GET", "PUT", "POST", "PATCH" veya "DELETE" | 
| <*api-operasyon*> | Dize | Aramak için API işlemi | 
|||| 

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*diğer eyleme özgü-giriş-özellikleri*> | JSON Nesnesi | Bu özel eylem için geçerli olan diğer giriş özellikleri | 
| <*yeniden deneme davranışı*> | JSON Nesnesi | 408, 429 ve 5XX durum kodu ve herhangi bir bağlantı özel durumu olan aralıklı hatalar için yeniden deneme davranışını özelleştirer. Daha fazla bilgi için yeniden [deneme ilkelerine](../logic-apps/logic-apps-exception-handling.md#retry-policies)bakın. | 
| <*sorgu parametreleri*> | JSON Nesnesi | API çağrısına dahil edilecek tüm sorgu parametreleri. <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesne çağrıya ekler. `?api-version=2018-01-01` | 
| <*diğer eyleme özgü özellikler*> | JSON Nesnesi | Bu özel eylem için geçerli olan diğer özellikler | 
|||| 

*Örnek*

Bu tanım, Microsoft tarafından yönetilen bir API olan Office 365 Outlook bağlayıcısı için **e-posta gönder** eylemini açıklar: 

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

### <a name="apiconnectionwebhook-action"></a>APIConnectionWebhook eylemi

Bu eylem, Microsoft tarafından yönetilen bir [API](../connectors/apis-list.md)kullanarak bir bitiş noktasına HTTP üzerinden bir abonelik isteği gönderir, bitiş noktasının yanıt gönderebileceği bir *geri arama URL'si* sağlar ve bitiş noktasının yanıt vermesi için bitiş noktasını bekler. Daha fazla bilgi için [Endpoint aboneliklerine](#subscribe-unsubscribe)bakın.

```json
"<action-name>": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "subscribe": {
         "method": "<method-type>",
         "uri": "<api-subscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-type>" },
         "retryPolicy": "<retry-behavior>",
         "queries": { "<query-parameters>" },
         "<other-action-specific-input-properties>"
      },
      "unsubscribe": {
         "method": "<method-type>",
         "uri": "<api-unsubscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-type>" },
         "<other-action-specific-properties>"
      },
   },
   "runAfter": {}
}
```

Yöntem *türü*> <gibi bazı değerler hem `"subscribe"` nesneler `"unsubscribe"` hem de nesneler için kullanılabilir.

*Gerekli*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*eylem adı*> | Dize | Bağlayıcı tarafından sağlanan eylemin adı | 
| <*yöntem tipi*> | Dize | Bir uç noktadan abone olmak veya aboneliğini sağlamak için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "PATCH" veya "DELETE" | 
| <*api-subscribe-URL*> | Dize | URI API abone olmak için kullanmak | 
|||| 

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*api-unsubscribe-URL*> | Dize | URI API aboneliği için kullanmak | 
| <*üstbilgi içeriği*> | JSON Nesnesi | İstek göndermek için herhangi bir üstbilgi <p>Örneğin, dili ayarlamak ve bir istek üzerine yazmak için: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*gövde içeriği*> | JSON Nesnesi | İstekte gönderilecek herhangi bir ileti içeriği | 
| <*kimlik doğrulama türü*> | JSON Nesnesi | İsteğin giden istekleri doğrulamak için kullandığı kimlik doğrulama modeli. Daha fazla bilgi için bkz: [Giden aramalara kimlik doğrulama ekle.](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound) |
| <*yeniden deneme davranışı*> | JSON Nesnesi | 408, 429 ve 5XX durum kodu ve herhangi bir bağlantı özel durumu olan aralıklı hatalar için yeniden deneme davranışını özelleştirer. Daha fazla bilgi için yeniden [deneme ilkelerine](../logic-apps/logic-apps-exception-handling.md#retry-policies)bakın. | 
| <*sorgu parametreleri*> | JSON Nesnesi | API çağrısına dahil edilecek tüm sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesne çağrıya ekler. `?api-version=2018-01-01` | 
| <*diğer eyleme özgü-giriş-özellikleri*> | JSON Nesnesi | Bu özel eylem için geçerli olan diğer giriş özellikleri | 
| <*diğer eyleme özgü özellikler*> | JSON Nesnesi | Bu özel eylem için geçerli olan diğer özellikler | 
|||| 

**Ayrıca, apiConnectionWebhook** eyleminin sınırlarını [HTTP asynchronous sınırladığı](#asynchronous-limits)şekilde belirtebilirsiniz.

<a name="compose-action"></a>

### <a name="compose-action"></a>Eylem oluşturma

Bu eylem, ifadeler de dahil olmak üzere birden çok girişten tek bir çıktı oluşturur. Hem çıktı hem de giriş, Diziler, JSON nesneleri, XML ve ikili gibi Azure Logic Apps'ın doğal olarak desteklediği herhangi bir türe sahip olabilir. Daha sonra eylemin çıktısını diğer eylemlerde kullanabilirsiniz. 

```json
"Compose": {
   "type": "Compose",
   "inputs": "<inputs-to-compose>",
   "runAfter": {}
},
```

*Gerekli* 

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*oluşturmak için girişler*> | Herhangi biri | Tek bir çıktı oluşturmak için girişler | 
|||| 

*Örnek 1*

<!-- markdownlint-disable MD038 -->
Bu eylem tanımı, bir iz bırakan `1234`alan ve değer `abcdefg ` ile birleştirir:
<!-- markdownlint-enable MD038 -->

```json
"Compose": {
   "type": "Compose",
   "inputs": "abcdefg 1234",
   "runAfter": {}
},
```

Bu eylemin oluşturduğu çıktı aşağıda veda eder:

`abcdefg 1234`

*Örnek 2*

Bu eylem tanımı içeren `abcdefg` bir dize değişkeni ve `1234`içeren bir tamsayı değişkeni birleştirir:

```json
"Compose": {
   "type": "Compose",
   "inputs": "@{variables('myString')}@{variables('myInteger')}",
   "runAfter": {}
},
```

Bu eylemin oluşturduğu çıktı aşağıda veda eder:

`"abcdefg1234"`

<a name="run-javascript-code"></a>

### <a name="execute-javascript-code-action"></a>JavaScript Kodu eylemini gerçekleştirin

Bu eylem bir JavaScript kod snippet çalışır `Result` ve daha sonraki eylemlerin başvurulabileceği bir belirteç yoluyla sonuçları döndürür.

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

| Değer | Tür | Açıklama |
|-------|------|-------------|
| <*JavaScript-kod-snippet*> | Değişir | Çalıştırmak istediğiniz JavaScript kodu. Kod gereksinimleri ve daha fazla bilgi için [bkz.](../logic-apps/logic-apps-add-run-inline-code.md) <p>Öznitelik `code` olarak, kod parçacığınız salt `workflowContext` okunur nesnesini giriş olarak kullanabilir. Bu nesne, kodunuzu iş akışınızdaki tetikleyicive önceki eylemlerin sonuçlarına erişim sağlayan alt özelliklere sahiptir. `workflowContext` Nesne hakkında daha fazla bilgi [için, kodunuzda Başvuru tetikleyicisi ve eylem sonuçları konusuna](../logic-apps/logic-apps-add-run-inline-code.md#workflowcontext)bakın. |
||||

*Bazı durumlarda gerekli*

Öznitelik, `explicitDependencies` tetikleyiciden, önceki eylemlerden veya her ikisinin de kod parçacığınızın bağımlılıkları olarak sonuçları açıkça eklemek istediğinizi belirtir. Bu bağımlılıkları ekleme hakkında daha fazla bilgi için [satır altı kodu için parametre ekle'ye](../logic-apps/logic-apps-add-run-inline-code.md#add-parameters)bakın. 

Öznitelik `includeTrigger` için, belirtebilir `true` `false` veya değerleri.

| Değer | Tür | Açıklama |
|-------|------|-------------|
| <*önceki eylemler*> | Dize dizisi | Belirtilen eylem adlarınızın yer verdiği bir dizi. Eylem adlarının alt çizer (_), boşlukların (" ") kullanıldığı iş akışı tanımınızda görünen eylem adlarını kullanın. |
||||

*Örnek 1*

Bu eylem, mantık uygulamanızın adını alan kodu çalıştırır ve \<sonuç olarak "Mantık-uygulama adından> merhaba dünya" metnini döndürür. Bu örnekte, kod yalnızca `workflowContext.workflow.name` `workflowContext` okunur nesneüzerinden özelliğierişerek iş akışının adına başvurur. Nesneyi `workflowContext` kullanma hakkında daha fazla bilgi [için, kodunuzda Başvuru tetikleyicisi ve eylem sonuçları konusuna](../logic-apps/logic-apps-add-run-inline-code.md#workflowcontext)bakın.

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

Bu eylem, office 365 Outlook hesabına yeni bir e-posta geldiğinde tetikleyen bir mantık uygulamasında kod çalıştırır. Mantık uygulaması ayrıca, alınan e-postadaki içeriği onay isteğiyle birlikte ileten bir send onayı e-posta sı kullanır.

Kod, tetikleyicinin `Body` özelliğinden e-posta adreslerini ayıklar ve onay `SelectedOption` eylemindeki özellik değeriyle birlikte adresleri döndürür. Eylem açıkça öznitelik bir bağımlılık `explicitDependencies`  >  `actions` olarak onay e-posta eylem gönder içerir.

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

Bu eylem, daha önce oluşturulmuş bir [Azure işlevini](../azure-functions/functions-create-first-azure-function.md)çağırır.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------|  
| <*Azure fonksiyonu-Kimliği*> | Dize | Aramak istediğiniz Azure işlevi için kaynak kimliği. Bu değerin biçimi aşağıdavelvettir:<p>"/subscriptions/<*Azure-subscription-ID*>/resourceGroups/<*Azure kaynak grubu*>/sağlayıcıları/Microsoft.Web/siteler/<*Azure-işlev-app-name*>/fonksiyonlar/<*Azure işlev adı*>" | 
| <*yöntem tipi*> | Dize | İşlevçağırmak için kullanılacak HTTP yöntemi: "GET", "PUT", "POST", "PATCH" veya "DELETE" <p>Belirtilmemişse, varsayılan "POST" yöntemidir. | 
||||

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------|  
| <*üstbilgi içeriği*> | JSON Nesnesi | Çağrıyla birlikte gönderilecek üstbilgi <p>Örneğin, dili ayarlamak ve bir istek üzerine yazmak için: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*gövde içeriği*> | JSON Nesnesi | İstekte gönderilecek herhangi bir ileti içeriği | 
| <*sorgu parametreleri*> | JSON Nesnesi | API çağrısına dahil edilecek tüm sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesne çağrıya ekler. `?api-version=2018-01-01` | 
| <*diğer eyleme özgü-giriş-özellikleri*> | JSON Nesnesi | Bu özel eylem için geçerli olan diğer giriş özellikleri | 
| <*diğer eyleme özgü özellikler*> | JSON Nesnesi | Bu özel eylem için geçerli olan diğer özellikler | 
||||

Mantık uygulamanızı kaydettiğinizde, Logic Apps motoru başvurulan işlev üzerinde şu denetimleri gerçekleştirir:

* İş akışınız işleve erişebilmeli.

* İş akışınız yalnızca standart bir HTTP tetikleyicisi veya genel JSON webhook tetikleyicisi kullanabilir. 

  Logic Apps motoru, çalışma zamanında kullanılan tetikleyicinin URL'sini alır ve önbelleğe alır. Ancak, önbelleğe alınmış URL'yi herhangi bir işlem geçersiz kılırsa, **Işlev** eylemi çalışma zamanında başarısız olur. Bu sorunu gidermek için, mantık uygulamasının tetikleyici URL'yi yeniden alabilmesi ve önbelleğe alabilmesi için mantık uygulamasını yeniden kaydedin.

* İşlev tanımlı bir rota olamaz.

* Yalnızca "işlev" ve "anonim" yetkilendirme düzeylerine izin verilir. 

*Örnek*

Bu eylem tanımı, daha önce oluşturulan "GetProductID" işlevini çağırır:

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

### <a name="http-action"></a>HTTP eylem

Bu eylem, belirtilen HTTP veya HTTPS bitiş noktasına bir istek gönderir ve iş akışının çalışıp çalışmadığını belirlemek için yanıtı denetler.

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "<method-type>",
      "uri": "<HTTP-or-HTTPS-endpoint-URL>",
      "headers": { "<header-content>" },
      "queries": { "<query-parameters>" },
      "body": "<body-content>",
      "authentication": { "<authentication-type-and-property-values>" },
      "retryPolicy": {
         "type": "<retry-behavior>"
      },
   },
   "runAfter": {}
}
```

*Gerekli*

| Özellik | Değer | Tür | Açıklama |
|----------|-------|------|-------------|
| `method` | <*yöntem tipi*> | Dize | Giden isteği göndermek için kullanılacak yöntem: "GET", "PUT", "POST", "PATCH" veya "DELETE" |
| `uri` | <*HTTP-veya-HTTPS-endpoint-URL*> | Dize | Giden isteği göndermek istediğiniz HTTP veya HTTPS bitiş noktası URL'si. Maksimum dize boyutu: 2 KB <p>Bir Azure hizmeti veya kaynağı için bu URI sözdizimi kaynak kimliğini ve erişmek istediğiniz kaynağa giden yolu içerir. |
|||||

*Isteğe bağlı*

| Özellik | Değer | Tür | Açıklama |
|----------|-------|------|-------------|
| `headers` | <*üstbilgi içeriği*> | JSON Nesnesi | İsteğe eklemeniz gereken üstbilgi <p>Örneğin, dili ve türü ayarlamak için: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| `queries` | <*sorgu parametreleri*> | JSON Nesnesi | İstekte kullanmanız gereken sorgu parametreleri <p>Örneğin, `"queries": { "api-version": "2018-01-01" }` nesne çağrıya ekler. `?api-version=2018-01-01` |
| `body` | <*gövde içeriği*> | JSON Nesnesi | İstekle birlikte yük olarak gönderilecek ileti içeriği |
| `authentication` | <*kimlik doğrulama-türü ve özellik değerleri*> | JSON Nesnesi | İsteğin giden istekleri doğrulamak için kullandığı kimlik doğrulama modeli. Daha fazla bilgi için bkz: [Giden aramalara kimlik doğrulama ekle.](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound) Zamanlayıcının `authority` ötesinde, özellik desteklenir. Belirtilmediğinde, varsayılan değer `https://management.azure.com/`, ancak farklı bir değer kullanabilirsiniz. |
| `retryPolicy` > `type` | <*yeniden deneme davranışı*> | JSON Nesnesi | 408, 429 ve 5XX durum kodu ve herhangi bir bağlantı özel durumu olan aralıklı hatalar için yeniden deneme davranışını özelleştirer. Daha fazla bilgi için yeniden [deneme ilkelerine](../logic-apps/logic-apps-exception-handling.md#retry-policies)bakın. |
| <*diğer eyleme özgü-giriş-özellikleri*> | <*giriş özelliği*> | JSON Nesnesi | Bu özel eylem için geçerli olan diğer giriş özellikleri |
| <*diğer eyleme özgü özellikler*> | <*özellik değeri*> | JSON Nesnesi | Bu özel eylem için geçerli olan diğer özellikler |
|||||

*Örnek*

Bu eylem tanımı, belirtilen bitiş noktasına bir istek göndererek en son haberleri alır:

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

### <a name="join-action"></a>Eyleme katılma

Bu eylem, bir dizideki tüm öğelerden bir dize oluşturur ve bu öğeleri belirtilen sınır dışı layıcı karakterle ayırır. 

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Dizi*> | Dizi | Kaynak öğeleri sağlayan dizi veya ifade. Bir ifade belirtirseniz, bu ifadeyi çift tırnak la birlikte içine alabilirsiniz. | 
| <*Sınırlayıcı*> | Tek karakterli dize | Dizedeki her öğeyi ayıran karakter | 
|||| 

*Örnek*

Bu tamsayı dizilimi içeren daha önce oluşturulmuş bir "myIntegerArray" değişkeniniz olduğunu varsayalım: 

`[1,2,3,4]` 

Bu eylem tanımı, bir ifadedeki `variables()` işlevi kullanarak değişkenden değerleri alır ve virgülle ayrılan bu değerlerle bu dizeyi oluşturur:`"1,2,3,4"`

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

### <a name="parse-json-action"></a>Parse JSON eylem

Bu eylem, JSON içeriğindeki özelliklerden kullanıcı dostu alanlar veya *belirteçler* oluşturur. Daha sonra bunun yerine belirteçleri kullanarak mantık uygulaması bu özelliklere erişebilirsiniz. Örneğin, Azure Hizmet Veri Tos'u ve Azure Cosmos DB gibi hizmetlerden JSON çıktısını kullanmak istediğinizde, bu eylemi mantık uygulamanıza ekleyebilir, böylece söz konusu çıktıdaki verilere daha kolay başvuruda bulunabilirsiniz.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*JSON kaynak*> | JSON Nesnesi | Ayrışdırmak istediğiniz JSON içeriği | 
| <*JSON-şema*> | JSON Nesnesi | Eylemin kaynak JSON içeriğini ayrışdırma için kullandığı JSON içeriğinin altında yatan sonucu açıklayan JSON şeması. <p>**İpucu**: Logic Apps Designer'da, eylemin şema oluşturabilmesi için şema sağlayabilir veya örnek bir yük sağlayabilirsiniz. | 
|||| 

*Örnek*

Bu eylem tanımı, iş akışınızda ancak **Yalnızca Ayrı Ayrı JSON** eylemini izleyen eylemlerde kullanabileceğiniz bu belirteçleri oluşturur:

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

Bu örnekte, "içerik" özelliği, eylemin ayrıştırması için JSON içeriğini belirtir. Bu JSON içeriğini şema oluşturmak için örnek yük olarak da sağlayabilirsiniz.

```json
"content": {
   "Member": { 
      "FirstName": "Sophie",
      "LastName": "Owen",
      "Email": "Sophie.Owen@contoso.com"
   }
},
```

"Şema" özelliği JSON içeriğini tanımlamak için kullanılan JSON şema belirtir:

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

Bu eylem, belirli bir koşul veya filtreyi temel alan başka bir dizideki öğelerden bir dizi oluşturur.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Dizi*> | Dizi | Kaynak öğeleri sağlayan dizi veya ifade. Bir ifade belirtirseniz, bu ifadeyi çift tırnak la birlikte içine alabilirsiniz. |
| <*koşul veya filtre*> | Dize | Kaynak dizideki öğeleri filtreleme için kullanılan koşul <p>**Not**: Hiçbir değer koşulu karşılamıyorsa, eylem boş bir dizi oluşturur. |
|||| 

*Örnek*

Bu eylem tanımı, belirtilen değerden daha büyük değerlere sahip bir dizi oluşturur, iki:

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

Bu eylem, bir HTTP isteğine yanıt için yük oluşturur. 

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yanıt durumu kodu*> | Tamsayı | Gelen isteğe gönderilen HTTP durum kodu. Varsayılan kod "200 Tamam"dır, ancak kod 2xx, 4xx veya 5xx ile başlayan, ancak 3xxx ile olmayan geçerli bir durum kodu olabilir. | 
|||| 

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*yanıt başlıkları*> | JSON Nesnesi | Yanıta dahil ediletilen bir veya daha fazla üstbilgi | 
| <*yanıt gövdesi*> | Çeşitli | Bir dize, JSON nesnesi ve hatta önceki bir eylemden ikili içerik olabilir yanıt gövdesi, | 
|||| 

*Örnek*

Bu eylem tanımı, belirtilen durum kodu, ileti gövdesi ve ileti üstbilgileriyle bir HTTP isteğine yanıt oluşturur:

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

*Kısıtlamalar*

Diğer eylemlerin aksine, **Yanıt** eyleminin özel kısıtlamaları vardır: 

* İş akışınız **Yanıt** eylemini yalnızca iş akışı bir HTTP isteği tetikleyicisiyle başladığında kullanabilir, bu da iş akışınızın bir HTTP isteği tarafından tetiklemesi gerektiği anlamına gelir.

* İş akışınız **Yanıt** eylemini **Foreach** döngüleri, Sıralı döngüler ve paralel dallar dahil *olmak* üzere **döngülere kadar** dışında herhangi bir yerde kullanabilir. 

* Özgün HTTP isteği, iş akışınızın yanıtını yalnızca **Yanıt** eyleminin gerektirdiği tüm eylemler [HTTP isteği zaman önceliği sınırı](../logic-apps/logic-apps-limits-and-config.md#request-limits)içinde tamamlandığında alır.

  Ancak, iş akışınız başka bir mantık uygulamasını iç içe geçen iş akışı olarak adlandırıyorsa, iç içe geçen iş akışı ne kadar zaman geçerse geçsin, ana iş akışı bitene kadar üst iş akışı bekler.

* İş akışınız **Yanıt** eylemini ve eşzamanlı yanıt deseni kullandığında, bu komut birden çok çalıştırma oluşturduğundan, iş akışı tetikleyici tanımındaki **splitOn** komutunu da kullanamaz. PUT yöntemi kullanıldığında bu servis talebini denetleyin ve doğruysa bir "kötü istek" yanıtı döndürün.

  Aksi takdirde, iş akışınız **splitOn** komutunu ve **Yanıt** eylemini kullanıyorsa, iş akışı eşsenkronize çalışır ve hemen "202 KABUL EDİlMİş" yanıtını döndürür.

* İş akışınız yürütülmesi **Yanıt** eylemine ulaştığında, ancak gelen istek zaten bir yanıt aldığında, **Yanıt** eylemi çakışma nedeniyle "Başarısız" olarak işaretlenir. Ve sonuç olarak, mantık uygulaması çalıştırmak da "Başarısız" durumu ile işaretlenir.

<a name="select-action"></a>

### <a name="select-action"></a>Eylem seçin

Bu eylem, belirtilen haritayı temel alan öğeleri başka bir diziden dönüştürerek JSON nesneleri ile bir dizi oluşturur. Çıktı dizisi ve kaynak dizisi her zaman aynı sayıda öğeye sahiptir. Çıktı dizisindeki nesnelerin sayısını değiştiremeseniz de, bu nesneler arasında özellikleri ve değerlerini ekleyebilir veya kaldırabilirsiniz. Özellik, `select` kaynak dizideki öğeleri dönüştürmek için eşlemi tanımlayan en az bir anahtar değer çiftini belirtir. Anahtar değeri çifti, bir özelliği ve değerini çıktı dizisindeki tüm nesneler arasında temsil eder.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Dizi*> | Dizi | Kaynak öğeleri sağlayan dizi veya ifade. Bir ifadeyi çift tırnak la kapattığından emin olun. <p>**Not**: Kaynak dizi boşsa, eylem boş bir dizi oluşturur. | 
| <*anahtar adı*> | Dize | <*ifadeden* kaynaklanan sonuca atanan özellik adı> <p>Çıktı dizisindeki tüm nesnelere yeni bir özellik eklemek için, bu özellik için <*anahtar adı*> ve özellik değeri için> <bir *ifade* sağlayın. <p>Bir özelliği dizideki tüm nesnelerden kaldırmak için, bu özellik için <*anahtar adı*> atlayın. | 
| <*Ifa -de*> | Dize | Kaynak dizideki öğeyi dönüştüren ve sonucu anahtar *adına* atayan ifade <> | 
|||| 

**Select** eylemi çıktı olarak bir dizi oluşturur, bu nedenle bu çıktıyı kullanmak isteyen herhangi bir eylem bir diziyi kabul etmeli veya diziyi tüketici eyleminin kabul ettiği türe dönüştürmeniz gerekir. Örneğin, çıktı dizisini bir dize dönüştürmek için, bu diziyi **Oluşturma** eylemine geçirebilir ve sonra diğer eylemlerinizdeki **Oluşturma** eyleminden çıktıya başvurulayabilirsiniz.

*Örnek*

Bu eylem tanımı, bir tamsayı dizisinden bir JSON nesne dizisi oluşturur. Eylem kaynak dizi aracılığıyla yinelenir, ifadeyi `@item()` kullanarak her bir karşıcı değeri alır ve`number`her JSON nesnesindeki " " özelliğine her değeri atar:

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

Bu eylemin oluşturduğu dizi aşağıda veda edin:

`[ { "number": 1 }, { "number": 2 }, { "number": 3 } ]`

Bu dizi çıktısını diğer eylemlerde kullanmak için, bu çıktıyı bir **Oluştur** eylemine geçirin:

```json
"Compose": {
   "type": "Compose",
   "inputs": "@body('Select')",
   "runAfter": {
      "Select": [ "Succeeded" ]
   }
},
```

Daha sonra diğer eylemlerinizde **Compose** (örneğin, **Office 365 Outlook - E-posta** eylemi gönder:

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

Bu eylem, bir diziden bir CSV veya HTML tablosu oluşturur. JSON nesnelerine sahip diziler için bu eylem, nesnelerin özellik adlarından sütun üstbilgisini otomatik olarak oluşturur. Diğer veri türlerine sahip diziler için sütun üstbilgilerini ve değerlerini belirtmeniz gerekir. Örneğin, bu dizi, bu eylemin sütun üstbilgi adları için kullanabileceği "ID" ve "Product_Name" özelliklerini içerir:

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| \<CSV *veya* HTML>| Dize | Oluşturmak istediğiniz tablonun biçimi | 
| <*Dizi*> | Dizi | Tablonun kaynak öğelerini sağlayan dizi veya ifade <p>**Not**: Kaynak dizi boşsa, eylem boş bir tablo oluşturur. | 
|||| 

*Isteğe bağlı*

Sütun üstbilgileri ve değerlerini belirtmek veya `columns` özelleştirmek için diziyi kullanın. Çiftler `header-value` aynı üstbilgi adına sahipse, değerleri bu üstbilgi adı altında aynı sütunda görünür. Aksi takdirde, her benzersiz üstbilgi benzersiz bir sütun tanımlar.

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*sütun adı*> | Dize | Sütunun üstbilgi adı | 
| <*sütun değeri*> | Herhangi biri | Bu sütundaki değer | 
|||| 

*Örnek 1*

Şu anda bu diziyi içeren daha önce oluşturulmuş bir "myItemArray" değişkeniniz olduğunu varsayalım:

`[ {"ID": 0, "Product_Name": "Apples"}, {"ID": 1, "Product_Name": "Oranges"} ]`

Bu eylem tanımı "myItemArray" değişkeninden bir CSV tablosu oluşturur. `from` Özellik tarafından kullanılan ifade `variables()` işlevi kullanarak "myItemArray" gelen dizi alır:

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

Bu eylemin oluşturduğu CSV tablosu aşağıda veda edebilirsiniz: 

```
ID,Product_Name 
0,Apples 
1,Oranges 
```

*Örnek 2*

Bu eylem tanımı "myItemArray" değişkeninden bir HTML tablosu oluşturur. `from` Özellik tarafından kullanılan ifade `variables()` işlevi kullanarak "myItemArray" gelen dizi alır:

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

Bu eylemin oluşturduğu HTML tablosu aşağıda veda edin: 

<table><thead><tr><th>Kimlik</th><th>Product_Name</th></tr></thead><tbody><tr><td>0</td><td>Elma</td></tr><tr><td>1</td><td>Portakal</td></tr></tbody></table>

*Örnek 3*

Bu eylem tanımı "myItemArray" değişkeninden bir HTML tablosu oluşturur. Ancak, bu örnek varsayılan sütun üstbilgi adlarını "Stock_ID" ve "Açıklama" ile geçersiz kılar ve "Açıklama" sütunundaki değerlere "Organik" sözcüğü ekler.

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

Bu eylemin oluşturduğu HTML tablosu aşağıda veda edin: 

<table><thead><tr><th>Stock_ID</th><th>Açıklama</th></tr></thead><tbody><tr><td>0</td><td>Organik Elmalar</td></tr><tr><td>1</td><td>Organik Portakal</td></tr></tbody></table>

<a name="terminate-action"></a>

### <a name="terminate-action"></a>Eylemi sonlandırma

Bu eylem, iş akışı örneği için çalıştırmayı durdurur, devam eden eylemleri iptal eder, kalan eylemleri atlar ve belirtilen durumu döndürür. Örneğin, mantık uygulamanızın bir hata durumundan tamamen çıkması gerektiğinde **Sonlandırma** eylemini kullanabilirsiniz. Bu eylem zaten tamamlanmış eylemleri etkilemez ve sıralı döngüler de dahil olmak üzere **Foreach** ve **Until** döngüleri içinde görüntüleyemez.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Durum*> | Dize | Çalıştırma için döndürülecek durum: "Başarısız", "İptal edildi", "Başarılı" |
|||| 

*Isteğe bağlı*

"runStatus" nesnesinin özellikleri yalnızca "runStatus" özelliği "Başarısız" durumuna ayarlandığında uygulanır.

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*hata kodu-veya adı*> | Dize | Hatanın kodu veya adı |
| <*hata iletisi*> | Dize | Hatayı açıklayan ileti veya metin ve uygulama kullanıcısının alabileceği eylemler | 
|||| 

*Örnek*

Bu eylem tanımı bir iş akışı çalışmasını durdurur, çalışma durumunu "Başarısız" olarak ayarlar ve durumu, hata kodunu ve hata iletisini döndürür:

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

Bu eylem, iş akışı yürütmesini belirtilen aralık için veya belirtilen süreye kadar duraklatır, ancak her ikisi birden duraklar.

*Belirtilen aralık*

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

*Belirtilen süre*

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*birim sayısı*> | Tamsayı | **Gecikme** eylemi için, bekleyecek birim sayısı | 
| <*Aralığı*> | Dize | **Gecikme** eylemi için bekleme aralığı: "İkinci", "Dakika", "Saat", "Gün", "Hafta", "Ay" | 
| <*tarih-zaman damgası*> | Dize | **Eyleme Kadar Geciktiriçin** yürütmeye devam etme tarihi ve saati. Bu değer [UTC tarih saati biçimini](https://en.wikipedia.org/wiki/Coordinated_Universal_Time)kullanmalıdır. | 
|||| 

*Örnek 1*

Bu eylem tanımı iş akışını 15 dakika duraklatabilir:

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

Bu eylem tanımı, iş akışını belirtilen süreye kadar duraklatabilir:

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

Bu eylem, diğer mantık uygulaması iş akışlarını ekleyip yeniden kullanabileceğiniz anlamına gelen önceden oluşturulmuş başka bir mantık uygulamasını çağırır. Çocuk mantık uygulamasının yanıt dönmesi koşuluyla, iç içe olan mantık uygulamasını izleyen eylemlerde, alt veya *iç içe yuvalanmış* mantık uygulamasından çıkan çıktıları da kullanabilirsiniz.

Logic Apps motoru aramak istediğiniz tetikleyiciye erişimi denetler, bu nedenle bu tetikleyiciye erişebileceğinizden emin olun. Ayrıca, iç içe mantık uygulaması aşağıdaki ölçütleri karşılamalıdır:

* Tetikleyici, İç içe geçmemi [istek](#request-trigger) veya [HTTP](#http-trigger) tetikleyicisi gibi çağrılabilir hale getirir

* Üst mantık uygulamanızla aynı Azure aboneliği

* Ana mantık uygulamanızdaki iç içe işlem mantığı uygulamasından çıkan çıktıları kullanmak için iç içe geçmemantığı uygulamasının bir [Yanıt](#response-action) eylemi olması gerekir

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*iç içe-mantık-uygulama adı*> | Dize | Aramak istediğiniz mantık uygulamasının adı | 
| <*tetikleyici adı*> | Dize | Aramak istediğiniz iç içe mantık uygulamasında ki tetikleyicinin adı | 
| <*Azure aboneliği-Kimliği*> | Dize | İç içe geçme mantığı uygulaması için Azure abonelik kimliği |
| <*Azure kaynak grubu*> | Dize | İç içe geçme mantığı uygulaması için Azure kaynak grubu adı |
| <*iç içe-mantık-uygulama adı*> | Dize | Aramak istediğiniz mantık uygulamasının adı |
||||

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------|  
| <*üstbilgi içeriği*> | JSON Nesnesi | Çağrıyla birlikte gönderilecek üstbilgi | 
| <*gövde içeriği*> | JSON Nesnesi | Aramayla birlikte gönderilecek herhangi bir ileti içeriği | 
||||

*Çıkışlar*

Bu eylemin çıktıları iç içe geçen mantık uygulamasının Yanıt eylemine bağlı olarak değişir. İç içe yer alan mantık uygulaması yanıt eylemi içermiyorsa, çıktılar boştur.

*Örnek*

"Start_search" eylemi başarıyla bittikten sonra, bu iş akışı eylem tanımı belirtilen girişlerde geçen "Get_product_information" adlı başka bir mantık uygulamasını çağırır:

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

## <a name="control-workflow-action-details"></a>İş akışı eylem ayrıntılarını denetleme

<a name="foreach-action"></a>

### <a name="foreach-action"></a>Foreach eylem

Bu döngü eylemi bir dizi boyunca yineler ve her dizi öğesi üzerinde eylemler gerçekleştirir. Varsayılan olarak, "her biri için" döngüsü en fazla döngü sayısına paralel olarak çalışır. Bu maksimum [için, Bkz. Limitler ve config.](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) ["Her biri için" döngüleri nasıl oluşturabilirsiniz](../logic-apps/logic-apps-control-flow-loops.md#foreach-loop)öğrenin.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*eylem-1... n*> | Dize | Her dizi öğesinde çalışan eylemlerin adları | 
| <*eylem tanımı-1... n*> | JSON Nesnesi | Çalışan eylemlerin tanımları | 
| <*for-her-expression*> | Dize | Belirtilen dizideki her öğeye başvuran ifade | 
|||| 

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Sayısı*> | Tamsayı | Varsayılan olarak, "her" döngü yinelemeleri [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar aynı anda (eşzamanlı veya paralel olarak) çalışır. Yeni bir <*sayısı*> değeri ayarlayarak bu sınırı değiştirmek için bkz: ["her" döngü eşzamanlılık](#change-for-each-concurrency)için değiştir. | 
| <*işlem seçeneği*> | Dize | "Her biri için" bir döngüyü paralel olarak değil, *operation-option* sırayla çalıştırmak `Sequential` için,> `1`> veya <*sayar,* ancak her ikisine de değil, <işlem seçeneğini ayarlayın. Daha fazla bilgi için bkz: ["Her biri için" döngüleri sırayla çalıştır.](#sequential-for-each) | 
|||| 

*Örnek*

Bu "her biri için" döngüsü, gelen bir e-postanın eklerini içeren dizideki her öğe için bir e-posta gönderir. Döngü, eki inceleyen bir kişiye ek de dahil olmak üzere bir e-posta gönderir.

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

Yalnızca tetikleyiciden çıktı olarak geçirilen bir dizibelirtmek için, bu ifade tetikleyici gövdeden <*dizi adı*> dizialır. Dizi yoksa bir hatayı önlemek için, ifade `?` işleci kullanır:

`@triggerBody()?['<array-name>']` 

<a name="if-action"></a>

### <a name="if-action"></a>Eğer eylem

*Koşullu*bir deyim olan bu eylem, bir koşulu temsil eden bir ifadeyi değerlendirir ve koşulun doğru mu yanlış mı olduğuna bağlı olarak farklı bir dal çalışır. Koşul doğruysa, koşul "Başarılı" durumuyla işaretlenir. [Koşullu deyimleri nasıl oluşturabilirsiniz](../logic-apps/logic-apps-control-flow-conditional-statement.md)öğrenin.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*Durum*> | JSON Nesnesi | Bir ifade olabilir durum, değerlendirmek için | 
| <*eylem-1*> | JSON Nesnesi | <*koşul*> doğru değerlendirdiğinde çalışacak eylem | 
| <*eylem tanımı*> | JSON Nesnesi | Eylemin tanımı | 
| <*eylem-2*> | JSON Nesnesi | <*koşul*> yanlış olarak değerlendirildiğinde çalışacak eylem | 
|||| 

Nesnelerdeki `actions` veya `else` nesnelerdeki eylemler şu durumları alır:

* Çalıştırıp başarılı olduklarında "başarılı"
* Çalıştırıp başarısız olduklarında "başarısız"
* İlgili şube çalışmadığında "atlanan"

*Örnek*

Bu durum, tamsayı değişkeninin değeri sıfırdan büyük olduğunda, iş akışının bir web sitesini denetler. Değişken sıfır veya daha azsa, iş akışı farklı bir web sitesini denetler.

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

#### <a name="how-conditions-use-expressions"></a>Koşullar ifadeleri nasıl kullanır?

İfadeleri koşullarda nasıl kullanabileceğinizi gösteren bazı örnekler aşağıda verilmiştir:
  
| JSON | Sonuç | 
|------|--------| 
| "ifade":@parameters" ('<*hasSpecialAction*>')" | Yalnızca Boolean ifadeleri için koşul, doğru değerlendiren herhangi bir değer için geçer. <p>Diğer türleri Boolean'a dönüştürmek için `empty()` `equals()`şu işlevleri kullanın: veya . | 
| "ifade":@greater" (eylemler('<*eylem*>').output.value, parameters('<*eşik*>')" | Karşılaştırma işlevleri için eylem yalnızca <*eylemden* çıktı> <*eşiğinin*> değerinden fazla olduğunda çalışır. | 
| "ifade":@or" (büyük('<*eylem*>').output.value, parameters('<*eşik*>'), daha az ('<*aynı eylem*>').output.value, 100)) | Mantık işlevleri ve iç içe boolean ifadeleri oluşturmak için, *eylem* <eylem> çıktı değeri> <*eşiği* daha fazla veya 100 altında olduğunda çalışır. | 
| "ifade":@equals" (uzunluk(<*eylem*>').outputs.errors), 0)) | Dizide herhangi bir öğe olup olmadığını denetlemek için dizi işlevlerini kullanabilirsiniz. `errors` Eylem, dizi boşolduğunda çalışır. | 
||| 

<a name="scope-action"></a>

### <a name="scope-action"></a>Kapsam eylemi

Bu eylem, eylemleri mantıksal olarak, bu kapsamdaki eylemleri çalıştırmayı tamamladıktan sonra kendi durumlarını alan *kapsamlara*gruplar. Daha sonra, diğer eylemlerin çalışıp çalışmadığını belirlemek için kapsam durumunu kullanabilirsiniz. [Kapsamları nasıl oluşturabilirsiniz](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)öğrenin.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------|  
| <*iç eylem-1... n*> | JSON Nesnesi | Kapsam içinde çalışan bir veya daha fazla eylem |
| <*eylem girişleri*> | JSON Nesnesi | Her eylem için girişler |
|||| 

<a name="switch-action"></a>

### <a name="switch-action"></a>Switch eylemi

Bu eylem, anahtar *deyimi*olarak da bilinir, diğer eylemleri *servis talepleri*ne kadar düzenler ve varsa varsayılan durum durumu dışında her servis talebine bir değer atar. İş akışınız çalıştığında, **Anahtar** eylemi bir ifade, nesne veya belirteç teki değeri her servis talebi için belirtilen değerlerle karşılaştırır. **Switch** eylemi eşleşen bir servis talebi bulursa, iş akışınız yalnızca bu servis talebiiçin eylemleri çalıştırAr. **Switch** eylemi her çalıştığında, yalnızca bir eşleşen servis talebi vardır veya eşleşme yoktur. Eşleşme yoksa, **Geçiş** eylemi varsayılan eylemleri çalıştırZ. [Anahtar deyimleri oluşturmayı](../logic-apps/logic-apps-control-flow-switch-statement.md)öğrenin.

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*ifade-nesne-veya belirteç*> | Değişir | İfade, JSON nesnesi veya değerlendirmek için belirteç | 
| <*eylem adı*> | Dize | Eşleşen servis talebi için çalışacak eylemin adı | 
| <*eylem tanımı*> | JSON Nesnesi | Eşleşen servis talebi için çalışacak eylem tanımı | 
| <*eşleşen değer*> | Değişir | Değerlendirilen sonuçla karşılaştırılacak değer | 
|||| 

*Isteğe bağlı*

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*varsayılan eylem adı*> | Dize | Eşleşen bir servis talebi olmadığında çalışacak varsayılan eylemin adı | 
| <*varsayılan eylem tanımı*> | JSON Nesnesi | Eşleşen bir servis talebi yokken çalışacak eylem tanımı | 
|||| 

*Örnek*

Bu eylem tanımı, onay isteğie yanıt veren kişinin "Onayla" seçeneğini mi yoksa "Reddet" seçeneğini mi seçtiğini değerlendirir. Bu seçime bağlı olarak, **Switch** eylemi, yanıtlayana başka bir e-posta göndermek için ancak her durumda farklı ifadeler içeren eşleşen servis talebi için eylemleri çalıştırır. 

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

### <a name="until-action"></a>Eyleme kadar

Bu döngü eylemi, belirtilen koşul doğru olana kadar çalışan eylemleri içerir. Döngü, diğer tüm eylemler çalıştırdıktan sonra son adım olarak koşulu denetler. `"actions"` Nesneye birden fazla eylem ekleyebilirsiniz ve eylemin en az bir sınır tanımlaması gerekir. ["Until" döngüleri nasıl oluşturabilirsiniz](../logic-apps/logic-apps-control-flow-loops.md#until-loop)öğrenin. 

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

| Değer | Tür | Açıklama | 
|-------|------|-------------| 
| <*eylem adı*> | Dize | Döngü içinde çalıştırmak istediğiniz eylemin adı | 
| <*eylem türü*> | Dize | Çalıştırmak istediğiniz eylem türü | 
| <*eylem girişleri*> | Çeşitli | Eylemin çalıştırılaması için girişler | 
| <*Durum*> | Dize | Döngü bitiş çalışma tüm eylemleri sonra değerlendirmek için koşul veya ifade | 
| <*döngü sayısı*> | Tamsayı | Eylemin çalıştırabileceği en çok döngü sayısının sınırı. Varsayılan sınır ve maksimum sınır hakkında daha fazla bilgi için [Azure Mantık Uygulamaları için Sınırlar ve yapılandırma'ya](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)bakın. | 
| <*döngü zaman ayarı*> | Dize | Döngünün çalıştırabileceği en uzun süre nin sınırı. Varsayılan `timeout` değer, `PT1H`gerekli [ISO 8601 biçimidir.](https://en.wikipedia.org/wiki/ISO_8601) |
|||| 

*Örnek*

Bu döngü eylem tanımı, bu koşullardan biri karşılanana kadar belirtilen URL'ye bir HTTP isteği gönderir:

* İstek "200 Tamam" durum kodu ile bir yanıt alır.
* Döngü 60 kez geçti.
* Döngü bir saat boyunca devam etti.

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

## <a name="webhooks-and-subscriptions"></a>Webhooks ve abonelikler

Webhook tabanlı tetikleyiciler ve eylemler uç noktaları düzenli olarak denetlemez, ancak bunun yerine bu uç noktalarda belirli olayları veya verileri bekleyin. Bu tetikleyiciler ve eylemler, bitiş noktasının yanıt gönderebileceği bir *geri arama URL'si* sağlayarak uç noktalara *abone* olur.

Arama, `subscribe` iş akışı herhangi bir şekilde değiştiğinde, örneğin kimlik bilgileri yenilendiğinde veya giriş parametreleri bir tetikleyici veya eylem için değiştiğinde gerçekleşir. Bu çağrı, standart HTTP eylemleri yle aynı parametreleri kullanır. 

Arama, `unsubscribe` bir işlem tetikleyiciyi veya eylemi geçersiz kıldığında otomatik olarak gerçekleşir, örneğin:

* Tetikleyiciyi siler veya devre dışı bırakma. 
* İş akışını silme veya devre dışı bırakma. 
* Aboneliği silme veya devre dışı bırakma. 

Bu çağrıları desteklemek `@listCallbackUrl()` için, ifade tetikleyici veya eylem için benzersiz bir "geri arama URL"si döndürür. Bu URL, hizmetin REST API'sini kullanan uç noktalar için benzersiz bir tanımlayıcıyı temsil eder. Bu işlevin parametreleri webhook tetikleyicisi veya eylemiyle aynıdır.

<a name="asynchronous-limits"></a>

## <a name="change-asynchronous-duration"></a>Eşzamanlı süreyi değiştirme

Hem tetikleyiciler hem de eylemler için, özellik ekleyerek asynchronous deseni için `limit.timeout` süreyi belirli bir zaman aralığıyla sınırlandırabilirsiniz. Bu şekilde, aralık zaman sona erdiğinde eylem tamamlanmadıysa, eylemin durumu `Cancelled` `ActionTimedOut` kodla işaretlenmiş olur. Özellik `timeout` [ISO 8601 biçimini](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)kullanır.

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

Tetikleyici veya eylem tanımına bu `runtimeConfiguration` özellikleri ekleyerek tetikleyiciler ve eylemler için varsayılan çalışma zamanı davranışını değiştirebilirsiniz.

| Özellik | Tür | Açıklama | Tetikleyici veya eylem | 
|----------|------|-------------|-------------------| 
| `runtimeConfiguration.concurrency.runs` | Tamsayı | Aynı anda (eşzamanlı veya paralel olarak) çalışabilen iş akışı örneklerinin [*sayısındaki varsayılan sınırı*](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) değiştirin. Bu değeri ayarlamak, arka uç sistemlerinin aldığı istek sayısını sınırlamaya yardımcı olabilir. <p>`runs` Özelliği, özelliği `1` `SingleInstance`' ye ayarlama `operationOptions` yla aynı şekilde çalışır. Her iki özelliği de ayarlayabilirsiniz, ancak her ikisini birden ayarlayamaabilirsiniz. <p>Varsayılan sınırı değiştirmek için [bkz.](#change-trigger-concurrency) [Trigger instances sequentially](#sequential-trigger) | Tüm tetikleyiciler | 
| `runtimeConfiguration.concurrency.maximumWaitingRuns` | Tamsayı | Mantık uygulamanız zaten en fazla eşzamanlı örnekleri çalıştırırken çalıştırmak için beklemesi gereken iş akışı örneklerinin sayısındaki [*varsayılan sınırı*](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) değiştirin. <p>Varsayılan sınırı değiştirmek için [bkz.](#change-waiting-runs) | Tüm tetikleyiciler | 
| `runtimeConfiguration.concurrency.repetitions` | Tamsayı | Aynı anda (eşzamanlı veya paralel olarak) çalışabilen "her biri için" döngü yinelemelerinin sayısındaki [*varsayılan sınırı*](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) değiştirin. <p>`repetitions` Özelliği, özelliği `1` `SingleInstance`' ye ayarlama `operationOptions` yla aynı şekilde çalışır. Her iki özelliği de ayarlayabilirsiniz, ancak her ikisini birden ayarlayamaabilirsiniz. <p>Varsayılan sınırı değiştirmek için bkz: ["Her biri için" eşzamanlılık değiştir](#change-for-each-concurrency) veya "her biri [için" döngüleri sırayla çalıştır.](#sequential-for-each) | Eylem: <p>[Foreach](#foreach-action) | 
| `runtimeConfiguration.paginationPolicy.minimumItemCount` | Tamsayı | Destekleyen ve pagination açık olan belirli eylemler için, bu değer almak için *en az* sonuç sayısını belirtir. <p>Pagination'ı açmak için [bkz.](../logic-apps/logic-apps-exceed-default-page-size-with-pagination.md) | Eylem: Çeşitli |
| `runtimeConfiguration.secureData.properties` | Dizi | Birçok tetikleyici ve eylemde, bu ayarlar girişleri, çıktıları veya her ikisini de mantık uygulamasının çalışma geçmişinden gizler. <p>Bu verileri koruma hakkında daha fazla bilgi edinmek için [bkz.](../logic-apps/logic-apps-securing-a-logic-app.md#secure-data-code-view) | Çoğu tetikleyici ve eylem |
| `runtimeConfiguration.staticResult` | JSON Nesnesi | Statik [sonuç](../logic-apps/test-logic-apps-mock-data-static-results.md) ayarını destekleyen ve açık olan `staticResult` eylemler için nesneşu özniteliklere sahiptir: <p>- `name`, geçerli eylemin statik sonuç tanım adı, hangi mantık `staticResults` uygulaması iş akışı `definition` özniteliği öznitelik içinde görünür başvurur. Daha fazla bilgi için, [İş Akışı Tanımı Dili için Statik sonuçlar - Şema başvurusuna](../logic-apps/logic-apps-workflow-definition-language.md#static-results)bakın. <p> - `staticResultOptions`, statik sonuçların `Enabled` geçerli eylem için olup olmadığını belirtir. <p>Statik sonuçları açmak için statik [sonuçlar ayarlayarak sahte verilerle test mantığı uygulamalarına](../logic-apps/test-logic-apps-mock-data-static-results.md) bakın | Eylem: Çeşitli |
||||| 

<a name="operation-options"></a>

## <a name="operation-options"></a>İşlem seçenekleri

Tetikleyici veya eylem tanımında `operationOptions` özellik ile tetikleyiciler ve eylemler için varsayılan davranışı değiştirebilirsiniz.

| İşlem seçeneği | Tür | Açıklama | Tetikleyici veya eylem | 
|------------------|------|-------------|-------------------| 
| `DisableAsyncPattern` | Dize | Eş zamanlı olarak değil, http tabanlı eylemleri eşzamanlı olarak çalıştırın. <p><p>Bu seçeneği ayarlamak için, [eylemleri eşzamanlı olarak çalıştır'a](#asynchronous-patterns)bakın. | Eylem: <p>[ApiConnection](#apiconnection-action), <br>[HTTP ,](#http-action) <br>[Yanıt](#response-action) | 
| `OptimizedForHighThroughput` | Dize | 5 dakikada eylem yürütme sayının [varsayılan sınırını](../logic-apps/logic-apps-limits-and-config.md#throughput-limits) [maksimum sınırla](../logic-apps/logic-apps-limits-and-config.md#throughput-limits)değiştirin. <p><p>Bu seçeneği ayarlamak için [bkz.](#run-high-throughput-mode) | Tüm eylemler | 
| `Sequential` | Dize | "Her" döngü yinelemelerini paralel olarak aynı anda değil, birer birer çalıştırın. <p>Bu seçenek, `runtimeConfiguration.concurrency.repetitions` özelliği `1`' ye ayarlama yla aynı şekilde çalışır. Her iki özelliği de ayarlayabilirsiniz, ancak her ikisini birden ayarlayamaabilirsiniz. <p><p>Bu seçeneği ayarlamak için bkz: ["Her biri için" döngüleri sırayla çalıştır.](#sequential-for-each)| Eylem: <p>[Foreach](#foreach-action) | 
| `SingleInstance` | Dize | Her mantık uygulaması örneği için tetikleyiciyi sırayla çalıştırın ve bir sonraki mantık uygulaması örneğini tetiklemeden önce daha önce etkin olan çalıştırmanın tamamlanmasını bekleyin. <p><p>Bu seçenek, `runtimeConfiguration.concurrency.runs` özelliği `1`' ye ayarlama yla aynı şekilde çalışır. Her iki özelliği de ayarlayabilirsiniz, ancak her ikisini birden ayarlayamaabilirsiniz. <p>Bu seçeneği ayarlamak için, [bkz.](#sequential-trigger) | Tüm tetikleyiciler | 
||||

<a name="change-trigger-concurrency"></a>

### <a name="change-trigger-concurrency"></a>Tetikleyici eşzamanlılığını değiştirme

Varsayılan olarak, mantık uygulaması iş akışı örneklerinin tümü aynı anda (eşzamanlı veya paralel olarak) çalışır. Bu davranış, her tetikleyici örneği önceki etkin iş akışı örneği çalışan bitmeden önce yangınları anlamına gelir. Ancak, aynı anda çalışan örnek sayısı varsayılan [sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)vardır. Aynı anda çalışan iş akışı örneklerinin sayısı bu sınıra ulaştığında, diğer yeni örneklerin çalışmasını beklemesi gerekir. Bu sınır, arka uç sistemlerinin aldığı istek sayısını denetlemeye yardımcı olur.

Tetikleyicinin eşzamanlılık denetimini açtığınızda, tetikleyici örnekleri [varsayılan sınıra](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)paralel olarak çalışır. Bu varsayılan eşzamanlılık sınırını değiştirmek için, kod görünümü düzenleyicisini veya Logic Apps Designer'ı kullanabilirsiniz, çünkü tasarımcı aracılığıyla eşzamanlılık ayarını değiştirmek, temel tetikleyici tanımındaki `runtimeConfiguration.concurrency.runs` özelliği ekler veya günceller ve bunun tersi. Bu özellik, paralel olarak çalışabilen en fazla yeni iş akışı örneği sayısını denetler.

Tetikleyicide eşzamanlılık etkinleştirmek istediğinizde göz önünde bulundurulması gereken bazı noktalar şunlardır:

* Eşzamanlılık etkinleştirildiğinde, [dizileri ayırmak](#split-on-debatch)için [SplitOn sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) önemli ölçüde azalır. Madde sayısı bu sınırı aşarsa, SplitOn özelliği devre dışı bırakılır.

* Eşzamanlılık denetimini etkinleştirdikten sonra eşzamanlılığı devre dışı kalamazsınız.

* Eşzamanlılık etkinleştirildiğinde, [dizileri ayırmak](#split-on-debatch)için [SplitOn sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) önemli ölçüde azalır. Madde sayısı bu sınırı aşarsa, SplitOn özelliği devre dışı bırakılır.

* Eşzamanlılık etkinleştirildiğinde, uzun süren bir mantık uygulaması örneği yeni mantık uygulaması örneklerinin bekleme durumuna girmesine neden olabilir. Bu durum, Azure Mantık Uygulamalarının yeni örnekler oluşturmasını engeller ve eşzamanlı çalıştırma sayısı belirtilen maksimum eşzamanlı çalıştırma sayısından az olsa bile gerçekleşir.

  * Bu durumu bölmek için, hala çalışan ilk örnekleri iptal *edin.*

    1. Mantık uygulamanızın menüsünde **Genel Bakış'ı**seçin.

    1. Çalışır **geçmişi** bölümünde, örneğin, hala çalışan en erken örneği seçin:

       ![En erken çalışan örneği seçin](./media/logic-apps-workflow-actions-triggers/waiting-runs.png)

       > [!TIP]
       > Yalnızca çalışmaya devam eden örnekleri görüntülemek için **Tüm** listeyi açın ve **Çalıştır'ı**seçin.

    1. **Mantık uygulaması altında çalıştır**, **çalıştır'ı iptal**et'i seçin.

       ![En erken çalışan örneği bulma](./media/logic-apps-workflow-actions-triggers/cancel-run.png)

  * Bu olasılığı n için, bu çalıştırmaları geciktirebilecek herhangi bir eyleme bir zaman aşımı ekleyin. Kod düzenleyicisinde çalışıyorsanız, [bkz.](#asynchronous-limits) Aksi takdirde, tasarımcıyı kullanıyorsanız aşağıdaki adımları izleyin:

    1. Mantık uygulamanızda, sağ üst köşede bir zaman ekineklemek istediğiniz eylemde elips (**...**) düğmesini seçin ve ardından **Ayarlar'ı**seçin.

       ![Eylem ayarlarını açma](./media/logic-apps-workflow-actions-triggers/action-settings.png)

    1. **Timeout**altında, [ISO 8601 formatında](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)zaman ödeme süresini belirtin.

       ![Zaman ara süresini belirtin](./media/logic-apps-workflow-actions-triggers/timeout.png)

* Mantık uygulamanızı sırayla çalıştırmak için, kod görünümü `1` düzenleyicisini veya tasarımcıyı kullanarak tetikleyicinin eşzamanlılığını ayarlayın. Tetikleyicinin `operationOptions` özelliğini kod görünümü `SingleInstance` düzenleyicisinde de ayarlamadığınızdan emin olun. Aksi takdirde, bir doğrulama hatası alırsınız. Daha fazla bilgi için [bkz.](#sequential-trigger)

#### <a name="edit-in-code-view"></a>Kod görünümünde edin 

Temel tetikleyici tanımında, `runtimeConfiguration.concurrency.runs` `1` `50`'den ' e kadar değişen bir değere sahip olabilecek özelliği ekleyin

Eş zamanlı çalıştırmaları 10 örnekle sınırlayan bir örnek aşağıda verilmiştir:

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

Daha fazla bilgi için [Runtime yapılandırma ayarlarına](#runtime-config-options)bakın.

#### <a name="edit-in-logic-apps-designer"></a>Mantık Uygulamaları Tasarımcısı'nda Edit

1. Tetikleyicinin sağ üst köşesinde elips (**...**) düğmesini seçin ve ardından **Ayarlar'ı**seçin.

1. **Eşzamanlılık Denetimi**altında, **Limit'i Açık**olarak ayarlayın. **Limit** 

1. **Paralellik derecesini** istediğiniz değere sürükleyin. Mantık uygulamanızı sırayla çalıştırmak için kaydırıcı değerini **1'e**sürükleyin.

<a name="change-for-each-concurrency"></a>

### <a name="change-for-each-concurrency"></a>"Her biri için" eşzamanlılığı değiştirme

Varsayılan olarak, "her" döngü yinelemeleri tüm aynı anda (eşzamanlı veya paralel olarak) çalıştırın. Bu davranış, her yineleme önceki yineleme çalışan bitirmeden önce çalışmaya başlar anlamına gelir. Ancak, aynı anda çalışan yineleme sayısı varsayılan [sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)vardır. Aynı anda çalışan yineleme sayısı bu sınıra ulaştığında, diğer yinelemelerin çalışmasını beklemeniz gerekir.

Varsayılan sınırı değiştirmek için, kod görünümü düzenleyicisini veya Logic Apps Designer'ı kullanabilirsiniz, çünkü `runtimeConfiguration.concurrency.repetitions` tasarımcı aracılığıyla eşzamanlılık ayarını değiştirmek özelliği temel "her" eylem tanımında ekler veya güncelleştirir veya günceller ve bunun tersi de olabilir. Bu özellik paralel olarak çalıştırılabilen en fazla yineleme sayısını denetler.

> [!NOTE] 
> "Her biri için" eylemini tasarımcıyı veya kod görünümü düzenleyicisini kullanarak sırayla çalışacak şekilde `operationOptions` ayarlarsanız, eylemin özelliğini kod görünümü `Sequential` düzenleyicisinde ayarlamayın. Aksi takdirde, bir doğrulama hatası alırsınız. Daha fazla bilgi için bkz: ["Her biri için" döngüleri sırayla çalıştır.](#sequential-for-each)

#### <a name="edit-in-code-view"></a>Kod görünümünde edin 

Temel "her biri için" tanımında, `runtimeConfiguration.concurrency.repetitions` arasında değişen bir değere sahip `1` `50`olabilecek özelliği ekleyin veya güncelleştirin.

Eş zamanlı çalıştırmaları 10 yinelemeyle sınırlayan bir örnek aşağıda verilmiştir:

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

Daha fazla bilgi için [Runtime yapılandırma ayarlarına](#runtime-config-options)bakın.

#### <a name="edit-in-logic-apps-designer"></a>Mantık Uygulamaları Tasarımcısı'nda Edit

1. Her eylem **için,** sağ üst köşeden, elipsleri (**...**) düğmesini seçin ve ardından **Ayarlar'ı**seçin.

1. **Eşzamanlılık Denetimi**altında, **Eşzamanlılık Denetimini** **Açık**olarak ayarlayın.

1. **Paralellik derecesini** istediğiniz değere sürükleyin. Mantık uygulamanızı sırayla çalıştırmak için kaydırıcı değerini **1'e**sürükleyin.

<a name="change-waiting-runs"></a>

### <a name="change-waiting-runs-limit"></a>Değişiklik bekleme çalıştırmaları sınırı

Varsayılan olarak, mantık uygulaması iş akışı örneklerinin tümü aynı anda (eşzamanlı veya paralel olarak) çalışır. Bu davranış, her tetikleyici örneği önceki etkin iş akışı örneği çalışan bitmeden önce yangınları anlamına gelir. Ancak, aynı anda çalışan örnek sayısı varsayılan [sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)vardır. Aynı anda çalışan iş akışı örneklerinin sayısı bu sınıra ulaştığında, diğer yeni örneklerin çalışmasını beklemesi gerekir.

Bekleyen çalıştırma sayısının varsayılan [sınırı](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)da vardır. Bekleme çalıştırma sayısı bu sınıra ulaştığında, Logic Apps altyapısı artık yeni çalıştırmaları kabul etmez. İstek ve webhook tetikleyicileri 429 hata döndürür ve yinelenen tetikleyiciler yoklama denemelerini atlamabaşlar.

Yalnızca tetikleyici [eşzamanlılıküzerindeki varsayılan sınırı değiştirmekle](#change-trigger-concurrency)birlikte, bekleme çalıştırmalarında varsayılan sınırı da değiştirebilirsiniz. Temel tetikleyici tanımında, `runtimeConfiguration.concurrency.maximumWaitingRuns` `1` `100`'den ' e kadar değişen bir değere sahip olabilecek özelliği ekleyin

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

Daha fazla bilgi için [Runtime yapılandırma ayarlarına](#runtime-config-options)bakın.

<a name="sequential-trigger"></a>

### <a name="trigger-instances-sequentially"></a>Örnekleri sırayla tetikleme

Her mantık uygulaması iş akışı örneğini yalnızca önceki örnek çalışma bittikten sonra çalıştırmak için, tetikleyiciyi sırayla çalışacak şekilde ayarlayın. Kod görünümü düzenleyicisini veya Logic Apps Designer'ı kullanabilirsiniz, çünkü tasarımcı `runtimeConfiguration.concurrency.runs` aracılığıyla eşzamanlılık ayarını değiştirmek, temel tetikleyici tanımındaki özelliği de ekler veya günceller ve bunun tersi de vardır.

> [!NOTE] 
> Bir tetikleyiciyi tasarımcıyı veya kod görünümü düzenleyicisini kullanarak sırayla çalışacak şekilde ayarladığınızda, tetikleyicinin `operationOptions` özelliğini kod görünümü `Sequential` düzenleyicisine ayarlamayın. Aksi takdirde, bir doğrulama hatası alırsınız. 

#### <a name="edit-in-code-view"></a>Kod görünümünde edin

Tetikleyici tanımında, bu özelliklerden birini ayarlayın, ancak her ikisini de ayarlamayın. 

`runtimeConfiguration.concurrency.runs` Özelliği şu `1`şekilde ayarlayın:

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

*-veya-*

`operationOptions` Özelliği şu `SingleInstance`şekilde ayarlayın:

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

Daha fazla bilgi için [Runtime yapılandırma ayarları](#runtime-config-options) ve [Çalışma seçeneklerine](#operation-options)bakın.

#### <a name="edit-in-logic-apps-designer"></a>Mantık Uygulamaları Tasarımcısı'nda Edit

1. Tetikleyicinin sağ üst köşesinde elips (**...**) düğmesini seçin ve ardından **Ayarlar'ı**seçin.

1. **Eşzamanlılık Denetimi**altında, **Limit'i Açık**olarak ayarlayın. **Limit** 

1. **Paralellik derecesini** sayıya `1`sürükleyin. 

<a name="sequential-for-each"></a>

### <a name="run-for-each-loops-sequentially"></a>"Her biri için" döngüleri sırayla çalıştırın

Yalnızca önceki yineleme çalışma bittikten sonra bir "her" döngü yinelemesini çalıştırmak için, "her biri için" eylemini sırayla çalışacak şekilde ayarlayın. Kod görünümü düzenleyicisini veya Logic Apps Designer'ı kullanabilirsiniz, çünkü tasarımcı aracılığıyla `runtimeConfiguration.concurrency.repetitions` eylemin eşzamanlılıkını değiştirmek, temel eylem tanımındaki özelliği de ekler veya günceller ve bunun tersi de vardır.

> [!NOTE] 
> Tasarımcıyı veya kod görünümü düzenleyicisini kullanarak sırayla çalışacak bir "her biri için" eylemi `operationOptions` ayarladığınızda, eylemin özelliğini kod görünümü `Sequential` düzenleyicisinde ayarlamayın. Aksi takdirde, bir doğrulama hatası alırsınız. 

#### <a name="edit-in-code-view"></a>Kod görünümünde edin

Eylem tanımında, bu özelliklerden birini ayarlayın, ancak her ikisini de ayarlamayın. 

`runtimeConfiguration.concurrency.repetitions` Özelliği şu `1`şekilde ayarlayın:

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

*-veya-*

`operationOptions` Özelliği şu `Sequential`şekilde ayarlayın:

```json
"For_each" {
   "type": "Foreach",
   "actions": { "<actions-to-run>" },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "operationOptions": "Sequential"
}
```

Daha fazla bilgi için [Runtime yapılandırma ayarları](#runtime-config-options) ve [Çalışma seçeneklerine](#operation-options)bakın.

#### <a name="edit-in-logic-apps-designer"></a>Mantık Uygulamaları Tasarımcısı'nda Edit

1. **Her** eylemin sağ üst köşesinde elips (**...**) düğmesini seçin ve ardından **Ayarlar'ı**seçin.

1. **Eşzamanlılık Denetimi**altında, **Eşzamanlılık Denetimini** **Açık**olarak ayarlayın.

1. **Paralellik derecesini** sayıya `1`sürükleyin.

<a name="asynchronous-patterns"></a>

### <a name="run-actions-synchronously"></a>Eylemleri eşzamanlı olarak çalıştırma

Varsayılan olarak, tüm HTTP tabanlı eylemler standart eşzamanlı işlem deseni izler. Bu desen, HTTP tabanlı bir eylem belirtilen bitiş noktasına bir istek gönderdiğinde, uzak sunucunun "202 KABUL EDİlMİş" yanıtını geri gönderdiğini belirtir. Bu yanıt, sunucunun işleme isteğini kabul ettiği anlamına gelir. Logic Apps altyapısı, 202 olmayan yanıt olan işlem durana kadar yanıtın konum üstbilgisi tarafından belirtilen URL'yi kontrol etmeye devam eder.

Ancak, isteklerin bir zaman önceliği sınırı vardır, bu nedenle uzun süren eylemler `operationOptions` için, özelliği eylemin `DisableAsyncPattern` girişleri altında ekleyerek ve ayarlayarak eşzamanlı davranışı devre dışı kullanabilirsiniz.

```json
"<some-long-running-action>": {
   "type": "Http",
   "inputs": { "<action-inputs>" },
   "operationOptions": "DisableAsyncPattern",
   "runAfter": {}
}
```

Daha fazla bilgi için [İşlem seçeneklerine](#operation-options)bakın.

<a name="run-high-throughput-mode"></a>

### <a name="run-in-high-throughput-mode"></a>Yüksek iş elde modunda çalıştırın

Tek bir mantık uygulaması tanımı için, her 5 dakikada bir gerçekleştirdiği eylem sayısı [varsayılan bir sınıra](../logic-apps/logic-apps-limits-and-config.md#throughput-limits)sahiptir. Bu sınırı mümkün olan [en yüksek sınıra](../logic-apps/logic-apps-limits-and-config.md#throughput-limits) yükseltmek için `operationOptions` özelliği `OptimizedForHighThroughput`' ye ayarlayın. Bu ayar, mantık uygulamanızı "yüksek iş sahibi" moduna sokar.

> [!NOTE]
> Yüksek iş elde modu önizlemede. Gerektiğinde birden fazla mantık uygulamasına iş yükü de dağıtabilirsiniz.

```json
"<action-name>": {
   "type": "<action-type>",
   "inputs": { "<action-inputs>" },
   "operationOptions": "OptimizedForHighThroughput",
   "runAfter": {}
}
```

<a name="authenticate-triggers-actions"></a>

## <a name="authenticate-triggers-and-actions"></a>Tetikleyicileri ve eylemleri doğrula

HTTP ve HTTPS uç noktaları farklı kimlik doğrulama türlerini destekler. Giden aramaları veya bu uç noktalara erişmek için isteklerde bulunmak için kullandığınız tetikleyiciye veya eyleme bağlı olarak, farklı kimlik doğrulama türleri aralıklarından seçim yapabilirsiniz. Daha fazla bilgi için bkz: [Giden aramalara kimlik doğrulama ekle.](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)

## <a name="next-steps"></a>Sonraki adımlar

* [İş Akışı Tanımı Dili](../logic-apps/logic-apps-workflow-definition-language.md) hakkında daha fazla bilgi edinin
