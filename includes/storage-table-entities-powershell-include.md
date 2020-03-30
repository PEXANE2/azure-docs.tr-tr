---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/27/2019
ms.author: tamram
ms.openlocfilehash: 9a60c624b181a1efd2f6deebd349daa82214a8a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188046"
---
<!--created by Robin Shahan to go in the articles for table storage w/powershell.
    There is one for Azure Table Storage and one for Azure Cosmos DB Table API -->

## <a name="managing-table-entities"></a>Tablo varlıklarını yönetme

Artık bir tablonuz olduğuna göre, tablodaki varlıkları veya satırları nasıl yönetebildiğinize bakalım. 

Varlıklar en fazla 255 özellikleri, üç sistem özellikleri de dahil olmak üzere olabilir: **PartitionKey**, **RowKey**ve **Timestamp**. **PartitionKey** ve **RowKey**değerlerini eklemek ve güncelleştirmekten siz sorumlusunuz. Sunucu, değiştirilemeyen **Timestamp**değerini yönetir. **PartitionKey** ve **RowKey** birlikte bir tablodaki tüm varlığı benzersiz bir şekilde tanımlar.

* **PartitionKey**: Varlığın depolanan bölümü belirler.
* **RowKey**: Bölüm içindeki varlığı benzersiz bir şekilde tanımlar.

Bir varlık için en fazla 252 özel özellik tanımlayabilirsiniz. 

### <a name="add-table-entities"></a>Tablo varlıkları ekleme

**Add-AzTableRow'u**kullanarak tabloya varlıklar ekleyin. Bu örnekler, değerler `partition1` ve `partition2`, ve satır anahtarları ile bölüm tuşları kullanır devlet kısaltmaları eşit. Her varlıktaki özellikler `username` `userid`ve . 

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

### <a name="query-the-table-entities"></a>Tablo varlıklarını sorgula

**Get-AzTableRow** komutunu kullanarak tablodaki varlıkları sorgulayabilirsiniz.

> [!NOTE]
> Cmdlets **Get-AzureStorageTableRowAll**, **Get-AzureStorageTableByPartitionKey**, **Get-AzureStorageTableRowByColumnName**ve **Get-AzureStorageTableTableByCustomFilter** azalır ve gelecekteki sürüm güncelleştirmesinde kaldırılır.

#### <a name="retrieve-all-entities"></a>Tüm varlıkları alma

```powershell
Get-AzTableRow -table $cloudTable | ft
```

Bu komut aşağıdaki tabloya benzer sonuçlar verir:

| Userıd | kullanıcı adı | partition | rowkey |
|----|---------|---------------|----|
| 1 | Chris | bölüm1 | CA |
| 3 | Christine | bölüm1 | WA |
| 2 | Jessie | bölüm2 | NM |
| 4 | Steven | bölüm2 | TX |

#### <a name="retrieve-entities-for-a-specific-partition"></a>Belirli bir bölüm için varlıkları alma

```powershell
Get-AzTableRow -table $cloudTable -partitionKey $partitionKey1 | ft
```

Sonuçlar aşağıdaki tabloya benzer:

| Userıd | kullanıcı adı | partition | rowkey |
|----|---------|---------------|----|
| 1 | Chris | bölüm1 | CA |
| 3 | Christine | bölüm1 | WA |

#### <a name="retrieve-entities-for-a-specific-value-in-a-specific-column"></a>Belirli bir sütundaki belirli bir değere ait varlıkları alma

```powershell
Get-AzTableRow -table $cloudTable `
    -columnName "username" `
    -value "Chris" `
    -operator Equal
```

Bu sorgu bir kayıt alır.

|alan|value|
|----|----|
| Userıd | 1 |
| kullanıcı adı | Chris |
| PartitionKey | bölüm1 |
| RowKey      | CA |

#### <a name="retrieve-entities-using-a-custom-filter"></a>Özel bir filtre kullanarak varlıkları alma 

```powershell
Get-AzTableRow `
    -table $cloudTable `
    -customFilter "(userid eq 1)"
```

Bu sorgu bir kayıt alır.

|alan|value|
|----|----|
| Userıd | 1 |
| kullanıcı adı | Chris |
| PartitionKey | bölüm1 |
| RowKey      | CA |

### <a name="updating-entities"></a>Varlıkları güncelleştirme 

Varlıkları güncelleştirmek için üç adım vardır. İlk olarak, değiştirmek için varlık alın. İkincisi, değişikliği yap. Üçüncü olarak, **Update-AzTableRow**kullanarak değişikliği işlemek.

Kullanıcı adı = 'Jessie' kullanıcı adı = 'Jessie2' olması için varlığı güncelleştirin. Bu örnek, .NET türlerini kullanarak özel bir filtre oluşturmanın başka bir yolunu da gösterir.

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

Sonuçlar Jessie2 rekorunu gösteriyor.

|alan|value|
|----|----|
| Userıd | 2 |
| kullanıcı adı | Jessie2 |
| PartitionKey | bölüm2 |
| RowKey      | NM |

### <a name="deleting-table-entities"></a>Tablo varlıklarını silme

Tablodaki bir varlığı veya tüm varlıkları silebilirsiniz.

#### <a name="deleting-one-entity"></a>Bir varlığı silme

Tek bir varlığı silmek için, bu varlığa bir başvuru alın ve **onu Remove-AzTableRow'a**boruyla getirin.

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

#### <a name="delete-all-entities-in-the-table"></a>Tablodaki tüm varlıkları silme

Tablodaki tüm varlıkları silmek için, bunları alır ve sonuçları cmdlet'i kaldırabilirsiniz. 

```powershell
# Get all rows and pipe the result into the remove cmdlet.
Get-AzTableRow `
    -table $cloudTable | Remove-AzTableRow -table $cloudTable 

# List entities in the table (there won't be any).
Get-AzTableRow -table $cloudTable | ft
```
