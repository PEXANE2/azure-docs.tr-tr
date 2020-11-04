---
title: Tümleşik çözümler oluşturun
description: Azure SYNAPSE Analytics 'te adanmış bir SQL havuzu ile tümleştirilen çözüm araçları ve iş ortakları.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 9f92128266c41912868f6ab74abaa2d374c6d236
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93324499"
---
# <a name="integrate-other-services-with-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Diğer hizmetleri Azure SYNAPSE Analytics 'te adanmış bir SQL havuzuyla tümleştirin.

Azure SYNAPSE Analytics içindeki adanmış SQL havuzu özelliği, kullanıcıların Azure 'daki diğer birçok hizmet ile tümleştirilmesine olanak sağlar. SYNAPSE SQL kullanarak, Özel SQL havuzu kaynağı aracılığıyla bir veri ambarı oluşturabilirsiniz. Bu, bazıları şunlar dahil olmak üzere çeşitli ek hizmetlerden yararlanabilir:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

Azure genelinde Tümleştirme Hizmetleri hakkında daha fazla bilgi için, [Tümleştirme iş ortakları](sql-data-warehouse-partner-data-integration.md)makalesini gözden geçirin.

## <a name="power-bi"></a>Power BI

Power BI tümleştirme, bir veri ambarının işlem gücünü Power BI dinamik raporlama ve görselleştirmesiyle birleştirmenizi sağlar. Power BI tümleştirmesi şu anda aşağıdakileri içermektedir:

* **Doğrudan bağlantı** : adanmış SQL havuzu kullanılarak sağlanan bir veri ambarına karşı mantıksal bir şekilde aşağı doğru bağlantı ile daha gelişmiş bir bağlantı. İtme, geniş ölçekli analiz işlemlerinin daha hızlı tamamlanmasını sağlar.
* **Power BI açın** : ' Power BI aç ' düğmesi, bağlanmak için basitleştirilmiş bir yöntem için örnek bilgilerini Power BI geçirir.

Daha fazla bilgi için bkz. [Power BI Ile tümleştirme](sql-data-warehouse-get-started-visualize-with-power-bi.md)veya [Power BI belgeleri](https://powerbi.microsoft.com/blog/exploring-azure-sql-data-warehouse-with-power-bi/).

## <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory, kullanıcılara karmaşık ayıklama ve yükleme işlem hatları oluşturmak için yönetilen bir platform sağlar. Azure Data Factory ile adanmış SQL havuzu tümleştirmesi şunları içerir:

* **Saklı yordamlar** : saklı yordamların yürütülmesini düzenleyin.
* **Kopyala** : VERILERI adanmış SQL havuzuna TAŞıMAK için ADF kullanın. Bu işlem, tüm kapak altındaki ADF 'nin standart veri taşıma mekanizmasını veya PolyBase 'i kullanabilir.

Daha fazla bilgi için bkz. [Azure Data Factory tümleştirme](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

## <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning, büyük bir tahmine dayalı araç kümesi kullanarak karmaşık modeller oluşturmanıza olanak sağlayan, tam olarak yönetilen bir analiz hizmetidir. Adanmış SQL havuzu, bu modeller için hem kaynak hem de hedef olarak desteklenir ve aşağıdaki işlevlere sahiptir:

* **Verileri oku:** Özel bir SQL havuzunda T-SQL kullanarak modelleri ölçeklendirin.
* **Veri yaz:** Tüm modellerden ayrılmış bir SQL havuzuna yapılan değişiklikleri geri yürütün.

Daha fazla bilgi için bkz. [Azure Machine Learning tümleştirme](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Azure Stream Analytics, Azure Olay Hub 'ından oluşturulan olay verilerinin işlenmesi ve kullanılması için karmaşık, tam olarak yönetilen bir altyapısıdır.  Adanmış SQL havuzuyla tümleştirme, akış verilerinin, daha derin ve daha gelişmiş analizler sağlayan ilişkisel verilerle birlikte etkili bir şekilde işlenmesini ve depolanmasını sağlar.  

* **Iş çıkışı:** Stream Analytics işlerden gelen çıktıyı doğrudan adanmış bir SQL havuzuna gönderin.

Daha fazla bilgi için bkz. [Azure Stream Analytics tümleştirme](sql-data-warehouse-integrate-azure-stream-analytics.md).
