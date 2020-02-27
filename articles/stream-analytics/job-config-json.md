---
title: Azure Stream Analytics JobConfig. JSON alanları
description: Bu makalede, Visual Studio Code işleri oluşturmak için kullanılan Azure Stream Analytics JobConfig. JSON dosyası için desteklenen alanlar listelenmektedir.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 27ed553035ce9d7abf57ffe93078df9c17b8408c
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77617963"
---
# <a name="azure-stream-analytics-jobconfigjson-fields"></a>Azure Stream Analytics JobConfig. JSON alanları

[Visual Studio Code kullanılarak Azure Stream Analytics işi oluşturmak](quick-create-vs-code.md)Için kullanılan *jobconfig. JSON* dosyasında aşağıdaki alanlar desteklenir.

```json
{
    "DataLocale": "string",
    "OutputErrorPolicy": "string",
    "EventsLateArrivalMaxDelayInSeconds": "integer",
    "EventsOutOfOrderMaxDelayInSeconds": "integer",
    "EventsOutOfOrderPolicy": "string",
    "StreamingUnits": "integer",
    "CompatibilityLevel": "string",
    "UseSystemAssignedIdentity": "boolean",
    "GlobalStorage": {
        "AccountName": "string",
        "AccountKey": "string",
    },
    "DataSourceCredentialDomain": "string",
    "ScriptType": "string",
    "Tags": {}
}
```

|Adı|Tür|Gerekli|Değer|
|----|----|--------|-----|
|DataLocale|string|Hayır|Stream Analytics işinin veri yerel ayarı. Değer, desteklenen bir ad olmalıdır. Belirtilmemişse, varsayılan olarak ' en-US ' olur.|
|OutputErrorPolicy|string|Hayır|Çıkışa ulaşan olaylara uygulanacak ilkeyi belirtir ve hatalı biçimlendirilmiş (eksik sütun değerleri, yanlış tür veya boyutun sütun değerleri), dış depolamaya yazılamaz. -Durdur veya bırak|
|Eventslatevarvalmaxdelayınseconds|integer|Hayır|Olayların geç olarak dahil edildiği sürenin saniye cinsinden en fazla toleransable gecikme süresi. Desteklenen Aralık-1 ile 1814399 arasında (20.23:59:59 gün) ve sonsuza kadar beklemeyi belirtmek için-1 kullanılır. Özelliği yoksa,-1 değeri olacak şekilde yorumlanır.|
|Eventsoutofordermaxdelayınseconds|integer|Hayır|Sıra dışı olayların doğru şekilde ayarlanabilebileceği, saniye cinsinden en fazla toleransız gecikme.|
|EventsOutOfOrderPolicy|string|Hayır|Giriş olay akışında sıra dışı gelen olaylara uygulanacak ilkeyi gösterir. -Ayarla veya bırak|
|Streammingunits|integer|Yes|Akış işinin kullandığı akış birimi sayısını belirtir.|
|CompatibilityLevel|string|Hayır|Akış işinin belirli çalışma zamanı davranışlarını denetler. -Kabul edilebilir değerler şunlardır "1,0", "1,1", "1,2"|
|Usesystemassignedıdentity|boole|Hayır|Bu işin, yönetilen bir Azure Active Directory kimliği kullanarak diğer Azure hizmetleriyle kendi kendine iletişim kurmasını sağlamak için true olarak ayarlayın.|
|GlobalStorage. AccountName|string|Hayır|Genel depolama hesabı, SQL başvuru verileri anlık görüntüleri gibi Stream Analytics işiniz ile ilgili içeriği depolamak için kullanılır.|
|GlobalStorage. AccountKey|string|Hayır|Genel depolama hesabı için karşılık gelen anahtar.|
|DataSourceCredentialDomain|string|Hayır|Kimlik bilgisi yerel depolaması için ayrılmış özellik.|
|ScriptType|string|Yes|Bu kaynak dosyanın türünün belirtildiği ayrılmış özellik. JobConfig. JSON için kabul edilebilir değer "JobConfig".|
|Etiketler|JSON anahtar-değer çiftleri|Hayır|Etiketler, birden fazla kaynağa ve kaynak grubuna aynı etiketi uygulayarak kaynakları kategorilere ayırmanıza ve birleştirilmiş faturalandırmayı görüntülemenize olanak tanıyan ad/değer çiftleridir. Etiket adları büyük/küçük harfe duyarsızdır ve etiket değerleri büyük/küçük harfe duyarlıdır.|

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code Azure Stream Analytics iş oluşturma](quick-create-vs-code.md)
* [Visual Studio Code kullanarak örnek verilerle yerel olarak Stream Analytics sorguları test edin](visual-studio-code-local-run.md)
* [Visual Studio Code kullanarak canlı akış girişine karşı Test Stream Analytics sorguları](visual-studio-code-local-run-live-input.md) , [CI/CD NPM paketini kullanarak bir Azure Stream Analytics işi dağıtma](setup-cicd-vs-code.md)
*