---
title: Azure Akışı Analytics JobConfig.json alanları
description: Bu makalede, Visual Studio Code'ta iş oluşturmak için kullanılan Azure Akışı Analytics JobConfig.json dosyası için desteklenen alanlar listelenir.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 27ed553035ce9d7abf57ffe93078df9c17b8408c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617963"
---
# <a name="azure-stream-analytics-jobconfigjson-fields"></a>Azure Akışı Analytics JobConfig.json alanları

Aşağıdaki alanlar Visual [Studio Code kullanarak bir Azure Akışı Analytics iş oluşturmak](quick-create-vs-code.md)için kullanılan *JobConfig.json* dosyasında desteklenir.

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
|DataLocale|string|Hayır|Akış analizi işinin veri yerel durumu. Değer desteklenen bir adı olmalıdır. Hiçbiri belirtilmemişse varsayılan olarak 'en-US' olarak belirtilir.|
|OutputErrorPolicy|string|Hayır|Çıktıya gelen ve hatalı biçimlendirilmiş (eksik sütun değerleri, yanlış tür veya boyuttaki sütun değerleri) nedeniyle dış depolamaya yazılamayacak olaylara uygulanacak ilkeyi gösterir. - Dur veya Bırak|
|EventsLateArrivalMaxDelayinSeconds|integer|Hayır|Geç gelen olayların dahil edilebildiği saniyecinsinden maksimum tolere edilebilir gecikme. Desteklenen aralık -1 ila 1814399 (20.23:59:59 gün) ve -1 süresiz bekleyin belirtmek için kullanılır. Özellik yoksa, -1 değerine sahip olduğu yorumlanır.|
|OlaylarOutOfOrderMaxDelayinSeconds|integer|Hayır|Sıra dışı olayların sırayla geri alınabilmesi için ayarlanabilen saniye cinsinden maksimum tolere edilebilir gecikme.|
|OlaylarOutOfOrderPolitikası|string|Hayır|Giriş olay akışında sıra dışı gelen olaylara uygulanacak ilkeyi gösterir. - Ayarla veya Bırak|
|Akış Birimleri|integer|Evet|Akış işinin kullandığı akış birimi sayısını belirtir.|
|Compatibilitylevel|string|Hayır|Akış işinin belirli çalışma zamanı davranışlarını denetler. - Kabul edilebilir değerler "1.0", "1.1", "1.2"|
|UseSystemAssignedIdentity|boole|Hayır|Yönetilen Azure Etkin Dizin Kimliği'ni kullanarak bu işin diğer Azure hizmetleriyle kendisi gibi iletişim kurmasını sağlamak için doğru ayarlayın.|
|GlobalStorage.AccountName|string|Hayır|Genel depolama hesabı, SQL başvuru verileri anlık görüntüleri gibi akış analizi işinizle ilgili içeriği depolamak için kullanılır.|
|GlobalStorage.AccountKey|string|Hayır|Genel depolama hesabı için karşılık gelen anahtar.|
|DataSourceCredentialDomain|string|Hayır|Kimlik bilgisi yerel depolama için Ayrılmış Özellik.|
|Scripttype|string|Evet|Bu kaynak dosyanın türünü gösteren ayrılmış özellik. Kabul edilebilir değer JobConfig.json için "JobConfig"dur.|
|Etiketler|JSON anahtar değeri çiftleri|Hayır|Etiketler, aynı etiketi birden çok kaynak ve kaynak grubuna uygulayarak kaynakları kategorilere ayırmanızı ve birleştirilmiş faturalandırmayı görüntülemenizi sağlayan ad/değer çiftleridir. Etiket adları büyük/küçük harf duyarsızve etiket değerleri büyük/küçük harf duyarlıdır.|

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code'ta Azure Akışı Analizi işi oluşturun](quick-create-vs-code.md)
* [Visual Studio Code kullanarak örnek verilerle test Akışı Analizi sorguları](visual-studio-code-local-run.md)
* [Visual Studio Code](visual-studio-code-local-run-live-input.md)
*Kullanarak Canlı Akış girişine karşı test Akışı Analizi SORGULARI[CI/CD npm paketini kullanarak bir Azure Akışı Analizi işini dağıtın](setup-cicd-vs-code.md)