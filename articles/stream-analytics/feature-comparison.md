---
title: Azure Akış Analizi özellik karşılaştırması
description: Bu makalede, Azure portalı, Visual Studio ve Visual Studio Code'taki Azure Akış Analizi bulutu ve IoT Edge işleri için desteklenen özellikler karşılaştırılır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: cdae6a3b7319aefa9d4f19b5d613d1afb8b6804a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235319"
---
# <a name="azure-stream-analytics-feature-comparison"></a>Azure Akış Analizi özellik karşılaştırması

Azure Akış Analizi ile, [Azure portalı,](stream-analytics-quick-create-portal.md) [Visual Studio](stream-analytics-quick-create-vs.md)ve Visual [Studio Code'u](quick-create-vs-code.md)kullanarak bulutta ve IoT Edge'de akış çözümleri oluşturabilirsiniz. Bu makaledeki tablolar, her iki iş türü için her platform tarafından desteklenen özellikleri gösterir.

## <a name="cloud-job-features"></a>Bulut iş özellikleri


|Özellik  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Çapraz platform     |Mac</br>Linux</br>Windows         |Windows        |Mac</br>Linux</br>Windows          |
|Komut dosyası yazma     |Evet         |Evet         |Evet         |
|Script Intellisense     |Sözdizimi vurgulama         |Sözdizimi vurgulama</br>Kod tamamlama</br>Hata işaretçisi         |Sözdizimi vurgulama</br>Kod tamamlama</br>Hata işaretçisi         |
|Tüm girdi, çıktı ve iş yapılandırmaları türlerini tanımlama     |Evet         |Evet         |Evet         |
|Kaynak denetimi     |Hayır         |Evet         |Evet         |
|CI/CD desteği     |Kısmi         |Evet         |Evet         |
|Giriş ve çıktıları birden çok sorgu arasında paylaşma     |Hayır         |Evet         |Evet         |
|Örnek bir dosyayla sorgu testi     |Evet         |Evet        |Evet         |
|Canlı veri yerel test     |Hayır         |Evet       |Evet      |
|İşleri listele ve iş varlıklarını görüntüleyin     |Evet         |Evet        |Evet         |
|Yerel bir projeye iş verme     |Hayır         |Evet         |Evet         |
|İş gönderme, başlatma ve durdurma     |Evet         |Evet         |Evet         |
|İş ölçümlerini ve diyagramı görüntüleme     |Evet         |Evet         |Portalda açma         |
|İş çalışma zamanı hatalarını görüntüleme     |Evet         |Evet         |Hayır         |
|Tanılama günlükleri     |Evet         |Hayır         |Hayır         |
|Özel ileti özellikleri     |Evet         |Evet         |Hayır       |
|C# özel kod fonksiyonu ve Deserializer|Salt okunur modu|Evet|Hayır|
|JavaScript UDF ve UDA     |Evet         |Evet         |Yalnızca Windows         |
|Machine Learning Hizmeti     |Evet        |Evet         |Hayır         |
|Machine Learning Studio     |Evet, ancak sorgu sınanamıyor        |Evet |Hayır         |
|Uyumluluk düzeyi     |1.0</br>1.1</br>1.2 (varsayılan)         |1.0</br>1.1</br>1.2 (varsayılan)           |1.0</br>1.1</br>1.2 (varsayılan)           |
|Dahili ML tabanlı Anomali Algılama fonksiyonları     |Evet         |Evet         |Evet         |
|Dahili GeoSpatial fonksiyonları     |Evet         |Evet         |Evet         |



## <a name="iot-edge-job-features"></a>IoT Edge iş özellikleri

|Özellik  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|İş yazma     |Evet         |Evet         |Hayır         |
|Kaynak denetimi     |Hayır         |Evet         |Hayır         |
|Yerel bir projeye iş verme     |Hayır         |Evet         |Hayır         |
|Örnek bir dosyayla sorgu testi     |Evet         |Evet         |Hayır         |
|Giriş ve çıktıları birden çok sorgu arasında paylaşma     |Hayır         |Evet         |Hayır         |
|C# UDF     |Hayır         |Evet         |Hayır         |
|İş gönderme     |Evet         |Evet         |Hayır         |
|İşleri listele ve iş varlıklarını görüntüleyin     |Evet         |Evet         |Hayır         |
|İş ölçümlerini ve diyagramı görüntüleme     |Evet         |Kısmi         |Hayır         |
|İş çalışma zamanı hatalarını görüntüleme     |Evet         |Kısmi         |Hayır         |
|CI/CD desteği     |Hayır         |Hayır         |Hayır         |


## <a name="next-steps"></a>Sonraki adımlar

* [IoT Edge üzerinde Azure Stream Analytics](stream-analytics-edge.md)
* [Öğretici: Azure Akışı Analytics IoT Edge işi için C# kullanıcı tanımlı bir işlev yazın (Önizleme)](stream-analytics-edge-csharp-udf.md)
* [Visual Studio araçlarını kullanarak Stream Analytics IoT Edge işlerini geliştirin](stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [Azure Akış Analizi işlerini görüntülemek için Visual Studio'yı kullanın](stream-analytics-vs-tools.md)
* [Visual Studio Code (Önizleme) ile Azure Akış Analizini Keşfedin](visual-studio-code-explore-jobs.md)


