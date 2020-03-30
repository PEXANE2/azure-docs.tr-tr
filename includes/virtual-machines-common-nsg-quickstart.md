---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 09/12/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: ec6cbcbc93fe87634c87caeb0041b75ec916a22f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188341"
---
Bir alt ağ da veya VM ağ arabirimi üzerinde ağ filtresi oluşturarak azure'daki sanal makineye (VM) bir bağlantı noktası açar veya bir bitiş noktası oluşturursunuz. Hem gelen hem de giden trafiği kontrol eden bu filtreleri, trafiği alan kaynağa bağlı bir ağ güvenlik grubuna yerebilirsiniz.

Bu makaledeki örnek, standart TCP bağlantı noktası 80'i kullanan bir ağ filtresinin nasıl oluşturulurolacağını gösterir (uygun hizmetleri zaten başlattığınız ve VM'de işletim sistemi güvenlik duvarı kurallarını açtığınız varsayılır).

Standart TCP bağlantı noktası 80'de web isteklerini sunmak üzere yapılandırılmış bir VM oluşturduktan sonra şunları yapabilirsiniz:

1. Ağ güvenlik grubu oluşturun.

2. Trafiğe izin veren gelen bir güvenlik kuralı oluşturun ve değerleri aşağıdaki ayarlara atayın:

   - **Hedef bağlantı noktası aralıkları**: 80

   - **Kaynak bağlantı noktası aralıkları**: * (herhangi bir kaynak bağlantı noktasına izin verir)

   - **Öncelik değeri**: Varsayılan catch-all inbound kuralından daha az 65.500 ve daha yüksek öncelikli bir değer girin.

3. Ağ güvenlik grubunu VM ağ arabirimi veya alt ağıyla ilişkilendirin.

Bu örnek, HTTP trafiğine izin vermek için basit bir kural kullansa da, daha karmaşık ağ yapılandırmaları oluşturmak için ağ güvenlik gruplarını ve kurallarını da kullanabilirsiniz. 




