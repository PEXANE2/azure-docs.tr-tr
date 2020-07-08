---
title: Sorgu dilini anlama
description: Kaynak grafik tablolarını ve kullanılabilir kusto veri türlerini, işleçlerini ve Azure Kaynak Graf ile kullanılabilir işlevleri açıklar.
ms.date: 06/29/2020
ms.topic: conceptual
ms.openlocfilehash: 4c545a8a5113f800545660a3ea812b61711630c2
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85970459"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Azure Kaynak Grafiği sorgu dilini anlama

Azure Kaynak grafiğinin sorgu dili, bir dizi işleci ve işlevi destekler. Her iş ve [kusto sorgu diline (KQL)](/azure/kusto/query/index)göre çalışır. Kaynak Graph tarafından kullanılan sorgu dili hakkında bilgi edinmek için, [KQL öğreticisi](/azure/kusto/query/tutorial)ile başlayın.

Bu makalede kaynak Graph tarafından desteklenen dil bileşenleri ele alınmaktadır:

- [Kaynak grafik tabloları](#resource-graph-tables)
- [Kaynak Grafiği özel dil öğeleri](#resource-graph-custom-language-elements)
- [Desteklenen KQL dil öğeleri](#supported-kql-language-elements)
- [Kaçış karakterleri](#escape-characters)

## <a name="resource-graph-tables"></a>Kaynak grafik tabloları

Kaynak Grafiği Azure Resource Manager kaynak türlerini ve bunların özelliklerini depolayan veriler için birkaç tablo sağlar. Bu tablolar, `join` `union` ilişkili kaynak türlerinden özellikleri almak için veya işleçleriyle birlikte kullanılabilir. Kaynak grafiğinde kullanılabilen tabloların listesi aşağıda verilmiştir:

|Kaynak grafik tabloları |Açıklama |
|---|---|
|Kaynaklar |Sorguda tanımlanmazsa varsayılan tablo. Çoğu Kaynak Yöneticisi kaynak türü ve özelliği burada bulunur. |
|ResourceContainers |Abonelik (Önizleme-- `Microsoft.Resources/subscriptions` ) ve kaynak grubu ( `Microsoft.Resources/subscriptions/resourcegroups` ) kaynak türleri ve verileri içerir. |
|Danışmanlaştırın kaynakları |İle _ilgili_ kaynakları içerir `Microsoft.Advisor` . |
|AlertsManagementResources |İle _ilgili_ kaynakları içerir `Microsoft.AlertsManagement` . |
|HealthResources |İle _ilgili_ kaynakları içerir `Microsoft.ResourceHealth` . |
|MaintenanceResources |İle _ilgili_ kaynakları içerir `Microsoft.Maintenance` . |
|SecurityResources |İle _ilgili_ kaynakları içerir `Microsoft.Security` . |

Kaynak türleri dahil olmak üzere kapsamlı bir liste için bkz. [Başvuru: desteklenen tablolar ve kaynak türleri](../reference/supported-tables-resources.md).

> [!NOTE]
> _Kaynaklar_ varsayılan tablodur. _Kaynaklar_ tablosu sorgulanırken, `join` veya kullanılmamışsa tablo adının sağlanması gerekmez `union` . Ancak önerilen uygulama, her zaman sorguya ilk tabloyu dahil etmek için kullanılır.

Her tabloda hangi kaynak türlerinin kullanılabildiğini öğrenmek için portalda kaynak grafiği gezginini kullanın. Alternatif olarak, `<tableName> | distinct type` kaynak türlerinin bir listesini almak için gibi bir sorgu kullanın, belirtilen kaynak grafik tablosu ortamınızda var olan kaynağı destekler.

Aşağıdaki sorguda basit gösterilmektedir `join` . Sorgu sonucu sütunları bir araya ve birleştirilmiş tablodaki tüm yinelenen sütun adlarını, bu örnekteki _Resourcecontainer_ 'leri **1**' de eklenmiş şekilde harmanlar. _Resourcecontainers_ tablosunda hem abonelikler hem de kaynak grupları için türler olduğundan, _kaynak tablosundan kaynağa_ katılması için her iki tür de kullanılabilir.

```kusto
Resources
| join ResourceContainers on subscriptionId
| limit 1
```

Aşağıdaki sorguda daha karmaşık bir kullanımı gösterilmektedir `join` . Sorgu, birleştirilmiş tabloyu abonelik kaynakları ile sınırlandırır ve `project` yalnızca özgün alan _SubscriptionID_ ve _ad_ alanı, _SubName_olarak yeniden adlandırılır. Alan, `join` _kaynaklar_bölümünde zaten mevcut olduğundan, yeniden adlandırma, _name1_ olarak eklenmesini önler. Özgün tablo ile filtrelenmiştir `where` ve aşağıdakiler `project` her iki tablodan sütunları içerir. Sorgu sonucu, türünü, anahtar kasasının adını ve içindeki aboneliğin adını gösteren tek bir Anahtar Kasası.

```kusto
Resources
| where type == 'microsoft.keyvault/vaults'
| join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId
| project type, name, SubName
| limit 1
```

> [!NOTE]
> `join`İle sonuçları sınırlandırırken `project` , `join` Yukarıdaki örnekteki SubscriptionID, yukarıdaki örnekteki _abonelik_ , ' ye dahil olmalıdır `project` .

## <a name="resource-graph-custom-language-elements"></a>Kaynak Grafiği özel dil öğeleri

### <a name="shared-query-syntax-preview"></a><a name="shared-query-syntax"></a>Paylaşılan sorgu sözdizimi (Önizleme)

Önizleme özelliği olarak, [paylaşılan sorguya](../tutorials/create-share-query.md) doğrudan kaynak Graph sorgusunda erişilebilir. Bu senaryo, standart sorguları paylaşılan sorgular olarak oluşturmayı ve bunları yeniden kullanmayı mümkün kılar. Bir kaynak grafiği sorgusunda paylaşılan bir sorgu çağırmak için `{{shared-query-uri}}` söz dizimini kullanın. Paylaşılan sorgunun URI 'SI, bu sorgunun **Ayarlar** sayfasındaki paylaşılan SORGUNUN _kaynak kimliğidir_ . Bu örnekte, paylaşılan sorgu URI 'imiz `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SharedQueries/providers/Microsoft.ResourceGraph/queries/Count VMs by OS` .
Bu URI, başka bir sorguda başvurmak istediğimiz paylaşılan sorgunun abonelik, kaynak grubu ve tam adını gösterir. Bu sorgu, öğreticide oluşturulan bir sorgu ile aynıdır [: sorgu oluşturma ve paylaşma](../tutorials/create-share-query.md).

> [!NOTE]
> Paylaşılan bir sorguya paylaşılan sorgu olarak başvuran bir sorgu kaydedemezsiniz.

Örnek 1: yalnızca paylaşılan sorguyu kullanın

Bu kaynak Graph sorgusunun sonuçları, paylaşılan sorguda depolanan sorgu ile aynıdır.

```kusto
{{/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SharedQueries/providers/Microsoft.ResourceGraph/queries/Count VMs by OS}}
```

Örnek 2: paylaşılan sorguyu daha büyük bir sorgunun parçası olarak dahil etme

Bu sorgu önce paylaşılan sorguyu kullanır ve ardından `limit` sonuçları daha fazla kısıtlamak için kullanır.

```kusto
{{/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SharedQueries/providers/Microsoft.ResourceGraph/queries/Count VMs by OS}}
| where properties_storageProfile_osDisk_osType =~ 'Windows'
```

## <a name="supported-kql-language-elements"></a>Desteklenen KQL dil öğeleri

Kaynak Grafiği tüm KQL [veri türlerini](/azure/kusto/query/scalar-data-types/), [skaler işlevleri](/azure/kusto/query/scalarfunctions), [skaler işleçleri](/azure/kusto/query/binoperators)ve [toplama işlevlerini](/azure/kusto/query/any-aggfunction)destekler. Belirli [tablolu işleçler](/azure/kusto/query/queries) , bazılarının farklı davranışları olan kaynak Graph tarafından desteklenir.

### <a name="supported-tabulartop-level-operators"></a>Desteklenen tablolu/en üst düzey işleçler

Aşağıda belirli örneklere sahip kaynak Graph tarafından desteklenen KQL tablolu işleçlerinin listesi verilmiştir:

|KQL |Kaynak Grafiği örnek sorgusu |Notlar |
|---|---|---|
|[biriktirme](/azure/kusto/query/countoperator) |[Ana kasaları say](../samples/starter.md#count-keyvaults) | |
|[ayrı](/azure/kusto/query/distinctoperator) |[Belirli bir diğer ad için farklı değerleri göster](../samples/starter.md#distinct-alias-values) | |
|[genişletmeyi](/azure/kusto/query/extendoperator) |[Sanal makineleri işletim sistemi türüne göre sayma](../samples/starter.md#count-os) | |
|[ayrılma](/azure/kusto/query/joinoperator) |[Abonelik adı olan Anahtar Kasası](../samples/advanced.md#join) |Birleşim türleri desteklenir: [ınnerunique](/azure/kusto/query/joinoperator#default-join-flavor), [Inner](/azure/kusto/query/joinoperator#inner-join), [soltouter](/azure/kusto/query/joinoperator#left-outer-join). `join`Tek bir sorgudaki 3 sınırı. Yayın katılımı gibi özel JOIN stratejilerine izin verilmez. Tek bir tablo içinde veya _kaynaklar_ Ile _resourcecontainers_ tabloları arasında kullanılabilir. |
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
|[birleşim](/azure/kusto/query/unionoperator) |[İki sorgudan alınan sonuçları tek bir sonuç halinde birleştirin](../samples/advanced.md#unionresults) |Tek tablo izin verildi: _T_ `| union` \[ `kind=` `inner` \| `outer` \] \[ `withsource=` _ColumnName_ \] _tablosu_. `union`Tek bir sorgudaki 3 Tag sınırı. `union`Bacak tablolarının benzer çözümüne izin verilmez. Tek bir tablo içinde veya _kaynaklar_ Ile _resourcecontainers_ tabloları arasında kullanılabilir. |
|[olmadığı](/azure/kusto/query/whereoperator) |[Depolama içeren kaynakları göster](../samples/starter.md#show-storage) | |

## <a name="escape-characters"></a>Kaçış karakterleri

Ya da dahil olanlar gibi bazı özellik adları, `.` `$` sorgunun sarmalanması veya kaçışlanması ya da özellik adının yanlış yorumlanması ve beklenen sonuçları sağlamamalıdır.

- `.`-Özellik adını şu şekilde kaydırın:`['propertyname.withaperiod']`
  
  OData özelliğini sarmalayan örnek sorgu _. tür_:

  ```kusto
  where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.['odata.type']
  ```

- `$`-Özellik adındaki karakteri kaçış. Kullanılan kaçış karakteri, Shell kaynak grafiğine göre çalıştırılır.

  - **Bash** - `\`

    Bash içindeki özellik _ \$ türünü_ iptal eden örnek sorgu:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.\$type
    ```

  - **cmd** -karakterden kaçmayın `$` .

  - **PowerShell** - ``` ` ```

    PowerShell 'deki özellik _ \$ türünü_ iptal eden örnek sorgu:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.`$type
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Başlangıç sorgularında](../samples/starter.md)kullanılan dil.
- Gelişmiş [sorgularda](../samples/advanced.md)gelişmiş kullanımlar bölümüne bakın.
- [Kaynakları araştırma](explore-resources.md)hakkında daha fazla bilgi edinin.