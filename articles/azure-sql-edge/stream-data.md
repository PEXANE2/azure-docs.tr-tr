---
title: Azure SQL Edge 'de veri akışı (Önizleme)
description: Azure SQL Edge 'de (Önizleme) veri akışı hakkında bilgi edinin.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 866c74fbdfcfcef7cbb7d6cddb360c4265a2f776
ms.sourcegitcommit: eeba08c8eaa1d724635dcf3a5e931993c848c633
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84669622"
---
# <a name="data-streaming-in-azure-sql-edge-preview"></a>Azure SQL Edge 'de veri akışı (Önizleme)

Azure SQL Edge (Önizleme), veri akışını uygulamak için aşağıdaki seçenekleri sağlar: 

- Azure 'da oluşturulan Azure Stream Analytics Edge işlerinin dağıtımı. Daha fazla bilgi için bkz. [Azure Stream Analytics Işleri dağıtma](deploy-dacpac.md).
- Azure 'da akış işleri yapılandırmaya gerek olmadan Azure SQL Edge 'de akış işleri oluşturmak için T-SQL akışı kullanma. 

Azure SQL Edge 'de veri akışı uygulamak için her iki seçeneği de kullanmak mümkün olsa da, bunlardan yalnızca birini kullanmalısınız. Her ikisini de kullanırken, veri akışı işlemlerinin çalışmasını etkileyen yarış durumları olabilir.

T-SQL akışı, bu makalenin odaklanmaktadır. Aynı anda birden çok kaynaktan alınan yüksek hacimli verileri çözümlemek ve işlemek için gerçek zamanlı veri akışı, analiz ve olay işleme sağlar. T-SQL akışı, Microsoft Azure [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction) güçlendirir ve aynı yüksek performanslı akış motoru kullanılarak oluşturulmuştur. Özelliği, Edge üzerinde çalışan Azure Stream Analytics tarafından sunulan benzer bir özellik kümesini destekler.

Stream Analytics olduğu gibi, T-SQL akışı, cihazlar, algılayıcılar ve uygulamalar dahil olmak üzere çeşitli IoT giriş kaynaklarından ayıklanan, desenlerdeki desenleri ve ilişkileri tanır. Bu desenleri, eylemleri tetiklemek ve iş akışlarını başlatmak için kullanabilirsiniz. Örneğin, uyarılar oluşturabilir, bir raporlama veya görselleştirme çözümüne bilgi verebilir veya daha sonra kullanmak üzere verileri saklayabilirsiniz. 

T-SQL akışı şunları yapmanıza yardımcı olabilir:

* IoT cihazlarından gerçek zamanlı telemetri akışlarını çözümleyin.
* Otonom ve sürücüsüz taşıtlarından oluşturulan verilerin gerçek zamanlı analizini kullanın.
* Yüksek değerli endüstriyel veya üretim varlıklarının uzaktan izlenmesini ve tahmine dayalı bakımını kullanın.
* Bir belirsiz veya enerji grubundaki IoT algılayıcı okumalarında anomali algılama ve model tanıma özelliğini kullanın.

## <a name="how-does-t-sql-streaming-work"></a>T-SQL akışı nasıl çalışır?

T-SQL akışı, [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction#how-does-stream-analytics-work)ile tam olarak aynı şekilde çalışmaktadır. Örneğin, gerçek zamanlı veri akışını işlemek için akış *işi* kavramını kullanır. 

Stream Analytics işi aşağıdakilerden oluşur:

- **Akış girişi**: Bu, veri akışını okumak için bir veri kaynağına bağlantıları tanımlar. Azure SQL Edge Şu anda aşağıdaki akış giriş türlerini desteklemektedir:
    - Edge hub 'ı
    - Kafka (Kafka girdileri desteği şu anda yalnızca Azure SQL Edge 'in Intel/AMD64 sürümlerinde kullanılabilir.)

- **Akış çıkışı**: Bu, veri akışını yazmak için bir veri kaynağına bağlantıları tanımlar. Azure SQL Edge Şu anda aşağıdaki akış çıkış türlerini desteklemektedir
    - Edge hub 'ı
    - SQL (SQL çıktısı, Azure SQL Edge veya uzak SQL Server ya da Azure SQL veritabanı örneği içinde yerel bir veritabanı olabilir.) 
    - Azure Blob depolama

- **Akış sorgusu**: Bu, akış çıktısına yazılmadan önce, giriş akışına uygulanacak dönüştürme, toplamalar, filtre, sıralama ve birleştirmeleri tanımlar. Akış sorgusu, Stream Analytics tarafından kullanılan sorgu dilini temel alır. Daha fazla bilgi için bkz. [Stream Analytics sorgu dili](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?).

> [!IMPORTANT]
> T-SQL akışı, Stream Analytics aksine, şu anda [aramalar için başvuru verilerini kullanmayı](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-use-reference-data) veya [BIR akış IŞINDE UDF ve uda 'yı kullanmayı](https://docs.microsoft.com/azure/stream-analytics/streaming-technologies#you-want-to-write-udfs-udas-and-custom-deserializers-in-a-language-other-than-javascript-or-c)desteklememektedir.

> [!NOTE]
> T-SQL akışı yalnızca Stream Analytics tarafından desteklenen dil yüzeyi alanının bir alt kümesini destekler. Daha fazla bilgi için bkz. [Stream Analytics sorgu dili](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?).

## <a name="limitations-and-restrictions"></a>Sınırlamalar ve kısıtlamalar

T-SQL akışı için aşağıdaki sınırlamalar ve kısıtlamalar geçerlidir. 

- Belirli bir zamanda yalnızca bir akış işi etkin olabilir. Başka bir iş başlatılmadan önce zaten çalışmakta olan işlerin durdurulması gerekir.
- Her akış işi yürütmesi tek iş parçacıklı. Akış işi birden çok sorgu içeriyorsa, her sorgu seri sırayla değerlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL Edge 'de Stream Analytics işi oluşturma (Önizleme)](create-stream-analytics-job.md)
- [Azure SQL Edge 'de akış işleriyle ilişkili meta verileri görüntüleme (Önizleme)](streaming-catalog-views.md)
- [Dış akış oluştur](create-external-stream-transact-sql.md)