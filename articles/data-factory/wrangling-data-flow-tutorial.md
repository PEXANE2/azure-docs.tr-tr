---
title: Azure Data Factory denetimi veri akışı ile çalışmaya başlama
description: Denetimi veri akışını kullanarak Azure Data Factory veri hazırlama hakkında bir öğretici
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: e9a4f7266522fc17c33e2086af6cb7367bbed39e
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91650689"
---
# <a name="prepare-data-with-wrangling-data-flow"></a>Denetimi veri akışı ile veri hazırlama

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

> [!NOTE]
> Wrangling veri akışı şu anda genel önizlemede bir avılamaz

## <a name="create-a-wrangling-data-flow"></a>Denetimi veri akışı oluşturma

Azure Data Factory içinde bir denetimi veri akışı oluşturmanın iki yolu vardır. Tek yönlü, artı simgesine tıklamanız ve fabrika kaynakları bölmesinde **veri akışı** ' nı seçmesidir.

![Fabrika kaynakları bölmesinde veri akışını gösteren ekran görüntüsü.](media/wrangling-data-flow/tutorial7.png)

Diğer yöntem, işlem hattı tuvalinin Etkinlikler bölmesinde bulunur. **Taşıma ve dönüştürme** Accordion açın ve **veri akışı** etkinliğini tuvale sürükleyin.

Her iki yöntemde, açılan bölmede **Yeni veri akışı oluştur** ' u seçin ve **Wrangling veri akışı**' nı seçin. Tamam'a tıklayın.

![Wrangling veri akışı seçeneğini vurgulayan ekran görüntüsü.](media/wrangling-data-flow/tutorial1.png)

## <a name="author-a-wrangling-data-flow"></a>Denetimi veri akışı yazma

Denetimi veri akışınız için bir **kaynak veri kümesi** ekleyin. Mevcut bir veri kümesini seçebilir ya da yeni bir veri kümesi oluşturabilirsiniz. Havuz veri kümesi de seçebilirsiniz. Bir veya daha fazla kaynak veri kümesi seçebilirsiniz, ancak şu anda yalnızca bir havuza izin verilir. Bir havuz veri kümesi seçilmesi isteğe bağlıdır, ancak en az bir kaynak veri kümesi gereklidir.

> [!NOTE]
> Sınırlı önizleme için yalnızca ADLS Gen 2 ile ayrılmış metin desteklenir. 

![Denetimi](media/wrangling-data-flow/tutorial4.png)

Power Query çevrimiçi mashup düzenleyicisini açmak için **Oluştur** ' a tıklayın.

![Power Query çevrimiçi karma düzenleyici 'yi açan Oluştur düğmesini gösteren ekran görüntüsü.](media/wrangling-data-flow/tutorial5.png)

Kod içermeyen veri hazırlama kullanarak denetimi veri akışınızı yazın. Kullanılabilir işlevlerin listesi için bkz. [dönüştürme işlevleri](wrangling-data-flow-functions.md).

![Denetimi veri akışınızı yazma sürecini gösteren ekran görüntüsü.](media/wrangling-data-flow/tutorial6.png)

## <a name="running-and-monitoring-a-wrangling-data-flow"></a>Bir denetimi veri akışını çalıştırma ve izleme

Bir denetimi veri akışının işlem hattı hata ayıklama çalıştırmasını yürütmek için işlem hattı tuvalinde **Hata Ayıkla** ' ya tıklayın. Veri akışınızı yayımladığınızda, **tetikleyici artık** son yayımlanan işlem hattının isteğe bağlı bir çalıştırmasını yürütür. Wrangling veri akışları, tüm mevcut Azure Data Factory tetikleyicilerle zaman alabilir.

![Bir denetimi veri akışının nasıl ekleneceğini gösteren ekran görüntüsü.](media/wrangling-data-flow/tutorial3.png)

Tetiklenen bir denetimi veri akışı etkinliği çalışmasının çıkışını görselleştirmek için **izleyici** sekmesine gidin.

![Tetiklenen bir denetimi veri akışı etkinliğinin çıkışını gösteren ekran görüntüsü.](media/wrangling-data-flow/tutorial2.png)

## <a name="next-steps"></a>Sonraki adımlar

[Eşleme veri akışı oluşturmayı](tutorial-data-flow.md)öğrenin.