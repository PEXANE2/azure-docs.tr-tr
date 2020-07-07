---
title: Azure Stream Analytics özelliği karşılaştırması
description: Bu makalede Azure portal, Visual Studio ve Visual Studio Code Azure Stream Analytics bulut ve IoT Edge işleri için desteklenen özellikler karşılaştırılır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: b169a067f05de422b714e18a4d7ebb9c9e7f0428
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82133494"
---
# <a name="azure-stream-analytics-feature-comparison"></a>Azure Stream Analytics özelliği karşılaştırması

Azure Stream Analytics, bulutta ve IoT Edge [Azure Portal](stream-analytics-quick-create-portal.md), [Visual Studio](stream-analytics-quick-create-vs.md)ve [Visual Studio Code](quick-create-vs-code.md)kullanarak akış çözümleri oluşturabilirsiniz. Bu makaledeki tablolarda her iki iş türü için her platform tarafından desteklenen özellikler gösterilir.

> [!NOTE]
> Visual Studio ve Visual Studio Code araçları, Çin Doğu, Çin Kuzey, Almanya Orta ve Almanya kuzeydoğu bölgelerinde bulunan işleri desteklemez.

## <a name="cloud-job-features"></a>Bulut işi özellikleri


|Özellik  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Platformlar arası     |Mac</br>Linux</br>Windows         |Windows        |Mac</br>Linux</br>Windows          |
|Betik yazma     |Yes         |Yes         |Yes         |
|Betik IntelliSense     |Söz dizimi vurgulama         |Söz dizimi vurgulama</br>Kod tamamlama</br>Hata işaretçisi         |Söz dizimi vurgulama</br>Kod tamamlama</br>Hata işaretçisi         |
|Tüm giriş, çıkış ve iş yapılandırması türlerini tanımlayın     |Yes         |Yes         |Yes         |
|Kaynak denetimi     |No         |Yes         |Yes         |
|CI/CD desteği     |Kısmi         |Yes         |Yes         |
|Birden çok sorgu arasında girişleri ve çıkışları paylaşma     |No         |Yes         |Yes         |
|Örnek dosya ile sorgu testi     |Yes         |Yes        |Yes         |
|Canlı veri yerel testi     |No         |Yes       |Yes      |
|İşleri listeleme ve iş varlıklarını görüntüleme     |Yes         |Yes        |Yes         |
|Yerel bir projeye bir işi dışarı aktarma     |No         |Yes         |Yes         |
|İşleri gönderme, başlatma ve durdurma     |Yes         |Yes         |Yes         |
|İş ölçümlerini ve Diyagramı görüntüleme     |Yes         |Yes         |Portalda açma         |
|İş çalışma zamanı hatalarını görüntüleme     |Yes         |Yes         |No         |
|Kaynak günlükleri     |Evet         |Hayır         |Hayır         |
|Özel ileti özellikleri     |Yes         |Yes         |No       |
|C# özel kod işlevi ve seri hale getirici|Salt okunurdur modu|Evet|No|
|JavaScript UDF ve UDA     |Yes         |Yes         |Yalnızca Windows         |
|Machine Learning Hizmeti     |Yes        |Yes         |No         |
|Machine Learning Studio     |Evet, ancak sorgu test edilemez        |Evet |No         |
|Uyumluluk düzeyi     |1.0</br>1.1</br>1,2 (varsayılan)         |1.0</br>1.1</br>1,2 (varsayılan)           |1.0</br>1.1</br>1,2 (varsayılan)           |
|Yerleşik ML tabanlı anomali algılama işlevleri     |Yes         |Yes         |Yes         |
|Yerleşik Jeo-uzamsal işlevler     |Yes         |Yes         |Yes         |



## <a name="iot-edge-job-features"></a>IoT Edge iş özellikleri

|Özellik  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|İş yazma     |Yes         |Yes         |No         |
|Kaynak denetimi     |No         |Evet         |No         |
|Yerel bir projeye bir işi dışarı aktarma     |No         |Evet         |No         |
|Örnek dosya ile sorgu testi     |Yes         |Yes         |No         |
|Birden çok sorgu arasında girişleri ve çıkışları paylaşma     |No         |Evet         |No         |
|C# UDF     |No         |Evet         |No         |
|İşleri gönder     |Yes         |Yes         |No         |
|İşleri listeleme ve iş varlıklarını görüntüleme     |Yes         |Yes         |No         |
|İş ölçümlerini ve Diyagramı görüntüleme     |Yes         |Kısmi         |No         |
|İş çalışma zamanı hatalarını görüntüleme     |Yes         |Kısmi         |No         |
|CI/CD desteği     |No         |Hayır         |Hayır         |


## <a name="next-steps"></a>Sonraki adımlar

* [IoT Edge üzerinde Azure Stream Analytics](stream-analytics-edge.md)
* [Öğretici: Azure Stream Analytics IoT Edge işi için C# Kullanıcı tanımlı bir işlev yazma (Önizleme)](stream-analytics-edge-csharp-udf.md)
* [Visual Studio araçlarını kullanarak Stream Analytics IoT Edge işleri geliştirme](stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [Azure Stream Analytics işleri görüntülemek için Visual Studio 'Yu kullanma](stream-analytics-vs-tools.md)
* [Visual Studio Code ile Azure Stream Analytics araştırma (Önizleme)](visual-studio-code-explore-jobs.md)


