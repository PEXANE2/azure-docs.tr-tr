---
title: Alanlara JobConfig.jsAzure Stream Analytics
description: Bu makalede Visual Studio Code iş oluşturmak için kullanılan dosya JobConfig.jsAzure Stream Analytics için desteklenen alanlar listelenmektedir.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 02/14/2020
ms.openlocfilehash: 0676b987725a33049d9da3256bdd4e6dc8028d00
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045187"
---
# <a name="azure-stream-analytics-jobconfigjson-fields"></a>Alanlara JobConfig.jsAzure Stream Analytics

Aşağıdaki alanlar, [Visual Studio Code kullanarak Azure Stream Analytics iş oluşturmak](quick-create-vs-code.md)için kullanılan dosya *JobConfig.js* desteklenir.

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

|Name|Tür|Gerekli|Değer|
|----|----|--------|-----|
|DataLocale|dize|No|Stream Analytics işinin veri yerel ayarı. Değer, desteklenen bir ad olmalıdır. Belirtilmemişse, varsayılan olarak ' en-US ' olur.|
|OutputErrorPolicy|dize|No|Çıkışa ulaşan olaylara uygulanacak ilkeyi belirtir ve hatalı biçimlendirilmiş (eksik sütun değerleri, yanlış tür veya boyutun sütun değerleri), dış depolamaya yazılamaz. -Durdur veya bırak|
|Eventslatevarvalmaxdelayınseconds|integer|Hayır|Olayların geç olarak dahil edildiği sürenin saniye cinsinden en fazla toleransable gecikme süresi. Desteklenen Aralık-1 ile 1814399 arasında (20.23:59:59 gün) ve sonsuza kadar beklemeyi belirtmek için-1 kullanılır. Özelliği yoksa,-1 değeri olacak şekilde yorumlanır.|
|Eventsoutofordermaxdelayınseconds|integer|Hayır|Sıra dışı olayların doğru şekilde ayarlanabilebileceği, saniye cinsinden en fazla toleransız gecikme.|
|EventsOutOfOrderPolicy|dize|No|Giriş olay akışında sıra dışı gelen olaylara uygulanacak ilkeyi gösterir. -Ayarla veya bırak|
|Streammingunits|integer|Evet|Akış işinin kullandığı akış birimi sayısını belirtir.|
|CompatibilityLevel|dize|No|Akış işinin belirli çalışma zamanı davranışlarını denetler. -Kabul edilebilir değerler şunlardır "1,0", "1,1", "1,2"|
|Usesystemassignedıdentity|boole|Hayır|Bu işin, yönetilen bir Azure Active Directory kimliği kullanarak diğer Azure hizmetleriyle kendi kendine iletişim kurmasını sağlamak için true olarak ayarlayın.|
|GlobalStorage. AccountName|dize|No|Genel depolama hesabı, SQL başvuru verileri anlık görüntüleri gibi Stream Analytics işiniz ile ilgili içeriği depolamak için kullanılır.|
|GlobalStorage. AccountKey|dize|No|Genel depolama hesabı için karşılık gelen anahtar.|
|DataSourceCredentialDomain|dize|No|Kimlik bilgisi yerel depolaması için ayrılmış özellik.|
|ScriptType|string|Evet|Bu kaynak dosyanın türünün belirtildiği ayrılmış özellik. JobConfig.jsiçin kabul edilebilir değer "JobConfig" dir.|
|Etiketler|JSON anahtar-değer çiftleri|Hayır|Etiketler, birden fazla kaynağa ve kaynak grubuna aynı etiketi uygulayarak kaynakları kategorilere ayırmanıza ve birleştirilmiş faturalandırmayı görüntülemenize olanak tanıyan ad/değer çiftleridir. Etiket adları büyük/küçük harfe duyarsızdır ve etiket değerleri büyük/küçük harfe duyarlıdır.|

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code Azure Stream Analytics iş oluşturma](quick-create-vs-code.md)
* [Visual Studio Code kullanarak örnek verilerle yerel olarak Stream Analytics sorguları test edin](visual-studio-code-local-run.md)
* [Visual Studio Code kullanarak canlı akış girişine göre Stream Analytics sorguları yerel olarak test](visual-studio-code-local-run-live-input.md) 
* edin [CI/CD NPM paketini kullanarak bir Azure Stream Analytics Işi dağıtma](setup-cicd-vs-code.md)