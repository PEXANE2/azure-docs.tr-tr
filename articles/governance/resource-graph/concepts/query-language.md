---
title: Sorgu dilini anlama
description: Kaynak grafik tablolarını ve kullanılabilir kusto veri türlerini, işleçlerini ve Azure Kaynak Graf ile kullanılabilir işlevleri açıklar.
ms.date: 03/07/2020
ms.topic: conceptual
ms.openlocfilehash: 2f4be4d86a340867e1ad3015ff288f98fc54cecf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78927491"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Azure Kaynak Grafiği sorgu dilini anlama

Azure Kaynak grafiğinin sorgu dili, bir dizi işleci ve işlevi destekler. Her iş ve [kusto sorgu diline (KQL)](/azure/kusto/query/index)göre çalışır. Kaynak Graph tarafından kullanılan sorgu dili hakkında bilgi edinmek için, [KQL öğreticisi](/azure/kusto/query/tutorial)ile başlayın.

Bu makalede kaynak Graph tarafından desteklenen dil bileşenleri ele alınmaktadır:

- [Kaynak grafik tabloları](#resource-graph-tables)
- [Desteklenen KQL dil öğeleri](#supported-kql-language-elements)
- [Kaçış karakterleri](#escape-characters)

## <a name="resource-graph-tables"></a>Kaynak grafik tabloları

Kaynak Grafiği Kaynak Yöneticisi kaynak türlerini ve bunların özelliklerini depolayan veriler için birkaç tablo sağlar. Bu tablolar, ilişkili kaynak türlerinden `join` özellikleri `union` almak için veya işleçleriyle birlikte kullanılabilir. Kaynak grafiğinde kullanılabilen tabloların listesi aşağıda verilmiştir:

|Kaynak grafik tabloları |Açıklama |
|---|---|
|Kaynaklar |Sorguda tanımlanmazsa varsayılan tablo. Çoğu Kaynak Yöneticisi kaynak türü ve özelliği burada bulunur. |
|ResourceContainers |Abonelik (Önizleme-- `Microsoft.Resources/subscriptions`) ve kaynak grubu (`Microsoft.Resources/subscriptions/resourcegroups`) kaynak türleri ve verileri içerir. |
|Danışmanlaştırın kaynakları |İle `Microsoft.Advisor` _ilgili_ kaynakları içerir. |
|AlertsManagementResources |İle `Microsoft.AlertsManagement` _ilgili_ kaynakları içerir. |
|MaintenanceResources |İle `Microsoft.Maintenance` _ilgili_ kaynakları içerir. |
|SecurityResources |İle `Microsoft.Security` _ilgili_ kaynakları içerir. |

Kaynak türleri dahil olmak üzere kapsamlı bir liste için bkz. [Başvuru: desteklenen tablolar ve kaynak türleri](../reference/supported-tables-resources.md).

> [!NOTE]
> _Kaynaklar_ varsayılan tablodur. _Kaynaklar_ tablosu sorgulanırken, veya `join` `union` kullanılmamışsa tablo adının sağlanması gerekmez. Ancak önerilen uygulama, her zaman sorguya ilk tabloyu dahil etmek için kullanılır.

Her tabloda hangi kaynak türlerinin kullanılabildiğini öğrenmek için portalda kaynak grafiği gezginini kullanın. Alternatif olarak, kaynak türlerinin bir listesini almak `<tableName> | distinct type` için gibi bir sorgu kullanın, belirtilen kaynak grafik tablosu ortamınızda var olan kaynağı destekler.

Aşağıdaki sorguda basit `join`gösterilmektedir. Sorgu sonucu sütunları bir araya ve birleştirilmiş tablodaki tüm yinelenen sütun adlarını, bu örnekteki _Resourcecontainer_ 'leri **1**' de eklenmiş şekilde harmanlar. _Resourcecontainers_ tablosunda hem abonelikler hem de kaynak grupları için türler olduğundan, _kaynak tablosundan kaynağa_ katılması için her iki tür de kullanılabilir.

```kusto
Resources
| join ResourceContainers on subscriptionId
| limit 1
```

Aşağıdaki sorguda daha karmaşık bir kullanımı gösterilmektedir `join`. Sorgu, birleştirilmiş `project` tabloyu abonelik kaynakları ile sınırlandırır ve yalnızca özgün alan _SubscriptionID_ ve _ad_ alanı, _SubName_olarak yeniden adlandırılır. Alan, kaynaklar bölümünde `join` zaten mevcut olduğundan, yeniden adlandırma, _name1_ olarak eklenmesini _Resources_önler. Özgün tablo ile `where` filtrelenmiştir ve aşağıdakiler `project` her iki tablodan sütunları içerir. Sorgu sonucu, türünü, anahtar kasasının adını ve içindeki aboneliğin adını gösteren tek bir Anahtar Kasası.

```kusto
Resources
| where type == 'microsoft.keyvault/vaults'
| join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId
| project type, name, SubName
| limit 1
```

> [!NOTE]
> İle `join` `project`sonuçları sınırlandırırken, `join` yukarıdaki örnekteki SubscriptionID, yukarıdaki örnekteki _abonelik_ , ' ye dahil olmalıdır. `project`

## <a name="supported-kql-language-elements"></a>Desteklenen KQL dil öğeleri

Kaynak Grafiği tüm KQL [veri türlerini](/azure/kusto/query/scalar-data-types/), [skaler işlevleri](/azure/kusto/query/scalarfunctions), [skaler işleçleri](/azure/kusto/query/binoperators)ve [toplama işlevlerini](/azure/kusto/query/any-aggfunction)destekler. Belirli [tablolu işleçler](/azure/kusto/query/queries) , bazılarının farklı davranışları olan kaynak Graph tarafından desteklenir.

### <a name="supported-tabulartop-level-operators"></a>Desteklenen tablolu/en üst düzey işleçler

Aşağıda belirli örneklere sahip kaynak Graph tarafından desteklenen KQL tablolu işleçlerinin listesi verilmiştir:

|KQL |Kaynak Grafiği örnek sorgusu |Notlar |
|---|---|---|
|[biriktirme](/azure/kusto/query/countoperator) |[Ana kasaları say](../samples/starter.md#count-keyvaults) | |
|[distinct](/azure/kusto/query/distinctoperator) |[Belirli bir diğer ad için farklı değerleri göster](../samples/starter.md#distinct-alias-values) | |
|[genişletmeyi](/azure/kusto/query/extendoperator) |[Sanal makineleri işletim sistemi türüne göre sayma](../samples/starter.md#count-os) | |
|[ayrılma](/azure/kusto/query/joinoperator) |[Abonelik adı olan Anahtar Kasası](../samples/advanced.md#join) |Birleşim türleri desteklenir: [ınnerunique](/azure/kusto/query/joinoperator#default-join-flavor), [Inner](/azure/kusto/query/joinoperator#inner-join), [soltouter](/azure/kusto/query/joinoperator#left-outer-join). Tek bir sorgudaki `join` 3 sınırı. Yayın katılımı gibi özel JOIN stratejilerine izin verilmez. Tek bir tablo içinde veya _kaynaklar_ Ile _resourcecontainers_ tabloları arasında kullanılabilir. |
|[sınırlı](/azure/kusto/query/limitoperator) |[Tüm genel IP adreslerini listele](../samples/starter.md#list-publicip) |Eş anlamlısı`take` |
|[mvexpand](/azure/kusto/query/mvexpandoperator) | | Eski işleç yerine kullanın `mv-expand` . _RowLimit_ en fazla 400. Varsayılan değer 128 ' dir. |
|[MV-Genişlet](/azure/kusto/query/mvexpandoperator) |[Belirli yazma konumlarına sahip Cosmos DB listeleyin](../samples/advanced.md#mvexpand-cosmosdb) |_RowLimit_ en fazla 400. Varsayılan değer 128 ' dir. |
|[siparişi](/azure/kusto/query/orderoperator) |[Ada göre sıralanan kaynakları Listele](../samples/starter.md#list-resources) |Eş anlamlısı`sort` |
|[Proje](/azure/kusto/query/projectoperator) |[Ada göre sıralanan kaynakları Listele](../samples/starter.md#list-resources) | |
|[Proje-dışarıda](/azure/kusto/query/projectawayoperator) |[Sütunları sonuçlardan kaldır](../samples/advanced.md#remove-column) | |
|[düzenine](/azure/kusto/query/sortoperator) |[Ada göre sıralanan kaynakları Listele](../samples/starter.md#list-resources) |Eş anlamlısı`order` |
|[ölçütü](/azure/kusto/query/summarizeoperator) |[Azure kaynaklarını sayma](../samples/starter.md#count-resources) |Yalnızca Basitleştirilmiş ilk sayfa |
|[almanız](/azure/kusto/query/takeoperator) |[Tüm genel IP adreslerini listele](../samples/starter.md#list-publicip) |Eş anlamlısı`limit` |
|[Sayfanın Üstü](/azure/kusto/query/topoperator) |[Ada ve işletim sistemi türlerine göre ilk beş sanal makineyi göster](../samples/starter.md#show-sorted) | |
|[birleşim](/azure/kusto/query/unionoperator) |[İki sorgudan alınan sonuçları tek bir sonuç halinde birleştirin](../samples/advanced.md#unionresults) |Tek tablo izin verildi _T_ `| union` \[ `kind=` `inner` \| `outer` : \] T\] _Table_ _ColumnName_ ColumnName tablosu. \[ `withsource=` Tek bir sorgudaki `union` 3 Tag sınırı. `union` Bacak tablolarının benzer çözümüne izin verilmez. Tek bir tablo içinde veya _kaynaklar_ Ile _resourcecontainers_ tabloları arasında kullanılabilir. |
|[olmadığı](/azure/kusto/query/whereoperator) |[Depolama içeren kaynakları göster](../samples/starter.md#show-storage) | |

## <a name="escape-characters"></a>Kaçış karakterleri

`.` Ya `$`da dahil olanlar gibi bazı özellik adları, sorgunun sarmalanması veya kaçışlanması ya da özellik adının yanlış yorumlanması ve beklenen sonuçları sağlamamalıdır.

- `.`-Özellik adını şu şekilde kaydırın:`['propertyname.withaperiod']`
  
  OData özelliğini sarmalayan örnek sorgu _. tür_:

  ```kusto
  where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.['odata.type']
  ```

- `$`-Özellik adındaki karakteri kaçış. Kullanılan kaçış karakteri, Shell kaynak grafiğine göre çalıştırılır.

  - **Bash** - `\`

    Bash içindeki özellik _ \$türünü_ iptal eden örnek sorgu:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.\$type
    ```

  - **cmd** - `$` karakterden kaçmayın.

  - **PowerShell** - ``` ` ```

    PowerShell 'deki özellik _ \$türünü_ iptal eden örnek sorgu:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.`$type
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Başlangıç sorgularında](../samples/starter.md)kullanılan dil.
- Gelişmiş [sorgularda](../samples/advanced.md)gelişmiş kullanımlar bölümüne bakın.
- [Kaynakları araştırma](explore-resources.md)hakkında daha fazla bilgi edinin.