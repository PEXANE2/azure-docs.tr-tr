---
title: Özel durum işleme & hata günlüğe kaydetme senaryosu
description: Azure Logic Apps'ta gelişmiş özel durum işleme ve hata günlüğü için gerçek kullanım durumu ve senaryosu
services: logic-apps
ms.suite: integration
author: hedidin
ms.reviewer: klam, estfan, logicappspm
ms.topic: article
ms.date: 07/29/2016
ms.openlocfilehash: 1bb6e28c9dcae01f3233178706d2a24156fa509a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76902692"
---
# <a name="scenario-exception-handling-and-error-logging-for-logic-apps"></a>Senaryo: Mantık uygulamaları için özel durum işleme ve hata günlüğü

Bu senaryo, özel durum işlemeyi daha iyi desteklemek için bir mantık uygulamasını nasıl genişletebileceğinizi açıklar. "Azure Logic Apps özel durum ve hata işlemeyi destekliyor mu?" sorusunu yanıtlamak için gerçek kullanım örneği kullandık.

> [!NOTE]
> Geçerli Azure Mantık Uygulamaları şeması, eylem yanıtları için standart bir şablon sağlar. Bu şablon, bir API uygulamasından döndürülen hem dahili doğrulama hem de hata yanıtlarını içerir.

## <a name="scenario-and-use-case-overview"></a>Senaryo ve kullanım örneği genel bakış

Bu senaryoiçin kullanım durumu olarak hikaye aşağıda verilmiştir: 

Tanınmış bir sağlık kuruluşu, Microsoft Dynamics CRM Online'ı kullanarak bir hasta portalı oluşturacak bir Azure çözümü geliştirmemiz için bizi görevlendirdi. Dynamics CRM Online hasta portalı ve Salesforce arasında randevu kayıtları göndermeleri gerekiyordu. Hl7 [FHIR](https://www.hl7.org/implement/standards/fhir/) standardını tüm hasta kayıtları için kullanmamız istendi.

Projenin iki temel gereksinimi vardı:  

* Dynamics CRM Online portalından gönderilen kayıtları günlüğe kaydetmek için bir yöntem
* İş akışı içinde oluşan hataları görüntülemenin bir yolu

> [!TIP]
> Bu proje yle ilgili üst düzey bir video [için, Tümleştirme Kullanıcı Grubu'na](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "Entegrasyon Kullanıcı Grubu")bakın.

## <a name="how-we-solved-the-problem"></a>Sorunu nasıl çözdük

Günlük ve hata kayıtları için bir depo olarak [Azure Cosmos DB'yi](https://azure.microsoft.com/services/cosmos-db/ "Azure Cosmos DB") seçtik (Cosmos DB kayıtları belge olarak ifade eder). Azure Logic Apps'ın tüm yanıtlar için standart bir şablonu olduğundan, özel bir şema oluşturmamız gerekmez. Hem hata hem de günlük kayıtları için **Ekleme** ve **Sorgu** için bir API uygulaması oluşturabiliriz. Ayrıca API uygulaması içinde her biri için bir şema tanımlayabiliriz.  

Bir diğer gereklilik de, belirli bir tarihten sonra kayıtları temizlemekti. Cosmos DB, her kayıt veya koleksiyon için Bir Time **to Live** değeri ayarlamamıza olanak sağlayan [Time to Live](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Yaşama Zamanı") (TTL) adlı bir özelliğe sahiptir. Bu özellik, Cosmos DB'deki kayıtları niçin el ile silme gereğini ortadan kaldırmamadadır.

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için bir Cosmos DB veritabanı ve iki koleksiyon (Günlük ve Hatalar) oluşturmanız gerekir.

## <a name="create-the-logic-app"></a>Mantıksal uygulamayı oluşturma

İlk adım mantık uygulaması oluşturmak ve Mantık App Designer uygulama açmaktır. Bu örnekte, üst-alt mantık uygulamaları kullanıyoruz. Ebeveyni zaten oluşturduğumuz ve bir alt mantık uygulaması oluşturacağız varsayalım.

Dynamics CRM Online'dan gelen kaydı kaydedeceğiz, en baştan başlayalım. Ana mantık uygulaması bu çocuğu tetiklediği için Bir **İstek** tetikleyicisi kullanmalıyız.

### <a name="logic-app-trigger"></a>Mantık uygulaması tetikleyicisi

Aşağıdaki örnekte gösterildiği gibi bir **İstek** tetikleyicisi kullanıyoruz:

``` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

```


## <a name="steps"></a>Adımlar

Dynamics CRM Online portalından hasta kaydının kaynağını (isteği) kaydetmemiz gerekir.

1. Dynamics CRM Online'dan yeni bir randevu kaydı almalıyız.

   CRM gelen tetik **CRM PatentId,** **kayıt türü,** **Yeni veya Güncellenmiş Kayıt** (yeni veya güncelleme Boolean değeri) ve **SalesforceId**ile bize sağlar. **SalesforceId** yalnızca bir güncelleştirme için kullanıldığından geçersiz olabilir.
   CRM **PatientID** ve **Kayıt Türü'ni**kullanarak CRM kaydını alıyoruz.

2. Daha sonra, Logic App Designer'da gösterildiği gibi Azure Cosmos DB SQL API uygulamamızın **InsertLogEntry** işlemini eklememiz gerekir.

   **Giriş girişi ekleme**

   ![Giriş Girişi Ekle](media/logic-apps-scenario-error-and-exception-handling/lognewpatient.png)

   **Hata girişi ekleme**

   ![Giriş Girişi Ekle](media/logic-apps-scenario-error-and-exception-handling/insertlogentry.png)

   **Kayıt hatası oluşturma yı denetleme**

   ![Koşul](media/logic-apps-scenario-error-and-exception-handling/condition.png)

## <a name="logic-app-source-code"></a>Mantık uygulaması kaynak kodu

> [!NOTE]
> Aşağıdaki örnekler yalnızca örneklerdir. Bu öğretici üretimde bulunan bir uygulamaya dayandığından, **Kaynak Düğümün** değeri randevu zamanlamasıile ilgili özellikleri görüntülemeyebilir.> 

### <a name="logging"></a>Günlüğe Kaydetme

Aşağıdaki mantık uygulama kodu örneği, günlüğe kaydetmenin nasıl işleyeceğini gösterir.

#### <a name="log-entry"></a>Giriş girişi

Burada bir günlük girişi eklemek için mantık uygulaması kaynak kodu.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Günlük isteği

İşte API uygulamasına gönderilen günlük isteği iletisi.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}"
        }
    }

```


#### <a name="log-response"></a>Günlük yanıtı

İşte API uygulamasından günlük yanıt mesajı.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "/"0400fc2f-0000-0000-0000-575b3ff00000/"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Şimdi hata işleme adımlarına bakalım.

### <a name="error-handling"></a>Hata işleme

Aşağıdaki mantık uygulama kodu örneği, hata işlemeyi nasıl uygulayabileceğinizi gösterir.

#### <a name="create-error-record"></a>Hata kaydı oluşturma

Burada bir hata kaydı oluşturmak için mantık uygulama kaynak kodudur.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}             
```

#### <a name="insert-error-into-cosmos-db--request"></a>Cosmos DB'ye hata ekleme-- istek

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MasterID_mp__c/":/"/",/"C_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"ONY_ID__c/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-cosmos-db--response"></a>Cosmos DB'ye hata ekleme-- yanıt

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "/"0c00eaac-0000-0000-0000-575b3fdc0000/"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/":/"DO - Degree level is DO/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MterID_mp__c/":/"/",/"Medicis_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"XXXXXXX/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Salesforce hata yanıtı

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="return-the-response-back-to-parent-logic-app"></a>Yanıtı ana mantık uygulamasına geri döndürme

Yanıtı aldıktan sonra yanıtı ana mantık uygulamasına geri geçirebilirsiniz.

#### <a name="return-success-response-to-parent-logic-app"></a>Üst mantık uygulamasına başarı yanıtını döndür

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "    Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-parent-logic-app"></a>Üst mantık uygulamasına hata yanıtı döndür

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="cosmos-db-repository-and-portal"></a>Cosmos DB deposu ve portalı

Çözümümüz [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db)ile özellikler ekledi.

### <a name="error-management-portal"></a>Hata yönetimi portalı

Hataları görüntülemek için Cosmos DB'deki hata kayıtlarını görüntülemek için bir MVC web uygulaması oluşturabilirsiniz. **Liste,** **Ayrıntılar,** **Düzenle**ve **Sil** işlemleri geçerli sürümde yer almaktadır.

> [!NOTE]
> İşlemi edin: Cosmos DB belgenin tamamının yerini alır. **Liste** ve **Ayrıntı** görünümlerinde gösterilen kayıtlar yalnızca örneklerdir. Bunlar gerçek hasta randevu kayıtları değildir.

Aşağıda, daha önce açıklanan yaklaşımla oluşturulan MVC uygulama ayrıntılarından örnekler verilmiştir.

#### <a name="error-management-list"></a>Hata yönetimi listesi
![Hata Listesi](media/logic-apps-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Hata yönetimi ayrıntı görünümü
![Hata Ayrıntıları](media/logic-apps-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Günlük yönetim portalı

Günlükleri görüntülemek için bir MVC web uygulaması da oluşturduk. Aşağıda, daha önce açıklanan yaklaşımla oluşturulan MVC uygulama ayrıntılarından örnekler verilmiştir.

#### <a name="sample-log-detail-view"></a>Örnek günlük ayrıntı görünümü
![Günlük Detay Görünümü](media/logic-apps-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>API uygulama ayrıntıları

#### <a name="logic-apps-exception-management-api"></a>Logic Apps özel durum yönetimi API

Açık kaynak Azure Logic Apps özel durum yönetimi API uygulamamız burada açıklandığı gibi işlevsellik sağlar - iki denetleyici vardır:

* **ErrorController,** Azure Cosmos DB koleksiyonuna bir hata kaydı (belge) ekler.
* **LogController** Azure Cosmos DB koleksiyonuna günlük kaydı (belge) ekler.

> [!TIP]
> Her iki `async Task<dynamic>` denetleyici de işlemleri kullanarak operasyonların çalışma zamanında çözülmesini sağlar, böylece işlemin gövdesinde Azure Cosmos DB şeması oluşturabiliriz. 
> 

Azure Cosmos DB'deki her belgenin benzersiz bir kimliği olmalıdır. Unix `PatientId` zaman damgası değerine (çift) dönüştürülen bir zaman damgası kullanıyor ve ekliyoruz. Kesirli değeri kaldırmak için değeri kesiyoruz.

Hata denetleyiciapi'mizin kaynak kodunu [GitHub'dan](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/LogicAppsExceptionManagementApi/Controllers/LogController.cs)görüntüleyebilirsiniz.

Bir mantık uygulamasından API'yi aşağıdaki sözdizimini kullanarak çağırırız:

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Önceki kod örneğindeki **ifade, Başarısız'ın** *Create_NewPatientRecord* durumunu denetler.

## <a name="summary"></a>Özet

* Bir mantık uygulamasında günlük ve hata işlemeyi kolayca uygulayabilirsiniz.
* Azure Cosmos DB'yi günlük ve hata kayıtları (belgeler) için depo olarak kullanabilirsiniz.
* Günlük ve hata kayıtlarını görüntülemek için bir portal oluşturmak için MVC'yi kullanabilirsiniz.

### <a name="source-code"></a>Kaynak kod

Logic Apps özel durum yönetimi API uygulamasının kaynak kodu bu [GitHub deposunda](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Mantık Uygulaması Özel Durum Yönetimi API")mevcuttur.

## <a name="next-steps"></a>Sonraki adımlar

* [Daha fazla mantık uygulaması örneklerini ve senaryoları görüntüleyin](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Mantıksal uygulamaları izleme](../logic-apps/monitor-logic-apps.md)
* [Mantıksal uygulama dağıtımı otomatikleştirme](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)
