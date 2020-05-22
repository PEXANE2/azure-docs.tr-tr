---
title: Azure 'da yük dengeleyici TCP boşta kalma zaman aşımını yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure Load Balancer TCP boşta kalma zaman aşımını nasıl yapılandıracağınızı öğrenin.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/09/2020
ms.author: allensu
ms.openlocfilehash: 09d15877088fb6356419a9d31f8bef3164e76029
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83780594"
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Azure Load Balancer için TCP boşta kalma zaman aşımı ayarlarını yapılandırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell 'i yerel olarak çalıştırıyorsanız `Connect-AzAccount` Azure ile bir bağlantı oluşturmak için öğesini de çalıştırmanız gerekir.

## <a name="tcp-idle-timeout"></a>TCP boşta kalma zaman aşımı
Azure Load Balancer için boşta kalma zaman aşımı ayarı 4 dakika ila 30 dakika. Varsayılan olarak, 4 dakikaya ayarlanır. İşlem yapılmayan bir süre, zaman aşımı değerinden uzunsa, TCP veya HTTP oturumunun istemci ile bulut hizmetiniz arasında korunduğundan emin olmaz.

Bağlantı kapalıyken, istemci uygulamanız şu hata iletisini alabilir: "temel alınan bağlantı kapatıldı: etkin tutulması beklenen bir bağlantı sunucu tarafından kapatıldı."

Ortak bir uygulama, TCP etkin tutma özelliğini kullanmaktır. Bu uygulama, bağlantının daha uzun bir süre için etkin kalmasını önler. Daha fazla bilgi için, bkz. bu [.NET örnekleri](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Etkin tut özelliği etkinken, paketler bağlantı üzerinde işlem yapılmayan dönemler sırasında gönderilir. Canlı tut paketleri boştaki zaman aşımı değerine ulaşılmadığını ve bağlantının uzun bir süre boyunca korunmasını güvence altına aldığından emin olun.

Bu ayar yalnızca gelen bağlantılar için geçerlidir. Bağlantıyı kaybetmekten kaçınmak için, boşta kalma zaman aşımı ayarından daha az bir aralığa sahip TCP etkin tutmayı yapılandırın veya boşta kalma zaman aşımı değerini artırın. Bu senaryoları desteklemek için yapılandırılabilir bir boşta kalma zaman aşımı desteği eklenmiştir.

TCP etkin tutma, pil ömrünün kısıtlama olmadığı senaryolar için geçerlidir. Mobil uygulamalar için önerilmez. Bir mobil uygulamada TCP etkin tutma kullanmak cihaz pilinin daha hızlı tükenmesini sağlayabilir.

![TCP zaman aşımı](./media/load-balancer-tcp-idle-timeout/image1.png)

Aşağıdaki bölümlerde, genel IP ve yük dengeleyici kaynakları için boşta kalma zaman aşımı ayarlarının nasıl değiştirileceği açıklanır.

>[!NOTE]
> TCP boşta kalma zaman aşımı, UDP protokolünde Yük Dengeleme kurallarını etkilemez.


## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>Örnek düzeyi genel IP 'niz için TCP zaman aşımını 15 dakikaya yapılandırın

```azurepowershell-interactive
$publicIP = Get-AzPublicIpAddress -Name MyPublicIP -ResourceGroupName MyResourceGroup
$publicIP.IdleTimeoutInMinutes = "15"
Set-AzPublicIpAddress -PublicIpAddress $publicIP
```

`IdleTimeoutInMinutes` isteğe bağlıdır. Ayarlanmamışsa, varsayılan zaman aşımı 4 dakikadır. Kabul edilebilir zaman aşımı aralığı 4 ila 30 dakikadır.

## <a name="set-the-tcp-timeout-on-a-load-balanced-rule-to-15-minutes"></a>Yük dengeli bir kuralda TCP zaman aşımını 15 dakikaya ayarlayın

Yük Dengeleyici için boşta kalma zaman aşımını ayarlamak için, yük dengeli kuralda ' ıdletimeoutınminutes ' ayarlanır. Örneğin:

```azurepowershell-interactive
$lb = Get-AzLoadBalancer -Name "MyLoadBalancer" -ResourceGroup "MyResourceGroup"
$lb | Set-AzLoadBalancerRuleConfig -Name myLBrule -IdleTimeoutInMinutes 15
```
## <a name="next-steps"></a>Sonraki adımlar

[İç Yük Dengeleyiciye genel bakış](load-balancer-internal-overview.md)

[Internet 'e yönelik yük dengeleyiciyi yapılandırmaya başlama](quickstart-create-standard-load-balancer-powershell.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)
