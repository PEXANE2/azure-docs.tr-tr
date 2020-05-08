---
title: Eşleme veri akışında dönüştürme var
description: Azure Data Factory eşleme veri akışında var olan dönüştürmeyi kullanarak mevcut satırları denetleyin
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/07/2020
ms.openlocfilehash: 805b51bf4e6d8feab9539f660dfc72ca78b82d5c
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82982641"
---
# <a name="exists-transformation-in-mapping-data-flow"></a>Eşleme veri akışında dönüştürme var

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Var olan dönüştürme, verilerinizin başka bir kaynakta veya akışta bulunup bulunmadığını denetleyen bir satır filtreleme dönüşümünüze sahiptir. Çıkış akışı, sol akıştaki mevcut veya doğru akışta bulunmayan tüm satırları içerir. Var olan dönüştürme ```SQL WHERE EXISTS``` ve ile ```SQL WHERE NOT EXISTS```benzerdir.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4vZKz]

## <a name="configuration"></a>Yapılandırma

1. **Sağ Akış** açılan menüsünde var olan veri akışını kontrol ettiğiniz verileri seçin.
1. Mevcut **tür** ayarında verilerin mevcut olup olmadığını mı yoksa mevcut mi olduğunu belirtin.
1. **Özel bir ifade**isteyip istemediğinizi seçin.
1. Mevcut koşullarınız olarak karşılaştırmak istediğiniz anahtar sütunları seçin. Varsayılan olarak, veri akışı her akıştaki bir sütun arasında eşitlik arar. Hesaplanan bir değer ile karşılaştırmak için, sütun açılan listesinin üzerine gelin ve **hesaplanan sütun**' u seçin.

![Mevcut ayarlar](media/data-flow/exists.png "1 var")

### <a name="multiple-exists-conditions"></a>Birden çok mevcut koşul

Her akıştaki birden çok sütunu karşılaştırmak için var olan bir satırın yanındaki artı simgesine tıklayarak yeni bir mevcut koşul ekleyin. Her ek koşul bir "and" ifadesiyle birleştirilir. İki sütunu karşılaştırmak aşağıdaki ifadeyle aynıdır:

`source1@column1 == source2@column1 && source1@column2 == source2@column2`

### <a name="custom-expression"></a>Özel ifade

"Ve" ve "eşittir" dışındaki işleçleri içeren serbest biçimli bir ifade oluşturmak için **özel ifade** alanını seçin. Mavi kutuya tıklayarak veri akışı ifade Oluşturucusu aracılığıyla özel bir ifade girin.

![Özel ayarları var](media/data-flow/exists1.png "Özel var")

## <a name="broadcast-optimization"></a>Yayın iyileştirmesi

![Yayın katılımı](media/data-flow/broadcast.png "Yayın katılımı")

Birleşimler, aramalar ve mevcut dönüşümde, bir veya her iki veri akışı çalışan düğümü belleğine sığması halinde **yayını**etkinleştirerek performansı iyileştirebilirsiniz. Spark altyapısı, varsayılan olarak bir kenar yayınlanıp yayınlanmayacağını otomatik olarak karar verir. Hangi tarafın yayınlanmak üzere el ile seçmek için, **sabit**' i seçin.

Birleşimlerinizin zaman aşımı hatalarıyla çalışmadığı **durumlar dışında yayınlamayı** devre dışı bırakmanız önerilmez.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<leftStream>, <rightStream>
    exists(
        <conditionalExpression>,
        negate: { true | false },
        broadcast: { 'auto' | 'left' | 'right' | 'both' | 'off' }
    ) ~> <existsTransformationName>
```

### <a name="example"></a>Örnek

Aşağıdaki örnek, sol akış `checkForChanges` `NameNorm2` ve sağ Akış `TypeConversions`alan adlı bir mevcut dönüşümdir.  Exists koşulu, her akıştaki `NameNorm2@EmpID == TypeConversions@EmpID && NameNorm2@Region == DimEmployees@Region` `EMPID` ve `Region` sütunlarının her ikisi de eşleşiyorsa true döndüren ifadedir. Varlığını denetliyoruz, `negate` yanlış şeklindedir. En iyileştirme sekmesinde herhangi bir yayını etkinleştirmedik, bu `broadcast` nedenle değer `'none'`içeriyor.

Data Factory UX 'de, bu dönüşüm aşağıdaki görüntüye benzer şekilde görünür:

![Mevcut örnek](media/data-flow/exists-script.png "Mevcut örnek")

Bu dönüşüm için veri akışı betiği aşağıdaki kod parçacığında verilmiştir:

```
NameNorm2, TypeConversions
    exists(
        NameNorm2@EmpID == TypeConversions@EmpID && NameNorm2@Region == DimEmployees@Region,
        negate:false,
        broadcast: 'auto'
    ) ~> checkForChanges
```

## <a name="next-steps"></a>Sonraki adımlar

Benzer dönüşümler [arama](data-flow-lookup.md) ve [birleşimdir](data-flow-join.md).
