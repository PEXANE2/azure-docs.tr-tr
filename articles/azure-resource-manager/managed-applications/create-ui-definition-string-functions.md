---
title: UI tanımı dize işlevleri oluştur
description: Azure yönetilen uygulamalar için Kullanıcı arabirimi tanımları oluştururken kullanılacak dize işlevlerini açıklar
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: c662948542c36cd93f889ca045ee245c15c7bb11
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099913"
---
# <a name="createuidefinition-string-functions"></a>Createuıdefinition dize işlevleri

JSON dizeleriyle kullanılacak bu işlevler.

## <a name="concat"></a>concat

Bir veya daha fazla dizeyi birleştirir.

Örneğin, çıkış değeri `element1` Eğer ise `"Contoso"` , bu örnek dizeyi döndürür `"Demo Contoso app"` :

```json
"[concat('Demo ', steps('step1').element1, ' app')]"
```

## <a name="endswith"></a>endsWith

Bir dizenin bir değer ile bitip bitmeyeceğini belirler.

Aşağıdaki örnek, doğru döndürür.

```json
"[endsWith('tuvwxyz', 'xyz')]"
```

## <a name="guid"></a>guid

Genel olarak benzersiz bir dize (GUID) oluşturur.

Aşağıdaki örnek şöyle bir değer döndürür `"c7bc8bdc-7252-4a82-ba53-7c468679a511"` :

```json
"[guid()]"
```

## <a name="indexof"></a>IndexOf

Bir dize içindeki bir değerin ilk konumunu veya bulunmazsa-1 değerini döndürür.

Aşağıdaki örnek 2 değerini döndürür.

```json
"[indexOf('abcdef', 'cd')]"
```

## <a name="lastindexof"></a>LastIndexOf

Bir dizedeki değerin son konumunu döndürür veya bulunmazsa-1 ' i döndürür.

Aşağıdaki örnek 3 döndürür.

```json
"[lastIndexOf('test', 't')]"
```

## <a name="replace"></a>değiştirin

Geçerli dizedeki belirtilen dizenin tüm oluşumlarının başka bir dizeyle değiştirildiği bir dizeyi döndürür.

Aşağıdaki örnek şunu döndürür `"Contoso.com web app"` :

```json
"[replace('Contoso.net web app', '.net', '.com')]"
```

## <a name="startswith"></a>startsWith

Bir dizenin bir değerle başlatılıp başlatılmayacağını belirler.

Aşağıdaki örnek, doğru döndürür.

```json
"[startsWith('abcdefg', 'ab')]"
```

## <a name="substring"></a>dizeden

Belirtilen dizenin alt dizesini döndürür. Alt dize belirtilen dizinde başlar ve belirtilen uzunluğa sahiptir.

Aşağıdaki örnek şunu döndürür `"web"` :

```json
"[substring('Contoso.com web app', 12, 3)]"
```

## <a name="tolower"></a>toLower

Küçük harfe dönüştürülmüş bir dize döndürür.

Aşağıdaki örnek şunu döndürür `"contoso"` :

```json
"[toLower('CONTOSO')]"
```

## <a name="toupper"></a>toUpper

Büyük harfe dönüştürülmüş bir dize döndürür.

Aşağıdaki örnek şunu döndürür `"CONTOSO"` :

```json
"[toUpper('contoso')]"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager giriş için bkz. [Azure Resource Manager genel bakış](../management/overview.md).

