---
title: Giden bağlantı kimlik doğrulaması
description: Azure Zamanlayıcısı için giden kimlik doğrulamasını nasıl ayarlayıp kaldırılamayı öğrenin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/15/2016
ms.openlocfilehash: bcd14e618323aec1c7ce47fcebb25099fa96be81
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898504"
---
# <a name="outbound-authentication-for-azure-scheduler"></a>Azure Zamanlayıcısı için giden kimlik doğrulaması

> [!IMPORTANT]
> [Azure Logic Apps,](../logic-apps/logic-apps-overview.md) [kullanımdan kaldırılan](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)Azure Zamanlayıcısı'nın yerini alıyor. Zamanlayıcı'da ayarladığınız işlerle çalışmaya devam etmek için lütfen mümkün olan en kısa sürede [Azure Logic Apps'a geçirin.](../scheduler/migrate-from-scheduler-to-logic-apps.md) 
>
> Zamanlayıcı artık Azure portalında kullanılamıyor, ancak işlerinizi ve iş koleksiyonlarınızı yönetebilmeniz için [ŞU anda REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlets'i](scheduler-powershell-reference.md) kullanılabilir durumda kalır.

Azure Zamanlayıcısı işlerinin, diğer Azure hizmetleri, Salesforce.com, Facebook ve güvenli özel web siteleri gibi kimlik doğrulaması gerektiren hizmetleri araması gerekebilir. Çağrılan hizmet, Zamanlayıcı işinin istenen kaynaklara erişip erişemeyeceğini belirleyebilir. 

Zamanlayıcı bu kimlik doğrulama modellerini destekler: 

* SSL/TLS istemci sertifikalarını kullanırken *müşteri sertifikası* kimlik doğrulaması
* *Temel* kimlik doğrulama
* *Active Directory OAuth* kimlik doğrulaması

## <a name="add-or-remove-authentication"></a>Kimlik doğrulama ekleme veya kaldırma

* Bir Zamanlayıcı işine kimlik doğrulama eklemek için, işi oluşturduğunuzda veya güncellediğinizde, öğeye `authentication` JavaScript Nesne Gösterimi (JSON) alt öğesini `request` ekleyin. 

  Yanıtlar, nesnedeki BIR PUT, PATCH veya POST isteği aracılığıyla Zamanlayıcı `authentication` hizmetine aktarılan sırları asla döndürmez. 
  Yanıtlar gizli bilgileri geçersiz kılacak şekilde ayarlar veya kimlik doğrulaması yapılan varlığı temsil eden genel bir belirteç kullanabilir. 

* Zamanlayıcı işinden kimlik doğrulamasını kaldırmak için, iş üzerinde açıkça bir PUT `authentication` veya PATCH isteği çalıştırın ve nesneyi null olarak ayarlayın. Yanıt, kimlik doğrulama özelliği içermez.

## <a name="client-certificate"></a>İstemci sertifikası

### <a name="request-body---client-certificate"></a>İstek gövdesi - Müşteri sertifikası

`ClientCertificate` Modeli kullanarak kimlik doğrulaması eklerken, istek gövdesindeki bu ek öğeleri belirtin.  

| Öğe | Gerekli | Açıklama |
|---------|----------|-------------|
| **kimlik doğrulama** (üst öğe) | SSL istemci sertifikası kullanmak için kimlik doğrulama nesnesi |
| **Türü** | Evet | Kimlik doğrulama türü. SSL istemci sertifikaları için `ClientCertificate`değer. |
| **Pfx** | Evet | PFX dosyasının temel 64 kodlanmış içeriği |
| **parola** | Evet | PFX dosyasına erişmek için parola |
||| 

### <a name="response-body---client-certificate"></a>Yanıt gövdesi - İstemci sertifikası 

Bir istek kimlik doğrulama bilgileriyle gönderildiğinde, yanıt bu kimlik doğrulama öğelerini içerir.

| Öğe | Açıklama | 
|---------|-------------| 
| **kimlik doğrulama** (üst öğe) | SSL istemci sertifikası kullanmak için kimlik doğrulama nesnesi |
| **Türü** | Kimlik doğrulama türü. SSL istemci sertifikaları için `ClientCertificate`değer. |
| **certificateThumbprint** |Sertifikanın parmak izi |
| **sertifikaSubjectName** |Sertifika konusu ayırt edici ad |
| **sertifikaSona ermesi** | Sertifikanın son kullanma tarihi |
||| 

### <a name="sample-rest-request---client-certificate"></a>Örnek REST isteği - İstemci sertifikası

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled"
  }
}
```

### <a name="sample-rest-response---client-certificate"></a>Örnek REST yanıtı - İstemci sertifikası

```json
HTTP/1.1 200 OKCache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="basic"></a>Temel

### <a name="request-body---basic"></a>İstek gövdesi - Temel

`Basic` Modeli kullanarak kimlik doğrulaması eklerken, istek gövdesindeki bu ek öğeleri belirtin.

| Öğe | Gerekli | Açıklama |
|---------|----------|-------------|
| **kimlik doğrulama** (üst öğe) | Temel kimlik doğrulamasını kullanmak için kimlik doğrulama nesnesi | 
| **Türü** | Evet | Kimlik doğrulama türü. Temel kimlik doğrulaması için `Basic`değer. | 
| **Username** | Evet | Kimlik doğrulaması için kullanıcı adı | 
| **parola** | Evet | Kimlik doğrulaması için parola |
|||| 

### <a name="response-body---basic"></a>Yanıt gövdesi - Temel

Bir istek kimlik doğrulama bilgileriyle gönderildiğinde, yanıt bu kimlik doğrulama öğelerini içerir.

| Öğe | Açıklama | 
|---------|-------------|
| **kimlik doğrulama** (üst öğe) | Temel kimlik doğrulamasını kullanmak için kimlik doğrulama nesnesi |
| **Türü** | Kimlik doğrulama türü. Temel kimlik doğrulaması için `Basic`değer. |
| **Username** | Kimlik doğrulaması kullanıcı adı |
||| 

### <a name="sample-rest-request---basic"></a>Örnek REST isteği - Temel

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled"
  }
}
```

### <a name="sample-rest-response---basic"></a>Örnek REST yanıtı - Temel

```json
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"Http"
      },
      "recurrence":{  
         "frequency":"Minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"Enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="active-directory-oauth"></a>Aktif Dizin OAuth

### <a name="request-body---active-directory-oauth"></a>İstek gövdesi - Active Directory OAuth 

`ActiveDirectoryOAuth` Modeli kullanarak kimlik doğrulaması eklerken, istek gövdesindeki bu ek öğeleri belirtin.

| Öğe | Gerekli | Açıklama |
|---------|----------|-------------|
| **kimlik doğrulama** (üst öğe) | Evet | ActiveDirectoryOAuth kimlik doğrulaması kullanmak için kimlik doğrulama nesnesi |
| **Türü** | Evet | Kimlik doğrulama türü. ActiveDirectoryOAuth kimlik doğrulaması için `ActiveDirectoryOAuth`değer. |
| **Kiracı** | Evet | Azure AD kiracısının kiracı tanımlayıcısı. Azure AD kiracısının kiracı tanımlayıcısını bulmak için `Get-AzureAccount` Azure PowerShell'de çalıştırın. |
| **Seyirci** | Evet | Bu `https://management.core.windows.net/`değer' e ayarlanır. | 
| **Clientıd** | Evet | Azure AD uygulaması için istemci tanımlayıcısı | 
| **Gizli** | Evet | Belirteci isteyen istemci için gizli | 
|||| 

### <a name="response-body---active-directory-oauth"></a>Yanıt gövdesi - Active Directory OAuth

Bir istek kimlik doğrulama bilgileriyle gönderildiğinde, yanıt bu kimlik doğrulama öğelerini içerir.

| Öğe | Açıklama |
|---------|-------------|
| **kimlik doğrulama** (üst öğe) | ActiveDirectoryOAuth kimlik doğrulaması kullanmak için kimlik doğrulama nesnesi |
| **Türü** | Kimlik doğrulama türü. ActiveDirectoryOAuth kimlik doğrulaması için `ActiveDirectoryOAuth`değer. | 
| **Kiracı** | Azure AD kiracısının kiracı tanımlayıcısı |
| **Seyirci** | Bu `https://management.core.windows.net/`değer' e ayarlanır. |
| **Clientıd** | Azure AD uygulaması için istemci tanımlayıcısı |
||| 

### <a name="sample-rest-request---active-directory-oauth"></a>Örnek REST isteği - Active Directory OAuth

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "Http"
    },
    "recurrence": {
      "frequency": "Minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "Enabled"
  }
}
```

### <a name="sample-rest-response---active-directory-oauth"></a>Örnek REST yanıtı - Active Directory OAuth

```json
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{
   "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type": "Microsoft.Scheduler/jobCollections/jobs",
   "name": "southeastasiajc/httpjob",
   "properties": {
      "startTime": "2015-05-14T14:10:00Z",
      "action": {  
         "request": {
            "uri": "https://mywebserviceendpoint.com",
            "method": "GET",
            "headers": {  
               "x-ms-version": "2013-03-01"
            },
            "authentication": {  
               "tenant": "microsoft.onmicrosoft.com",
               "audience": "https://management.core.windows.net/",
               "clientId": "dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type": "ActiveDirectoryOAuth"
            }
         },
         "type": "Http"
      },
      "recurrence": {  
         "frequency": "minute",
         "endTime": "2016-04-10T08:00:00Z",
         "interval": 1
      },
      "state": "Enabled",
      "status": {  
         "lastExecutionTime": "2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime": "2016-03-16T19:11:00Z",
         "executionCount": 5,
         "failureCount": 5,
         "faultedCount": 1
      }
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)
* [Azure Scheduler REST API başvurusu](/rest/api/scheduler)
* [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)