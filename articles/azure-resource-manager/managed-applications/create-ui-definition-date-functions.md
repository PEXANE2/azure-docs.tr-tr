---
title: UI tanımı Tarih işlevleri oluştur
description: Tarih değerleriyle çalışırken kullanılacak işlevleri açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: 80484fd15e51bae7036c43bd3ca8fe8167387ede
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87099925"
---
# <a name="createuidefinition-date-functions"></a>Createuıdefinition Tarih işlevleri

Tarih değerleriyle çalışırken kullanılacak işlevler.

## <a name="addhours"></a>addHours

Belirtilen zaman damgasına tamsayı saat sayısını ekler.

Aşağıdaki örnek şunu döndürür `"1991-01-01T00:59:59.000Z"` :

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="addminutes"></a>addMinutes

Belirtilen zaman damgasına tamsayı cinsinden bir sayı ekler.

Aşağıdaki örnek şunu döndürür `"1991-01-01T00:00:59.000Z"` :

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

## <a name="addseconds"></a>addSeconds
Belirtilen zaman damgasına tamsayı cinsinden bir sayı ekler.

Aşağıdaki örnek şunu döndürür `"1991-01-01T00:00:00.000Z"` :

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

## <a name="utcnow"></a>utcNow

Yerel bilgisayardaki geçerli tarih ve saatin ISO 8601 biçiminde bir dize döndürür.

Aşağıdaki örnek şunu döndürebilir `"1990-12-31T23:59:59.000Z"` :

```json
"[utcNow()]"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager giriş için bkz. [Azure Resource Manager genel bakış](../management/overview.md).
