---
title: Azure Stream Analytics özelliği karşılaştırması
description: Bu makalede Azure portal, Visual Studio ve Visual Studio Code Azure Stream Analytics bulut ve IoT Edge işleri için desteklenen özellikler karşılaştırılır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 8cdba27ac949584e1fa96e3f7b0874f4fc0d4212
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75443651"
---
# <a name="azure-stream-analytics-feature-comparison"></a>Azure Stream Analytics özelliği karşılaştırması

Azure Stream Analytics, bulutta ve IoT Edge [Azure Portal](stream-analytics-quick-create-portal.md), [Visual Studio](stream-analytics-quick-create-vs.md)ve [Visual Studio Code](quick-create-vs-code.md)kullanarak akış çözümleri oluşturabilirsiniz. Bu makaledeki tablolarda her iki iş türü için her platform tarafından desteklenen özellikler gösterilir.

## <a name="cloud-job-features"></a>Bulut işi özellikleri


|Özellik  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Platformlar arası     |Mac</br>Linux</br>Windows         |Windows        |Mac</br>Linux</br>Windows          |
|Betik yazma     |Evet         |Evet         |Evet         |
|Betik IntelliSense     |Söz dizimi vurgulama         |Söz dizimi vurgulama</br>Kod tamamlama</br>Hata işaretçisi         |Söz dizimi vurgulama</br>Kod tamamlama</br>Hata işaretçisi         |
|Tüm giriş, çıkış ve iş yapılandırması türlerini tanımlayın     |Evet         |Evet         |Evet         |
|Kaynak denetimi     |Hayır         |Evet         |Evet         |
|CI/CD desteği     |Kısmi         |Evet         |Evet         |
|Birden çok sorgu arasında girişleri ve çıkışları paylaşma     |Hayır         |Evet         |Evet         |
|Örnek dosya ile sorgu testi     |Evet         |Evet        |Evet         |
|Canlı veri yerel testi     |Hayır         |Evet       |Evet      |
|İşleri listeleme ve iş varlıklarını görüntüleme     |Evet         |Evet        |Evet         |
|Yerel bir projeye bir işi dışarı aktarma     |Hayır         |Evet         |Evet         |
|İşleri gönderme, başlatma ve durdurma     |Evet         |Evet         |Evet         |
|İş ölçümlerini ve Diyagramı görüntüleme     |Evet         |Evet         |Portalda açma         |
|İş çalışma zamanı hatalarını görüntüleme     |Evet         |Evet         |Hayır         |
|Tanılama günlükleri     |Evet         |Hayır         |Hayır         |
|Özel ileti özellikleri     |Evet         |Evet         |Hayır       |
|C#özel kod işlevi ve seri hale getirici|Salt okunurdur modu|Evet|Hayır|
|JavaScript UDF ve UDA     |Evet         |Evet         |Yalnızca Windows         |
|Machine Learning Hizmeti     |Evet, ancak sorgu test edilemez        |Evet         |Hayır         |
|Machine Learning Studio     |Evet, ancak sorgu test edilemez        |Evet |Hayır         |
|Uyumluluk düzeyi     |1.0</br>1.1</br>1,2 (varsayılan)         |1.0</br>1.1</br>1,2 (varsayılan)           |1.0</br>1.1</br>1,2 (varsayılan)           |
|Yerleşik ML tabanlı anomali algılama işlevleri     |Evet         |Evet         |Evet         |
|Yerleşik Jeo-uzamsal işlevler     |Evet         |Evet         |Evet         |



## <a name="iot-edge-job-features"></a>IoT Edge iş özellikleri

|Özellik  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|İş yazma     |Evet         |Evet         |Hayır         |
|Kaynak denetimi     |Hayır         |Evet         |Hayır         |
|Yerel bir projeye bir işi dışarı aktarma     |Hayır         |Evet         |Hayır         |
|Örnek dosya ile sorgu testi     |Evet         |Evet         |Hayır         |
|Birden çok sorgu arasında girişleri ve çıkışları paylaşma     |Hayır         |Evet         |Hayır         |
|C#SÜRÜMÜNDE     |Hayır         |Evet         |Hayır         |
|İşleri gönder     |Evet         |Evet         |Hayır         |
|İşleri listeleme ve iş varlıklarını görüntüleme     |Evet         |Evet         |Hayır         |
|İş ölçümlerini ve Diyagramı görüntüleme     |Evet         |Kısmi         |Hayır         |
|İş çalışma zamanı hatalarını görüntüleme     |Evet         |Kısmi         |Hayır         |
|CI/CD desteği     |Hayır         |Hayır         |Hayır         |


## <a name="next-steps"></a>Sonraki adımlar

* [IOT Edge üzerinde Azure Stream Analytics](stream-analytics-edge.md)
* [Öğretici: Azure Stream Analytics IoT Edge C# işi için Kullanıcı tanımlı bir işlev yazma (Önizleme)](stream-analytics-edge-csharp-udf.md)
* [Visual Studio araçlarını kullanarak Stream Analytics IoT Edge işleri geliştirme](stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [Azure Stream Analytics işleri görüntülemek için Visual Studio](stream-analytics-vs-tools.md)
* [Visual Studio Code ile Azure Stream Analytics araştırma (Önizleme)](visual-studio-code-explore-jobs.md)


