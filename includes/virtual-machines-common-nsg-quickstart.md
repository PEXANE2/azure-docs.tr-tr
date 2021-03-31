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
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "67188341"
---
Bir alt ağda veya VM ağı arabiriminde ağ filtresi oluşturarak Azure 'da bir bağlantı noktası Açarsınız veya bir uç nokta oluşturursunuz. Hem gelen hem de giden trafiği denetleyen, trafiği alan kaynağa bağlı bir ağ güvenlik grubundaki bu filtreleri yerleştirebilirsiniz.

Bu makaledeki örnekte, standart TCP bağlantı noktası 80 ' i kullanan bir ağ filtresi oluşturma işlemi gösterilmektedir (ilgili hizmetleri zaten başlattığınız ve VM 'de herhangi bir işletim sistemi güvenlik duvarı kuralını açtığınız varsayılır).

Standart TCP bağlantı noktası 80 üzerinde Web istekleri sunacak şekilde yapılandırılmış bir VM oluşturduktan sonra şunları yapabilirsiniz:

1. Ağ güvenlik grubu oluşturma.

2. Trafiğe izin veren bir gelen güvenlik kuralı oluşturun ve aşağıdaki ayarlara değerler atayın:

   - **Hedef bağlantı noktası aralıkları**: 80

   - **Kaynak bağlantı noktası aralıkları**: * (herhangi bir kaynak bağlantı noktasına izin verir)

   - **Öncelik değeri**: varsayılan catch-all reddetme kuralı 'ndan daha düşük bir değer olan 65.500 ve üzeri bir değer girin.

3. Ağ güvenlik grubunu VM ağ arabirimiyle veya alt ağıyla ilişkilendirin.

Bu örnek HTTP trafiğine izin vermek için basit bir kural kullansa da, daha karmaşık ağ yapılandırması oluşturmak için ağ güvenlik grupları ve kuralları da kullanabilirsiniz. 




