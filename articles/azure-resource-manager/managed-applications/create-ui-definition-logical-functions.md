---
title: UI tanımı mantıksal işlevleri oluştur
description: Mantıksal işlemler gerçekleştirme işlevlerini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: 00d2f0eeb5d353c8ebd7ad30f6866f890d6cb42e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099921"
---
# <a name="createuidefinition-logical-functions"></a>Createuıdefinition mantıksal işlevleri

Bu işlevler, koşullu ifadelerde kullanılabilir. Bazı işlevler tüm JSON veri türlerini desteklemiyor olabilir.

## <a name="and"></a>ve

`true`Tüm parametrelerin değerlendirmesi halinde döndürür `true` . Bu işlev, yalnızca Boole türünden iki veya daha fazla parametreyi destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[and(equals(0, 0), equals('web', 'web'), less(1, 2))]"
```

Aşağıdaki örnek şunu döndürür `false` :

```json
"[and(equals(0, 0), greater(1, 2))]"
```

## <a name="coalesce"></a>Coalesce

İlk null olmayan parametrenin değerini döndürür. Bu işlev tüm JSON veri türlerini destekler.

`element1`Ve tanımsız olduğunu varsayın `element2` . Aşağıdaki örnek şunu döndürür `"Contoso"` :

```json
"[coalesce(steps('demoStep').element1, steps('demoStep').element2, 'Contoso')]"
```

Bu işlev özellikle, sayfa yüklendikten sonra Kullanıcı eylemi nedeniyle gerçekleşen isteğe bağlı çağrının bağlamında faydalıdır. Bir örnek, Kullanıcı arabirimindeki bir alana yerleştirilmiş olan kısıtlamaların, **Başlangıçta görünür olmayan** başka bir alanın şu anda seçili olan değerine bağlıdır. Bu durumda, `coalesce()` Kullanıcı alanla etkileşime geçtiğinde istenen etkiye sahip olsa da, işlevin sayfa yükleme sırasında sözdizimsel olarak geçerli olmasına izin vermek için kullanılabilir.

`DropDown`Kullanıcının birkaç farklı veritabanı türünden seçmesine izin veren bunu göz önünde bulundurun:

```
{
    "name": "databaseType",
    "type": "Microsoft.Common.DropDown",
    "label": "Choose database type",
    "toolTip": "Choose database type",
    "defaultValue": "Oracle Database",
    "visible": "[bool(steps('section_database').connectToDatabase)]"
    "constraints": {
        "allowedValues": [
            {
                "label": "Azure Database for PostgreSQL",
                "value": "postgresql"
            },
            {
                "label": "Oracle Database",
                "value": "oracle"
            },
            {
                "label": "Azure SQL",
                "value": "sqlserver"
            }
        ],
        "required": true
    },
```

Bu alanın geçerli seçilen değerindeki başka bir alanın eylemini yapmak için, `coalesce()` burada gösterildiği gibi kullanın:

```
"regex": "[concat('^jdbc:', coalesce(steps('section_database').databaseConnectionInfo.databaseType, ''), '.*$')]",
```

Bu, `databaseType` Başlangıçta görünür olmadığından ve bu nedenle bir değere sahip olmadığından gereklidir. Bu, tüm ifadenin doğru değerlendirilmesine neden olur.

## <a name="equals"></a>equals

`true`Her iki parametrenin de aynı tür ve değere sahip olup olmadığını döndürür. Bu işlev tüm JSON veri türlerini destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[equals(0, 0)]"
```

Aşağıdaki örnek şunu döndürür `true` :

```json
"[equals('web', 'web')]"
```

Aşağıdaki örnek şunu döndürür `false` :

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

## <a name="greater"></a>büyüktür

`true`İlk parametrenin ikinci parametreden kesinlikle büyük olup olmadığını döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `false` :

```json
"[greater(1, 2)]"
```

Aşağıdaki örnek şunu döndürür `true` :

```json
"[greater('9', '10')]"
```

## <a name="greaterorequals"></a>greaterOrEquals

`true`İlk parametrenin ikinci parametreye eşit veya ondan büyük olup olmadığını döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[greaterOrEquals(2, 2)]"
```

## <a name="if"></a>if

Bir koşulun doğru veya yanlış olduğunu temel alarak bir değer döndürür. İlk parametre sınanacak durumdur. İkinci parametre, koşul doğru ise döndürülecek değerdir. Üçüncü parametre, koşul yanlış ise döndürülecek değerdir.

Aşağıdaki örnek döndürür `yes` .

```json
"[if(equals(42, mul(6, 7)), 'yes', 'no')]"
```

## <a name="less"></a>daha az

`true`İlk parametrenin ikinci parametreden kesinlikle daha az olup olmadığını döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[less(1, 2)]"
```

Aşağıdaki örnek şunu döndürür `false` :

```json
"[less('9', '10')]"
```

## <a name="lessorequals"></a>lessOrEquals

`true`İlk parametrenin ikinci parametreye eşit veya ondan küçük olup olmadığını döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[lessOrEquals(2, 2)]"
```

## <a name="not"></a>not

`true`Parametresi olarak değerlendirilirse döndürür `false` . Bu işlev yalnızca Boole türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[not(false)]"
```

Aşağıdaki örnek şunu döndürür `false` :

```json
"[not(equals(0, 0))]"
```

## <a name="or"></a>veya

`true`Parametrelerden en az biri olarak değerlendiriliyorsa, döndürür `true` . Bu işlev, yalnızca Boole türünden iki veya daha fazla parametreyi destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[or(equals(0, 0), equals('web', 'web'), less(1, 2))]"
```

Aşağıdaki örnek şunu döndürür `true` :

```json
"[or(equals(0, 0), greater(1, 2))]"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager giriş için bkz. [Azure Resource Manager genel bakış](../management/overview.md).
