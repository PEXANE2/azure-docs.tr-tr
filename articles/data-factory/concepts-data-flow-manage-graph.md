---
title: Veri akışı grafikleri
description: Veri fabrikası veri akış grafikleri ile nasıl çalışilir?
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/04/2019
ms.openlocfilehash: 1618011dc3c281cb2ef06ef80cfc7c8e69c4e091
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415500"
---
# <a name="mapping-data-flow-graphs"></a>Veri akışı grafiklerini eşleme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Haritalama veri akışları tasarım yüzeyi, yukarıdan aşağıya, soldan sağa veri akışları oluşturduğunuz bir "inşaat" yüzeyidir. Her dönüştürmeye artı (+) sembolü ile bağlı bir araç kutusu vardır. Serbest biçimli BIR DAG ortamında düğümleri kenarlardan bağlamak yerine iş mantığınıza odaklanın.

Aşağıda veri akışı grafiğini yönetmek için yerleşik mekanizmalar verilmiştir.

## <a name="move-nodes"></a>Düğümleri taşıma

![Toplu Dönüşüm seçenekleri](media/data-flow/agghead.png "toplayıcı üstbilgi")

Sürükle ve bırak paradigması olmadan, bir dönüşüm düğümünü "taşımanın" yolu, gelen akışı değiştirmektir. Bunun yerine, "gelen akışı" değiştirerek dönüşümleri hareket ettirebilirsiniz.

## <a name="streams-of-data-inside-of-data-flow"></a>Veri akışı içindeki veri akışları

Azure Veri Fabrikası Veri Akışı'nda akışlar veri akışını temsil eder. Dönüşüm ayarları bölmesinde bir "Gelen Akış" alanı görürsünüz. Bu, hangi gelen veri akışının bu dönüşümü beslediğini söyler. Gelen Akış adını tıklatıp başka bir veri akışı seçerek grafikteki dönüşüm düğümünüzün fiziksel konumunu değiştirebilirsiniz. Bu akıştaki sonraki tüm dönüşümlerle birlikte geçerli dönüşüm daha sonra yeni konuma taşınır.

Bir dönüşümü ondan sonra bir veya daha fazla dönüşümle hareket ettiriseniz, veri akışındaki yeni konum yeni bir dal aracılığıyla birleştirilir.

Seçtiğiniz düğümden sonra sonraki dönüşümler yoksa, yalnızca bu dönüşüm yeni konuma taşınır.

## <a name="hide-graph-and-show-graph"></a>Grafiği gizleme ve grafiği göster

Dönüşüm yapılandırmaları üzerinde çalışırken alt bölmeyi tam ekrana genişletebileceğiniz en alt yapılandırma bölmesinin en sağ tarafında bir düğme vardır. Bu, grafiğin yapılandırmalarında gezinmek için "önceki" ve "sonraki" düğmelerini kullanmanıza olanak sağlar. Grafik görünümüne geri dönmek için aşağı düğmesini tıklatın ve bölünmüş ekrana geri dönün.

## <a name="search-graph"></a>Arama grafiği

Tasarım yüzeyindeki arama düğmesiyle grafiği arayabilirsiniz.

![Arama](media/data-flow/search001.png "Arama grafiği")

## <a name="next-steps"></a>Sonraki adımlar

Veri Akışı tasarımınızı tamamladıktan sonra hata ayıklama düğmesini açın ve [doğrudan veri akışı tasarımcısında](concepts-data-flow-debug-mode.md) veya boru hattı hata [ayıklama](control-flow-execute-data-flow-activity.md)modunda hata ayıklama modunda test edin.
