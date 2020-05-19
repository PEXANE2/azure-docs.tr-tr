---
title: Azure SQL Edge 'de veri akışı (Önizleme)
description: Azure SQL Edge 'de veri akışı hakkında bilgi edinin (Önizleme)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 15b6b80f9924b050b388d74adc1d67db6a386134
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83597313"
---
# <a name="data-streaming-in-azure-sql-edge-preview"></a>Azure SQL Edge 'de veri akışı (Önizleme)

Azure SQL Edge (Önizleme), veri akışını uygulamak için iki farklı seçenek sağlar. 

1. Azure 'da oluşturulan Azure Akış Analizi Edge işleri dağıtımı. Azure SQL Edge 'de Azure Akış Analizi Edge işlerinin nasıl dağıtılacağı hakkında daha fazla bilgi için bkz. [Azure Stream Analytics Işleri dağıtma](deploy-dacpac.md).
2. Azure 'da akış işleri yapılandırmaya gerek olmadan SQL Edge 'de akış işleri oluşturmak için yeni **T-SQL akış** özelliğini kullanma. 

SQL Edge 'de veri akışı uygulamak için her iki seçeneği de kullanmak mümkün olsa da, yalnızca bir tane kullanılması önerilir. Her ikisini de kullanırken, veri akışı işlemlerinin çalışmasını etkileyen olası yarış durumları olabilir.

Bu belgenin geri kalanı, birden fazla kaynaktan aynı anda yüksek hacime sahip verileri çözümlemek ve işlemek için gerçek zamanlı veri akışı, analiz ve olay işleme sağlayan yeni özellik olan **T-SQL akışına**başvurur. *T-SQL akışı* , Microsoft Azure [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction) güçlendirir ve kenarda çalışan Azure Stream Analytics tarafından sunulan benzer bir özellik kümesini destekler.

Azure Stream Analytics olduğu gibi, T-SQL akışı, cihazlar, algılayıcılar ve uygulamalar dahil olmak üzere birçok IoT giriş kaynağından ayıklanan bilgilerden ve ilişkilerin tanınmasını mümkün hale getirmenizi sunar. Bu desenler eylemleri tetiklemek ve uyarı oluşturma, raporlama veya görselleştirme çözümüne bilgi akışı veya daha sonra kullanmak üzere verileri depolama gibi iş akışlarını başlatmak için kullanılabilir. 

T-SQL akışını ne zaman kullanacağınızı aşağıdaki senaryolar örnektir:

* IoT cihazlarından gerçek zamanlı telemetri akışlarını çözümleyin.
* Otonom ve sürücüsüz taşıtlarından oluşturulan verilerin gerçek zamanlı analizi.
* Yüksek değerli endüstriyel veya üretim varlıklarının uzaktan izlenmesi ve tahmine dayalı bakımı.
* Bir tarım veya enerji grubundaki IoT algılayıcı okuma ve/veya model tanıma.

## <a name="how-does-t-sql-streaming-work"></a>T-SQL akışı nasıl çalışır?

T-SQL akışı, [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction#how-does-stream-analytics-work)gibi tamamen aynı şekilde çalışarak, örneğin gerçek zamanlı veri akışını işlemeye yönelik akış işleri kavramını kullanır. 

Stream Analytics işi, aşağıdakilerden oluşur:

- Akış girişi-akış girişi, veri akışını okumak için bir veri kaynağına bağlantıları tanımlar. Azure SQL Edge Şu anda aşağıdaki akış giriş türlerini desteklemektedir:
    - Edge hub 'ı
    - Kafka-Kafka girdileri desteği şu anda yalnızca Azure SQL Edge 'in Intel/AMD64 sürümlerinde kullanılabilir.

- Akış çıkışı-akış çıkışı, veri akışının yazılacağı bir veri kaynağına yönelik bağlantıları tanımlar. Azure SQL Edge Şu anda aşağıdaki akış çıkış türlerini desteklemektedir
    - Edge hub 'ı
    - SQL-SQL çıktısı, SQL Edge örneği veya uzak SQL Server ya da Azure SQL veritabanı içindeki bir yerel veritabanı olabilir. 
    - Azure Blob Depolama

- Akış sorgusu-akış sorgusu, akış çıktısına yazılmadan önce giriş akışına uygulanması gereken dönüştürme, toplamalar, filtre, sıralama ve birleştirmeleri tanımlar. Akış sorgusu, Azure Stream Analytics tarafından kullanılan aynı sorgu dilini temel alır. Azure Stream Analytics sorgu dili hakkında daha fazla bilgi için [Stream Analytics sorgu diline](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?)bakın.

> [!IMPORTANT]
> T-SQL akışı, Azure Stream Analytics farklı olarak, [aramalar için başvuru verilerinin kullanımını](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-use-reference-data) veya [Stream IŞINDE UDF ve uda 'yı kullanmayı](https://docs.microsoft.com/azure/stream-analytics/streaming-technologies#you-want-to-write-udfs-udas-and-custom-deserializers-in-a-language-other-than-javascript-or-c)desteklemez.

> [!NOTE]
> T-SQL akışı yalnızca Azure Stream Analytics tarafından desteklenen dil yüzeyi alanının bir alt kümesini destekler. Azure Stream Analytics sorgu dili hakkında daha fazla bilgi için bkz. [Stream Analytics sorgu dili](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?).

## <a name="limitations-and-restrictions"></a>Sınırlamalar ve kısıtlamalar

T-SQL akışı için aşağıdaki sınırlamalar ve kısıtlamalar geçerlidir. 

- Herhangi bir zamanda yalnızca bir akış işi etkin olabilir. Zaten yürütülmekte olan işlerin, başka bir iş başlatılmadan önce durdurulması gerekir.
- Her akış işi yürütmesi tek iş parçacıklıdır. Akış işi birden çok sorgu içeriyorsa, her sorgu seri sırayla değerlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL Edge 'de Stream Analytics işi oluşturma (Önizleme)](create-stream-analytics-job.md)
- [Azure SQL Edge 'de akış işleriyle ilişkili meta verileri görüntüleme (Önizleme)](streaming-catalog-views.md)
- [Dış akış oluştur](create-external-stream-transact-sql.md)