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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188104"
---
 Azure Bulut Kabuğu'nu kullanıyorsanız, 'Deneyin' seçeneğini tıkladıktan sonra Otomatik olarak Azure hesabınızda oturum açabilirsiniz. Yerel oturum açmak için PowerShell konsolunuzu yüksek ayrıcalıklarla açın ve bağlanmak için cmdlet çalıştırın.

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