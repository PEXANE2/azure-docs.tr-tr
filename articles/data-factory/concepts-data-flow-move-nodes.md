---
title: Azure Data Factory veri akışı taşıma düğümleri
description: Azure Data Factory eşleme veri akışı diyagramında düğümleri taşıma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 631a103491b70ca016b94af01995aeeb3f29c77a
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72754677"
---
# <a name="mapping-data-flow-move-nodes"></a>Veri akışı taşıma düğümlerini eşleme



![Toplu dönüştürme seçenekleri](media/data-flow/agghead.png "Toplayıcı üst bilgisi")

Azure Data Factory veri akışı tasarım yüzeyi, yukarıdan aşağı doğru veri akışları oluşturduğunuz bir "yapım" yüzeyidir. Her bir dönüşümün bir artı (+) simgesiyle eklenmiş bir araç kutusu vardır. Serbest biçimli bir DAG ortamındaki kenarlar aracılığıyla düğümleri bağlamak yerine iş mantığınızı yoğunlaşın.

Bu nedenle, bir sürükle ve bırak paradigması olmadan, bir dönüşüm düğümünü "taşıma" yolu, gelen akışı değiştirlemektir. Bunun yerine, "gelen akışı" değiştirerek dönüşümleri taşıyabilirsiniz.

## <a name="streams-of-data-inside-of-data-flow"></a>Veri akışı içindeki veri akışları

Azure Data Factory veri akışı ' nda akışlar, veri akışını temsil eder. Dönüştürme ayarları bölmesinde, "gelen akış" alanını görürsünüz. Bu, size gelen veri akışının bu dönüşümü beslemeyi olduğunu söyler. Gelen akış adına tıklayıp başka bir veri akışı seçerek, grafikteki dönüştürme düğümünüz fiziksel konumunu değiştirebilirsiniz. O akıştaki sonraki dönüşümlerle birlikte geçerli dönüşüm yeni konuma taşınır.

Bir dönüştürmeyi sonrasında bir veya daha fazla dönüşümle taşıyorsanız, veri akışındaki yeni konum yeni bir dal ile birleştirilir.

Seçtiğiniz düğümden sonra sonraki dönüşümünüz yoksa, yalnızca o dönüşüm yeni konuma taşınır.

## <a name="next-steps"></a>Sonraki adımlar

Veri akışı tasarımınızı tamamladıktan sonra, hata ayıklama düğmesini açın ve doğrudan [veri akışı Tasarımcısı](concepts-data-flow-debug-mode.md) 'nda veya işlem [hattı hata ayıklamada](control-flow-execute-data-flow-activity.md)hata ayıklama modunda test edin.
