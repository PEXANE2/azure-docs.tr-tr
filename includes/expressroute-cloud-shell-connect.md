---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/01/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1aca39a7ff162aa3c42fdb3ca5999c71091ec02e
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188104"
---
 Azure Cloud Shell kullanıyorsanız, ' dene ' ' ye tıkladıktan sonra Azure hesabınızda otomatik olarak oturum açın. Yerel olarak oturum açmak için, PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve bağlanmak için cmdlet 'ini çalıştırın.

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