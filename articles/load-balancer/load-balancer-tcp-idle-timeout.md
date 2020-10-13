---
title: Azure 'da yük dengeleyici TCP sıfırlaması ve boşta kalma zaman aşımını yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure Load Balancer TCP boşta kalma zaman aşımını nasıl yapılandıracağınızı öğrenin.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/09/2020
ms.author: allensu
ms.openlocfilehash: 26c4c01aaf6abe6b9c9ac6daf6836d7b660ba21e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91649873"
---
# <a name="configure-tcp-idle-timeout-for-azure-load-balancer"></a>Azure Load Balancer için TCP boşta kalma zaman aşımını yapılandırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell 'i yerel olarak çalıştırıyorsanız `Connect-AzAccount` Azure ile bir bağlantı oluşturmak için öğesini de çalıştırmanız gerekir.

Azure Load Balancer, 5 dakika ila 120 dakika arasında bir boşta kalma zaman aşımı ayarına sahiptir. Varsayılan olarak, 4 dakikaya ayarlanır. İşlem yapılmayan bir süre, zaman aşımı değerinden uzunsa, TCP veya HTTP oturumunun istemci ile bulut hizmetiniz arasında korunduğundan emin olmaz. [TCP boşta kalma zaman aşımı](load-balancer-tcp-reset.md)hakkında daha fazla bilgi edinin.

Aşağıdaki bölümlerde, genel IP ve yük dengeleyici kaynakları için boşta kalma zaman aşımı ayarlarının nasıl değiştirileceği açıklanır.


## <a name="configure-the-tcp-idle-timeout-for-your-public-ip"></a>Genel IP 'niz için TCP boşta kalma zaman aşımını yapılandırın

```azurepowershell-interactive
$publicIP = Get-AzPublicIpAddress -Name MyPublicIP -ResourceGroupName MyResourceGroup
$publicIP.IdleTimeoutInMinutes = "15"
Set-AzPublicIpAddress -PublicIpAddress $publicIP
```

`IdleTimeoutInMinutes` isteğe bağlıdır. Ayarlanmamışsa, varsayılan zaman aşımı 4 dakikadır. Kabul edilebilir zaman aşımı aralığı 4 ila 120 dakikadır.

## <a name="set-the-tcp-idle-timeout-on-rules"></a>Kurallar üzerinde TCP boşta kalma zaman aşımını ayarlama

Yük Dengeleyici için boşta kalma zaman aşımını ayarlamak için, yük dengeli kuralda ' ıdletimeoutınminutes ' ayarlanır. Örneğin:

```azurepowershell-interactive
$lb = Get-AzLoadBalancer -Name "MyLoadBalancer" -ResourceGroup "MyResourceGroup"
$lb | Set-AzLoadBalancerRuleConfig -Name myLBrule -IdleTimeoutInMinutes 15
```

## <a name="next-steps"></a>Sonraki adımlar

[İç Yük Dengeleyiciye genel bakış](load-balancer-internal-overview.md)

[Internet 'e yönelik yük dengeleyiciyi yapılandırmaya başlama](quickstart-load-balancer-standard-public-powershell.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)
