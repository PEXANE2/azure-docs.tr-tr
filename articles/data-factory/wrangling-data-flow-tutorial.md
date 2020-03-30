---
title: Azure Veri Fabrikası'nda veri akışıyla mücadeleye başlama
description: Çekişmeli veri akışını kullanarak Azure Veri Fabrikası'nda veri hazırlama konusunda bir öğretici
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: 8ae109045381dba77610dedc5bb95e233b213eee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73682273"
---
# <a name="prepare-data-with-wrangling-data-flow"></a>Çekişmeli veri akışıyla veri hazırlama

## <a name="create-a-wrangling-data-flow"></a>Çekişmeli bir veri akışı oluşturma

Azure Veri Fabrikası'nda çekişmeli bir veri akışı oluşturmanın iki yolu vardır. Bunun bir yolu artı simgesini tıklatmak ve fabrika kaynakları bölmesinde **Veri Akışı'nı** seçmektir.

![Çekişme](media/wrangling-data-flow/tutorial7.png)

Diğer yöntem, boru hattı tuvalinin etkinlik bölmesindedir. Taşı **ve Dönüştür** akordeonu açın ve **Veri akışı** etkinliğini tuvale sürükleyin.

Her iki yöntemde de, açılan yan **bölmede, yeni veri akışı oluştur'u** seçin ve **Wrangling veri akışını**seçin. Tamam'a tıklayın.

![Çekişme](media/wrangling-data-flow/tutorial1.png)

## <a name="author-a-wrangling-data-flow"></a>Çekişmeli bir veri akışı yazma

Çekişmeli veri akışınız için bir **Kaynak veri kümesi** ekleyin. Varolan bir veri kümesi seçebilir veya yeni bir veri kümesi oluşturabilirsiniz. Ayrıca bir lavabo veri kümesi seçebilirsiniz. Bir veya daha fazla kaynak veri kümesi seçebilirsiniz, ancak şu anda yalnızca bir lavaboya izin verilir. Lavabo veri kümesi seçmek isteğe bağlıdır, ancak en az bir kaynak veri kümesi gereklidir.

> [!NOTE]
> Sınırlı önizleme için yalnızca ADLS Gen 2 Sınırlı Metin desteklenir. 

![Çekişme](media/wrangling-data-flow/tutorial4.png)

Power Query Online mashup düzenleyicisini açmak için **Oluştur'u** tıklatın.

![Çekişme](media/wrangling-data-flow/tutorial5.png)

Kodsuz veri hazırlamayı kullanarak çekişmeli veri akışınızı yazar. Kullanılabilir işlevler listesi için [dönüşüm işlevlerine](wrangling-data-flow-functions.md) bakın/

![Çekişme](media/wrangling-data-flow/tutorial6.png)

## <a name="running-and-monitoring-a-wrangling-data-flow"></a>Çekişmeli bir veri akışının çalıştırılması ve izlenmesi

Bir boru hattı hata ayıklama işlemini yürütmek için, boru hattı tuvalinde **Hata Ayıklama'yı** tıklatın. Veri akışınızı yayımladıktan **sonra, Trigger şimdi** yayınlanan son ardışık hattın isteğe bağlı çalışmasını yürütür. Dönen veri akışları, varolan tüm Azure Veri Fabrikası tetikleyicileriyle zamanlanabilir.

![Çekişme](media/wrangling-data-flow/tutorial3.png)

Tetiklenen bir veri akışı çalışmasının çıktısını görselleştirmek için **Monitör** sekmesine gidin.

![Çekişme](media/wrangling-data-flow/tutorial2.png)

## <a name="next-steps"></a>Sonraki adımlar

[Eşleme veri akışı](tutorial-data-flow.md)oluşturmayı öğrenin.