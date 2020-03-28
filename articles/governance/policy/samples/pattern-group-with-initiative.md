---
title: 'Desen: İnisiyatiflerle gruplandırma ilke tanımları'
description: Bu Azure İlkesi deseni, ilke tanımlarının bir girişimde nasıl gruplayalanabildiğini bir örnek sunar
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 41c2b0cf3b8f677cdc408e85088c3ca6c2049d6b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77172859"
---
# <a name="azure-policy-pattern-group-policy-definitions"></a>Azure İlkesi deseni: grup ilkesi tanımları

Girişim, ilke tanımları grubudur. İlişkili ilke tanımlarını tek bir nesnede gruplandırarak, birden çok atama olacak tek bir atama oluşturabilirsiniz.

## <a name="sample-initiative-definition"></a>Örnek girişim tanımı

Bu girişim, her biri **tagName** ve **tagValue** parametrelerini alan iki ilke tanımı dağıtLar. Girişimin kendisi iki parametreye sahiptir: **costCenterValue** ve **productNameValue**.
Bu girişim parametrelerinin her biri gruplanmış ilke tanımlarının her birine sağlanır. Bu tasarım, gerektiğinde uygulamak için oluşturulan atama sayısını sınırlandırırken varolan ilke tanımlarının yeniden kullanımını en üst düzeye çıkarır.

:::code language="json" source="~/policy-templates/patterns/pattern-group-with-initiative.json":::

### <a name="explanation"></a>Açıklama

#### <a name="initiative-parameters"></a>Girişim parametreleri

Bir girişim, gruplanmış ilke tanımlarına aktarılan kendi parametrelerini tanımlayabilir.
Bu örnekte, hem **costCenterValue** hem de **productNameValue** girişim parametreleri olarak tanımlanır. Değerler, girişim atandığında sağlanır.

:::code language="json" source="~/policy-templates/patterns/pattern-group-with-initiative.json" range="5-18":::

#### <a name="includes-policy-definitions"></a>İlke tanımlarını içerir

Dahil edilen her ilke tanımı, ilke tanımı parametreleri kabul **ederse, ilke DefinitionId** ve **bir parametre** dizisini sağlamalıdır. Aşağıdaki snippet'te, dahil edilen ilke tanımı iki parametre alır: **tagName** ve **tagValue**. **tagName** bir edebi ile tanımlanır, ancak **tagValue** girişimi tarafından tanımlanan parametre **costCenterValue** kullanır. Bu değer geçişi yeniden kullanımı artırır.

:::code language="json" source="~/policy-templates/patterns/pattern-group-with-initiative.json" range="30-40":::

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.