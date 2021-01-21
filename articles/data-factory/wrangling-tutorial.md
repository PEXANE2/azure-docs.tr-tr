---
title: Azure Data Factory denetimi veri akışı ile çalışmaya başlama
description: Denetimi veri akışını kullanarak Azure Data Factory veri hazırlama hakkında bir öğretici
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/19/2021
ms.openlocfilehash: f32739b06920f6b20dc87b8e1fbd2884c323a859
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634324"
---
# <a name="prepare-data-with-data-wrangling"></a>Veri denetimi ile veri hazırlama

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Data Factory 'deki veri denetimi, ADF 'de yerel olarak etkileşimli Power Query Mash-ups oluşturmanızı ve ardından bunları ADF işlem hattının içinde ölçeklendirmenizi sağlar.

> [!NOTE]
> ADF 'deki Power Query Acity Şu anda genel önizlemede

## <a name="create-a-power-query-activity"></a>Power Query etkinliği oluşturma

Azure Data Factory Power Query oluşturmanın iki yolu vardır. Tek yönlü, artı simgesine tıklamanız ve fabrika kaynakları bölmesinde **veri akışı** ' nı seçmesidir.

> [!NOTE]
> Daha önce veri akış iş akışında veri denetimi özelliği bulunur. Şimdi, veri denetimi Mash 'i şuradan oluşturacaksınız ```New > Power query```

![Fabrika kaynakları bölmesinde Power Query gösteren ekran görüntüsü.](media/data-flow/power-query-wrangling.png)

Diğer yöntem, işlem hattı tuvalinin Etkinlikler bölmesinde bulunur. **Power Query** Accordion açın ve **Power Query** etkinliğini tuval üzerine sürükleyin.

![Wrangling veri akışı seçeneğini vurgulayan ekran görüntüsü.](media/data-flow/power-query-activity.png)

## <a name="author-a-wrangling-data-flow"></a>Denetimi veri akışı yazma

Power Query Mash için bir **kaynak veri kümesi** ekleyin. Mevcut bir veri kümesini seçebilir ya da yeni bir veri kümesi oluşturabilirsiniz. Havuz veri kümesi de seçebilirsiniz. Bir veya daha fazla kaynak veri kümesi seçebilirsiniz, ancak şu anda yalnızca bir havuza izin verilir. Bir havuz veri kümesi seçilmesi isteğe bağlıdır, ancak en az bir kaynak veri kümesi gereklidir.

![Denetimi](media/wrangling-data-flow/tutorial4.png)

Power Query çevrimiçi mashup düzenleyicisini açmak için **Oluştur** ' a tıklayın.

![Power Query çevrimiçi karma düzenleyici 'yi açan Oluştur düğmesini gösteren ekran görüntüsü.](media/wrangling-data-flow/tutorial5.png)

Kod içermeyen veri hazırlama kullanarak denetimi veri akışınızı yazın. Kullanılabilir işlevlerin listesi için bkz. [dönüştürme işlevleri](wrangling-functions.md). ADF, ```M``` ADF veri akışı Spark ortamını kullanarak Power Query ölçekli bir şekilde yürütebilmeniz için betiği bir veri akışı betiğine çevirir.

![Denetimi veri akışınızı yazma sürecini gösteren ekran görüntüsü.](media/wrangling-data-flow/tutorial6.png)

## <a name="running-and-monitoring-a-wrangling-data-flow"></a>Bir denetimi veri akışını çalıştırma ve izleme

Bir denetimi veri akışının işlem hattı hata ayıklama çalıştırmasını yürütmek için işlem hattı tuvalinde **Hata Ayıkla** ' ya tıklayın. Veri akışınızı yayımladığınızda, **tetikleyici artık** son yayımlanan işlem hattının isteğe bağlı bir çalıştırmasını yürütür. Wrangling veri akışları, tüm mevcut Azure Data Factory tetikleyicilerle zaman alabilir.

![Bir denetimi veri akışının nasıl ekleneceğini gösteren ekran görüntüsü.](media/wrangling-data-flow/tutorial3.png)

Tetiklenen bir denetimi veri akışı etkinliği çalışmasının çıkışını görselleştirmek için **izleyici** sekmesine gidin.

![Tetiklenen bir denetimi veri akışı etkinliğinin çıkışını gösteren ekran görüntüsü.](media/wrangling-data-flow/tutorial2.png)

## <a name="next-steps"></a>Sonraki adımlar

[Eşleme veri akışı oluşturmayı](tutorial-data-flow.md)öğrenin.
