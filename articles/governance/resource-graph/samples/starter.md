---
title: Başlangıç sorgu örnekleri
description: Kaynakları sayma, kaynakları sıralama veya belirli bir etiket gibi bazı başlangıç sorgularını çalıştırmak için Azure Kaynak Grafiği ' ni kullanın.
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/18/2019
ms.topic: quickstart
ms.service: resource-graph
ms.openlocfilehash: 431c2d5066421efdfa4725d39fc40169b80d9cb2
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72431520"
---
# <a name="starter-resource-graph-queries"></a>Başlangıç Kaynak Grafiği sorguları

Azure Kaynak Grafiği ile sorguları anlamanın il adımı, [Sorgu Dili](../concepts/query-language.md)’nin temel bir anlayışıdır. [Azure Veri Gezgini](../../../data-explorer/data-explorer-overview.md)’ne önceden aşina değilseniz, aradığınız kaynaklar için isteklerin nasıl oluşturulacağını anlamak üzere temelleri gözden geçirmeniz önerilir.

Aşağıdaki başlangıç sorgularını inceleyeceğiz:

> [!div class="checklist"]
> - [Azure kaynaklarını sayma](#count-resources)
> - [Anahtar Kasası kaynaklarını sayma](#count-keyvaults)
> - [Ada göre sıralanmış kaynakları listeleme](#list-resources)
> - [Tüm sanal makineleri ada göre azalan düzende sıralı olarak gösterme](#show-vms)
> - [Ada ve işletim sistemi türüne göre ilk beş sanal makineyi gösterme](#show-sorted)
> - [Sanal makineleri işletim sistemi türüne göre sayma](#count-os)
> - [Depolama içeren kaynakları göster](#show-storage)
> - [Tüm genel IP adreslerini listele](#list-publicip)
> - [Aboneliğe göre yapılandırılmış IP adreslerine sahip kaynakları sayma](#count-resources-by-ip)
> - [Belirli bir etiket değerine sahip kaynakları listeleme](#list-tag)
> - [Belirli bir değerine sahip tüm depolama hesaplarını listeleme](#list-specific-tag)
> - [Bir sanal makine kaynağı için diğer adları göster](#show-aliases)
> - [Belirli bir diğer ad için farklı değerleri göster](#distinct-alias-values)
> - [İlişkilendirilmemiş ağ güvenlik gruplarını göster](#unassociated-nsgs)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="language-support"></a>Dil desteği

Azure CLI (bir uzantı yoluyla) ve Azure PowerShell (bir modül yoluyla), Azure Kaynak Grafiği’ni destekler. Aşağıdaki sorgulardan herhangi birini çalıştırmadan önce ortamınızın hazır olduğundan emin olun. Seçtiğiniz kabuk ortamını yükleme ve doğrulama adımları için, bkz. [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) ve [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module).

## <a name="a-namecount-resourcescount-azure-resources"></a><a name="count-resources"/>Azure kaynaklarını sayma

Bu sorgu, erişiminiz bulunan aboneliklerde var olan Azure kaynaklarının sayısını döndürür. Ayrıca, seçtiğiniz kabukta uygun Azure Kaynak Grafı bileşenlerinin yüklü ve çalışma düzeninde olduğunu doğrulamak için iyi bir sorgudur.

```kusto
Resources
| summarize count()
```

```azurecli-interactive
az graph query -q "Resources | summarize count()"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | summarize count()"
```

## <a name="a-namecount-keyvaultscount-key-vault-resources"></a><a name="count-keyvaults">Anahtar Kasası kaynaklarını sayma

Bu sorgu döndürülen kayıt sayısını saymak için `summarize` yerine `count` kullanır. Sayıma yalnızca anahtar kasaları dahildir.

```kusto
Resources
| where type =~ 'microsoft.compute/virtualmachines'
| count
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.compute/virtualmachines' | count"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.compute/virtualmachines' | count"
```

## <a name="a-namelist-resourceslist-resources-sorted-by-name"></a>ada göre sıralanmış <a name="list-resources"/> liste kaynağı

Bu sorgu herhangi bir türdeki kaynakların yalnızca **name**, **type** ve **location** özelliklerini döndürür. `order by` kullanarak özellikleri **name** özelliğine göre artan (`asc`) düzende sıralar.

```kusto
Resources
| project name, type, location
| order by name asc
```

```azurecli-interactive
az graph query -q "Resources | project name, type, location | order by name asc"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name, type, location | order by name asc"
```

## <a name="a-nameshow-vmsshow-all-virtual-machines-ordered-by-name-in-descending-order"></a><a name="show-vms"/>Ada göre sıralanmış tüm sanal makineleri azalan sırada göster

Yalnızca sanal makineleri (`Microsoft.Compute/virtualMachines` türündeki) listelemek için sonuçlarda **type** özelliğini eşleştirebiliriz. Önceki sorguya benzer şekilde `desc`, `order by`’yi azalan olması için değiştirir. Eşleme türündeki `=~`, Kaynak Grafiği’nin büyük/küçük harfe duyarlı olmadığını bildirir.

```kusto
Resources
| project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

```azurecli-interactive
az graph query -q "Resources | project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

## <a name="a-nameshow-sortedshow-first-five-virtual-machines-by-name-and-their-os-type"></a><a name="show-sorted"/>Ilk beş sanal makineyi ada göre ve işletim sistemi türlerine göre göster

Bu sorgu `top`’i yalnızca ada göre sıralanmış beş eşleşen kaydı almak için kullanır. Azure kaynağının türü `Microsoft.Compute/virtualMachines`’dir. `project`, Azure Kaynak Grafiği’ne hangi özelliklerin dahil edileceğini bildirir.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| project name, properties.storageProfile.osDisk.osType
| top 5 by name desc
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

## <a name="a-namecount-oscount-virtual-machines-by-os-type"></a>@no__t-işletim sistemi türüne göre sanal makine sayısı

Bir önceki sorguyu oluşturmada, hâlâ `Microsoft.Compute/virtualMachines` türündeki Azure kaynaklarına göre sınırlandırıyoruz, ancak geri gönderilen kayıtların sayısını sınırlandırmıyoruz.
Bunu yerine, değerleri (bu örnekte `properties.storageProfile.osDisk.osType`) özelliğe göre gruplandırma ve toplamayı tanımlamak için `summarize` ve `count()` kullandık. Bu dizenin tam nesnede nasıl göründüğüne ilişkin bir örnek için, bkz. [ kaynakları keşfetme - sanal makine bulma](../concepts/explore-resources.md#virtual-machine-discovery).

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

Aynı sorguyu yazmanın farklı bir yolu da, bir özelliği `extend` yapmak ve sorgu içinde kullanılması için, bu durumda **os** olan geçici bir ad verilmesidir. **os** daha sonra, bir önceki örnekte olduğu gibi `summarize` ve `count()` tarafından kullanılır.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| extend os = properties.storageProfile.osDisk.osType
| summarize count() by tostring(os)
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

> [!NOTE]
> `=~`, büyük/küçük harfe duyarlı olmayan eşleşmeye izin verirken, sorgudaki özelliklerin (örneğin, **properties.storageProfile.osDisk.osType** gibi) kullanımının, durumun doğru olmasını gerektirdiğini unutmayın. Özellik yanlış durumsa, yine de bir değer döndürebilir, ancak gruplandırma veya özetleme yanlış olur.

## <a name="a-nameshow-storageshow-resources-that-contain-storage"></a><a name="show-storage"/>Depolama içeren kaynakları göster

Eşleştirilecek türü şekilde açık olarak tanımlamak yerine, bu örnek sorgu, **depo** sözcüğünü `contains` eden tüm Azure kaynaklarını bulur.

```kusto
Resources
| where type contains 'storage' | distinct type
```

```azurecli-interactive
az graph query -q "Resources | where type contains 'storage' | distinct type"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'storage' | distinct type"
```

## <a name="a-namelist-publiciplist-all-public-ip-addresses"></a>Tüm genel IP adreslerini <a name="list-publicip"/>List

Önceki sorguya benzer şekilde, **publicIPAddresses** sözcüğünü içeren bir tür olan her şeyi bulun.
Bu sorgu yalnızca **Properties. ıpaddress @no__t-** 1 @ no__t-2 ' nin yalnızca **Properties. IPAddress**ve en üst kısımdaki sonuçları @no__t
100. Seçtiğiniz kabuğa bağlı olarak tırnak işaretlerinin dışına çıkmanız gerekebilir.

```kusto
Resources
| where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| project properties.ipAddress
| limit 100
```

```azurecli-interactive
az graph query -q "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

## <a name="a-namecount-resources-by-ipcount-resources-that-have-ip-addresses-configured-by-subscription"></a>abonelik tarafından yapılandırılan IP adreslerine sahip <a name="count-resources-by-ip"/>Count kaynakları

Önceki örnek sorguyu kullanarak ve `summarize` ile `count()` ekleyerek, yapılandırılmış IP adreslerine sahip kaynakların aboneliğe göre listesini elde edebiliriz.

```kusto
Resources
| where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| summarize count () by subscriptionId
```

```azurecli-interactive
az graph query -q "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

## <a name="a-namelist-taglist-resources-with-a-specific-tag-value"></a>@no__t-belirli bir etiket değerine sahip olan kaynakları-0Listeleme

Sonuçları, etiket gibi Azure kaynak türünden başka özelliklere göre de sınırlandırabiliriz. Bu örnekte, Azure kaynaklarını **Dahili** değerine sahip **Ortam** etiket adıyla filtreliyoruz.

```kusto
Resources
| where tags.environment=~'internal'
| project name
```

```azurecli-interactive
az graph query -q "Resources | where tags.environment=~'internal' | project name"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where tags.environment=~'internal' | project name"
```

Kaynağın sahip olduğu etiketleri ve değerlerini de sağlamak için `project` sözcüğüne **tags** özelliğini ekleyin.

```kusto
Resources
| where tags.environment=~'internal'
| project name, tags
```

```azurecli-interactive
az graph query -q "Resources | where tags.environment=~'internal' | project name, tags"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where tags.environment=~'internal' | project name, tags"
```

## <a name="a-namelist-specific-taglist-all-storage-accounts-with-specific-tag-value"></a>@no__t-belirli bir etiket değerine sahip tüm depolama hesaplarını listeleme

Önceki örneğin filtre işleviyle birleştirerek Azure kaynak türünü **type** özelliğine göre filtreleyin. Bu sorgu da aramayı belirli bir etiket adına ve değerine sahip olan belirli Azure kaynağı türleriyle sınırlar.

```kusto
Resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

> [!NOTE]
> Bu örnek, eşleştirme için `=~` koşullu yerine `==` kullanır. `==` büyük küçük harfe duyarlı bir eşleşmedir.

## <a name="a-nameshow-aliasesshow-aliases-for-a-virtual-machine-resource"></a><a name="show-aliases"/>Bir sanal makine kaynağı için diğer adları göster

Azure [ilkesi diğer adları](../../policy/concepts/definition-structure.md#aliases) , Azure ilkesi tarafından kaynak uyumluluğunu yönetmek için kullanılır. Azure Kaynak Grafiği, bir kaynak türünün _diğer adlarını_ döndürebilir. Bu değerler, özel bir ilke tanımı oluştururken diğer adların geçerli değerlerini karşılaştırmak için yararlıdır. _Diğer adlar_ dizisi, bir sorgunun sonuçlarında varsayılan olarak sağlanmaz. Sonuçlara açıkça eklemek için `project aliases` kullanın.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| limit 1
| project aliases
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1 | project aliases"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1 | project aliases" | ConvertTo-Json
```

## <a name="a-namedistinct-alias-valuesshow-distinct-values-for-a-specific-alias"></a><a name="distinct-alias-values"/>Belirli bir diğer ad için ayrı değerleri göster

Diğer adların değerini tek bir kaynakta görmek faydalı olur, ancak abonelikler arasında sorgulama yapmak için Azure Kaynak grafiğinin kullanıldığı doğru değeri göstermez. Bu örnek, belirli bir diğer adın tüm değerlerine bakar ve ayrı değerleri döndürür.

```kusto
Resources
| where type=~'Microsoft.Compute/virtualMachines'
| extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType']
| distinct tostring(alias)"
```

```azurecli-interactive
az graph query -q "Resources | where type=~'Microsoft.Compute/virtualMachines' | extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType'] | distinct tostring(alias)"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type=~'Microsoft.Compute/virtualMachines' | extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType'] | distinct tostring(alias)"
```

## <a name="a-nameunassociated-nsgsshow-unassociated-network-security-groups"></a><a name="unassociated-nsgs"/> ilişkilendirilmemiş ağ güvenlik grupları göster

Bu sorgu, bir ağ arabirimiyle veya alt ağıyla ilişkilendirilmemiş ağ güvenlik gruplarını (NSG 'ler) döndürür.

```kusto
Resources
| where type =~ "microsoft.network/networksecuritygroups" and isnull(properties.networkInterfaces) and isnull(properties.subnets)
| project name, resourceGroup
| sort by name asc
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.network/networksecuritygroups' and isnull(properties.networkInterfaces) and isnull(properties.subnets) | project name, resourceGroup | sort by name asc"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.network/networksecuritygroups' and isnull(properties.networkInterfaces) and isnull(properties.subnets) | project name, resourceGroup | sort by name asc"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu dili](../concepts/query-language.md) hakkında daha fazla bilgi edinin
- [Kaynakları keşfetmeyi](../concepts/explore-resources.md) öğrenin
- Bkz. [Gelişmiş sorgular](advanced.md) örnekleri