---
title: Azure Veri Fabrikasında Web Etkinliği
description: Veri Fabrikası tarafından desteklenen denetim akışı etkinliklerinden biri olan Web Aktivitesi'ni, bir ardışık kaynaktan REST bitiş noktasını çağırmak için nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/19/2018
ms.openlocfilehash: c700c9786f3bec4c79cae904a95deb5fd1c670b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260871"
---
# <a name="web-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Web etkinliği
Web Etkinliği bir Data Factory işlem hattından özel bir REST uç noktasını çağırmak için kullanılabilir. Etkinlik tarafından kullanılacak ve erişilecek veri kümelerini ve bağlı hizmetleri geçirebilirsiniz.

> [!NOTE]
> Web Etkinliği yalnızca herkese açık URL'leri arayabilir. Özel bir sanal ağda barındırılan URL'ler için desteklenmez.

## <a name="syntax"></a>Sözdizimi

```json
{
   "name":"MyWebActivity",
   "type":"WebActivity",
   "typeProperties":{
      "method":"Post",
      "url":"<URLEndpoint>",
      "headers":{
         "Content-Type":"application/json"
      },
      "authentication":{
         "type":"ClientCertificate",
         "pfx":"****",
         "password":"****"
      },
      "datasets":[
         {
            "referenceName":"<ConsumedDatasetName>",
            "type":"DatasetReference",
            "parameters":{
               ...
            }
         }
      ],
      "linkedServices":[
         {
            "referenceName":"<ConsumedLinkedServiceName>",
            "type":"LinkedServiceReference"
         }
      ]
   }
}

```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | Web etkinliğinin adı | Dize | Evet
type | **WebActivity**olarak ayarlanmalıdır. | Dize | Evet
method | Hedef bitiş noktası için Dinlenme API yöntemi. | Dize. <br/><br/>Desteklenen Türleri: "GET", "POST", "PUT" | Evet
url | Hedef bitiş noktası ve yol | String (veya string sonuç Türü ile ifade). Etkinlik, bitiş noktasından yanıt almazsa bir hatayla 1 dakika içinde zaman alacaktır. | Evet
Üstbilgi | İsteğe gönderilen üstbilgi. Örneğin, bir istek üzerinde dil ve `"headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }`yazı ayarlamak için: . | String (veya sonuç string Türü ile ifade) | Evet, İçerik türü üstbilgi gereklidir. `"headers":{ "Content-Type":"application/json"}`
body | Bitiş noktasına gönderilen yükü temsil eder.  | String (veya string sonuç Türü ile ifade). <br/><br/>[İstek yük şeması](#request-payload-schema) bölümünde istek yükü şemasına bakın. | POST/PUT yöntemleri için gereklidir.
kimlik doğrulaması | Bitiş noktasını aramak için kullanılan kimlik doğrulama yöntemi. Desteklenen Türler "Temel veya İstemci Sertifikası" dır. Daha fazla bilgi için [Kimlik Doğrulama](#authentication) bölümüne bakın. Kimlik doğrulaması gerekli değilse, bu özelliği hariç tolun. | String (veya sonuç string Türü ile ifade) | Hayır
veri kümeleri | Bitiş noktasına geçen veri kümelerinin listesi. | Veri kümesi başvuruları dizisi. Boş bir dizi olabilir. | Evet
linkedServices | Bitiş noktasına geçen bağlantılı hizmetlerin listesi. | Bağlantılı hizmet başvuruları dizisi. Boş bir dizi olabilir. | Evet

> [!NOTE]
> Web etkinliğinin çağırdığı REST uç noktaları json türünden bir yanıt döndürmelidir. Etkinlik, bitiş noktasından yanıt almazsa bir hatayla 1 dakika içinde zaman alacaktır.

Aşağıdaki tabloJSON içeriği için gereksinimleri gösterir:

| Değer türü | İstek gövdesi | Yanıt gövdesi |
|---|---|---|
|JSON nesnesi | Destekleniyor | Destekleniyor |
|JSON dizisi | Destekleniyor <br/>(Şu anda, JSON dizileri bir hata sonucu çalışmıyor. Düzeltme devam ediyor.) | Desteklenmeyen |
| JSON değeri | Destekleniyor | Desteklenmeyen |
| JSON tipi olmayan | Desteklenmeyen | Desteklenmeyen |
||||

## <a name="authentication"></a>Kimlik doğrulaması

Aşağıda, web etkinliğinde desteklenen kimlik doğrulama türleri verilmiştir.

### <a name="none"></a>None

Kimlik doğrulama gerekli değilse, "kimlik doğrulama" özelliğini içermez.

### <a name="basic"></a>Temel

Temel kimlik doğrulamayla kullanılacak kullanıcı adı ve parolabelirtin.

```json
"authentication":{
   "type":"Basic",
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>İstemci sertifikası

PFX dosyasının ve parolanın base64 kodlanmış içeriğini belirtin.

```json
"authentication":{
   "type":"ClientCertificate",
   "pfx":"****",
   "password":"****"
}
```

### <a name="managed-identity"></a>Yönetilen Kimlik

Veri fabrikası için yönetilen kimliği kullanarak erişim belirteci isteneceği kaynak urisini belirtin. Azure Kaynak Yönetimi API'sini `https://management.azure.com/`aramak için . Yönetilen kimliklerin nasıl çalıştığı hakkında daha fazla bilgi [için Azure kaynaklarına genel bakış sayfasına](/azure/active-directory/managed-identities-azure-resources/overview)bakın.

```json
"authentication": {
    "type": "MSI",
    "resource": "https://management.azure.com/"
}
```

> [!NOTE]
> Veri fabrikanız bir git deposuyla yapılandırıldıysa, temel veya istemci sertifikası kimlik doğrulamasını kullanmak için kimlik bilgilerinizi Azure Anahtar Kasası'nda depolamanız gerekir. Azure Veri Fabrikası parolaları git'de depolamaz.

## <a name="request-payload-schema"></a>Yük şeması isteyin
POST/PUT yöntemini kullandığınızda, gövde özelliği bitiş noktasına gönderilen yükü temsil eder. Yükün bir parçası olarak bağlantılı hizmetleri ve veri kümelerini geçirebilirsiniz. İşte yük için şema:

```json
{
    "body": {
        "myMessage": "Sample",
        "datasets": [{
            "name": "MyDataset1",
            "properties": {
                ...
            }
        }],
        "linkedServices": [{
            "name": "MyStorageLinkedService1",
            "properties": {
                ...
            }
        }]
    }
}
```

## <a name="example"></a>Örnek
Bu örnekte, ardışık ardışık web etkinliği bir REST bitiş noktası çağırır. Bir Azure SQL bağlantılı hizmeti ve bir Azure SQL veri kümesini bitiş noktasına geçirir. REST bitiş noktası, Azure SQL sunucusuna bağlanmak için Azure SQL bağlantı dizesini kullanır ve SQL sunucusu örneğinin adını döndürür.

### <a name="pipeline-definition"></a>Boru hattı tanımı

```json
{
    "name": "<MyWebActivityPipeline>",
    "properties": {
        "activities": [
            {
                "name": "<MyWebActivity>",
                "type": "WebActivity",
                "typeProperties": {
                    "method": "Post",
                    "url": "@pipeline().parameters.url",
                    "headers": {
                        "Content-Type": "application/json"
                    },
                    "authentication": {
                        "type": "ClientCertificate",
                        "pfx": "*****",
                        "password": "*****"
                    },
                    "datasets": [
                        {
                            "referenceName": "MySQLDataset",
                            "type": "DatasetReference",
                            "parameters": {
                                "SqlTableName": "@pipeline().parameters.sqlTableName"
                            }
                        }
                    ],
                    "linkedServices": [
                        {
                            "referenceName": "SqlLinkedService",
                            "type": "LinkedServiceReference"
                        }
                    ]
                }
            }
        ],
        "parameters": {
            "sqlTableName": {
                "type": "String"
            },
            "url": {
                "type": "String"
            }
        }
    }
}

```

### <a name="pipeline-parameter-values"></a>Boru hattı parametre değerleri

```json
{
    "sqlTableName": "department",
    "url": "https://adftes.azurewebsites.net/api/execute/running"
}

```

### <a name="web-service-endpoint-code"></a>Web hizmeti bitiş noktası kodu

```csharp

[HttpPost]
public HttpResponseMessage Execute(JObject payload)
{
    Trace.TraceInformation("Start Execute");

    JObject result = new JObject();
    result.Add("status", "complete");

    JArray datasets = payload.GetValue("datasets") as JArray;
    result.Add("sinktable", datasets[0]["properties"]["typeProperties"]["tableName"].ToString());

    JArray linkedServices = payload.GetValue("linkedServices") as JArray;
    string connString = linkedServices[0]["properties"]["typeProperties"]["connectionString"].ToString();

    System.Data.SqlClient.SqlConnection sqlConn = new System.Data.SqlClient.SqlConnection(connString);

    result.Add("sinkServer", sqlConn.DataSource);

    Trace.TraceInformation("Stop Execute");

    return this.Request.CreateResponse(HttpStatusCode.OK, result);
}

```

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası tarafından desteklenen diğer kontrol akışı etkinliklerine bakın:

- [İşlem Hattı Çalıştırma Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her Bir Etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
