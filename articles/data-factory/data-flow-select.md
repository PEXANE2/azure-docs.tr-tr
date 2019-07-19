---
title: Azure Data Factory eşleme veri akışı dönüşümü seçme
description: Azure Data Factory eşleme veri akışı dönüşümü seçme
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 15c74637a2dc42ec44f582878b5505d94637cd7b
ms.sourcegitcommit: da0a8676b3c5283fddcd94cdd9044c3b99815046
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314205"
---
# <a name="azure-data-factory-mapping-data-flow-select-transformation"></a>Azure Data Factory eşleme veri akışı dönüşümü seçme
[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Seçiciliği (sütun sayısını azaltma), diğer ad sütunları ve akış adları ve sütunları yeniden sıralama için bu dönüşümü kullanın.

## <a name="how-to-use-select-transformation"></a>Seçme dönüşümünü kullanma
Seçim dönüşümü, bir akışın tamamına veya bu akıştaki sütunlara diğer ad atamanıza, farklı adlar atamanıza (diğer adlar) ve ardından bu yeni adlara daha sonra veri akışınızda başvurmasına olanak tanır. Bu dönüşüm, kendine katılacak senaryolar için yararlıdır. ADF veri akışında kendi kendine katılmayı uygulama yolu bir akış almak, bunu "yeni dal" ile dallandırma ve sonra hemen sonra da "Select" dönüştürmesi eklemektir. Bu akış artık özgün akışa geri katılabilmek için kullanabileceğiniz yeni bir ada sahip olur ve kendi kendine katılması oluşturursunuz:

![Kendi kendine Birleştir](media/data-flow/selfjoin.png "Kendi kendine Birleştir")

Yukarıdaki diyagramda, select Transform en üstte. Bu, özgün akışın "OrigSourceBatting" olarak diğer adını alıyor. Aşağıdaki Vurgulanan birleşim dönüşümünden, bu Select diğer ad akışını sağ birleşim olarak kullandığımızdan, Iç birleştirmenin sağ & sağ tarafında aynı anahtara başvurmamızı sağlayabilirsiniz.

Ayrıca, veri akışınızdan sütunları seçmek için bir yöntem olarak da kullanılabilir. Örneğin, havuzunuzu tanımlanmış 6 sütunlarınız varsa, ancak yalnızca belirli bir 3 ' ü dönüştürmek ve havuza akışa almak istiyorsanız, select Transform 'u kullanarak yalnızca bu 3 ' ü seçebilirsiniz.

> [!NOTE]
> Yalnızca belirli sütunları seçmek için "Tümünü Seç" seçeneğini kapatmanız gerekir

![Dönüşüm seçin](media/data-flow/select001.png "Diğer ad Seç")

## <a name="options"></a>Seçenekler
* "Select" için varsayılan ayar tüm gelen sütunları dahil etmek ve bu özgün adları tutmak içindir. Seçim dönüşümünün adını ayarlayarak akışa diğer ad verebilirsiniz.
* Tek tek sütunlara, "Tümünü Seç" seçimini kaldırın ve alt kısımdaki sütun eşlemesini kullanın.
* Giriş veya çıkış meta verilerinden yinelenen sütunları kaldırmak için Yinelenenleri atla ' yı seçin.

![Yinelenenleri atla](media/data-flow/select-skip-dup.png "Yinelenenleri atla")

## <a name="next-steps"></a>Sonraki adımlar
* Yeniden adlandırma, yeniden sıralama ve diğer ad Sütunlarını Seç ' i kullandıktan sonra, verileri bir veri deposuna eklemek için [Havuz dönüştürmeyi](data-flow-sink.md) kullanın.
