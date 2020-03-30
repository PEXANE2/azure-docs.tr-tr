---
title: Ağ güvenlik grubu için tanılama günlüğü
titlesuffix: Azure Virtual Network
description: Bir Azure ağ güvenlik grubu için olay ve kural sayacı tanı günlüklerini nasıl etkinleştireceklerini öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/04/2018
ms.author: kumud
ms.openlocfilehash: 9829e713f19ab9755e9dc79d676446c8048e09b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75751190"
---
# <a name="diagnostic-logging-for-a-network-security-group"></a>Ağ güvenlik grubu için tanılama günlüğü

Ağ güvenlik grubu (NSG), sanal ağ alt ağına, ağ arabirimine veya her ikisine de trafik sağlayan veya reddeden kurallar içerir. Bir NSG için tanısal günlüğe kaydetmeyi etkinleştirdiğinizde, aşağıdaki bilgi kategorilerini günlüğe kaydedebilirsiniz:

* **Olay:** Mac adresine bağlı olarak NSG kurallarının VM'lere uygulandığı girişler kaydedilir.
* **Kural sayacı:** Trafiği reddetmek veya izin vermek için her NSG kuralının kaç kez uygulandığına yönelik girişler içerir. Bu kuralların durumu her 60 saniyede bir toplanır.

Tanılama günlükleri yalnızca Azure Kaynak Yöneticisi dağıtım modeli aracılığıyla dağıtılan NSG'ler için kullanılabilir. Klasik dağıtım modeli aracılığıyla dağıtılan NSG'ler için tanıgünlüğe izin veremezsiniz. İki modeli daha iyi anlamak için [Azure dağıtım modellerini anlama](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bölümüne bakın.

Tanılama verileri toplamak istediğiniz *her* NSG için tanılama günlüğü ayrı olarak etkinleştirilir. Bunun yerine operasyonel veya etkinlik günlükleriyle ilgileniyorsanız, Azure [etkinlik günlüğe kaydetme'ye](../azure-monitor/platform/platform-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Tanısal günlüğe kaydetmeyi etkinleştirmek için [Azure Portalını,](#azure-portal) [PowerShell'i](#powershell)veya [Azure CLI'yi](#azure-cli) kullanabilirsiniz.

### <a name="azure-portal"></a>Azure Portalı

1. [Portalda](https://portal.azure.com)oturum aç.
2. **Tüm hizmetleri**seçin, ardından ağ *güvenlik gruplarını*yazın. **Ağ güvenlik grupları** arama sonuçlarında göründüğünde, onu seçin.
3. Günlüğe kaydetmeyi etkinleştirmek istediğiniz NSG'yi seçin.
4. **MONITORING**altında, **Tanılama günlüklerini**seçin ve ardından aşağıdaki resimde gösterildiği gibi **tanılamayı Aç'ı**seçin:

   ![Tanılamayı açma](./media/virtual-network-nsg-manage-log/turn-on-diagnostics.png)

5. **Tanılama ayarları**altında, aşağıdaki bilgileri girin veya seçin ve sonra **Kaydet'i**seçin:

    | Ayar                                                                                     | Değer                                                          |
    | ---------                                                                                   |---------                                                       |
    | Adı                                                                                        | Senin seçtiğin bir isim.  Örneğin: *myNsgDiagnostics*      |
    | **Bir depolama hesabına arşivleme**, **etkinlik merkezine akış**ve Günlük **Analitiğine Gönder** | Seçtiğiniz kadar hedef seçebilirsiniz. Her biri hakkında daha fazla bilgi edinmek [için, Günlük varış noktalarına](#log-destinations)bakın.                                                                                                                                           |
    | LOG                                                                                         | Ya da her iki günlük kategoriyi seçin. Her kategori için günlüğe kaydedilen veriler hakkında daha fazla bilgi edinmek için [Günlük kategorilerine](#log-categories)bakın.                                                                                                                                             |
6. Günlükleri görüntüleyin ve çözümle. Daha fazla bilgi için [bkz.](#view-and-analyze-logs)

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure Bulut Kabuğu'nda](https://shell.azure.com/powershell)veya bilgisayarınızdan PowerShell çalıştırarak takip eden komutları çalıştırabilirsiniz. Azure Bulut Kabuğu ücretsiz bir etkileşimli kabuktır. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell'i bilgisayarınızdan çalıştırAcaksanız, Azure PowerShell modülüne, sürüm 1.0.0 veya daha sonrasına ihtiyacınız vardır. Yüklü `Get-Module -ListAvailable Az` sürümü bulmak için bilgisayarınızda çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, `Connect-AzAccount` [gerekli izinlere](virtual-network-network-interface.md#permissions)sahip bir hesapla Azure'da oturum açmanız da gerekir.

Tanısal günlüğe kaydetmeyi etkinleştirmek için, varolan bir NSG'nin kimliğine ihtiyacınız vardır. Varolan bir NSG'niz yoksa, [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup)ile bir tane oluşturabilirsiniz.

[Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup)ile tanılama günlüğe kaydetmeyi etkinleştirmek istediğiniz ağ güvenlik grubunu alın. Örneğin, *myResourceGroup*adlı bir kaynak grubunda bulunan *myNsg* adlı bir NSG almak için aşağıdaki komutu girin:

```azurepowershell-interactive
$Nsg=Get-AzNetworkSecurityGroup `
  -Name myNsg `
  -ResourceGroupName myResourceGroup
```

Tanılama günlüklerini üç hedef türüne yazabilirsiniz. Daha fazla bilgi için, [Günlük varış noktalarına](#log-destinations)bakın. Bu makalede, günlükler örnek olarak *Log Analytics* hedefine gönderilir. [Get-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/get-azoperationalinsightsworkspace)ile varolan bir Log Analytics çalışma alanını alın. Örneğin, *myWorkspace* adlı bir kaynak grubunda *myWorkspace*adlı varolan bir çalışma alanını almak için aşağıdaki komutu girin:

```azurepowershell-interactive
$Oms=Get-AzOperationalInsightsWorkspace `
  -ResourceGroupName myWorkspaces `
  -Name myWorkspace
```

Varolan bir çalışma alanınız yoksa, [New-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace)ile bir çalışma alanı oluşturabilirsiniz.

Günlükleri etkinleştirebileceğiniz iki günlük kategorisi vardır. Daha fazla bilgi için [Günlük kategorilerini](#log-categories)görün. [Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting)ile NSG için tanısal günlük etkinleştirin. Aşağıdaki örnek, nsg ve daha önce aldığınız çalışma alanı için disleri kullanarak bir NSG için çalışma alanına hem olay hem de sayaç kategori verilerini kaydeder:

```azurepowershell-interactive
Set-AzDiagnosticSetting `
  -ResourceId $Nsg.Id `
  -WorkspaceId $Oms.ResourceId `
  -Enabled $true
```

Verileri her ikisi yerine yalnızca bir kategori veya diğer in `-Categories` günlüğe kaydetmek istiyorsanız, seçeneği önceki komuta ekleyin ve ardından *NetworkSecurityGroupEvent* veya *NetworkSecurityGroupRuleCounter'* i ekleyin. Günlük Analizi çalışma alanından farklı bir [hedefe](#log-destinations) giriş yapmak istiyorsanız, bir Azure [Depolama hesabı](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Olay Hub'ı](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)için uygun parametreleri kullanın.

Günlükleri görüntüleyin ve çözümle. Daha fazla bilgi için [bkz.](#view-and-analyze-logs)

### <a name="azure-cli"></a>Azure CLI

[Azure Bulut Kabuğu'nda](https://shell.azure.com/bash)veya Bilgisayarınızdan Azure CLI'yi çalıştırarak takip eden komutları çalıştırabilirsiniz. Azure Bulut Kabuğu ücretsiz bir etkileşimli kabuktır. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. CLI'yi bilgisayarınızdan çalıştırın, sürüm 2.0.38 veya daha sonrasına ihtiyacınız olur. Yüklü `az --version` sürümü bulmak için bilgisayarınızda çalıştırın. Yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli?view=azure-cli-latest)bakın. CLI'yi yerel olarak çalıştırıyorsanız, gerekli `az login` izinlere sahip bir hesapla Azure'da oturum açmanız da [gerekir.](virtual-network-network-interface.md#permissions)

Tanısal günlüğe kaydetmeyi etkinleştirmek için, varolan bir NSG'nin kimliğine ihtiyacınız vardır. Varolan bir NSG'niz yoksa, [az network nsg oluşturilen](/cli/azure/network/nsg#az-network-nsg-create)bir tane oluşturabilirsiniz.

[Az network nsg show](/cli/azure/network/nsg#az-network-nsg-show)ile tanılama günlüğe kaydetmeyi etkinleştirmek istediğiniz ağ güvenlik grubunu alın. Örneğin, *myResourceGroup*adlı bir kaynak grubunda bulunan *myNsg* adlı bir NSG almak için aşağıdaki komutu girin:

```azurecli-interactive
nsgId=$(az network nsg show \
  --name myNsg \
  --resource-group myResourceGroup \
  --query id \
  --output tsv)
```

Tanılama günlüklerini üç hedef türüne yazabilirsiniz. Daha fazla bilgi için, [Günlük varış noktalarına](#log-destinations)bakın. Bu makalede, günlükler örnek olarak *Log Analytics* hedefine gönderilir. Daha fazla bilgi için [Günlük kategorilerini](#log-categories)görün.

Az monitör tanı ayarları oluşturarak NSG için tanılama günlüğe [kaydetmeyi etkinleştirin.](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) Aşağıdaki örnek, hem olay hem de sayaç kategori verilerini *myWorkspace* *adlı*varolan bir çalışma alanına ve daha önce aldığınız NSG kimliğine kaydeder:

```azurecli-interactive
az monitor diagnostic-settings create \
  --name myNsgDiagnostics \
  --resource $nsgId \
  --logs '[ { "category": "NetworkSecurityGroupEvent", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "NetworkSecurityGroupRuleCounter", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --workspace myWorkspace \
  --resource-group myWorkspaces
```

Varolan bir çalışma alanınız yoksa, [Azure portalını](../azure-monitor/learn/quick-create-workspace.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [PowerShell'i](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace)kullanarak bir çalışma alanı oluşturabilirsiniz. Günlükleri etkinleştirebileceğiniz iki günlük kategorisi vardır.

Verileri yalnızca bir kategori veya diğeri için günlüğe kaydetmek istiyorsanız, önceki komutta veri günlüğe kaydetmek istemediğiniz kategoriyi kaldırın. Günlük Analizi çalışma alanından farklı bir [hedefe](#log-destinations) giriş yapmak istiyorsanız, bir Azure [Depolama hesabı](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Olay Hub'ı](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)için uygun parametreleri kullanın.

Günlükleri görüntüleyin ve çözümle. Daha fazla bilgi için [bkz.](#view-and-analyze-logs)

## <a name="log-destinations"></a>Günlük hedefleri

Tanılama verileri şu olabilir:
- Denetim veya el ile denetim için [azure depolama hesabına yazılır.](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Kaynak tanılama ayarlarını kullanarak bekletme süresini (gün içinde) belirtebilirsiniz.
- Üçüncü taraf bir hizmet veya PowerBI gibi özel analiz çözümü tarafından yutulmak üzere [etkinlik hub'ına akış.](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Monitor günlüklerine yazılır.](../azure-monitor/platform/resource-logs-collect-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="log-categories"></a>Günlük kategorileri

JSON biçimlendirilmiş veriler aşağıdaki günlük kategorileri için yazılır:

### <a name="event"></a>Olay

Olay günlüğü, MAC adresine bağlı olarak VM'lere hangi NSG kurallarının uygulandığı hakkında bilgiler içerir. Aşağıdaki veriler her olay için günlüğe kaydedilir. Aşağıdaki örnekte, veriler IP adresi 192.168.1.4 ve 00-0D-3A-92-6A-7C MAC adresi ne sahip bir sanal makine için kaydedilir:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "priority":"[PRIORITY-SPECIFIED-IN-RULE]",
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "conditions":{
            "protocols":"[PROTOCOLS-SPECIFIED-IN-RULE]",
            "destinationPortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourcePortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourceIP":"[SOURCE-IP-OR-RANGE-SPECIFIED-IN-RULE]",
            "destinationIP":"[DESTINATION-IP-OR-RANGE-SPECIFIED-IN-RULE]"
            }
        }
}
```

### <a name="rule-counter"></a>Kural sayacı

Kural sayacı günlüğü, kaynaklara uygulanan her kural hakkında bilgi içerir. Aşağıdaki örnek veriler, her kural uygulandığında günlüğe kaydedilir. Aşağıdaki örnekte, veriler IP adresi 192.168.1.4 ve 00-0D-3A-92-6A-7C MAC adresi ne sahip bir sanal makine için kaydedilir:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "matchedConnections":125
        }
}
```

> [!NOTE]
> İletişimin kaynak IP adresi günlüğe kaydedilmez. Ancak, tüm kural sayacı bilgilerinin yanı sıra iletişimi başlatan kaynak IP adresini kaydeden bir NSG için [NSG akış günlüğünü](../network-watcher/network-watcher-nsg-flow-logging-portal.md) etkinleştirebilirsiniz. NSG akış günlüğü verileri bir Azure Depolama hesabına yazılır. Azure Ağ İzleyicisi'nin [trafik analizi](../network-watcher/traffic-analytics.md) özelliğiyle verileri analiz edebilirsiniz.

## <a name="view-and-analyze-logs"></a>Günlükleri görüntüleme ve çözümleme

Tanılama günlüğü verilerini nasıl görüntüleyeceğiz öğrenmek için [Azure Tanı Günlükleri genel görünümüne](../azure-monitor/platform/platform-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın. Tanılama verilerini aşağıdakilere gönderirseniz:
- **Azure Monitor günlükleri**: Gelişmiş öngörüler için [ağ güvenliği grubu analizi](../azure-monitor/insights/azure-networking-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-security-group-analytics-solution-in-azure-monitor
) çözümünü kullanabilirsiniz. Çözüm, sanal bir makinedeki ağ arabiriminin MAC adresi başına trafiğe izin veren veya reddeden NSG kuralları için görselleştirmeler sağlar.
- **Azure Depolama hesabı**: Veriler PT1H.json dosyasına yazılır. Aşağıdakileri bulabilirsiniz:
  - Olay günlüğü aşağıdaki yolda:`insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`
  - Kural sayacı aşağıdaki şekilde günlük:`insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`

## <a name="next-steps"></a>Sonraki adımlar

- Daha önce denetim veya işlem günlükleri olarak bilinen [Etkinlik günlüğe kaydetme](../azure-monitor/platform/platform-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)hakkında daha fazla bilgi edinin. Azure dağıtım modeli aracılığıyla oluşturulan NSG'ler için varsayılan olarak etkinlik günlüğü etkinleştirilir. Etkinlik günlüğündeki NSG'lerde hangi işlemlerin tamamlandığını belirlemek için, aşağıdaki kaynak türlerini içeren girişleri arayın:
  - Microsoft.ClassicNetwork/networkSecurityGroups
  - Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
  - Microsoft.Network/networkSecurityGroups
  - Microsoft.Network/networkSecurityGroups/securityRules
- Tanılama bilgilerini nasıl günlüğe kaydedin, her akış için kaynak IP adresini eklemek için [NSG akış günlüğe bakın.](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
