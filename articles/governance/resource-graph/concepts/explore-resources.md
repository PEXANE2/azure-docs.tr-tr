---
title: Azure kaynaklarınızı inceleme
description: Kaynaklarınızı keşfetmek ve bunların nasıl bağlandıklarını keşfetmek için Kaynak Grafiği sorgu dilini kullanmayı öğrenin.
ms.date: 10/18/2019
ms.topic: conceptual
ms.openlocfilehash: 0c191915b8c558d80ffef554ef758a35157e035c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76156990"
---
# <a name="explore-your-azure-resources-with-resource-graph"></a>Kaynak Grafiği ile Azure kaynaklarınızı keşfedin

Azure Kaynak Grafiği, Azure kaynaklarınızı hızlı ve ölçekte keşfetme ve keşfetme olanağı sağlar. Hızlı yanıtlar için tasarlanmış olan bu özellik, ortamınız ve Azure kaynaklarınızı oluşturan özellikler hakkında bilgi edinmenin harika bir yoludur.

## <a name="explore-virtual-machines"></a>Sanal makineleri keşfedin

Azure'da yaygın bir kaynak sanal bir makinedir. Kaynak türü olarak, sanal makinelerin sorgulanabilecek birçok özelliği vardır. Her özellik, tam olarak aradığınız kaynağı filtreleme veya bulma seçeneği sunar.

### <a name="virtual-machine-discovery"></a>Sanal makine bulma

Ortamımızdan tek bir VM almak ve döndürülen özelliklere bakmak için basit bir sorguyla başlayalım.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| limit 1
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1" | ConvertTo-Json -Depth 100
```

> [!NOTE]
> Azure PowerShell `Search-AzGraph` cmdlet varsayılan olarak bir **PSCustomObject** döndürür. Çıktının Azure CLI tarafından döndürülenle aynı görünmesi `ConvertTo-Json` için cmdlet kullanılır. **Derinlik** için varsayılan değer _2'dir._ _100'e_ ayarlanması, döndürülen tüm düzeyleri dönüştürmelidir.

JSON sonuçları aşağıdaki örneğe benzer şekilde yapılandırılmıştır:

```json
[
  {
    "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
    "kind": "",
    "location": "westus2",
    "managedBy": "",
    "name": "ContosoVM1",
    "plan": {},
    "properties": {
      "hardwareProfile": {
        "vmSize": "Standard_B2s"
      },
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "/subscriptions/<subscriptionId>/MyResourceGroup/providers/Microsoft.Network/networkInterfaces/contosovm1535",
            "resourceGroup": "MyResourceGroup"
          }
        ]
      },
      "osProfile": {
        "adminUsername": "localAdmin",
        "computerName": "ContosoVM1",
        "secrets": [],
        "windowsConfiguration": {
          "enableAutomaticUpdates": true,
          "provisionVMAgent": true
        }
      },
      "provisioningState": "Succeeded",
      "storageProfile": {
        "dataDisks": [],
        "imageReference": {
          "offer": "WindowsServer",
          "publisher": "MicrosoftWindowsServer",
          "sku": "2016-Datacenter",
          "version": "latest"
        },
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage",
          "diskSizeGB": 127,
          "managedDisk": {
            "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
            "resourceGroup": "MyResourceGroup",
            "storageAccountType": "Premium_LRS"
          },
          "name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
          "osType": "Windows"
        }
      },
      "vmId": "bbb9b451-6dc7-4117-bec5-c971eb1118c6"
    },
    "resourceGroup": "MyResourceGroup",
    "sku": {},
    "subscriptionId": "<subscriptionId>",
    "tags": {},
    "type": "microsoft.compute/virtualmachines"
  }
]
```

Özellikler bize sanal makine kaynağının kendisi, SKU, OS, diskler, etiketler ve üyesi olduğu kaynak grubu ve abonelik her şeyi hakkında ek bilgiler söyler.

### <a name="virtual-machines-by-location"></a>Konuma göre sanal makineler

Sanal makineler kaynağı hakkında öğrendiklerimizi alarak, konum **özelliğini** kullanarak tüm sanal makineleri konuma göre sayalım. Sorguyu güncelleştirmek için sınırı kaldırırız ve konum değerlerinin sayısını özetleyeceğiz.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by location
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by location"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by location"
```

JSON sonuçları aşağıdaki örneğe benzer şekilde yapılandırılmıştır:

```json
[
  {
    "count_": 386,
    "location": "eastus"
  },
  {
    "count_": 215,
    "location": "southcentralus"
  },
  {
    "count_": 59,
    "location": "westus"
  }
]
```

Artık her Azure bölgesinde kaç sanal makinemiz olduğunu görebiliyoruz.

### <a name="virtual-machines-by-sku"></a>Sanal makineler SKU tarafından

Orijinal sanal makine özelliklerine geri dönersek, **Standard_B2s**SKU boyutuna sahip tüm sanal makineleri bulmaya çalışalım. JSON döndü baktığımızda, biz **özellikleri.hardwareprofile.vmsize**saklanır bakın. Bu boyutla eşleşen tüm VM'leri bulmak ve sadece VM ve bölgenin adını döndürmek için sorguyu güncelleştireceğiz.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s'
| project name, resourceGroup
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | project name, resourceGroup"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | project name, resourceGroup"
```

### <a name="virtual-machines-connected-to-premium-managed-disks"></a>Premium yönetilen disklere bağlı sanal makineler

Bu **Standard_B2s** sanal makinelere iliştirilen premium yönetilen disklerin ayrıntılarını almak istersek, sorguyu genişleterek bize bu yönetilen disklerin kaynak kimliğini verebiliriz.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s'
| extend disk = properties.storageProfile.osDisk.managedDisk
| where disk.storageAccountType == 'Premium_LRS'
| project disk.id
```

> [!NOTE]
> SKU almak için başka bir yolu **diğer adlar** özelliği **Microsoft.Compute/virtualMachines/sku.name**kullanarak olurdu. Diğer [adları göster'i](../samples/starter.md#show-aliases) görün ve farklı takma ad değerlerini örnekler [göster.](../samples/starter.md#distinct-alias-values)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | extend disk = properties.storageProfile.osDisk.managedDisk | where disk.storageAccountType == 'Premium_LRS' | project disk.id"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | extend disk = properties.storageProfile.osDisk.managedDisk | where disk.storageAccountType == 'Premium_LRS' | project disk.id"
```

Sonuç, disk i'lerinin listesidir.

### <a name="managed-disk-discovery"></a>Yönetilen disk bulma

Önceki sorgudaki ilk kayıtla, yönetilen diskte bulunan ve ilk sanal makineye eklenen özellikleri inceleriz. Güncelleştirilmiş sorgu disk kimliğini kullanır ve türünü değiştirir.

Örneğin önceki sorgudan örnek çıktı:

```json
[
  {
    "disk_id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166"
  }
]
```

```kusto
Resources
| where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'
```

Sorguyu çalıştırmadan önce, **türün** artık **Microsoft.Compute/disks**olması gerektiğini nasıl bilebilirdik?
Tam kimlik bakarsanız, dize parçası olarak **/providers/Microsoft.Compute/disks/** görürsünüz. Bu dize parçası, hangi türü arayacağınıza dair size bir ipucu verir. Alternatif bir yöntem türüne göre sınırı kaldırmak ve bunun yerine yalnızca kimlik alanına göre arama olacaktır. Kimlik benzersiz olduğundan, yalnızca bir kayıt döndürülür ve üzerindeki **tür** özelliği bu ayrıntıyı sağlar.

> [!NOTE]
> Bu örneğin çalışması için, kimlik alanını kendi ortamınızdan gelen bir sonuçla değiştirmeniz gerekir.

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'"
```

JSON sonuçları aşağıdaki örneğe benzer şekilde yapılandırılmıştır:

```json
[
  {
    "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
    "kind": "",
    "location": "westus2",
    "managedBy": "",
    "name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
    "plan": {},
    "properties": {
      "creationData": {
        "createOption": "Empty"
      },
      "diskSizeGB": 127,
      "diskState": "ActiveSAS",
      "provisioningState": "Succeeded",
      "timeCreated": "2018-09-14T12:17:32.2570000Z"
    },
    "resourceGroup": "MyResourceGroup",
    "sku": {
      "name": "Premium_LRS",
      "tier": "Premium"
    },
    "subscriptionId": "<subscriptionId>",
    "tags": {
      "environment": "prod"
    },
    "type": "microsoft.compute/disks"
  }
]
```

## <a name="explore-virtual-machines-to-find-public-ip-addresses"></a>Genel IP adreslerini bulmak için sanal makineleri keşfedin

Bu sorgu kümesi ilk olarak sanal makinelere bağlı tüm ağ arabirimlerini (NIC) kaynaklarını bulur ve depolar. Ardından sorgular, genel bir IP adresi olan her IP adresi kaynağını bulmak ve bu değerleri depolamak için NIC'lerin listesini kullanır. Son olarak, sorgular ortak IP adreslerinin bir listesini sağlar.

```azurecli-interactive
# Use Resource Graph to get all NICs and store in the 'nics.txt' file
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project nic = tostring(properties['networkProfile']['networkInterfaces'][0]['id']) | where isnotempty(nic) | distinct nic | limit 20" --output table | tail -n +3 > nics.txt

# Review the output of the query stored in 'nics.txt'
cat nics.txt
```

```azurepowershell-interactive
# Use Resource Graph to get all NICs and store in the $nics variable
$nics = Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project nic = tostring(properties['networkProfile']['networkInterfaces'][0]['id']) | where isnotempty(nic) | distinct nic | limit 20"

# Review the output of the query stored in the variable
$nics.nic
```

NIC'e bağlı genel bir IP adresinin bulunduğu ilgili ağ arabirimi kaynakları ayrıntılarını almak için bir sonraki sorgudaki dosyayı (Azure CLI) veya değişkeni (Azure PowerShell) kullanın.

```azurecli-interactive
# Use Resource Graph with the 'nics.txt' file to get all related public IP addresses and store in 'publicIp.txt' file
az graph query -q="Resources | where type =~ 'Microsoft.Network/networkInterfaces' | where id in ('$(awk -vORS="','" '{print $0}' nics.txt | sed 's/,$//')') | project publicIp = tostring(properties['ipConfigurations'][0]['properties']['publicIPAddress']['id']) | where isnotempty(publicIp) | distinct publicIp" --output table | tail -n +3 > ips.txt

# Review the output of the query stored in 'ips.txt'
cat ips.txt
```

```azurepowershell-interactive
# Use Resource Graph  with the $nics variable to get all related public IP addresses and store in $ips variable
$ips = Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Network/networkInterfaces' | where id in ('$($nics.nic -join "','")') | project publicIp = tostring(properties['ipConfigurations'][0]['properties']['publicIPAddress']['id']) | where isnotempty(publicIp) | distinct publicIp"

# Review the output of the query stored in the variable
$ips.publicIp
```

Son olarak, ilgili nesne ve ekrandan gerçek genel IP adresini almak için dosyada (Azure CLI) veya değişkende (Azure PowerShell) depolanan genel IP adresi kaynaklarının listesini kullanın.

```azurecli-interactive
# Use Resource Graph with the 'ips.txt' file to get the IP address of the public IP address resources
az graph query -q="Resources | where type =~ 'Microsoft.Network/publicIPAddresses' | where id in ('$(awk -vORS="','" '{print $0}' ips.txt | sed 's/,$//')') | project ip = tostring(properties['ipAddress']) | where isnotempty(ip) | distinct ip" --output table
```

```azurepowershell-interactive
# Use Resource Graph with the $ips variable to get the IP address of the public IP address resources
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Network/publicIPAddresses' | where id in ('$($ips.publicIp -join "','")') | project ip = tostring(properties['ipAddress']) | where isnotempty(ip) | distinct ip"
```

Operatörle tek bir sorguda bu adımları nasıl gerçekleştireceklerini görmek için, [ağ arabirimi ve genel IP](../samples/advanced.md#join-vmpip) örneği içeren Sanal Makineler Listesi'ne bakın. `join`

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu dili](query-language.md)hakkında daha fazla bilgi edinin.
- [Başlangıç sorgularında](../samples/starter.md)kullanılan dile bakın.
- [Gelişmiş sorgularda](../samples/advanced.md)gelişmiş kullanımlara bakın.
