---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/29/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 24d146da7946176c92902698d0f52ae01baf79ee
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93061668"
---
PowerShell 'i yerel olarak çalıştırıyorsanız, PowerShell konsolunu yükseltilmiş ayrıcalıklarla açın ve Azure hesabınıza bağlanın. *Connect-AzAccount* cmdlet 'i sizden kimlik bilgilerini ister. Kimlik doğrulamasından sonra, Azure PowerShell için kullanılabilir olmaları için hesap ayarlarınızı indirir.

PowerShell 'i yerel olarak çalıştırmak yerine Azure Cloud Shell kullanıyorsanız, *Connect-AzAccount* komutunu çalıştırmanız gerekmediğini fark edeceksiniz. Azure Cloud Shell, **yeniden dene**' yi seçtikten sonra Azure hesabınıza otomatik olarak bağlanır.

1. PowerShell 'i yerel olarak çalıştırıyorsanız oturum açın.

   ```azurepowershell
   Connect-AzAccount
   ```

1. Birden fazla aboneliğiniz varsa Azure aboneliklerinizin bir listesini alın.

   ```azurepowershell-interactive
   Get-AzSubscription
   ```

1. Kullanmak istediğiniz aboneliği belirtin.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "Name of subscription"
   ```
