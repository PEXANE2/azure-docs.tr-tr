---
title: Azure Stream Analytics için girişleri anlayın
description: Bu makalede, akış girişini başvuru veri girişi ile karşılaştıran bir Azure Stream Analytics işinde giriş kavramı açıklanır.
author: jseb225
ms.author: krishmam
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/29/2020
ms.openlocfilehash: 843e05a68a9fd9a72f14847a941d69c672094b66
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93077022"
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Azure Stream Analytics için girişleri anlayın

Azure Stream Analytics işleri bir veya daha fazla veri girişi 'ne bağlanır. Her giriş, var olan bir veri kaynağına yönelik bağlantıyı tanımlar. Stream Analytics, Event Hubs, IoT Hub ve BLOB depolama dahil olmak üzere çeşitli türlerdeki olay kaynaklarından gelen verileri kabul eder. Girişler, her iş için yazdığınız akış SQL sorgusunda adı tarafından başvurulur. Sorguda, verileri Blend veya başvuru verilerine arama ile akış verilerini karşılaştırmak ve sonuçları çıkışlara geçirmek için birden çok girişi birleştirebilirsiniz. 

Stream Analytics giriş olarak dört tür kaynakla birinci sınıf tümleştirmeye sahiptir:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/) 
- [Azure Data Lake Storage 2. Nesil](../storage/blobs/data-lake-storage-introduction.md) 

Bu giriş kaynakları, Stream Analytics işiniz veya farklı bir abonelikle aynı Azure aboneliğinde bulunabilir.

[Azure Portal](stream-analytics-quick-create-portal.md#configure-job-input), [Azure POWERSHELL](https://docs.microsoft.com/powershell/module/az.streamanalytics/New-azStreamAnalyticsInput), [.NET API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), [REST API](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-inputs)ve [Visual Studio 'yu](stream-analytics-tools-for-visual-studio-install.md) kullanarak Stream Analytics iş girişleri oluşturabilir, düzenleyebilir ve test edebilirsiniz.

## <a name="stream-and-reference-inputs"></a>Akış ve başvuru girişleri
Veriler bir veri kaynağına gönderildiği için Stream Analytics işi tarafından kullanılır ve gerçek zamanlı olarak işlenir. Girişler iki türe ayrılır: veri akışı girişleri ve başvuru verisi girişleri.

### <a name="data-stream-input"></a>Veri akışı girişi
Veri akışı zaman içinde sınırsız bir olay sırasıdır. Stream Analytics işlerinin en az bir veri akışı girişi içermesi gerekir. Event Hubs, IoT Hub, Azure Data Lake Storage 2. ve BLOB depolama alanı veri akışı giriş kaynakları olarak desteklenir. Event Hubs, birden fazla cihazdan ve hizmetten olay akışlarını toplamak için kullanılır. Bu akışlar sosyal medya etkinlik akışları, hisse senedi bilgileri veya sensörlerden alınan veriler içerebilir. IoT Hub 'Ları Nesnelerin İnterneti (IoT) senaryolarında bağlı cihazlardan veri toplamak için iyileştirilmiştir.  BLOB depolama, günlük dosyaları gibi bir akış olarak toplu verileri almak için bir giriş kaynağı olarak kullanılabilir.  

Akış veri girişleri hakkında daha fazla bilgi için bkz. [Stream Analytics giriş olarak akış verileri](stream-analytics-define-inputs.md)

### <a name="reference-data-input"></a>Başvuru veri girişi
Stream Analytics Ayrıca *başvuru verileri* olarak bilinen girişi destekler. Başvuru verileri tamamen statiktir ya da yavaş değişir. Genellikle bağıntı ve arama gerçekleştirmek için kullanılır. Örneğin, statik değerleri aramak için bir SQL JOIN gerçekleştirdiğinize çok benzer şekilde, veri akışı girişinde verileri başvuru verileri verilerine katabilirsiniz. Azure Blob depolama, Azure Data Lake Storage 2. ve Azure SQL veritabanı şu anda başvuru verileri için giriş kaynağı olarak desteklenmektedir. Sorgu karmaşıklığı ve ayrılan akış birimlerine bağlı olarak, başvuru veri kaynağı Blobları boyut olarak 300 MB 'a kadar bir sınıra sahiptir (daha fazla ayrıntı için başvuru verileri belgelerinin [boyut sınırlaması](stream-analytics-use-reference-data.md#size-limitation) bölümüne bakın).

Başvuru verileri girişleri hakkında daha fazla bilgi için bkz. [Stream Analytics aramalar için başvuru verilerini kullanma](stream-analytics-use-reference-data.md)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
