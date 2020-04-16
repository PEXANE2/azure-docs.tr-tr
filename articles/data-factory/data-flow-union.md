---
title: Veri akışı birleşimi dönüşümeşleme
description: Azure Veri Fabrikası haritalama veri akışı Yeni Şube Dönüşümü
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 02/12/2019
ms.openlocfilehash: 3bdf443a052ba8884a361774154f4d68cc180dfb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417836"
---
# <a name="azure-data-factory-mapping-data-flow-union-transformation"></a>Azure Veri Fabrikası eşleme veri akışı birliği dönüşümü

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Birleşim, birden çok veri akışını, Bu akışların SQL Birliği ile birleştirir. Her giriş akışındaki şemaların tümü, bir birleştirme anahtarına gerek kalmadan veri akışınızın içinde birleştirilir.

Hem kaynak verileri hem de veri akışınızdaki varolan dönüşümlerden gelen akışlar da dahil olmak üzere, yapılandırılan her satırın yanındaki "+" simgesini seçerek ayarlar tablosundaki n-akış sayısını birleştirebilirsiniz.

![Birlik dönüşümü](media/data-flow/union.png "Birleşim")

Bu durumda, birden çok kaynaktan (bu örnekte, üç farklı kaynak dosyası) farklı meta verileri birleştirebilir ve bunları tek bir akışta birleştirebilirsiniz:

![Birlik dönüşümüne genel bakış](media/data-flow/union111.png "Birlik 1")

Bunu başarmak için, eklemek istediğiniz tüm kaynağı ekleyerek Birlik Ayarları'na ek satırlar ekleyin. Ortak bir arama veya birleştirme anahtarına gerek yoktur:

![Birleşim dönüşüm ayarları](media/data-flow/unionsettings.png "Birlik ayarları")

Birliğinizden sonra bir Select dönüşümü ayarlarsanız, üstbilgisiz kaynaklardan adverişsiz üst üste binen alanları veya alanları yeniden adlandırabilirsiniz. Bu örnekte üç farklı kaynaktan 132 toplam sütuniçeren birleştirme meta verilerini görmek için "İncele"yi tıklatın:

![Birlik dönüşüm finali](media/data-flow/union333.png "Birlik 3")

## <a name="name-and-position"></a>Ad ve konum

"Ada göre birleşim" seçeneğini seçtiğinizde, her sütun değeri yeni bir bir leştirilmiş meta veri şemasıyla her kaynaktan karşılık gelen sütuna düşer.

"Konuma göre birleştirme" seçeneğini seçerseniz, her sütun değeri karşılık gelen her kaynaktan özgün konuma düşer ve her kaynaktan gelen verilerin aynı akışa eklendiği yeni bir veri akışıyla sonuçlanır:

![Birlik çıktısı](media/data-flow/unionoutput.png "Sendika Çıktısı")

## <a name="next-steps"></a>Sonraki adımlar

[Join](data-flow-join.md) and [Exists](data-flow-exists.md)gibi benzer dönüşümleri keşfedin.
