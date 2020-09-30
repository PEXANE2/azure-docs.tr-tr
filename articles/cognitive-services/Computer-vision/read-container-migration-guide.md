---
title: Read v3. x OCR kapsayıcılarına geçme
titleSuffix: Azure Cognitive Services
description: V3 okuma OCR kapsayıcılarına geçiş yapmayı öğrenin
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 09/21/2020
ms.author: aahi
ms.openlocfilehash: 714a4709eceea875798940de962716d34437f2a9
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91530468"
---
# <a name="migrate-to-the-read-v3x-ocr-containers"></a>Read v3. x OCR kapsayıcılarına geçiş

Görüntü İşleme okuma OCR kapsayıcısının 2. sürümünü kullanıyorsanız, uygulamanızı kapsayıcının 3. x sürümünü kullanacak şekilde yükseltme hakkında bilgi edinmek için bu makaleyi kullanın. 


## <a name="configuration-changes"></a>Yapılandırma değişiklikleri

* `ReadEngineConfig:ResultExpirationPeriod` artık desteklenmiyor. Okuma kapsayıcısının, 48 saat sonra bir istekle ilişkili sonuçları ve meta verileri kaldıran yerleşik bir cron işi vardır.
* `Cache:Redis:Configuration` artık desteklenmiyor. Önbellek v3. x kapsayıcılarında kullanılmaz, bu yüzden ayarlamanız gerekmez.

## <a name="api-changes"></a>API değişiklikleri

Read v3. x kapsayıcıları Görüntü İşleme API'si sürüm 3 ' ü kullanır ve aşağıdaki uç noktalara sahiptir:

#### <a name="version-31-preview"></a>[Sürüm 3,1-Önizleme](#tab/version-3-1)

* `/vision/v3.1-preview.2/read/analyzeResults/{operationId}`
* `/vision/v3.1-preview.2/read/analyze`
* `/vision/v3.1-preview.2/read/syncAnalyze`

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

* `/vision/v3.0/read/analyzeResults/{operationId}`
* `/vision/v3.0/read/analyze`
* `/vision/v3.0/read/syncAnalyze`

---

Uygulamalarınızı bulut tabanlı okuma API 'sinin sürüm 3 ' ü kullanacak şekilde güncelleştirme hakkında ayrıntılı bilgi için bkz. [v3 REST API geçiş kılavuzu görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/upgrade-api-versions) . Bu bilgiler kapsayıcı için de geçerlidir. Eşitleme işlemlerinin yalnızca kapsayıcılar için desteklendiğini unutmayın.

## <a name="memory-requirements"></a>Bellek gereksinimleri

Gereksinimler ve öneriler, 29 satır ve toplam 803 karakter içeren taranmış bir iş harfinin 8 MB 'lik bir görüntüsünü kullanarak saniyede tek bir istek ile kıyaslamayı temel alır. Aşağıdaki tabloda, her bir okuma kapsayıcısı için kaynakların en düşük ve önerilen ayırması açıklanmaktadır.

|Kapsayıcı  |Minimum | Önerilen  |
|---------|---------|------|
|Okuma 3,0-Önizleme     | 8 çekirdek, 16 GB bellek         | 8 çekirdek, 24 GB bellek
|Okuma 3,1-Önizleme | 8 çekirdek, 16 GB bellek         | 8 çekirdek, 24 GB bellek

Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.

Çekirdek ve bellek, `--cpus` `--memory` Docker Run komutunun bir parçası olarak kullanılan ve ayarlarına karşılık gelir.

## <a name="storage-implementations"></a>Depolama uygulamaları

>[!NOTE]
> MongoDB, kapsayıcının 3. x sürümünde artık desteklenmiyor. Bunun yerine kapsayıcılar, Azure depolama ve çevrimdışı dosya sistemlerini destekler.

| Uygulama |  Gerekli çalışma zamanı bağımsız değişkenleri |
|---------|---------|
|Dosya düzeyi (varsayılan)   | Çalışma zamanı bağımsız değişkeni gerekmez. `/share` Dizin kullanılacak. |
|Azure Blob | `Storage:ObjectStore:AzureBlob:ConnectionString={AzureStorageConnectionString}` |

## <a name="queue-implementations"></a>Kuyruk uygulamaları

Kapsayıcının v3. x dosyasında, Kbbitmq Şu anda desteklenmiyor. Desteklenen destek uygulamaları şunlardır:

| Uygulama | Çalışma zamanı bağımsız değişkenleri | Amaçlanan kullanım |
|---------|---------|-------|
| Bellekte (varsayılan) | Çalışma zamanı bağımsız değişkeni gerekmez. | Geliştirme ve test |
| Azure Kuyrukları | `Queue:Azure:ConnectionString={AzureStorageConnectionString}` | Üretim |
| RabbitMQ  | Kullanılamaz | Üretim |

Eklenen artıklık için Read v3. x kapsayıcısı, çok kapsayıcılı bir planda çalışırken isteklerin kilitlenme durumunda başarılı bir şekilde işlenebilmesi için görünürlük zamanlayıcısını kullanır. 

Zamanlayıcıyı, `Queue:Azure:QueueVisibilityTimeoutInMilliseconds` başka bir çalışan tarafından işlendiği zaman bir iletinin görünmez olmasını ayarlayan ile birlikte ayarlayın. Sayfaların Redundantly işlenmesini önlemek için, zaman aşımı süresini 120 saniyeye ayarlamayı öneririz. Varsayılan değer 30 saniyedir.

| Varsayılan değer | Önerilen değer |
|---------|---------|
| 30000 |   120000 |


## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları](computer-vision-resource-container-config.md) yapılandırmayı gözden geçir
* Basılan ve el yazısı metni tanıma hakkında daha fazla bilgi edinmek için [görüntü işleme genel bakışı](overview.md) inceleyin
* Kapsayıcı tarafından desteklenen yöntemler hakkındaki ayrıntılar için [görüntü işleme API'si](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) başvurun.
* Görüntü İşleme işlevlerle ilgili sorunları çözmek için [sık sorulan sorular (SSS)](FAQ.md) bölümüne bakın.
* Daha fazla bilişsel [Hizmetler kapsayıcısı](../cognitive-services-container-support.md) kullanın
