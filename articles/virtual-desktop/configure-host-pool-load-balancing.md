---
title: Windows Sanal Masaüstü yük dengeleme yapılandırma - Azure
description: Windows Sanal Masaüstü ortamı için yük dengeleme yöntemini yapılandırma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 5d8670994791e360f5e3b30b90b4bea5d55464b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128300"
---
# <a name="configure-the-windows-virtual-desktop-load-balancing-method"></a>Windows Sanal Masaüstü yük dengeleme yöntemini yapılandırma

Ana bilgisayar havuzu için yük dengeleme yöntemini yapılandırmak, Windows Sanal Masaüstü ortamını gereksinimlerinize daha iyi uyacak şekilde ayarlamanıza olanak tanır.

>[!NOTE]
> Bu, kullanıcıların her zaman ana bilgisayar havuzundaki oturum ana bilgisayarında 1:1 eşlemesi olduğundan, kalıcı bir masaüstü ana bilgisayar havuzu için geçerli değildir.

## <a name="configure-breadth-first-load-balancing"></a>Genişlik-ilk yük dengeleme yapılandırma

Genişlik ilk yük dengeleme yeni kalıcı olmayan ana bilgisayar havuzları için varsayılan yapılandırmadır. Genişlik ilk yük dengeleme, ana bilgisayar havuzundaki tüm kullanılabilir oturum ana bilgisayarlarına yeni kullanıcı oturumları dağıtır. Genişlik-ilk yük dengelemesini yapılandırırken, ana bilgisayar havuzunda oturum ana bilgisayar başına maksimum oturum sınırı ayarlayabilirsiniz.

İlk olarak, PowerShell oturumunuzda kullanmak üzere [Windows Virtual Desktop PowerShell modülünü indirin ve içe aktarın.](/powershell/windows-virtual-desktop/overview/) Bundan sonra, hesabınızda oturum açabilmek için aşağıdaki cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Maksimum oturum limitini ayarlamadan genişlik-ilk yük dengelemesi gerçekleştirecek bir ana bilgisayar havuzunu yapılandırmak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer
```

Genişlik-ilk yük dengeleme gerçekleştirmek ve yeni bir maksimum oturum sınırı kullanmak için bir ana bilgisayar havuzu yapılandırmak için aşağıdaki PowerShell cmdlet çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer -MaxSessionLimit ###
```

## <a name="configure-depth-first-load-balancing"></a>Derinlik-ilk yük dengeleme yapılandırma

Derinlik ilk yük dengelemesi, yeni kullanıcı oturumlarını en yüksek sayıda bağlantıya sahip kullanılabilir bir oturum ana bilgisayara dağıtır, ancak maksimum oturum sınırı eşiğine ulaşmamıştır. Derinlik-ilk yük dengelemesini yapılandırırken, ana bilgisayar havuzunda oturum ana bilgisayar başına maksimum oturum sınırı belirlemeniz **gerekir.**

Derinlik ilk yük dengeleme gerçekleştirmek için bir ana bilgisayar havuzu yapılandırmak için aşağıdaki PowerShell cmdlet çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -DepthFirstLoadBalancer -MaxSessionLimit ###
```
