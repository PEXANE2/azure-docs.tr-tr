---
title: Azure Seri Konsolu etkinleştirme ve devre dışı kalım | Microsoft Dokümanlar
description: Azure Seri Konsolu hizmetini etkinleştirme ve devre dışı etme
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 8/20/2019
ms.author: alsin
ms.openlocfilehash: e09e08f8ba36cf576bc27551254225adee3bb0fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451298"
---
# <a name="enable-and-disable-the-azure-serial-console"></a>Azure Seri Konsolu'nu etkinleştirme ve devre dışı

Diğer kaynaklar gibi Azure Seri Konsolu da etkinleştirilebilir ve devre dışı bırakılabilir. Seri Konsol, global Azure'daki tüm abonelikler için varsayılan olarak etkinleştirilir. Şu anda, Seri Konsol'u devre dışı bırakmak tüm aboneliğiniz için hizmeti devre dışı bırakacaktır. Seri Konsol'u abonelik için devre dışı bırakmak veya yeniden etkinleştirmek, katılımcı düzeyinde erişim veya abonelik üzerinde gerektirir.

Ayrıca, önyükleme tanılama devre dışı bırakarak tek bir VM veya sanal makine ölçeği kümesi örneği için seri konsolu devre dışı bırakabilirsiniz. Hem VM/sanal makine ölçeği setinde hem de önyükleme tanılama depolama hesabınızda katılımcı düzeyinde erişim ekive erişim ekidir.

## <a name="vm-level-disable"></a>VM düzeyinde devre dışı
Seri konsol, önyükleme tanılama ayarını devre dışı bırakarak belirli bir VM veya sanal makine ölçeği için devre dışı bırakılabilir. VM veya sanal makine ölçeği kümesinin seri konsolunu devre dışı bırakabilmek için Azure portalından önyükleme tanılamasını kapatın. Seri konsolu sanal makine ölçeği kümesinde kullanıyorsanız, sanal makine ölçeği set örneklerinizi en son modele yükselttiniz.


## <a name="subscription-level-enabledisable"></a>Abonelik düzeyi etkinleştirme/devre dışı

> [!NOTE]
> Bu komutu çalıştırmadan önce doğru bulutta (Azure Genel Bulutu, Azure ABD Devlet Bulutu) olduğunuzdan emin olun. Bulutunuzu kontrol `az cloud list` edebilir ve `az cloud set -n <Name of cloud>`ayarlayabilirsiniz.

### <a name="azure-cli"></a>Azure CLI

Seri konsol, Azure CLI'deki aşağıdaki komutları kullanarak tüm abonelik için devre dışı bırakabilir ve yeniden etkinleştirilebilir (komutları çalıştırabileceğiniz Azure Bulut Kabuğu örneğini başlatmak için "Deneyin" düğmesini kullanabilirsiniz):

Bir abonelik için seri konsolunu devre dışı kullanabilirsiniz:
```azurecli-interactive
subscriptionId=$(az account show --output=json | jq -r .id)

az resource invoke-action --action disableConsole --ids "/subscriptions/$subscriptionId/providers/Microsoft.SerialConsole/consoleServices/default" --api-version="2018-05-01"
```

Abonelik için seri konsolu etkinleştirmek için aşağıdaki komutları kullanın:
```azurecli-interactive
subscriptionId=$(az account show --output=json | jq -r .id)

az resource invoke-action --action enableConsole --ids "/subscriptions/$subscriptionId/providers/Microsoft.SerialConsole/consoleServices/default" --api-version="2018-05-01"
```

Abonelik için seri konsolun geçerli etkin/devre dışı bırakılmış durumunu almak için aşağıdaki komutları kullanın:
```azurecli-interactive
subscriptionId=$(az account show --output=json | jq -r .id)

az resource show --ids "/subscriptions/$subscriptionId/providers/Microsoft.SerialConsole/consoleServices/default" --output=json --api-version="2018-05-01" | jq .properties
```

### <a name="powershell"></a>PowerShell

Seri konsol PowerShell kullanılarak da etkinleştirilebilir ve devre dışı bırakılır.

Bir abonelik için seri konsolunu devre dışı kullanabilirsiniz:
```azurepowershell-interactive
$subscription=(Get-AzContext).Subscription.Id

Invoke-AzResourceAction -Action disableConsole -ResourceId /subscriptions/$subscription/providers/Microsoft.SerialConsole/consoleServices/default -ApiVersion 2018-05-01
```

Abonelik için seri konsolu etkinleştirmek için aşağıdaki komutları kullanın:
```azurepowershell-interactive
$subscription=(Get-AzContext).Subscription.Id

Invoke-AzResourceAction -Action enableConsole -ResourceId /subscriptions/$subscription/providers/Microsoft.SerialConsole/consoleServices/default -ApiVersion 2018-05-01
```

## <a name="next-steps"></a>Sonraki adımlar
* [Linux VM'leri için Azure Seri Konsolu](./serial-console-linux.md) hakkında daha fazla bilgi edinin
* [Windows VM'ler için Azure Seri Konsolu](./serial-console-windows.md) hakkında daha fazla bilgi edinin
* Azure [Seri Konsolu'ndaki güç yönetimi seçenekleri](./serial-console-power-options.md) hakkında bilgi edinin