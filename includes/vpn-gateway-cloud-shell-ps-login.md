---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/10/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 50ce8530aca40eed07741f35be1a57bbd7cc1868
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77133618"
---
PowerShell konsolunuzu yüksek ayrıcalıklarla açın.

Azure PowerShell'i yerel olarak çalıştırıyorsanız, Azure hesabınıza bağlanın. *Connect-AzAccount* cmdlet kimlik bilgilerini ister. Kimlik doğrulaması yaptıktan sonra, hesap ayarlarınızı Azure PowerShell'e uygun olacak şekilde indirir. Bunun yerine Azure Bulut Shell kullanıyorsanız, *Connect-AzAccount'ı*çalıştırmanız gerekmez. Azure Bulut Shell, Azure hesabınıza otomatik olarak bağlanır.

```azurepowershell
Connect-AzAccount
```

Birden fazla aboneliğiniz varsa, Azure aboneliklerinizin bir listesini alın.

```azurepowershell-interactive
Get-AzSubscription
```

Kullanmak istediğiniz aboneliği belirtin.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Name of subscription"
```