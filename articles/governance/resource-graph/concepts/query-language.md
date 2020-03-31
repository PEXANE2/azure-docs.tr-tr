---
title: Sorgu dilini anlama
description: Kaynak Grafiği tablolarını ve kullanılabilir Kusto veri türlerini, işleçlerini ve Işlevleri Azure Kaynak Grafiği ile açıklar.
ms.date: 03/07/2020
ms.topic: conceptual
ms.openlocfilehash: 2f4be4d86a340867e1ad3015ff288f98fc54cecf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78927491"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Azure Kaynak Grafiği sorgu dilini anlama

Azure Kaynak Grafiği'nin sorgu dili bir dizi işleç ve işlevi destekler. Her iş ve [Kusto Query Language (KQL)](/azure/kusto/query/index)dayalı çalışır. Kaynak Grafiği tarafından kullanılan sorgu dili hakkında bilgi edinmek [için KQL için öğretici](/azure/kusto/query/tutorial)ile başlayın.

Bu makalede, Kaynak Grafiği tarafından desteklenen dil bileşenleri ni kapsar:

- [Kaynak Grafiği tabloları](#resource-graph-tables)
- [Desteklenen KQL dil öğeleri](#supported-kql-language-elements)
- [Kaçış karakterleri](#escape-characters)

## <a name="resource-graph-tables"></a>Kaynak Grafiği tabloları

Kaynak Grafiği, Kaynak Yöneticisi kaynak türleri ve özellikleri hakkında depolayan veriler için çeşitli tablolar sağlar. Bu tablolar, ilgili `join` `union` kaynak türlerinden özellikler almak için operatörlerle veya işleçlerle birlikte kullanılabilir. Kaynak Grafiği'nde bulunan tabloların listesi aşağıda veda edilebilebilir:

|Kaynak Grafiği tabloları |Açıklama |
|---|---|
|Kaynaklar |Sorguda tanımlanmamışsa varsayılan tablo. Kaynak Yöneticisi kaynak türlerinin ve özelliklerinin çoğu burada. |
|Kaynak Konteynerleri |Abonelik (önizleme -- `Microsoft.Resources/subscriptions`) ve`Microsoft.Resources/subscriptions/resourcegroups`kaynak grubu ( ) kaynak türleri ve verileri içerir. |
|Danışman Kaynakları |' _ile ilgili_ kaynaklar içerir. `Microsoft.Advisor` |
|UyarılarYönetimKaynakları |' _ile ilgili_ kaynaklar içerir. `Microsoft.AlertsManagement` |
|Bakım Kaynakları |' _ile ilgili_ kaynaklar içerir. `Microsoft.Maintenance` |
|Güvenlik Kaynakları |' _ile ilgili_ kaynaklar içerir. `Microsoft.Security` |

Kaynak türlerini içeren tam liste için Bkz. [Başvuru: Desteklenen tablolar ve kaynak türleri.](../reference/supported-tables-resources.md)

> [!NOTE]
> _Kaynaklar_ varsayılan tablodur. _Kaynaklar_ tablosunu sorgularken, kullanılmadığı veya `join` `union` kullanılmadığı sürece tablo adının sağlanması gerekmez. Ancak, önerilen uygulama her zaman sorguda ilk tabloyu eklemektir.

Her tabloda hangi kaynak türlerinin bulunduğunu keşfetmek için portaldaki Kaynak Grafiği Gezgini'ni kullanın. Alternatif olarak, ortamınızda bulunan `<tableName> | distinct type` kaynak grafik tablosunun desteklediği kaynak türlerinin listesini almak gibi bir sorgu kullanın.

Aşağıdaki sorgu basit `join`bir . Sorgu sonucu sütunları bir araya getirir ve bu örnekte birleştirilmiş tablodaki yinelenen sütun adları, _Kaynak Kapsayıcıları_ **1**ile eklenir. _Kaynak Kapsayıcılar_ tablosunda hem abonelikler hem de kaynak grupları için türleri olduğu için, _kaynaklar_ tablosundan kaynağa katılmak için her iki tür de kullanılabilir.

```kusto
Resources
| join ResourceContainers on subscriptionId
| limit 1
```

Aşağıdaki sorgu daha karmaşık bir `join`kullanımını gösterir. Sorgu, birleştirilmiş tabloyu abonelik kaynaklarıyla `project` ve yalnızca özgün alan _aboneliğiId'i_ ve _Alt Ad_olarak yeniden adlandırılan _ad_ alanını içerecek şekilde sınırlar. Alan yeniden adlandırma, `join` _alan Kaynaklar'da_zaten var _olduğundan, onu ad1_ olarak eklemeyi önler. Özgün tablo ile `where` filtrelenir ve `project` aşağıdaki her iki tablodan sütunlar içerir. Sorgu sonucu, türünü, anahtar kasasının adını ve içinde olduğu aboneliğin adını gösteren tek bir anahtar kasasıdır.

```kusto
Resources
| where type == 'microsoft.keyvault/vaults'
| join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId
| project type, name, SubName
| limit 1
```

> [!NOTE]
> `join` Sonuçları `project`sınırlandırırken , iki tabloyu `join` ilişkilendirmek için kullanılan özellik, yukarıdaki örnekte _aboneKimliği,_ `project`.

## <a name="supported-kql-language-elements"></a>Desteklenen KQL dil öğeleri

Kaynak Grafiği tüm KQL [veri türlerini,](/azure/kusto/query/scalar-data-types/) [skaler işlevleri,](/azure/kusto/query/scalarfunctions) [skaler işleçleri](/azure/kusto/query/binoperators)ve [toplama işlevlerini](/azure/kusto/query/any-aggfunction)destekler. Belirli [tabular işleçleri](/azure/kusto/query/queries) Kaynak Grafiği tarafından desteklenir, bazıları farklı davranışlara sahiptir.

### <a name="supported-tabulartop-level-operators"></a>Desteklenen tabular/üst düzey işleçler

Kaynak Grafiği tarafından belirli örneklerle desteklenen KQL tabular işleçlerinin listesi aşağıda verilmiştir:

|KQL |Kaynak Grafiği örnek sorgusu |Notlar |
|---|---|---|
|[count](/azure/kusto/query/countoperator) |[Anahtar kasalarını say](../samples/starter.md#count-keyvaults) | |
|[distinct](/azure/kusto/query/distinctoperator) |[Belirli bir takma ad için farklı değerleri göster](../samples/starter.md#distinct-alias-values) | |
|[Genişlet -mek](/azure/kusto/query/extendoperator) |[Sanal makineleri işletim sistemi türüne göre sayma](../samples/starter.md#count-os) | |
|[Katılın](/azure/kusto/query/joinoperator) |[Abonelik adı olan anahtar kasası](../samples/advanced.md#join) |Desteklenen tatlar katılın: [innerunique](/azure/kusto/query/joinoperator#default-join-flavor), [inner](/azure/kusto/query/joinoperator#inner-join), [leftouter](/azure/kusto/query/joinoperator#left-outer-join). Tek bir `join` sorguda 3 sınırı. Yayın birleştirme gibi özel birleştirme stratejilerine izin verilmez. Tek bir tablo içinde veya _Kaynaklar_ ve _Kaynak Kapsayıcıları_ tabloları arasında kullanılabilir. |
|[Sınırı](/azure/kusto/query/limitoperator) |[Tüm genel IP adreslerini listele](../samples/starter.md#list-publicip) |Eş anlamlısı`take` |
|[mvexpand](/azure/kusto/query/mvexpandoperator) | | Eski işleç, yerine kullanın. `mv-expand` _RowLimit_ max 400. Varsayılan değer 128'dir. |
|[mv-genişletmek](/azure/kusto/query/mvexpandoperator) |[Belirli yazma konumları ile Cosmos DB liste](../samples/advanced.md#mvexpand-cosmosdb) |_RowLimit_ max 400. Varsayılan değer 128'dir. |
|[Sipariş](/azure/kusto/query/orderoperator) |[Ada göre sıralanmış kaynakları listele](../samples/starter.md#list-resources) |Eş anlamlısı`sort` |
|[Proje](/azure/kusto/query/projectoperator) |[Ada göre sıralanmış kaynakları listele](../samples/starter.md#list-resources) | |
|[proje-away](/azure/kusto/query/projectawayoperator) |[Sütunları sonuçlardan kaldırma](../samples/advanced.md#remove-column) | |
|[Sıralama](/azure/kusto/query/sortoperator) |[Ada göre sıralanmış kaynakları listele](../samples/starter.md#list-resources) |Eş anlamlısı`order` |
|[Özetle](/azure/kusto/query/summarizeoperator) |[Azure kaynaklarını sayma](../samples/starter.md#count-resources) |Yalnızca basitleştirilmiş ilk sayfa |
|[almak](/azure/kusto/query/takeoperator) |[Tüm genel IP adreslerini listele](../samples/starter.md#list-publicip) |Eş anlamlısı`limit` |
|[Sayfanın Üstü](/azure/kusto/query/topoperator) |[İlk beş sanal makineyi ada ve işletim sistemi türüne göre göster](../samples/starter.md#show-sorted) | |
|[Birliği](/azure/kusto/query/unionoperator) |[İki sorgunun sonuçlarını tek bir sonuçta birleştirme](../samples/advanced.md#unionresults) |İzin verilen tek tablo: _T_ `| union` \[ `kind=` `inner` \| `outer` \] \[ `withsource=` _ColumnName_ \] _Tablosu_. Tek bir `union` sorguda 3 bacak sınırı. Bacak masalarının `union` bulanık çözünürlüğüne izin verilmez. Tek bir tablo içinde veya _Kaynaklar_ ve _Kaynak Kapsayıcıları_ tabloları arasında kullanılabilir. |
|[where](/azure/kusto/query/whereoperator) |[Depolama içeren kaynakları göster](../samples/starter.md#show-storage) | |

## <a name="escape-characters"></a>Kaçış karakterleri

A `.` veya `$`, sorguda paketlenmiş veya kaçan veya özellik adı yanlış yorumlanır ve beklenen sonuçları sağlamaz gibi bazı özellik adları.

- `.`- Özellik adını şu şekilde sarın:`['propertyname.withaperiod']`
  
  Özellik _odata.type_saran örnek sorgu :

  ```kusto
  where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.['odata.type']
  ```

- `$`- Özellik adındaki karakterden kaçış. Kullanılan kaçış karakteri kaynak grafiğinin çalıştırılacaya bağlı.

  - **bash** - `\`

    Bash'teki özellik _ \$türünden_ kaçan örnek sorgu:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.\$type
    ```

  - **cmd** - Karakter kaçmayın. `$`

  - **Powershell** - ``` ` ```

    PowerShell'deki özellik _ \$türünden_ kaçan örnek sorgu:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.`$type
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlangıç sorgularında](../samples/starter.md)kullanılan dile bakın.
- [Gelişmiş sorgularda](../samples/advanced.md)gelişmiş kullanımlara bakın.
- Kaynakları nasıl [keşfedebilirsiniz](explore-resources.md)hakkında daha fazla bilgi edinin.