---
title: Başlangıç sorgu örnekleri
description: Kaynakları sayma, kaynak siparişi verme veya belirli bir etiketle bazı başlangıç sorgularını çalıştırmak için Azure Kaynak Grafiği'ni kullanın.
ms.date: 11/21/2019
ms.topic: sample
ms.openlocfilehash: b966d8c239cb6ff706c967174bcea23bf25de374
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238401"
---
# <a name="starter-resource-graph-query-samples"></a>Başlangıç Kaynak Grafiği sorgu örnekleri

Azure Kaynak Grafiği ile sorguları anlamanın il adımı, [Sorgu Dili](../concepts/query-language.md)’nin temel bir anlayışıdır. [Kusto Sorgu Dili'ni (KQL)](/azure/kusto/query/index)zaten bilmiyorsanız, aradığınız kaynaklar için istekleri nasıl oluşturabileceğinizi anlamak [için KQL için öğreticiyi](/azure/kusto/query/tutorial) gözden geçirmeniz önerilir.

Aşağıdaki başlangıç sorgularını inceleyeceğiz:

> [!div class="checklist"]
> - [Azure kaynaklarını sayma](#count-resources)
> - [Anahtar kasa kaynaklarını sayma](#count-keyvaults)
> - [Ada göre sıralanmış kaynakları listele](#list-resources)
> - [Ada göre sıralanmış tüm sanal makineleri azalan sırada göster](#show-vms)
> - [İlk beş sanal makineyi ada ve işletim sistemi türüne göre göster](#show-sorted)
> - [Sanal makineleri işletim sistemi türüne göre sayma](#count-os)
> - [Depolama içeren kaynakları göster](#show-storage)
> - [Tüm genel IP adreslerini listele](#list-publicip)
> - [Aboneliğe göre yapılandırılmış IP adreslerine sahip kaynakları sayma](#count-resources-by-ip)
> - [Belirli bir etiket değerine sahip kaynakları listele](#list-tag)
> - [Belirli etiket değerine sahip tüm depolama hesaplarını listele](#list-specific-tag)
> - [Sanal makine kaynağı için diğer adları göster](#show-aliases)
> - [Belirli bir takma ad için farklı değerleri göster](#distinct-alias-values)
> - [İlişkili olmayan ağ güvenlik gruplarını göster](#unassociated-nsgs)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

## <a name="language-support"></a>Dil desteği

Azure CLI (bir uzantı yoluyla) ve Azure PowerShell (bir modül yoluyla), Azure Kaynak Grafiği’ni destekler. Aşağıdaki sorgulardan herhangi birini çalıştırmadan önce ortamınızın hazır olduğundan emin olun. Seçtiğiniz kabuk ortamını yükleme ve doğrulama adımları için, bkz. [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) ve [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module).

## <a name="count-azure-resources"></a><a name="count-resources" />Azure kaynaklarını sayma

Bu sorgu, erişiminiz bulunan aboneliklerde var olan Azure kaynaklarının sayısını döndürür. Ayrıca, seçtiğiniz kabukta uygun Azure Kaynak Grafı bileşenlerinin yüklü ve çalışma düzeninde olduğunu doğrulamak için iyi bir sorgudur.

```kusto
Resources
| summarize count()
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | summarize count()"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | summarize count()"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20summarize%20count()" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20summarize%20count()" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20summarize%20count()" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="count-key-vault-resources"></a><a name="count-keyvaults" />Anahtar kasa kaynaklarını sayma

Bu sorgu, `summarize` döndürülen kayıt sayısını saymak yerine kullanır. `count` Sayıma yalnızca anahtar kasaları dahildir.

```kusto
Resources
| where type =~ 'microsoft.keyvault/vaults'
| count
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.keyvault/vaults' | count"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.keyvault/vaults' | count"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'microsoft.keyvault%2Fvaults'%20%7C%20count" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'microsoft.keyvault%2Fvaults'%20%7C%20count" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'microsoft.keyvault%2Fvaults'%20%7C%20count" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="list-resources-sorted-by-name"></a><a name="list-resources" />Ada göre sıralanmış kaynakları listeleme

Bu sorgu herhangi bir türdeki kaynakların yalnızca **name**, **type** ve **location** özelliklerini döndürür. `order by` kullanarak özellikleri **name** özelliğine göre artan (`asc`) düzende sıralar.

```kusto
Resources
| project name, type, location
| order by name asc
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | project name, type, location | order by name asc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name, type, location | order by name asc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20project%20name%2C%20type%2C%20location%20%7C%20order%20by%20name%20asc" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20project%20name%2C%20type%2C%20location%20%7C%20order%20by%20name%20asc" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20project%20name%2C%20type%2C%20location%20%7C%20order%20by%20name%20asc" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="show-all-virtual-machines-ordered-by-name-in-descending-order"></a><a name="show-vms" />İsme göre sıralanmış tüm sanal makineleri azalan düzende göster

Yalnızca sanal makineleri (`Microsoft.Compute/virtualMachines` türündeki) listelemek için sonuçlarda **type** özelliğini eşleştirebiliriz. Önceki sorguya benzer şekilde `desc`, `order by`’yi azalan olması için değiştirir. Eşleme türündeki `=~`, Kaynak Grafiği’nin büyük/küçük harfe duyarlı olmadığını bildirir.

```kusto
Resources
| project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20project%20name%2C%20location%2C%20type%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20order%20by%20name%20desc" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20project%20name%2C%20location%2C%20type%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20order%20by%20name%20desc" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20project%20name%2C%20location%2C%20type%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20order%20by%20name%20desc" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="show-first-five-virtual-machines-by-name-and-their-os-type"></a><a name="show-sorted" />Ada ve işletim sistemi türüne göre ilk beş sanal makineyi gösterme

Bu sorgu `top`’i yalnızca ada göre sıralanmış beş eşleşen kaydı almak için kullanır. Azure kaynağının türü `Microsoft.Compute/virtualMachines`’dir. `project`, Azure Kaynak Grafiği’ne hangi özelliklerin dahil edileceğini bildirir.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| project name, properties.storageProfile.osDisk.osType
| top 5 by name desc
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20project%20name%2C%20properties.storageProfile.osDisk.osType%20%7C%20top%205%20by%20name%20desc" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20project%20name%2C%20properties.storageProfile.osDisk.osType%20%7C%20top%205%20by%20name%20desc" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20project%20name%2C%20properties.storageProfile.osDisk.osType%20%7C%20top%205%20by%20name%20desc" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="count-virtual-machines-by-os-type"></a><a name="count-os" />Sanal makineleri işletim sistemi türüne göre sayma

Bir önceki sorguyu oluşturmada, hâlâ `Microsoft.Compute/virtualMachines` türündeki Azure kaynaklarına göre sınırlandırıyoruz, ancak geri gönderilen kayıtların sayısını sınırlandırmıyoruz.
Bunu yerine, değerleri (bu örnekte `properties.storageProfile.osDisk.osType`) özelliğe göre gruplandırma ve toplamayı tanımlamak için `summarize` ve `count()` kullandık. Bu dizenin tam nesnede nasıl göründüğüne ilişkin bir örnek için, bkz. [ kaynakları keşfetme - sanal makine bulma](../concepts/explore-resources.md#virtual-machine-discovery).

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20summarize%20count()%20by%20tostring(properties.storageProfile.osDisk.osType)" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20summarize%20count()%20by%20tostring(properties.storageProfile.osDisk.osType)" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20summarize%20count()%20by%20tostring(properties.storageProfile.osDisk.osType)" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

Aynı sorguyu yazmanın farklı bir yolu da, bir özelliği `extend` yapmak ve sorgu içinde kullanılması için, bu durumda **os** olan geçici bir ad verilmesidir. **os** daha sonra, bir önceki örnekte olduğu gibi `summarize` ve `count()` tarafından kullanılır.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| extend os = properties.storageProfile.osDisk.osType
| summarize count() by tostring(os)
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20extend%20os%20%3D%20properties.storageProfile.osDisk.osType%20%7C%20summarize%20count()%20by%20tostring(os)" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20extend%20os%20%3D%20properties.storageProfile.osDisk.osType%20%7C%20summarize%20count()%20by%20tostring(os)" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20extend%20os%20%3D%20properties.storageProfile.osDisk.osType%20%7C%20summarize%20count()%20by%20tostring(os)" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

> [!NOTE]
> `=~`, büyük/küçük harfe duyarlı olmayan eşleşmeye izin verirken, sorgudaki özelliklerin (örneğin, **properties.storageProfile.osDisk.osType** gibi) kullanımının, durumun doğru olmasını gerektirdiğini unutmayın. Özellik yanlış durumdaysa, null veya yanlış bir değer döndürülür ve gruplandırma veya özetleme yanlış olur.

## <a name="show-resources-that-contain-storage"></a><a name="show-storage" />Depolama içeren kaynakları göster

Eşleştirilecek türü şekilde açık olarak tanımlamak yerine, bu örnek sorgu, **depo** sözcüğünü `contains` eden tüm Azure kaynaklarını bulur.

```kusto
Resources
| where type contains 'storage' | distinct type
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type contains 'storage' | distinct type"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'storage' | distinct type"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20contains%20'storage'%20%7C%20distinct%20type" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20contains%20'storage'%20%7C%20distinct%20type" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20contains%20'storage'%20%7C%20distinct%20type" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="list-all-public-ip-addresses"></a><a name="list-publicip" />Tüm genel IP adreslerini listele

Önceki sorguya benzer şekilde, **publicIPAddresses** sözcüğünü içeren bir tür olan her şeyi bulun.
Bu sorgu, yalnızca **properties.ipAddress**
`isnotempty`, yalnızca **properties.ipAddress**' i döndürmek ve `limit` sonuçları en üstteki sonuçlara eklemek için bu deseni genişletir
100. Seçtiğiniz kabuğa bağlı olarak tırnak işaretlerinin dışına çıkmanız gerekebilir.

```kusto
Resources
| where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| project properties.ipAddress
| limit 100
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20contains%20'publicIPAddresses'%20and%20isnotempty(properties.ipAddress)%20%7C%20project%20properties.ipAddress%20%7C%20limit%20100" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20contains%20'publicIPAddresses'%20and%20isnotempty(properties.ipAddress)%20%7C%20project%20properties.ipAddress%20%7C%20limit%20100" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20contains%20'publicIPAddresses'%20and%20isnotempty(properties.ipAddress)%20%7C%20project%20properties.ipAddress%20%7C%20limit%20100" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="count-resources-that-have-ip-addresses-configured-by-subscription"></a><a name="count-resources-by-ip" />Aboneliğe göre yapılandırılmış IP adreslerine sahip kaynakları sayma

Önceki örnek sorguyu kullanarak ve `summarize` ile `count()` ekleyerek, yapılandırılmış IP adreslerine sahip kaynakların aboneliğe göre listesini elde edebiliriz.

```kusto
Resources
| where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| summarize count () by subscriptionId
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20contains%20'publicIPAddresses'%20and%20isnotempty(properties.ipAddress)%20%7C%20summarize%20count%20()%20by%20subscriptionId" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20contains%20'publicIPAddresses'%20and%20isnotempty(properties.ipAddress)%20%7C%20summarize%20count%20()%20by%20subscriptionId" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20contains%20'publicIPAddresses'%20and%20isnotempty(properties.ipAddress)%20%7C%20summarize%20count%20()%20by%20subscriptionId" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="list-resources-with-a-specific-tag-value"></a><a name="list-tag" />Belirli bir etiket değerine sahip kaynakları listeleme

Sonuçları, etiket gibi Azure kaynak türünden başka özelliklere göre de sınırlandırabiliriz. Bu örnekte, Azure kaynaklarını **Dahili** değerine sahip **Ortam** etiket adıyla filtreliyoruz.

```kusto
Resources
| where tags.environment=~'internal'
| project name
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where tags.environment=~'internal' | project name"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where tags.environment=~'internal' | project name"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20tags.environment%3D~'internal'%20%7C%20project%20name" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20tags.environment%3D~'internal'%20%7C%20project%20name" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20tags.environment%3D~'internal'%20%7C%20project%20name" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

Kaynağın sahip olduğu etiketleri ve değerlerini de sağlamak için `project` sözcüğüne **tags** özelliğini ekleyin.

```kusto
Resources
| where tags.environment=~'internal'
| project name, tags
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where tags.environment=~'internal' | project name, tags"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where tags.environment=~'internal' | project name, tags"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20tags.environment%3D~'internal'%20%7C%20project%20name%2C%20tags" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20tags.environment%3D~'internal'%20%7C%20project%20name%2C%20tags" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20tags.environment%3D~'internal'%20%7C%20project%20name%2C%20tags" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="list-all-storage-accounts-with-specific-tag-value"></a><a name="list-specific-tag" />Belirli bir değerine sahip tüm depolama hesaplarını listeleme

Önceki örneğin filtre işleviyle birleştirerek Azure kaynak türünü **type** özelliğine göre filtreleyin. Bu sorgu da aramayı belirli bir etiket adına ve değerine sahip olan belirli Azure kaynağı türleriyle sınırlar.

```kusto
Resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Storage%2FstorageAccounts'%20%7C%20where%20tags%5B'tag%20with%20a%20space'%5D%3D%3D'Custom%20value'" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Storage%2FstorageAccounts'%20%7C%20where%20tags%5B'tag%20with%20a%20space'%5D%3D%3D'Custom%20value'" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Storage%2FstorageAccounts'%20%7C%20where%20tags%5B'tag%20with%20a%20space'%5D%3D%3D'Custom%20value'" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

> [!NOTE]
> Bu örnek, eşleştirme için `=~` koşullu yerine `==` kullanır. `==` büyük küçük harfe duyarlı bir eşleşmedir.

## <a name="show-aliases-for-a-virtual-machine-resource"></a><a name="show-aliases" />Sanal makine kaynağı için diğer adları göster

[Azure İlkesi takma adları,](../../policy/concepts/definition-structure.md#aliases) Azure İlkesi tarafından kaynak uyumluluğunu yönetmek için kullanılır. Azure Kaynak Grafiği, kaynak türünün _diğer adlarını_ döndürebilir. Bu değerler, özel bir ilke tanımı oluştururken diğer adların geçerli değerini karşılaştırmak için yararlıdır. _Diğer adlar_ dizisi varsayılan olarak sorgu sonuçlarında sağlanmaz. Sonuçlara açıkça eklemek için kullanın. `project aliases`

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| limit 1
| project aliases
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1 | project aliases"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1 | project aliases" | ConvertTo-Json
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20limit%201%20%7C%20project%20aliases" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20limit%201%20%7C%20project%20aliases" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'Microsoft.Compute%2FvirtualMachines'%20%7C%20limit%201%20%7C%20project%20aliases" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="show-distinct-values-for-a-specific-alias"></a><a name="distinct-alias-values" />Belirli bir takma ad için farklı değerleri göster

Tek bir kaynakta takma adların değerini görmek yararlıdır, ancak abonelikler arasında sorgu yapmak için Azure Kaynak Grafiği'ni kullanmanın gerçek değerini göstermez. Bu örnek, belirli bir diğer adın tüm değerlerini bakar ve farklı değerleri döndürür.

```kusto
Resources
| where type=~'Microsoft.Compute/virtualMachines'
| extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType']
| distinct tostring(alias)
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type=~'Microsoft.Compute/virtualMachines' | extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType'] | distinct tostring(alias)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type=~'Microsoft.Compute/virtualMachines' | extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType'] | distinct tostring(alias)"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%3D~'Microsoft.Compute%2FvirtualMachines'%20%7C%20extend%20alias%20%3D%20aliases%5B'Microsoft.Compute%2FvirtualMachines%2FstorageProfile.osDisk.managedDisk.storageAccountType'%5D%20%7C%20distinct%20tostring(alias)" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%3D~'Microsoft.Compute%2FvirtualMachines'%20%7C%20extend%20alias%20%3D%20aliases%5B'Microsoft.Compute%2FvirtualMachines%2FstorageProfile.osDisk.managedDisk.storageAccountType'%5D%20%7C%20distinct%20tostring(alias)" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%3D~'Microsoft.Compute%2FvirtualMachines'%20%7C%20extend%20alias%20%3D%20aliases%5B'Microsoft.Compute%2FvirtualMachines%2FstorageProfile.osDisk.managedDisk.storageAccountType'%5D%20%7C%20distinct%20tostring(alias)" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="show-unassociated-network-security-groups"></a><a name="unassociated-nsgs" />İlişkili olmayan ağ güvenlik gruplarını göster

Bu sorgu, ağ arabirimi veya alt ağla ilişkilendirilmeyen Ağ Güvenlik Grupları'nı (NSGs) döndürür.

```kusto
Resources
| where type =~ "microsoft.network/networksecuritygroups" and isnull(properties.networkInterfaces) and isnull(properties.subnets)
| project name, resourceGroup
| sort by name asc
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.network/networksecuritygroups' and isnull(properties.networkInterfaces) and isnull(properties.subnets) | project name, resourceGroup | sort by name asc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.network/networksecuritygroups' and isnull(properties.networkInterfaces) and isnull(properties.subnets) | project name, resourceGroup | sort by name asc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

![Kaynak Grafiği gezgini simgesi](../media/resource-graph-small.png) Bu sorguyı Azure Kaynak Grafiği Gezgini'nde deneyin:

- Azure portalı: yeni pencere simgesinde bağlantıyı <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'microsoft.network%2Fnetworksecuritygroups'%20and%20isnull(properties.networkInterfaces)%20and%20isnull(properties.subnets)%20%7C%20project%20name%2C%20resourceGroup%20%7C%20sort%20by%20name%20asc" target="_blank">aç portal.azure.com</a> ![](../../media/new-window.png)
- Azure Devlet portalı: yeni pencere simgesinde <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'microsoft.network%2Fnetworksecuritygroups'%20and%20isnull(properties.networkInterfaces)%20and%20isnull(properties.subnets)%20%7C%20project%20name%2C%20resourceGroup%20%7C%20sort%20by%20name%20asc" target="_blank">bağlantıyı aç portal.azure.us</a> ![](../../media/new-window.png)
- Azure China portalı: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%20%7C%20where%20type%20%3D~%20'microsoft.network%2Fnetworksecuritygroups'%20and%20isnull(properties.networkInterfaces)%20and%20isnull(properties.subnets)%20%7C%20project%20name%2C%20resourceGroup%20%7C%20sort%20by%20name%20asc" target="_blank">portal.azure.cn</a> ![Yeni pencere simgesinde bağlantıyı aç](../../media/new-window.png)

---

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu dili](../concepts/query-language.md)hakkında daha fazla bilgi edinin.
- Kaynakları nasıl [keşfedebilirsiniz](../concepts/explore-resources.md)hakkında daha fazla bilgi edinin.
- [Gelişmiş sorgu örneklerine](advanced.md)bakın.