---
title: NSG Akış günlüklerini yönetme - Azure CLI
titleSuffix: Azure Network Watcher
description: Bu sayfa, Azure CLI ile Azure Ağ İzleyicisi'ndeki Ağ Güvenliği Grubu Akışı günlüklerinin nasıl yönetilenini açıklar
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 285d19dbd0e7b8a94eada66f837d33b787006f09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840970"
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli"></a>Azure CLI ile Ağ Güvenliği Grubu Akış günlüklerini yapılandırma

> [!div class="op_single_selector"]
> - [Azure portalında](network-watcher-nsg-flow-logging-portal.md)
> - [Powershell](network-watcher-nsg-flow-logging-powershell.md)
> - [Azure CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Ağ Güvenliği Grubu akış günlükleri, Ağ Güvenlik Grubu aracılığıyla giriş ve çıkış IP trafiği hakkındaki bilgileri görüntülemenize olanak tanıyan Ağ İzleyicisi'nin bir özelliğidir. Bu akış günlükleri json biçiminde yazılır ve kural bazında giden ve gelen akışları gösterir, akış için geçerli nic, akış hakkında 5-tuple bilgi (Kaynak/Hedef IP, Kaynak/Hedef Bağlantı Noktası, Protokol) ve trafiğe izin verilip verilmediğini veya reddedilmişse.

Bu makaledeki adımları gerçekleştirmek için [Mac, Linux ve Windows (CLI) için Azure komut satırı arabirimini yüklemeniz](/cli/azure/install-azure-cli)gerekir.

## <a name="register-insights-provider"></a>Insights sağlayıcısını kaydetme

Akış günlüğe kaydetmenin başarılı bir şekilde çalışabilmesi için **Microsoft.Insights** sağlayıcısının kaydedilmesi gerekir. **Microsoft.Insights** sağlayıcısının kayıtlı olup olmadığından emin değilseniz, aşağıdaki komut dosyasını çalıştırın.

```azurecli
az provider register --namespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a>Ağ Güvenliği Grubu Akış günlüklerini etkinleştirme

Akış günlüklerini etkinleştirme komutu aşağıdaki örnekte gösterilmiştir:

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled true --nsg nsgName --storage-account storageAccountName
# Configure 
az network watcher flow-log configure --resource-group resourceGroupName --enabled true --nsg nsgName --storage-account storageAccountName  --format JSON --log-version 2
```

Belirttiğiniz depolama hesabı, ağ erişimini yalnızca Microsoft hizmetleri veya belirli sanal ağlarla sınırlandıran ağ kuralları nın ayarlanmasına olanak lanamaz. Depolama hesabı, akış günlüğünü etkinleştirdiğiniz NSG ile aynı veya farklı bir Azure aboneliğinde olabilir. Farklı abonelikler kullanıyorsanız, her ikisinin de aynı Azure Etkin Dizin kiracıyla ilişkilendirilmesi gerekir. Her abonelik için kullandığınız hesap [gerekli izinlere](required-rbac-permissions.md)sahip olmalıdır. 

Depolama hesabı ağ güvenlik grubundan farklı bir kaynak grubunda veya abonelikteyse, adı yerine depolama hesabının tam kimliğini belirtin. Örneğin, depolama hesabı *RG-Depolama*adlı bir kaynak grubunda ise , önceki komutta *storageAccountName* belirtmek yerine, */abonelikler/{SubscriptionID}/resourceGroups/RG-Storage/providers/Microsoft.Storage/storageAccounts/storageAccountName*.

## <a name="disable-network-security-group-flow-logs"></a>Ağ Güvenlik Grubu Akış günlüklerini devre dışı bırak

Akış günlüklerini devre dışı kalmak için aşağıdaki örneği kullanın:

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled false --nsg nsgName
```

## <a name="download-a-flow-log"></a>Akış günlüğü indirin

Akış günlüğünün depolama konumu oluşturmada tanımlanır. Bir depolama hesabına kaydedilen bu akış günlüklerine erişmek için kullanışlı bir araç, buradan indirilebilen Microsoft Azure Depolama Gezgini'dir:https://storageexplorer.com/

Bir depolama hesabı belirtilirse, akış günlüğü dosyaları aşağıdaki konumdaki bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```


## <a name="next-steps"></a>Sonraki Adımlar

[PowerBI ile NSG akış günlüklerinizi nasıl görselleştirebilirsiniz](network-watcher-visualize-nsg-flow-logs-power-bi.md) öğrenin

[NSG akış günlüklerinizi açık kaynak araçlarıyla nasıl görselleştirebilirsiniz](network-watcher-visualize-nsg-flow-logs-open-source-tools.md) öğrenin
