---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8ae22ec7f75b9cd0d7958977dfd97169706c9389
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188318"
---
Kullanılabilirlik grubu dinleyicisini oluşturduktan sonra, dinleyici kaynağı için RegisterAllProvidersIP ve HostRecordTTL küme parametrelerini ayarlamanız gerekebilir. Bu parametreler, bir arıza dan sonra yeniden bağlantı süresini azaltabilir ve bu da bağlantı zaman larını engelleyebilir. Bu parametreler in yanı sıra örnek kod hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover)

