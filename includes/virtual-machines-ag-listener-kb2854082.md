---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 28aab15dc67e051190e8d4e35e92240a56fe54a6
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188319"
---
Sonra, kümedeki herhangi bir sunucu Windows Server 2008 R2 veya Windows Server 2012 çalıştırıyorsa, düzeltme [KB2854082](https://support.microsoft.com/kb/2854082) ' nin her bir şirket içi sunucuda veya kümenin parçası olan Azure VM 'lerinde yüklü olduğunu doğrulamanız gerekir. Kümede olan ancak kullanılabilirlik grubunda olmayan tüm sunucu veya VM 'ler de bu düzeltmenin yüklü olması gerekir.

Küme düğümlerinin her biri için Uzak Masaüstü oturumunda, [KB2854082](https://support.microsoft.com/kb/2854082) 'i yerel bir dizine indirin. Sonra, düzeltmeyi her bir küme düğümüne sırayla yükler. Küme hizmeti şu anda küme düğümünde çalışıyorsa, düzeltme yüklemesinin sonunda sunucu yeniden başlatılır.

> [!WARNING]
> Küme hizmetini durdurmak veya sunucuyu yeniden başlatmak, kümenizin ve kullanılabilirlik grubunun çekirdek durumunu etkiler ve kümenizin çevrimdışı çalışmasına neden olabilir. Yükleme sırasında kümenizin yüksek oranda kullanılabilirliğini sürdürmek için aşağıdakileri yaptığınızdan emin olun:
> 
> * Küme, en iyi çekirdek sistem durumunda. 
> * Düzeltmeyi herhangi bir düğüme yüklemeden önce, tüm küme düğümleri çevrimiçi olarak bulunur.
> * Düzeltmeyi kümedeki başka bir düğüme yüklemeden önce, sunucuyu tamamen yeniden başlatmak da dahil olmak üzere düzeltme yüklemesinin tek bir düğümde tamamlanmasına izin verin.
> 
> 

