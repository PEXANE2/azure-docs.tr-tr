---
title: Tümleşik çözümler oluşturun
description: SQL Analytics kullanılarak sağlanan bir veri ambarı ile tümleştirilen çözüm araçları ve iş ortakları.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 8b38b50401c50ecfb9cd37c53ad013ca05e126c0
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153307"
---
# <a name="integrate-other-services-with-a-sql-analytics-data-warehouse"></a>Diğer hizmetleri bir SQL Analytics veri ambarı ile tümleştirme 
Azure SYNAPSE Analytics içindeki SQL Analytics özelliği, kullanıcıların Azure 'daki diğer birçok hizmet ile tümleştirilmesine olanak sağlar. SQL Analytics 'i kullanarak, SQL havuzu kaynağı aracılığıyla bir veri ambarı oluşturabilirsiniz. Bu işlem, bazı ek hizmetlerden daha sonra şunlar olabilir:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

Azure genelinde Tümleştirme Hizmetleri hakkında daha fazla bilgi için, [Tümleştirme iş ortakları](sql-data-warehouse-partner-data-integration.md)makalesini gözden geçirin.

## <a name="power-bi"></a>Power BI
Power BI tümleştirme, bir veri ambarının işlem gücünü Power BI dinamik raporlama ve görselleştirmesiyle birleştirmenizi sağlar. Power BI tümleştirme Şu anda şunları içerir:

* **Doğrudan Bağlan**: SQL havuzu kullanılarak sağlanan bir veri ambarına karşı mantıksal olarak aşağı doğru bir şekilde daha gelişmiş bir bağlantı. Aşağı itme daha büyük bir ölçekte daha hızlı analiz sağlar.
* **Power BI açın**: ' Power BI aç ' düğmesi, bağlanmak için basitleştirilmiş bir yöntem için örnek bilgilerini Power BI geçirir.

Daha fazla bilgi için bkz. [Power BI Ile tümleştirme](sql-data-warehouse-get-started-visualize-with-power-bi.md)veya [Power BI belgeleri](https://powerbi.microsoft.com/blog/exploring-azure-sql-data-warehouse-with-power-bi/).

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory, kullanıcılara karmaşık ayıklama ve yükleme işlem hatları oluşturmak için yönetilen bir platform sağlar. Azure Data Factory ile SQL havuzu tümleştirmesi şunları içerir:

* **Saklı yordamlar**: saklı yordamların yürütülmesini düzenleyin.
* **Kopyala**: verileri SQL havuzuna TAŞıMAK için ADF kullanın. Bu işlem, tüm kapak altındaki ADF 'nin standart veri taşıma mekanizmasını veya PolyBase 'i kullanabilir. 

Daha fazla bilgi için bkz. [Azure Data Factory tümleştirme](https://docs.microsoft.com/azure/data-factory/load-azure-sql-data-warehouse?toc=/azure/sql-data-warehouse/toc.json).

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning, büyük bir tahmine dayalı araç kümesi kullanarak karmaşık modeller oluşturmanıza olanak sağlayan, tam olarak yönetilen bir analiz hizmetidir. SQL havuzu, bu modeller için hem kaynak hem de hedef olarak desteklenir ve aşağıdaki işlevlere sahiptir:

* **Verileri oku:** SQL havuzunda T-SQL kullanarak modelleri ölçeklendirin.
* **Veri yaz:** Değişiklikleri herhangi bir modelden SQL havuzuna kaydedin.

Daha fazla bilgi için bkz. [Azure Machine Learning tümleştirme](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics, Azure Olay Hub 'ından oluşturulan olay verilerinin işlenmesi ve kullanılması için karmaşık, tam olarak yönetilen bir altyapısıdır.  SQL havuzuyla tümleştirme, akış verilerinin, daha derin ve daha gelişmiş analizler sağlayan ilişkisel verilerle birlikte etkili bir şekilde işlenmesini ve depolanmasını sağlar.  

* **Iş çıkışı:** Stream Analytics işlerden gelen çıktıyı doğrudan SQL havuzuna gönderin.

Daha fazla bilgi için bkz. [Azure Stream Analytics tümleştirme](sql-data-warehouse-integrate-azure-stream-analytics.md).


