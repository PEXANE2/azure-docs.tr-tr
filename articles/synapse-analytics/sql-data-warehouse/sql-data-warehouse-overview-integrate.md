---
title: Entegre çözümler oluşturun
description: Synapse SQL havuzuyla tümleşen çözüm araçları ve iş ortakları.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: c8e3598e55d3f90ab2b7401380406677f56c0ce3
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80586493"
---
# <a name="integrate-other-services-with-a-synapse-sql-pool"></a>Diğer hizmetleri Synapse SQL havuzuyla tümleştirme

Azure Synapse Analytics'teki Synapse SQL havuzu özelliği, kullanıcıların Azure'daki diğer hizmetlerin çoğuyla tümleştirmelerine olanak tanır. Synapse SQL'i kullanarak, SQL havuz kaynağı üzerinden bir veri ambarı oluşturabilirsiniz ve bunlardan bazıları şunlardır:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

Azure genelinde tümleştirme hizmetleriyle ilgili daha fazla bilgi için [Tümleştirme ortakları](sql-data-warehouse-partner-data-integration.md)makalesini inceleyin.

## <a name="power-bi"></a>Power BI
Power BI tümleştirmesi, bir veri ambarının bilgi işlem gücünü Power BI'nin dinamik raporlama sı ve görselleştirmesiyle birleştirmenize olanak tanır. Güç BI entegrasyonu şu anda şunları içerir:

* **Direct Connect**: SQL havuzu kullanılarak sağlanan bir veri ambarına karşı mantıksal pushdown ile daha gelişmiş bir bağlantı. Pushdown daha büyük bir ölçekte daha hızlı analiz sağlar.
* **Power BI'de Aç**: 'Power BI'de Aç' düğmesi, bağlanmak için basitleştirilmiş bir yol için örnek bilgilerini Power BI'ye aktarAr.

Daha fazla bilgi için Power BI veya [Power BI belgeleriyle](https://powerbi.microsoft.com/blog/exploring-azure-sql-data-warehouse-with-power-bi/) [Tümleştir'e](sql-data-warehouse-get-started-visualize-with-power-bi.md)bakın.

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Veri Fabrikası, kullanıcılara karmaşık ayıklama ve yükleme boru hatları oluşturmak için yönetilen bir platform sağlar. Azure Veri Fabrikası ile SQL havuzu tümleştirmesi şunları içerir:

* **Depolanan Yordamlar**: Depolanan yordamların yürütülmesini düzenlemek.
* **Kopya**: Verileri SQL havuzuna taşımak için ADF'yi kullanın. Bu işlem, kapakların altında ADF'nin standart veri hareket mekanizmasını veya PolyBase'i kullanabilir. 

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/load-azure-sql-data-warehouse?toc=/azure/sql-data-warehouse/toc.json)

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning, büyük bir tahmin araçları kümesi kullanarak karmaşık modeller oluşturmanıza olanak tanıyan tam olarak yönetilen bir analiz hizmetidir. SQL havuzu bu modeller için hem kaynak hem de hedef olarak desteklenir ve aşağıdaki işlevsellik vardır:

* **Verileri Okuyun:** T-SQL'i SQL havuzuna karşı kullanarak modelleri ölçekte yönlendirin.
* **Veri Yaz:** Herhangi bir modelden SQL havuzuna değişiklik gerçekleştirin.

Daha fazla bilgi için [bkz.](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Akış Analizi, Azure Etkinlik Hub'ından oluşturulan olay verilerini işlemek ve tüketmek için karmaşık ve tam olarak yönetilen bir altyapıdır.  SQL havuzu ile tümleştirme, akış verilerinin etkili bir şekilde işlenmesine ve ilişkisel verilerin yanında depolanmasına olanak sağlayarak daha derin, daha gelişmiş analizler sağlar.  

* **İş Çıktısı:** Akış Analizi işlerinden çıktıyı doğrudan SQL havuzuna gönderin.

Daha fazla bilgi için [bkz.](sql-data-warehouse-integrate-azure-stream-analytics.md)


