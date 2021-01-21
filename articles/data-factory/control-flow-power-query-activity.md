---
title: Azure Data Factory etkinlik Power Query
description: Bir Data Factory işlem hattının veri denetimi özellikleri için Power Query etkinliğini nasıl kullanacağınızı öğrenin
services: data-factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/18/2021
ms.openlocfilehash: 3314053e5b81c597d6d29015a5ebda6e171731d1
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634408"
---
# <a name="power-query-activity-in-data-factory"></a>Data Factory 'de Power Query etkinliği

Power Query etkinliği, bir Data Factory işlem hattında ölçeklendirmekte olan verileri yürütmek için Power Query Mash-ups oluşturup yürütmenize olanak tanır. Yeni kaynaklar menü seçeneğinden yeni bir Power Query Mash oluşturun veya işlem hattınızla bir güç etkinliği ekleyebilirsiniz.

![Fabrika kaynakları bölmesinde Power Query gösteren ekran görüntüsü.](media/data-flow/power-query-wrangling.png)

Daha önce Azure Data Factory içindeki veri denetimi veri akışı menü seçeneğinden yazıldı. Bu, yeni bir Power Query etkinliğinden yazmak üzere değiştirilmiştir. Etkileşimli veri araştırması gerçekleştirmek ve sonra çalışmanızı kaydetmek için Power Query Mash Düzenleyicisi içinde doğrudan çalışabilirsiniz. Tamamlandıktan sonra Power Query etkinliğinizi alabilir ve bir işlem hattına ekleyebilirsiniz. Azure Data Factory, verileri otomatik olarak ölçeklendirecek ve Azure Data Factory veri akışı Spark ortamını kullanarak veri wrangini kullanıma sunacaktır.

## <a name="translation-to-data-flow-script"></a>Veri akışı betiğine çeviri

Power Query etkinliğinizdeki ölçeklendirmeye ulaşmak için Azure Data Factory komut dosyanızı ```M``` bir veri akışı betiğine çevirir, böylece Azure Data Factory veri akışı Spark ortamını kullanarak Power Query ölçeklendirmenize olanak sağlayabilirsiniz. Kod içermeyen veri hazırlama kullanarak denetimi veri akışınızı yazın. Kullanılabilir işlevlerin listesi için bkz. [dönüştürme işlevleri](wrangling-functions.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Data Factory Power Query](wrangling-tutorial.md) kullanarak veri denetimi kavramları hakkında daha fazla bilgi edinin
