---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8ae22ec7f75b9cd0d7958977dfd97169706c9389
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188318"
---
Kullanılabilirlik grubu dinleyicisini oluşturduktan sonra, dinleyici kaynağı için RegisterAllProvidersIP ve HostRecordTTL küme parametrelerini ayarlamanız gerekebilir. Bu parametreler, bağlantı zaman aşımlarını engelleyebilecek bir yük devretmeden sonra yeniden bağlanma süresini azaltabilir. Bu parametrelerin yanı sıra örnek kod hakkında daha fazla bilgi için bkz. [bir kullanılabilirlik grubu dinleyicisi oluşturma veya yapılandırma](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).

