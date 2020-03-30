---
title: Azure'da yük bakiyesi TCP boşta zaman aşım süresini yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure Yük Bakiyesi TCP zaman aşımsüresini nasıl yapılandırıştırılacaöğrenin.
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
ms.openlocfilehash: d0bb73b58aa23e5f7eb784772acf37b05df463ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456837"
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Azure Yük Dengeleyicisi için TCP boşta zaman sonu ayarlarını yapılandırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, Azure `Connect-AzAccount` ile bağlantı oluşturmak için de çalışmanız gerekir.

## <a name="tcp-idle-timeout"></a>TCP Boşta Zaman Ala
Azure Yük Dengeleyicisi'nin 4 dakika ile 30 dakika arasında boşta zaman ayarı vardır. Varsayılan olarak, 4 dakika olarak ayarlanır. Bir hareketsizlik süresi zaman aşımı değerinden daha uzunsa, Istemci ile bulut hizmetiniz arasında TCP veya HTTP oturumunun korunanın garantisi yoktur.

Bağlantı kapatıldığında, istemci uygulamanız aşağıdaki hata iletisini alabilir: "Temel bağlantı kapatıldı: Canlı tutulması beklenen bir bağlantı sunucu tarafından kapatıldı."

Yaygın bir uygulama bir TCP tutmak-canlı kullanmaktır. Bu uygulama, bağlantıyı daha uzun süre etkin tutar. Daha fazla bilgi için bu [.NET örneklerine](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx)bakın. Canlı tutma özelliği sayesinde paketler bağlantıdaki hareketsizlik dönemlerinde gönderilir. Canlı paketler, boşta kalan zaman ayarı değerine ulaşılamamasını ve bağlantının uzun süre korunmasını sağlar.

Ayar yalnızca gelen bağlantılar için çalışır. Bağlantıyı kaybetmemek için, TCP'yi boşta zaman aralığı ayarından daha kısa bir aralıkla canlı tutmayı yapılandırın veya boşta kalan zaman aralığı değerini artırın. Bu senaryoları desteklemek için, yapılandırılabilir bir boşta zaman acısı için destek eklendi.

TCP, pil ömrünün kısıtolmadığı senaryolar için çalışır. Mobil uygulamalar için önerilmez. Bir mobil uygulamada tcp'yi canlı tutmak, cihazın pilini daha hızlı tüketebilir.

![TCP zaman](./media/load-balancer-tcp-idle-timeout/image1.png)

Aşağıdaki bölümlerde, genel IP ve yük bakiyesi kaynakları için boşta zaman sonu ayarlarını nasıl değiştirilen açıklanmıştır.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>Örnek seviye genel IP'niz için TCP zaman aşımını 15 dakikaya göre yapılandırın

```azurepowershell-interactive
$publicIP = Get-AzPublicIpAddress -Name MyPublicIP -ResourceGroupName MyResourceGroup
$publicIP.IdleTimeoutInMinutes = "15"
Set-AzPublicIpAddress -PublicIpAddress $publicIP
```

`IdleTimeoutInMinutes` isteğe bağlıdır. Ayarlanmazsa, varsayılan zaman dilimi 4 dakikadır. Kabul edilebilir zaman aralığı 4 ila 30 dakikadır.

## <a name="set-the-tcp-timeout-on-a-load-balanced-rule-to-15-minutes"></a>TCP zaman dışarısını yük dengeli kuralına göre 15 dakikaya ayarlama

Yük dengeleyicisi için boşta zaman ayırıcıyı ayarlamak için, 'IdleTimeoutInMinutes' yük dengeli kuralına göre ayarlanır. Örnek:

```azurepowershell-interactive
$lb = Get-AzLoadBalancer -Name "MyLoadBalancer" -ResourceGroup "MyResourceGroup"
$lb | Set-AzLoadBalancerRuleConfig -Name myLBrule -IdleTimeoutInMinutes 15
```
## <a name="next-steps"></a>Sonraki adımlar

[Dahili yük dengeleyicisi genel bakış](load-balancer-internal-overview.md)

[Internet'e bakan yük dengeleyicisi yapılandırmaya başlayın](quickstart-create-standard-load-balancer-powershell.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)
