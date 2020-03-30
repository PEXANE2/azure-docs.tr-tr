---
title: Azure Akış Analizi için girişleri anlama
description: Bu makalede, akış girişi ile referans veri girişi karşılaştırılan bir Azure Akış Analizi işinde giriş kavramı açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 6b841d6b47e009c3b01d9925e11d352c00ed5c19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426426"
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Azure Akış Analizi için girişleri anlama

Azure Akış Analizi işleri bir veya daha fazla veri girişine bağlanır. Her giriş varolan bir veri kaynağına bir bağlantı tanımlar. Akış Analizi, Event Hub'ları, IoT Hub'ı ve Blob depolama alanı da dahil olmak üzere çeşitli etkinlik kaynaklarından gelen verileri kabul eder. Girişler, her iş için yazdığınız akışlı SQL sorgusunda ada göre başvurur. Sorguda, verileri karıştırmak veya akış verilerini referans verileri yle karşılaştırmak ve sonuçları çıktılara aktarmak için birden çok girişi birleştirebilirsiniz. 

Akış Analizi, girdi olarak üç tür kaynakla birinci sınıf entegrasyona sahiptir:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/) 

Bu giriş kaynakları, Akış Analizi işinizle aynı Azure aboneliğinde veya farklı bir abonelikten yaşayabilir.

Stream Analytics iş girişlerini oluşturmak, bunları yapmak ve test etmek için [Azure portalını, Azure](stream-analytics-quick-create-portal.md#configure-job-input) [PowerShell'i](https://docs.microsoft.com/powershell/module/az.streamanalytics/New-azStreamAnalyticsInput), [.NET API'yi,](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions) [REST API'yi](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-input)ve [Visual Studio'yu](stream-analytics-tools-for-visual-studio-install.md) kullanabilirsiniz.

## <a name="stream-and-reference-inputs"></a>Akış ve referans girişleri
Veriler bir veri kaynağına itildikçe, Stream Analytics işi tarafından tüketilir ve gerçek zamanlı olarak işlenir. Girişler iki türe ayrılır: veri akışı girişleri ve başvuru verisi girişleri.

### <a name="data-stream-input"></a>Veri akışı girişi
Veri akışı, zaman içinde sınırsız bir olaylar dizisidir. Stream Analytics işlerinin en az bir veri akışı girişi içermesi gerekir. Veri akışı giriş kaynakları olarak Event Hubs, IoT Hub ve Blob depolama desteklenir. Olay Hub'ları, birden çok aygıt ve hizmetten olay akışları toplamak için kullanılır. Bu akışlar sosyal medya etkinlik akışlarını, hisse senedi ticaret bilgilerini veya sensörlerden gelen verileri içerebilir. IoT Hub'ları, Nesnelerin İnterneti (IoT) senaryolarında bağlı aygıtlardan veri toplamak için optimize edilebiyi optimize eder.  Blob depolama, günlük dosyaları gibi toplu verileri akış olarak sindiren bir giriş kaynağı olarak kullanılabilir.  

Veri girişleri akışı hakkında daha fazla bilgi için [Akış verilerini Akış Analizi'ne giriş olarak](stream-analytics-define-inputs.md) görün

### <a name="reference-data-input"></a>Referans veri girişi
Akış Analizi, *referans verisi*olarak bilinen girişi de destekler. Başvuru verileri ya tamamen statik tir ya da yavaş yavaş değişir. Genellikle korelasyon ve arama yapmak için kullanılır. Örneğin, statik değerleri aramak için bir SQL birleştirme gerçekleştireceğiniz gibi, referans verilerindeki verilere veri akışı girişindeki verileri de birleştirebilirsiniz. Azure Blob depolama ve Azure SQL Veritabanı şu anda başvuru verileri için giriş kaynakları olarak desteklenir. Başvuru veri kaynağı blobs sorgu karmaşıklığı ve ayrılan Akış Birimleri bağlı olarak boyutu 300 MB'a kadar bir sınırı vardır (daha fazla bilgi için referans veri belgelerinin [Boyut sınırlaması](stream-analytics-use-reference-data.md#size-limitation) bölümüne bakın).

Referans veri girişleri hakkında daha fazla bilgi için [bkz.](stream-analytics-use-reference-data.md)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
