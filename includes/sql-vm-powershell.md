---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: c6666f4417cde9e0f77cc965ded1d6bdb5dced34
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "67188329"
---
## <a name="start-your-powershell-session"></a>PowerShell oturumunuzu başlatma
 

[**Connect-az Account**](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount) cmdlet 'ini çalıştırın ve kimlik bilgilerinizi girmeniz için bir oturum açma ekranı görüntülenir. Azure portala giriş yapmak için kullandığınız kimlik bilgilerinin aynısını kullanın.

```powershell
Connect-AzAccount
```

Birden çok aboneliğiniz varsa, PowerShell oturumunuzun kullanması gereken aboneliği seçmek için [**set-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) cmdlet 'ini kullanın. Geçerli PowerShell oturumunun hangi abonelikte kullandığını görmek için [**Get-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext)komutunu çalıştırın. Tüm aboneliklerinizi görmek için [**Get-AzSubscription**](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription)komutunu çalıştırın.

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```

