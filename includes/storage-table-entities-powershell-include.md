---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/27/2019
ms.author: tamram
ms.openlocfilehash: 9a60c624b181a1efd2f6deebd349daa82214a8a4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188046"
---
<!--created by Robin Shahan to go in the articles for table storage w/powershell.
    There is one for Azure Table Storage and one for Azure Cosmos DB Table API -->

## <a name="managing-table-entities"></a>Tablo varlıklarını yönetme

Artık bir tablonuz olduğuna göre, tablodaki varlıkları veya satırları nasıl yönetebileceğinizi inceleyelim. 

Varlıklar üç sistem özelliği de dahil olmak üzere en fazla 255 özelliğe sahip olabilir: **Partitionkey**, **Rowkey**ve **timestamp**. **Partitionkey** ve **rowkey**değerlerini eklemekten ve güncelleştirmekten siz sorumlusunuz. Sunucu, değiştirilemeyen **zaman damgası**değerini yönetir. **Partitionkey** ve **rowkey** birlikte bir tablo içindeki her varlığı benzersiz şekilde tanımlar.

* **Partitionkey**: varlığın depolandığı bölümü belirler.
* **Rowkey**: varlığı bölüm içinde benzersiz şekilde tanımlar.

Bir varlık için en fazla 252 özel özellik tanımlayabilirsiniz. 

### <a name="add-table-entities"></a>Tablo varlıkları ekleme

**Add-AzTableRow**kullanarak bir tabloya varlık ekleyin. Bu örnekler, bölüm anahtarlarını değerleri `partition1` ve `partition2` ve satır anahtarlarıyla durum kısaltmalara eşit olarak kullanır. Her varlıktaki Özellikler `username` ve ' dir `userid` . 

```powershell
$partitionKey1 = "partition1"
$partitionKey2 = "partition2"

# add four rows 
Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey1 `
    -rowKey ("CA") -property @{"username"="Chris";"userid"=1}

Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey2 `
    -rowKey ("NM") -property @{"username"="Jessie";"userid"=2}

Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey1 `
    -rowKey ("WA") -property @{"username"="Christine";"userid"=3}

Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey2 `
    -rowKey ("TX") -property @{"username"="Steven";"userid"=4}
```

### <a name="query-the-table-entities"></a>Tablo varlıklarını sorgulama

**Get-AzTableRow** komutunu kullanarak bir tablodaki varlıkları sorgulayabilirsiniz.

> [!NOTE]
> **Get-Azurestopagetablerowall**, Get-Azurestokgetablerowbypartitionkey, **Get-Azurestokgetablerowbycolumnname**ve Get-Azurestokgetablerowbycustomfilter cmdlet 'leri kullanımdan kaldırılmıştır ve gelecekteki bir sürüm güncelleştirmesinde kaldırılacaktır. **Get-AzureStorageTableRowByPartitionKey** **Get-AzureStorageTableRowByCustomFilter**

#### <a name="retrieve-all-entities"></a>Tüm varlıkları al

```powershell
Get-AzTableRow -table $cloudTable | ft
```

Bu komut aşağıdaki tabloya benzer sonuçlar verir:

| UserID | kullanıcı adı | partition | rowkey |
|----|---------|---------------|----|
| 1 | Chris | partition2 | CA |
| 3 | Christine | partition2 | WA |
| 2 | Jessie | içindeki | NM |
| 4 | Steven | içindeki | TX |

#### <a name="retrieve-entities-for-a-specific-partition"></a>Belirli bir bölüm için varlıkları alma

```powershell
Get-AzTableRow -table $cloudTable -partitionKey $partitionKey1 | ft
```

Sonuçlar aşağıdaki tabloya benzer şekilde görünür:

| UserID | kullanıcı adı | partition | rowkey |
|----|---------|---------------|----|
| 1 | Chris | partition2 | CA |
| 3 | Christine | partition2 | WA |

#### <a name="retrieve-entities-for-a-specific-value-in-a-specific-column"></a>Belirli bir sütundaki belirli bir değer için varlıkları alma

```powershell
Get-AzTableRow -table $cloudTable `
    -columnName "username" `
    -value "Chris" `
    -operator Equal
```

Bu sorgu bir kayıt alır.

|alan|değer|
|----|----|
| UserID | 1 |
| kullanıcı adı | Chris |
| PartitionKey | partition2 |
| RowKey      | CA |

#### <a name="retrieve-entities-using-a-custom-filter"></a>Özel bir filtre kullanarak varlıkları alma 

```powershell
Get-AzTableRow `
    -table $cloudTable `
    -customFilter "(userid eq 1)"
```

Bu sorgu bir kayıt alır.

|alan|değer|
|----|----|
| UserID | 1 |
| kullanıcı adı | Chris |
| PartitionKey | partition2 |
| RowKey      | CA |

### <a name="updating-entities"></a>Varlıklar güncelleştiriliyor 

Varlıkları güncelleştirmek için üç adım vardır. İlk olarak, değiştirilecek varlığı alın. İkinci olarak, değişikliği yapın. Üçüncü olarak, **Update-AzTableRow**kullanarak değişikliği yürütün.

Username = ' JESSIE ' olan varlığı username = ' Jessie2 ' olacak şekilde güncelleştirin. Bu örnek ayrıca .NET türlerini kullanarak özel bir filtre oluşturmanın başka bir yolunu da gösterir.

```powershell
# Create a filter and get the entity to be updated.
[string]$filter = `
    [Microsoft.Azure.Cosmos.Table.TableQuery]::GenerateFilterCondition("username",`
    [Microsoft.Azure.Cosmos.Table.QueryComparisons]::Equal,"Jessie")
$user = Get-AzTableRow `
    -table $cloudTable `
    -customFilter $filter

# Change the entity.
$user.username = "Jessie2"

# To commit the change, pipe the updated record into the update cmdlet.
$user | Update-AzTableRow -table $cloudTable

# To see the new record, query the table.
Get-AzTableRow -table $cloudTable `
    -customFilter "(username eq 'Jessie2')"
```

Sonuçlar Jessie2 kaydını gösterir.

|alan|değer|
|----|----|
| UserID | 2 |
| kullanıcı adı | Jessie2 |
| PartitionKey | içindeki |
| RowKey      | NM |

### <a name="deleting-table-entities"></a>Tablo varlıklarını silme

Tablodaki bir varlığı veya tüm varlıkları silebilirsiniz.

#### <a name="deleting-one-entity"></a>Bir varlığı silme

Tek bir varlığı silmek için, bu varlığa bir başvuru alın ve bunu **Remove-AzTableRow**' a ekleyin.

```powershell
# Set filter.
[string]$filter = `
  [Microsoft.Azure.Cosmos.Table.TableQuery]::GenerateFilterCondition("username",`
  [Microsoft.Azure.Cosmos.Table.QueryComparisons]::Equal,"Jessie2")

# Retrieve entity to be deleted, then pipe it into the remove cmdlet.
$userToDelete = Get-AzTableRow `
    -table $cloudTable `
    -customFilter $filter
$userToDelete | Remove-AzTableRow -table $cloudTable

# Retrieve entities from table and see that Jessie2 has been deleted.
Get-AzTableRow -table $cloudTable | ft
```

#### <a name="delete-all-entities-in-the-table"></a>Tablodaki tüm varlıkları Sil

Tablodaki tüm varlıkları silmek için, bunları alır ve sonuçları kaldır cmdlet 'ine yönelolursunuz. 

```powershell
# Get all rows and pipe the result into the remove cmdlet.
Get-AzTableRow `
    -table $cloudTable | Remove-AzTableRow -table $cloudTable 

# List entities in the table (there won't be any).
Get-AzTableRow -table $cloudTable | ft
```
