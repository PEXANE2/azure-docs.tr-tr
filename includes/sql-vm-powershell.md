---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: c6666f4417cde9e0f77cc965ded1d6bdb5dced34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188329"
---
## <a name="start-your-powershell-session"></a>PowerShell oturumunuzu başlatma
 

[**Connect-Az Hesabı**](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount) cmdlet'ini çalıştırDığınızda kimlik bilgilerinizi girmek için oturum açma ekranı size sunulacaktır. Azure portala giriş yapmak için kullandığınız kimlik bilgilerinin aynısını kullanın.

```powershell
Connect-AzAccount
```

Birden fazla aboneliğiniz varsa, PowerShell oturumunuzun hangi aboneliği kullanması gerektiğini seçmek için [**Set-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) cmdlet'i kullanın. Geçerli PowerShell oturumunun hangi aboneliği kullandığını görmek için [**Get-AzContext'ı**](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext)çalıştırın. Tüm aboneliklerinizi görmek için [**Get-AzSubscription'ı**](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription)çalıştırın.

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```

