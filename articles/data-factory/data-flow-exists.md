---
title: Veri akışını eşlemede dönüşüm var
description: Azure Veri Fabrikası haritalama veri akışında varolan dönüşümü kullanarak varolan satırları denetleme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.openlocfilehash: efcc45dcf3565b70305323701810c49c4a720394
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930401"
---
# <a name="exists-transformation-in-mapping-data-flow"></a>Veri akışını eşlemede dönüşüm var

Var olan dönüştürme, verilerinizin başka bir kaynakta mı yoksa akışta mı var olduğunu denetleyen bir satır filtreleme dönüşümüdür. Çıktı akışı, sol akıştaki sağ akışta var olan veya olmayan tüm satırları içerir. Var olan dönüşüm ```SQL WHERE EXISTS``` benzer ```SQL WHERE NOT EXISTS```ve .

## <a name="configuration"></a>Yapılandırma

1. **Sağ akış** açılır düşüşünde var oluşu için hangi veri akışını denetlediğinizi seçin.
1. **Varla türü** ayarında var olacak verileri mi yoksa var olmadığını mı aradığınızı belirtin.
1. **Özel ifade**isteyip istemediğinizi seçin.
1. Var olan koşullar olarak karşılaştırmak istediğiniz anahtar sütunları seçin. Varsayılan olarak, veri akışı her akışta bir sütun arasında eşitlik arar. Hesaplanmış bir değer üzerinden karşılaştırmayapmak için sütun açılır sütunun üzerine gidin ve **Hesaplanan sütunu**seçin.

![Var ayarları var](media/data-flow/exists.png "var 1")

### <a name="multiple-exists-conditions"></a>Birden çok var olan koşullar

Her akıştan birden çok sütunu karşılaştırmak için, varolan bir satırın yanındaki artı simgesini tıklatarak yeni bir varolan koşul ekleyin. Her ek koşul bir "ve" deyimi ile birleştirilir. İki sütunu karşılaştırmak aşağıdaki ifadeyle aynıdır:

`source1@column1 == source2@column1 && source1@column2 == source2@column2`

### <a name="custom-expression"></a>Özel ifade

"ve" ve "eşittir" dışındaki işleçleri içeren bir serbest biçim **ifadesi** oluşturmak için Özel ifade alanını seçin. Mavi kutuya tıklayarak veri akışı ifade oluşturucu su aracılığıyla özel bir ifade girin.

![Özel ayarları var](media/data-flow/exists1.png "özel var")

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<leftStream>, <rightStream>
    exists(
        <conditionalExpression>,
        negate: { true | false },
        broadcast: {'none' | 'left' | 'right' | 'both'}
    ) ~> <existsTransformationName>
```

### <a name="example"></a>Örnek

Aşağıdaki örnek, sol akışı `checkForChanges` ve sağ `NameNorm2` akışı `TypeConversions`alan bir dönüştürme dir.  Var olan koşul, `NameNorm2@EmpID == TypeConversions@EmpID && NameNorm2@Region == DimEmployees@Region` her akıştaki `EMPID` sütunlar `Region` eşleşiyorsa doğru döndüren ifadedir. Var olup olmadığını kontrol `negate` ettiğimiz için yanlış. Optimize sekmesinde herhangi bir yayını etkinleştirmiyoruz, bu yüzden `broadcast` değeri `'none'`var.

Veri Fabrikası UX,bu dönüşüm aşağıdaki resim gibi görünür:

![Varvar örneği](media/data-flow/exists-script.png "Varvar örneği")

Bu dönüşüm için veri akışı komut dosyası aşağıdaki snippet bulunmaktadır:

```
NameNorm2, TypeConversions
    exists(
        NameNorm2@EmpID == TypeConversions@EmpID && NameNorm2@Region == DimEmployees@Region,
        negate:false,
        broadcast: 'none'
    ) ~> checkForChanges
```

## <a name="next-steps"></a>Sonraki adımlar

Benzer dönüşümler [Arama](data-flow-lookup.md) ve [Birleştirme'dir.](data-flow-join.md)
