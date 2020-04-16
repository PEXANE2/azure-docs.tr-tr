---
title: Azure Veri Fabrikası'nda Webhook etkinliği
description: Webhook etkinliği, kullanıcının belirlediği belirli ölçütlerle eklenen veri kümesini doğrulayana kadar ardışık hatlar yürütmeye devam etmez.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/25/2019
ms.openlocfilehash: 4056550ae0a71138d136878fc7e3aa5f6f8f4180
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417887"
---
# <a name="webhook-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Webhook etkinliği

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bir webhook etkinliği, özel kodunuz aracılığıyla boru hatlarının yürütülmesini denetleyebilir. Webhook etkinliği ile, müşterilerin kodu bir bitiş noktası arayabilir ve bir geri arama URL'si geçirebilirsiniz. Ardışık hat lar, bir sonraki faaliyete geçmeden önce geri arama çağrısını bekler.

## <a name="syntax"></a>Sözdizimi

```json

{
    "name": "MyWebHookActivity",
    "type": "WebHook",
    "typeProperties": {
        "method": "POST",
        "url": "<URLEndpoint>",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": {
            "key": "value"
        },
        "timeout": "00:03:00",
        "authentication": {
            "type": "ClientCertificate",
            "pfx": "****",
            "password": "****"
        }
    }
}

```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
**Adı** | Webhook etkinliğinin adı. | Dize | Evet |
**Türü** | "WebHook" olarak ayarlanmalıdır. | Dize | Evet |
**Yöntem** | Hedef bitiş noktası için REST API yöntemi. | Dize. Desteklenen tür "POST"dur. | Evet |
**Url** | Hedef bitiş noktası ve yol. | Bir dize veya bir dize **sonucuTürü** değeri ile bir ifade. | Evet |
**Üstbilgi** | İsteğe gönderilen üstbilgi. Aşağıda, bir istekte dili ve türü belirleyen `"headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }`bir örnek verilmiştir: . | Bir dize veya bir dize **sonucuTürü** değeri ile bir ifade. | Evet. Gibi `Content-Type` `"headers":{ "Content-Type":"application/json"}` bir üstbilgi gereklidir. |
**Vücut** | Bitiş noktasına gönderilen yükü temsil eder. | Geçerli JSON veya **Sonuç Türü** Değeri JSON olan bir ifade. Bkz. İstek yükünün şeması için yük [skeci isteyin.](https://docs.microsoft.com/azure/data-factory/control-flow-web-activity#request-payload-schema) | Evet |
**Kimlik doğrulama** | Bitiş noktasını aramak için kullanılan kimlik doğrulama yöntemi. Desteklenen türler "Temel" ve "Müşteri Sertifikası"dır. Daha fazla bilgi için bkz. [Kimlik doğrulaması](https://docs.microsoft.com/azure/data-factory/control-flow-web-activity#authentication). Kimlik doğrulaması gerekli değilse, bu özelliği hariç tolun. | Bir dize veya bir dize **sonucuTürü** değeri ile bir ifade. | Hayır |
**timeout** | Etkinliğin **callBackUri** tarafından çağrılan geri aramaiçin ne kadar beklemesi. Varsayılan değer 10 dakikadır ("00:10:00"). Değerler TimeSpan biçimi *d*var. *hh*:*mm*:*ss*. | Dize | Hayır |
**Geri arama da rapor durumu** | Kullanıcının webhook etkinliğinin başarısız durumunu bildirmesini sağlar. | Boole | Hayır |

## <a name="authentication"></a>Kimlik Doğrulaması

Webhook etkinliği aşağıdaki kimlik doğrulama türlerini destekler.

### <a name="none"></a>Hiçbiri

Kimlik doğrulama gerekli değilse, kimlik doğrulama özelliğini **eklemeyin.**

### <a name="basic"></a>Temel

Temel kimlik doğrulamayla kullanılacak kullanıcı adını ve parolayı belirtin.

```json
"authentication":{
   "type":"Basic",
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>İstemci sertifikası

PFX dosyasının ve parolanın Base64 kodlanmış içeriğini belirtin.

```json
"authentication":{
   "type":"ClientCertificate",
   "pfx":"****",
   "password":"****"
}
```

### <a name="managed-identity"></a>Yönetilen kimlik

Erişim belirteci istenen kaynak URI belirtmek için veri fabrikasının yönetilen kimliğini kullanın. Azure Kaynak Yönetimi API'sini `https://management.azure.com/`aramak için . Yönetilen kimliklerin nasıl çalıştığı hakkında daha fazla bilgi [için Azure kaynaklarına genel bakış için yönetilen kimliklere](/azure/active-directory/managed-identities-azure-resources/overview)bakın.

```json
"authentication": {
    "type": "MSI",
    "resource": "https://management.azure.com/"
}
```

> [!NOTE]
> Veri fabrikanız bir Git deposuyla yapılandırıldıysa, temel veya istemci sertifikası kimlik doğrulamasını kullanmak için kimlik bilgilerinizi Azure Anahtar Kasası'nda depolamanız gerekir. Azure Veri Fabrikası parolaları Git'de depolamaz.

## <a name="additional-notes"></a>Ek notlar

Veri Fabrikası, URL bitiş noktasına gönderilen gövdedeki ek özellik **callBackUri'yi** geçer. Veri Fabrikası, bu URI'nin belirtilen zaman ödeme değerinden önce çağrılmasını bekler. URI çağrılmazsa, etkinlik "TimedOut" durumuyla başarısız olur.

Özel bitiş noktasına çağrı başarısız olduğunda webhook etkinliği başarısız olur. Herhangi bir hata iletisi geri arama gövdesine eklenebilir ve daha sonraki bir etkinlikte kullanılabilir.

Her REST API çağrısı için, bitiş noktası bir dakika içinde yanıt vermezse istemci zaman ları dışarı. Bu davranış standart HTTP en iyi uygulamadır. Bu sorunu gidermek için 202 deseni uygulayın. Geçerli durumda, bitiş noktası 202 (Kabul) ve istemci anketleri döndürür.

İstekteki bir dakikalık zaman ayarı, etkinlik zaman ayarı ile ilgili değildir. İkincisi **callbackUri**tarafından belirtilen geri arama beklemek için kullanılır.

Geri arama URI'ye geri geçen ceset geçerli Bir JSON olmalıdır. Üstbilgiyi `Content-Type` `application/json`.

Geri arama **özelliğinde Rapor durumunu** kullandığınızda, geri aramayı yaparken gövdeye aşağıdaki kodu eklemeniz gerekir:

```json
{
    "Output": {
        // output object is used in activity output
        "testProp": "testPropValue"
    },
    "Error": {
        // Optional, set it when you want to fail the activity
        "ErrorCode": "testErrorCode",
        "Message": "error message to show in activity error"
    },
    "StatusCode": "403" // when status code is >=400, activity is marked as failed
}
```

## <a name="next-steps"></a>Sonraki adımlar

Veri Fabrikası tarafından desteklenen aşağıdaki kontrol akışı etkinliklerine bakın:

- [If Condition Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Çalıştırma Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her Bir Etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web Etkinliği](control-flow-web-activity.md)
- [Until Etkinliği](control-flow-until-activity.md)
