---
title: Azure Işlevi etkinliği Azure Data Factory | Microsoft Docs
description: Azure işlevi etkinliğini kullanarak bir Azure Işlevini Data Factory işlem hattında çalıştırma hakkında bilgi edinin
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
ms.openlocfilehash: 292fe858b85faef69b9df2dbdf54e7061ed56fa2
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70142499"
---
# <a name="azure-function-activity-in-azure-data-factory"></a>Azure Data Factory 'de Azure Işlevi etkinliği

Azure Işlevi etkinliği, [Azure işlevlerini](../azure-functions/functions-overview.md) bir Data Factory işlem hattında çalıştırmanızı sağlar. Bir Azure Işlevi çalıştırmak için, bir bağlı hizmet bağlantısı ve çalıştırmayı planladığınız Azure Işlevini belirten bir etkinlik oluşturmanız gerekir.

Bu özelliğin sekiz dakikalık bir girişi ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Run-Azure-Functions-from-Azure-Data-Factory-pipelines/player]

## <a name="azure-function-linked-service"></a>Azure Işlevi bağlı hizmeti

Azure işlevinin dönüş türü geçerli `JObject`bir olmalıdır. ( [Jarray](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_Linq_JArray.htm) öğesinin bir `JObject` *değil* olduğunu aklınızda bulundurun.) Diğer `JObject` herhangi bir dönüş türü başarısız olur ve Kullanıcı hata *yanıtı içeriğini başlatır geçerli bir JObject değildir*.

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| type   | Type özelliği şu şekilde ayarlanmalıdır: **AzureFunction** | evet |
| function app url | Azure İşlev Uygulaması URL 'SI. `https://<accountname>.azurewebsites.net`Biçim. Bu URL, Azure portal İşlev Uygulaması görüntülenirken **URL** bölümündeki değerdir  | evet |
| function key | Azure Işlevi için erişim anahtarı. İlgili işlevin **Yönet** bölümüne tıklayın ve **işlev anahtarını** ya da **ana bilgisayar anahtarını**kopyalayın. Daha fazla bilgi edinin: [Azure Işlevleri HTTP Tetikleyicileri ve bağlamaları](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) | evet |
|   |   |   |

## <a name="azure-function-activity"></a>Azure Işlevi etkinliği

| **Özellik**  | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| name  | İşlem hattındaki etkinliğin adı  | Dize | evet |
| type  | Etkinliğin türü ' AzureFunctionActivity ' | Dize | evet |
| linked service | Karşılık gelen Azure İşlev Uygulaması için Azure Işlevi bağlı hizmeti  | Bağlı hizmet başvurusu | evet |
| function name  | Bu etkinliğin çağırdığı Azure İşlev Uygulaması işlevin adı | Dize | evet |
| yöntemi  | İşlev çağrısı için REST API yöntemi | Dize desteklenen türler: "AL", "POST", "PUT"   | evet |
| header  | İsteğe gönderilen üst bilgiler. Örneğin, bir istek için dili ve türü ayarlamak için: "üstbilgiler": {"Accept-Language": "en-US", "Content-Type": "Application/JSON"} | Dize (veya dize resultType 'ı olan ifade) | Hayır |
| body  | işlevin API metoduna isteğiyle birlikte gönderilen gövde  | Dize (veya dize resultType 'ı olan ifade) veya nesne.   | PUT/POST yöntemleri için gereklidir |
|   |   |   | |

İstek [yükü şeması](control-flow-web-activity.md#request-payload-schema) 'nda istek yükü şeması bölümüne bakın.

## <a name="routing-and-queries"></a>Yönlendirme ve sorgular

Azure Işlevi etkinliği **yönlendirmeyi**destekler. Örneğin, Azure işlevinizin uç noktası `https://functionAPP.azurewebsites.net/api/<functionName>/<value>?code=<secret>` `functionName` varsa, Azure işlevi etkinliğinde `<functionName>/<value>`kullanmak için. Çalışma zamanında istenen `functionName` işlemi sağlamak için bu işlevi parametreleştirebilirsiniz.

Azure Işlevi etkinliği **sorguları**da destekler. Bir sorgu öğesinin `functionName`bir parçası olarak eklenmelidir. `HttpTriggerCSharp` Örneğin, işlev adı ve dahil etmek istediğiniz sorgu ise `name=hello`, bunu olarak `HttpTriggerCSharp?name=hello`Azure işlevi etkinliğinde oluşturabilirsiniz `functionName` . Değerin çalışma zamanında belirlenebilmesi için bu işlev parametreleştirilenebilir.

## <a name="timeout-and-long-running-functions"></a>Zaman aşımı ve uzun süre çalışan işlevler

Ayarlarda yapılandırdığınız `functionTimeout` ayardan bağımsız olarak, 230 saniye sonra Azure işlevleri zaman aşımına uğrar. Daha fazla bilgi için [bu makaleye](../azure-functions/functions-versions.md#timeout) bakın. Bu davranışı geçici olarak çözmek için zaman uyumsuz bir model izleyin veya Dayanıklı İşlevler kullanın. Dayanıklı İşlevler avantajı kendi durum izleme mekanizmasını sunduklarında, kendi uygulamanızı uygulamanız gerekmez.

[Bu makaledeki](../azure-functions/durable/durable-functions-overview.md)dayanıklı işlevler hakkında daha fazla bilgi edinin. [Bu örnek](../azure-functions/durable/durable-functions-http-api.md#http-api-url-discovery)gibi farklı bir URI ile yanıt döndüren dayanıklı işlevi çağırmak Için bir Azure işlevi etkinliği ayarlayabilirsiniz. , `statusQueryGetUri` İşlev çalışırken http durumu 202 ' i döndürdüğünden, bir Web etkinliği kullanarak işlevin durumunu yoklayabilmeniz gerekir. Yalnızca `url` alanı olarak `@activity('<AzureFunctionActivityName>').output.statusQueryGetUri`ayarlanmış bir Web etkinliği ayarlamanız yeterlidir. Dayanıklı Işlev tamamlandığında, işlevin çıktısı Web etkinliğinin çıktısı olur.


## <a name="sample"></a>Örnek

Bir tar dosyasının içeriğini [buraya](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV2/UntarAzureFilesWithAzureFunction)ayıklamak Için Azure işlevi kullanan bir Data Factory örneğini bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Data Factory işlem [hatları ve Azure Data Factory etkinliklerinde](concepts-pipelines-activities.md)etkinlikler hakkında daha fazla bilgi edinin.
