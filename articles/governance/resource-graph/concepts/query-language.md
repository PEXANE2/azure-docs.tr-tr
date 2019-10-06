---
title: Sorgu dilini anlayın
description: Kullanılabilir kusto işleçlerini ve Azure Kaynak Graph ile kullanılabilir işlevleri açıklar.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/22/2019
ms.topic: conceptual
ms.service: resource-graph
ms.openlocfilehash: 54bb0b4f21752b91ceb9d4004c153ff4d95006aa
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2019
ms.locfileid: "71976771"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Azure Kaynak Grafiği sorgu dilini anlama

Azure Kaynak grafiğinin sorgu dili, bir dizi işleci ve işlevi destekler. Her iş ve [Azure Veri Gezgini](../../../data-explorer/data-explorer-overview.md)göre çalışır.

Kaynak Graph tarafından kullanılan sorgu dili hakkında bilgi almanın en iyi yolu Azure Veri Gezgini [sorgu dilinin](/azure/kusto/query/index)belgeleriyle başlamadır. Dilin nasıl yapılandırıldığı ve desteklenen çeşitli işleçlerin ve işlevlerin birlikte nasıl çalıştığı hakkında bilgi sağlar.

## <a name="supported-tabular-operators"></a>Desteklenen tablolu işleçler

Kaynak grafiğinde desteklenen tablolu işleçlerin listesi aşağıda verilmiştir:

- [count](/azure/kusto/query/countoperator)
- [ayrı](/azure/kusto/query/distinctoperator)
- [genişletmeyi](/azure/kusto/query/extendoperator)
- [sınırlı](/azure/kusto/query/limitoperator)
- [sıralama ölçütü](/azure/kusto/query/orderoperator)
- [Proje](/azure/kusto/query/projectoperator)
- [Proje-dışarıda](/azure/kusto/query/projectawayoperator)
- [örnekli](/azure/kusto/query/sampleoperator)
- [örnek-benzersiz](/azure/kusto/query/sampledistinctoperator)
- [sıralama ölçütü](/azure/kusto/query/sortoperator)
- [ölçütü](/azure/kusto/query/summarizeoperator)
- [almanız](/azure/kusto/query/takeoperator)
- [Sayfanın Üstü](/azure/kusto/query/topoperator)
- [üst iç içe](/azure/kusto/query/topnestedoperator)
- [en büyük-hitters](/azure/kusto/query/tophittersoperator)
- [olmadığı](/azure/kusto/query/whereoperator)

## <a name="supported-functions"></a>Desteklenen işlevler

Kaynak grafiğinde desteklenen işlevlerin listesi aşağıdadır:

- [önce ()](/azure/kusto/query/agofunction)
- [buildschema ()](/azure/kusto/query/buildschema-aggfunction)
- [strcat ()](/azure/kusto/query/strcatfunction)
- [isnotempty ()](/azure/kusto/query/isnotemptyfunction)
- [ToString ()](/azure/kusto/query/tostringfunction)
- [zip ()](/azure/kusto/query/zipfunction)

## <a name="escape-characters"></a>Kaçış karakterleri

@No__t-0 veya `$` gibi bazı özellik adlarının, sorguda sarmalanması veya kaçışlanması ya da özellik adının yanlış yorumlanması ve beklenen sonuçları sağlamamalıdır.

- `.`-özellik adını şu şekilde kaydırın: `['propertyname.withaperiod']`
  
  OData özelliğini sarmalayan örnek sorgu _. tür_:

  ```kusto
  where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.['odata.type']
  ```

- `$`-özellik adındaki karakteri kaçış. Kullanılan kaçış karakteri, Shell kaynak grafiğine göre çalıştırılır.

  - **Bash** -  @ no__t-2

    Bash içinde _\$türünde_ Özellik kaçışlayan örnek sorgu:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.\$type
    ```

  - **cmd** -1 karakterden kaçmayın @no__t.

  - **PowerShell** -  @ no__t-2

    PowerShell 'de _\$türünde_ özelliğin kaçış örnek sorgusu:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.`$type
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Başlangıç sorgularında](../samples/starter.md) kullanılan dil
- Gelişmiş [sorgularda](../samples/advanced.md) gelişmiş kullanımlar konusuna bakın
- [Kaynakları keşfetmeyi](explore-resources.md) öğrenin