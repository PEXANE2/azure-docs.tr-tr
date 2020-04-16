---
title: Azure Veri Fabrikası'nda Azure İşlevi Etkinliği
description: Veri Fabrikası ardışık alanında Bir Azure İşi'ni çalıştırmak için Azure İşlevi etkinliğini nasıl kullanacağınızı öğrenin
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.openlocfilehash: ee2e59e794cf34a8fd5043a56867a81c2537f1ae
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415318"
---
# <a name="azure-function-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Azure İşlevi etkinliği
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]
Azure İşlevi etkinliği, Veri Fabrikası ardışık bir ardışık alanda [Azure İşlevlerini](../azure-functions/functions-overview.md) çalıştırmanızı sağlar. Bir Azure İşlevi çalıştırmak için, bağlantılı bir hizmet bağlantısı ve yürütmeyi planladığınız Azure İşi'ni belirten bir etkinlik oluşturmanız gerekir.

Bu özelliğin sekiz dakikalık tanıtımı ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Run-Azure-Functions-from-Azure-Data-Factory-pipelines/player]

## <a name="azure-function-linked-service"></a>Azure İşlevi'ne bağlı hizmet

Azure işlevinin dönüş türü geçerli `JObject`olmalıdır. [(JArray](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_Linq_JArray.htm) bir `JObject` *olmadığını* unutmayın.) Başarısız ve kullanıcı `JObject` hatası Yanıt İçeriği yükseltir dışında herhangi bir dönüş türü *geçerli bir JObject değildir.*

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| type   | Tür özelliği şu şekilde ayarlanmalıdır: **Azureİş** | evet |
| fonksiyon uygulaması url | Azure İşlevi Uygulaması'nın URL'si. Biçimi `https://<accountname>.azurewebsites.net`. Bu URL, Azure portalında İşlev Uygulamanızı görüntülerken **URL** bölümünün altındaki değerdir  | evet |
| fonksiyon tuşu | Azure İşlevi için erişim anahtarı. İlgili işlev için **Yönet** bölümüne tıklayın ve **İşlev Anahtarı'nı** veya Ana Bilgisayar **anahtarını**kopyalayın. Burada daha fazla bilgi edinin: [Azure İşlevler HTTP tetikleyicileri ve bağlamaları](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) | evet |
|   |   |   |

## <a name="azure-function-activity"></a>Azure İşlevi etkinliği

| **Özellik**  | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| ad  | Boru hattındaki etkinliğin adı  | Dize | evet |
| type  | Etkinlik türü 'AzureİşEtkinliği' | Dize | evet |
| bağlantılı hizmet | İlgili Azure İşlevi Uygulaması için Azure İşlevi bağlantılı hizmet  | Bağlantılı hizmet başvurusu | evet |
| işlev adı  | Bu etkinliğin çağırdığı Azure İşlevi Uygulaması'ndaki işlevin adı | Dize | evet |
| method  | İşlev çağrısı için REST API yöntemi | String Desteklenen Türleri: "GET", "POST", "PUT"   | evet |
| üst bilgi  | İsteğe gönderilen üstbilgi. Örneğin, bir istek üzerine dil ve yazı ayarlamak için: "üstbilgi": { "Accept-Language": "en-us", "İçerik Türü": "uygulama/json" } | String (veya sonuç string Türü ile ifade) | Hayır |
| body  | fonksiyonu api yöntemine istek le birlikte gönderilen gövde  | String (veya sonuç string Türü ile ifade) veya nesne.   | PUT/POST yöntemleri için gerekli |
|   |   |   | |

 [İstek yük şeması](control-flow-web-activity.md#request-payload-schema) bölümünde istek yükü şemasına bakın.

## <a name="routing-and-queries"></a>Yönlendirme ve sorgular

Azure İşlev Etkinliği **yönlendirmeyi**destekler. Örneğin, Azure İşlevinizin bitiş `https://functionAPP.azurewebsites.net/api/<functionName>/<value>?code=<secret>`noktası `functionName` varsa, Azure İşlev `<functionName>/<value>`Etkinliği'nde kullanılacak nokta . Çalışma zamanında istenileni `functionName` sağlamak için bu işlevi parametrenize verebilirsiniz.

Azure İşlev Etkinliği de **sorguları**destekler. Bir sorgunun bir parçası olarak `functionName`eklenmesi gerekir. Örneğin, işlev adı ve `HttpTriggerCSharp` eklemek istediğiniz sorgu olduğunda, `name=hello`Azure İşlev `functionName` Etkinliği'nde "Azure `HttpTriggerCSharp?name=hello`İşlev Etkinliği" olarak oluşturabilirsiniz. Bu işlev parametreli olabilir, böylece değer çalışma zamanında belirlenebilir.

## <a name="timeout-and-long-running-functions"></a>Zaman ve uzun çalışma işlevleri

Azure İşlevler, ayarlarda yapılandırdığınız `functionTimeout` ayara bakılmaksızın 230 saniye sonra zaman ları slenir. Daha fazla bilgi için [bu makaleye](../azure-functions/functions-versions.md#timeout)bakın. Bu davranışı niçin çözüme yönelik olarak, bir eşitleme deseni izleyin veya Dayanıklı İşlevler'i kullanın. Dayanıklı İşlevler'in yararı, kendi durum izleme mekanizmalarını sunmalarıdır, böylece kendi durumunuzu uygulamak zorunda kalmazsınız.

[Bu makalede](../azure-functions/durable/durable-functions-overview.md)Dayanıklı Fonksiyonlar hakkında daha fazla bilgi edinin. [Bu örnek](../azure-functions/durable/durable-functions-http-features.md#http-api-url-discovery)gibi farklı bir URI ile yanıt döndürecek Dayanıklı İşlev'i çağırmak için bir Azure İşlev Etkinliği ayarlayabilirsiniz. `statusQueryGetUri` İşlev çalışırken HTTP Status 202 döndürür olduğundan, bir Web Etkinliği kullanarak işlevin durumunu yoklayabilirsiniz. Alan ayarlı bir Web `url` Etkinliği `@activity('<AzureFunctionActivityName>').output.statusQueryGetUri`ayarlamanız yeterlidir. Dayanıklı İşlev tamamlandığında, işlevin çıktısı Web Etkinliği çıktısı olacaktır.


## <a name="sample"></a>Örnek

Burada katran [dosyasının](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV2/UntarAzureFilesWithAzureFunction)içeriğini ayıklamak için Azure İşlevi kullanan bir Veri Fabrikası'nın bir örneğini bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Veri Fabrikası'ndaki etkinlikler [ve Azure Veri Fabrikası'ndaki etkinlikler](concepts-pipelines-activities.md)hakkında daha fazla bilgi edinin.
