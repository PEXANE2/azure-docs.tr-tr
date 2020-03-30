---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 28aab15dc67e051190e8d4e35e92240a56fe54a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188319"
---
Ardından, kümedeki herhangi bir sunucu Windows Server 2008 R2 veya Windows Server 2012 çalıştırıyorsa, kümenin bir parçası olan şirket içi sunucuların veya Azure VM'lerinin her birinde [KB2854082](https://support.microsoft.com/kb/2854082) düzeltmesinin yüklü olduğunu doğrulamanız gerekir. Kümede bulunan ancak kullanılabilirlik grubunda olmayan herhangi bir sunucu veya VM de bu düzeltmeyi yüklü olmalıdır.

Küme düğümlerinin her biri için uzak masaüstü oturumunda, [KB2854082'yi](https://support.microsoft.com/kb/2854082) yerel bir dizine indirin. Ardından, her küme düğümüne hotfix'i sırayla yükleyin. Küme hizmeti şu anda küme düğümünde çalışıyorsa, sunucu düzeltme yüklemesinin sonunda yeniden başlatılır.

> [!WARNING]
> Küme hizmetini durdurma veya sunucuyu yeniden başlatma, kümenizin ve kullanılabilirlik grubunuzun çoğunluk durumunu etkiler ve kümenizin çevrimdışı olmasına neden olabilir. Yükleme sırasında kümenizin yüksek kullanılabilirliğini korumak için aşağıdakilerden emin olun:
> 
> * Küme optimal çoğunluk sağlığı ndadır. 
> * Düzeltmeyi herhangi bir düğüme yüklemeden önce, tüm küme düğümleri çevrimiçidir.
> * Düzeltmeyi kümedeki başka bir düğüme yüklemeden önce, sunucuyu tamamen yeniden başlatmak da dahil olmak üzere, hotfix yüklemesinin tek bir düğümüzerinde tamamlanmasına izin verin.
> 
> 

