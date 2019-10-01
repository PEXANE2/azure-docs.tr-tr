---
title: Windows sanal masaüstü yük dengeleme yöntemini Yapılandırma-Azure
description: Windows sanal masaüstü ortamı için yük dengeleme yöntemini yapılandırma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: helohr
ms.openlocfilehash: 3a940dbf592087878cb9dd19f856f1a3d94291c5
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71676771"
---
# <a name="configure-the-windows-virtual-desktop-load-balancing-method"></a>Windows Sanal Masaüstü yük dengeleme yöntemini yapılandırma

Bir konak havuzu için yük dengeleme yöntemini yapılandırmak, Windows sanal masaüstü ortamını gereksinimlerinize daha iyi uyacak şekilde ayarlamanıza olanak sağlar.

>[!NOTE]
> Bu, kullanıcıların konak havuzundaki bir oturum ana bilgisayarına her zaman bir 1:1 eşlemesi olduğundan, kalıcı bir masaüstü konak havuzu için geçerlidir.

## <a name="configure-breadth-first-load-balancing"></a>Enine ilk yük dengelemeyi yapılandırma

Enine ilk yük dengeleme, yeni kalıcı olmayan konak havuzları için varsayılan yapılandırmadır. Enine ilk yük dengeleme, yeni kullanıcı oturumlarını konak havuzundaki kullanılabilir tüm oturum konaklarına dağıtır. Enine ilk yük dengelemeyi yapılandırırken, konak havuzundaki oturum ana bilgisayarı başına en fazla oturum sınırı ayarlayabilirsiniz.

İlk olarak, henüz yapmadıysanız PowerShell oturumunuzda kullanmak üzere [Windows sanal masaüstü PowerShell modülünü indirip içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) . Bundan sonra hesabınızda oturum açmak için aşağıdaki cmdlet 'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Bir konak havuzunu, en fazla oturum sınırını ayarlamadan önce enine yük dengelemesi gerçekleştirecek şekilde yapılandırmak için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer
```

Bir konak havuzunu, birinci düzey yük dengelemeyi gerçekleştirecek şekilde yapılandırmak ve yeni bir en fazla oturum sınırı kullanmak için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer -MaxSessionLimit ###
```

## <a name="configure-depth-first-load-balancing"></a>Derinlik-ilk yük dengelemeyi yapılandırma

Derinlik-ilk yük dengeleme, yeni kullanıcı oturumlarını en fazla bağlantı sayısına sahip kullanılabilir bir oturum ana bilgisayarına dağıtır ancak en fazla oturum sınırı eşiğine ulaşmaz. Derinlik-ilk yük dengelemeyi yapılandırırken, konak havuzundaki oturum ana bilgisayarı başına en fazla oturum sınırı ayarlamanız **gerekir** .

Bir konak havuzunu derinlik-ilk yük dengelemesi gerçekleştirecek şekilde yapılandırmak için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -DepthFirstLoadBalancer -MaxSessionLimit ###
```
