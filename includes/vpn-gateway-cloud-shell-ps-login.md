---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/10/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 50ce8530aca40eed07741f35be1a57bbd7cc1868
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "77133618"
---
PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın.

Azure PowerShell yerel olarak çalıştırıyorsanız Azure hesabınıza bağlanın. *Connect-AzAccount* cmdlet 'i sizden kimlik bilgilerini ister. Kimlik doğrulamasından sonra, Azure PowerShell için kullanılabilir olmaları için hesap ayarlarınızı indirir. Bunun yerine Azure Cloud Shell kullanıyorsanız, *Connect-AzAccount*komutunu çalıştırmanız gerekmez. Azure Cloud Shell Azure hesabınıza otomatik olarak bağlanır.

```azurepowershell
Connect-AzAccount
```

Birden fazla aboneliğiniz varsa Azure aboneliklerinizin bir listesini alın.

```azurepowershell-interactive
Get-AzSubscription
```

Kullanmak istediğiniz aboneliği belirtin.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Name of subscription"
```