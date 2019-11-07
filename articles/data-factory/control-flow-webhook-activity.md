---
title: Azure Data Factory Web kancası etkinliği
description: Web kancası etkinliği, eklenen veri kümesini kullanıcının belirttiği ölçütlere göre doğrulayıp işlem hattının yürütülmesine devam etmez.
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
ms.openlocfilehash: 3734472d9026d4e355c08b36d5ba58974288daac
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73678220"
---
# <a name="webhook-activity-in-azure-data-factory"></a>Azure Data Factory Web kancası etkinliği
Özel kodunuzla işlem hattı yürütülmesini denetlemek için bir Web kancası etkinliği kullanabilirsiniz. Müşteriler, Web kancası etkinliğini kullanarak bir uç nokta çağırabilir ve geri çağırma URL 'SI geçirebilir. İşlem hattı çalıştırması, bir sonraki etkinliğe devam etmeden önce geri aramanın çağrılmasını bekler.

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
ad | Web kancası etkinliğinin adı | Dize | Evet |
type | **Web kancası**olarak ayarlanmalıdır. | Dize | Evet |
method | Hedef uç nokta için REST API yöntemi. | dizisinde. Desteklenen türler: ' POST ' | Evet |
url | Hedef uç nokta ve yol | Dize (veya dize resultType 'ı olan ifade). | Evet |
Bilgisinde | İsteğe gönderilen üst bilgiler. Örneğin, bir istek için dili ve türü ayarlamak için: "üstbilgiler": {"Accept-Language": "en-US", "Content-Type": "Application/JSON"}. | Dize (veya dize resultType 'ı olan ifade) | Evet, Content-Type üst bilgisi gereklidir. "üstbilgiler": {"Content-Type": "Application/JSON"} |
bölümü | Uç noktaya gönderilen yükü temsil eder. | Geri çağrı URI 'sine geri geçirilen gövde geçerli bir JSON olmalıdır. İstek [yükü şeması](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23request-payload-schema&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=ljUZv5csQQux2TT3JtTU9ZU8e1uViRzuX5DSNYkL0uE%3D&amp;reserved=0) 'nda istek yükü şeması bölümüne bakın. | Evet |
kimlik doğrulaması | Uç noktayı çağırmak için kullanılan kimlik doğrulama yöntemi. Desteklenen türler "temel" veya "ClientCertificate." Daha fazla bilgi için bkz. [kimlik doğrulama](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23authentication&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=GdA1%2Fh2pAD%2BSyWJHSW%2BSKucqoAXux%2F4L5Jgndd3YziM%3D&amp;reserved=0) bölümü. Kimlik doğrulaması gerekmiyorsa, bu özelliği dışlayın. | Dize (veya dize resultType 'ı olan ifade) | Hayır |
timeout | Etkinliğin &#39;callbackurı&#39; 'nin çağrılması için bekleyeceği süre. Etkinliğin ' callBackUri ' 'nin çağrılması ne kadar bekleyecektir. Varsayılan değer 10 dakikadır ("00:10:00"). Biçim zaman aralığı, d. hh: mm: ss | Dize | Hayır |

## <a name="additional-notes"></a>Ek notlar

Azure Data Factory, gövdedeki "callBackUri" adlı ek bir özelliği URL uç noktasına geçirecek ve bu URI 'nin zaman aşımı değeri belirtilenden önce çağrılmasını beklecektir. URI çağrılamaz, etkinlik ' Timeerme ' durumuyla başarısız olur.

Web kancası etkinliğinin kendisi yalnızca özel uç noktaya yapılan çağrı başarısız olduğunda başarısız olur. Herhangi bir hata iletisi geri çağırma gövdesine eklenebilir ve sonraki bir etkinlikte kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinliklerini görün:

- [If Koşulu Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
- [Bitiş Etkinliği](control-flow-until-activity.md)
