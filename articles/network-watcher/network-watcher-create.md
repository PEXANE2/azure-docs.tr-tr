---
title: Azure Ağ İzleyicisi örneği oluşturma | Microsoft Dokümanlar
description: Bir Azure bölgesinde Ağ İzleyicisini nasıl etkinleştirin öğrenin.
services: network-watcher
documentationcenter: na
author: damendo
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 77812a3765a027152c957f6dbb7c9b3811a2278f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77191188"
---
# <a name="create-an-azure-network-watcher-instance"></a>Azure Ağ İzleyicisi örneği oluşturma

Ağ İzleyicisi, Azure'da ve Azure'dan ağ senaryosu düzeyindeki koşulları izlemenizi ve tanılamanızı sağlayan bölgesel bir hizmettir. Senaryo düzeyi izleme, sorunları ağ düzeyi görünümüsonundan sonuna kadar tanılamanızı sağlar. Ağ İzleyicisi ile kullanılabilen ağ tanılama ve görselleştirme araçları, Azure'da ağınızı anlamanıza, tanılamanıza ve öngörüler elde etmeye yardımcı olur. Ağ İzleyicisi, bir Ağ İzleyicisi kaynağı nın oluşturulması yoluyla etkinleştirilir. Bu kaynak, Ağ İzleyicisi özelliklerini kullanmanıza olanak tanır.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="network-watcher-is-automatically-enabled"></a>Ağ İzleyicisi otomatik olarak etkinleştirilir
Aboneliğinizde sanal ağı oluşturur veya güncelleştirirken, Sanal Ağınızın bölgesinde Ağ İzleyicisi otomatik olarak etkinleştirilir. Ağ İzleyicisi'nin otomatik olarak etkinleştirmesi kaynaklarınızı veya ilişkili ücretleri hiçbir şekilde etkilemez.

#### <a name="opt-out-of-network-watcher-automatic-enablement"></a>Ağ İzleyicisi otomatik etkinleştirme devre dışı bırakma
Network Watcher otomatik etkinleştirme dışında bırakmak istiyorsanız, bunu aşağıdaki komutları çalıştırarak yapabilirsiniz:

> [!WARNING]
> Network Watcher otomatik etkinleştirme devre dışı bırakmak kalıcı bir değişikliktir. Bir kez devre dışı kaldığınızda [desteğe başvurmadan](https://azure.microsoft.com/support/options/) kabul edemezsiniz

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName DisableNetworkWatcherAutocreation -ProviderNamespace Microsoft.Network
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

```azurecli-interactive
az feature register --name DisableNetworkWatcherAutocreation --namespace Microsoft.Network
az provider register -n Microsoft.Network
```



## <a name="create-a-network-watcher-in-the-portal"></a>Portalda Ağ İzleyicisi Oluşturma

Tüm **Hizmetler** > **Ağ** > **Ağı İzleyicisine**gidin. Ağ İzleyicisi'ni etkinleştirmek istediğiniz tüm abonelikleri seçebilirsiniz. Bu eylem, kullanılabilen her bölgede bir Ağ İzleyicisi oluşturur.

![ağ izleyicisi oluşturma](./media/network-watcher-create/figure1.png)

Portalı kullanarak Ağ İzleyicisi'ni etkinleştirdiğinizde, Ağ İzleyicisi örneğinin adı otomatik olarak *region_name* örneğin etkinleştirildiği Azure bölgesine karşılık geldiği *NetworkWatcher_region_name* olarak ayarlanır. Örneğin, Batı Orta ABD bölgesinde etkinleştirilen bir Ağ *İzleyicisi NetworkWatcher_westcentralus*olarak adlandırılır.

Ağ İzleyicisi örneği otomatik olarak *NetworkWatcherRG*adlı bir kaynak grubunda oluşturulur. Kaynak grubu zaten yoksa oluşturulur.

Bir Ağ İzleyiciörneğinin adını ve içine yerleştirildiği kaynak grubunu özelleştirmek istiyorsanız, powershell, Azure CLI, REST API veya armclient yöntemlerini kullanarak sonraki bölümlerde açıklanan. Her seçenekte, içinde bir Ağ İzleyicisi oluşturmadan önce kaynak grubunun bulunması gerekir.  

## <a name="create-a-network-watcher-with-powershell"></a>PowerShell ile Ağ İzleyicisi Oluşturma

Ağ İzleyicisi örneğini oluşturmak için aşağıdaki örneği çalıştırın:

```powershell
New-AzNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-azure-cli"></a>Azure CLI ile Ağ İzleyicisi Oluşturma

Ağ İzleyicisi örneğini oluşturmak için aşağıdaki örneği çalıştırın:

```azurecli
az network watcher configure --resource-group NetworkWatcherRG --locations westcentralus --enabled
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>REST API ile Ağ İzleyicisi Oluşturma

ARMclient PowerShell kullanarak REST API aramak için kullanılır. ARMClient, [ARMClient'da Chocolatey'de](https://chocolatey.org/packages/ARMClient) bulunur.

### <a name="log-in-with-armclient"></a>ARMClient ile giriş yapın

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a>Ağ izleyicisini oluşturma

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="delete-a-network-watcher-in-the-portal"></a>Portaldaki Ağ İzleyicisini silme

Tüm **Hizmetler** > **Ağ** > **Ağı İzleyicisine**gidin.

Zaten orada değilseniz genel bakış sekmesini seçin. Ağ izleyicisini devre dışı bırakmak istediğiniz aboneliği seçmek için açılır dosyayı kullanın.
Oka tıklayarak seçtiğiniz aboneliğin bölgeler listesini genişletin. Herhangi bir şekilde, bağlam menüsüne erişmek için sağdaki 3 noktayı kullanın.
Devre dışı bırakmak için "Ağ izleyicisini devre dışı bırak" seçeneğini tıklayın. Bu adımı onaylamanız istenecektir. Devam etmek için Evet’e tıklayın.
Portalda, bunu her abonelikteki her bölge için ayrı ayrı yapmanız gerekir.


## <a name="delete-a-network-watcher-with-powershell"></a>PowerShell ile Ağ İzleyicisini Silme

Ağ İzleyicisi'nin bir örneğini silmek için aşağıdaki örneği çalıştırın:

```powershell
New-AzResourceGroup -Name NetworkWatcherRG -Location westcentralus
New-AzNetworkWatcher -Name NetworkWatcher_westcentralus -ResourceGroup NetworkWatcherRG -Location westcentralus
Remove-AzNetworkWatcher -Name NetworkWatcher_westcentralus -ResourceGroup NetworkWatcherRG
```

## <a name="next-steps"></a>Sonraki adımlar

Artık Ağ İzleyicisi'nin bir örneğine sahip olduğunuza göre, mevcut özellikler hakkında bilgi edinin:

* [Topoloji](network-watcher-topology-overview.md)
* [Paket yakalama](network-watcher-packet-capture-overview.md)
* [IP akışı doğrulama](network-watcher-ip-flow-verify-overview.md)
* [Sonraki atlama](network-watcher-next-hop-overview.md)
* [Güvenlik grubu görünümü](network-watcher-security-group-view-overview.md)
* [NSG akış günlüğü](network-watcher-nsg-flow-logging-overview.md)
* [Sanal Ağ Ağ Ağ Geçidi sorun giderme](network-watcher-troubleshoot-overview.md)
