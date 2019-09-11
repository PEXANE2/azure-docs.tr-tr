---
title: Azure Data Factory eşleme veri akışına genel bakış
description: Azure Data Factory veri akışlarını eşleştirmeye ilişkin genel bakış açıklaması
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 6f4c124c59584c8538d85ac61650661ae559a77b
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70123992"
---
# <a name="what-are-mapping-data-flows"></a>Eşleme veri akışları nelerdir?

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Veri akışlarını eşleme, Azure Data Factory 'de görsel olarak tasarlanan veri dönüşümlerdir. Veri akışları, veri mühendislerinin kod yazmadan grafik veri dönüştürme mantığı geliştirmesini sağlar. Elde edilen veri akışları, ölçeği genişletilmiş Azure Databricks kümeler kullanılarak Azure Data Factory işlem hatları dahilinde yürütülür.

Azure Data Factory veri akışı amacı, hiçbir kodlamaya gerek olmadan tam bir görsel deneyim sağlamaktır. Veri akışlarınız, ölçeği genişletilmiş veri işleme için kendi yürütme kümenizde yürütülür. Azure Data Factory, tüm kod çevirisi, yol iyileştirmesi ve veri akışı işlerinizin yürütülmesini işler.

Dönüştürme mantığınızı etkileşimli olarak doğrulayabilmeniz için hata ayıklama modunda veri akışları oluşturarak başlayın. Ardından, veri akışınızı işlem hattı hata ayıklamada çalıştırmak ve test etmek için işlem hattınızda bir veri akışı etkinliği ekleyin veya veri akışınızı bir işlem hattı etkinliğinden test etmek için işlem hattında "Şimdi Tetikle" seçeneğini kullanın.

Daha sonra veri akışı etkinliklerini yürütecek Azure Data Factory işlem hatlarını kullanarak veri akışı etkinliklerinizi planlayarak ve izleyebilirsiniz.

Veri akışı tasarım yüzeyinde hata ayıklama modu geçiş anahtarı, veri dönüştürmelerinin etkileşimli olarak oluşturulmasına olanak sağlar. Hata ayıklama modu, veri akışı oluşturma için veri hazırlama ve veri önizleme ortamı sağlar.

## <a name="begin-building-your-data-flow-logical-graph"></a>Veri akışı mantıksal grafınızı oluşturmaya başlayın

Yeni bir veri akışı oluşturmak için fabrika kaynakları altındaki + işaretini kullanarak veri akışları oluşturmaya başlayın.

![Yeni veri akışı](media/data-flow/newdataflow2.png "Yeni veri akışı")

Kaynak dönüşümünü yapılandırarak başlayın ve ardından + işaretini kullanarak sonraki adıma veri dönüşümü ekleyin. Mantıksal grafınızı oluştururken, "grafiği göster" ve "grafiği gizle" düğmesini kullanarak grafik ve yapılandırma modları arasında geçiş yapabilirsiniz.

![Grafiği göster](media/data-flow/showg.png "Grafiği göster")

## <a name="configure-transformation-logic"></a>Dönüşüm mantığını yapılandırma

![Grafiği gizle](media/data-flow/hideg.png "Grafiği gizle")

Grafınızı gizlemek, daha sonra dönüşüm düğümleriniz arasında gezinmenizi sağlar.

![Git](media/data-flow/showhide.png "Git")

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak dönüşümle başlayın](data-flow-source.md)
