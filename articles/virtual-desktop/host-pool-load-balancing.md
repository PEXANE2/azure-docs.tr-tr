---
title: Windows Virtual Desktop ana bilgisayar havuzu yük dengeleme - Azure
description: Windows Sanal Masaüstü ortamı için havuz yük dengeleme yöntemlerini barındırın.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 193821ed0df09b87f19e45a82ca42026405a0dc4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127862"
---
# <a name="host-pool-load-balancing-methods"></a>Ana bilgisayar havuzu yük dengeleme yöntemleri

Windows Sanal Masaüstü iki yük dengeleme yöntemini destekler. Her yöntem, ana bilgisayar havuzundaki bir kaynağa bağlandığında kullanıcının oturumunu hangi oturum ana bilgisayara barındıracağını belirler.

Aşağıdaki yük dengeleme yöntemleri Windows Sanal Masaüstü'nde kullanılabilir:

- Genişlik ilk yük dengeleme, kullanıcı oturumlarını ana bilgisayar havuzunda oturum ana bilgisayarları arasında eşit olarak dağıtmanızı sağlar.
- Derinlik-ilk yük dengeleme bir ana bilgisayar havuzunda kullanıcı oturumları ile bir oturum ana bilgisayar doygunluk sağlar. İlk oturum oturum sınırı eşiğine ulaştığında, yük dengeleyicisi yeni kullanıcı bağlantılarını sınırına ulaşana kadar ana bilgisayar havuzundaki bir sonraki oturum ana bilgisayara yönlendirir.

Her ana bilgisayar havuzu yalnızca ona özgü bir yük dengeleme türünü yapılandırabilir. Ancak, her iki yük dengeleme yöntemi de hangi ana bilgisayar havuzunda olurlarsa olsunlar aşağıdaki davranışları paylaşır:

- Bir kullanıcının ana bilgisayarda zaten bir oturumu varsa ve bu oturuma yeniden bağlanıyorsa, yük dengeleyicisi bunları varolan oturumuyla birlikte oturum ana bilgisayara başarıyla yönlendirir. Bu davranış, oturum ana bilgisayarının AllowNewConnections özelliği False olarak ayarlanınca bile geçerlidir.
- Bir kullanıcının ana bilgisayar havuzunda zaten bir oturumu yoksa, yük dengeleyicisi, yük dengeleme sırasında AllowNewConnections özelliği False olarak ayarlanmış oturum ana bilgisayarlarını dikkate almaz.

## <a name="breadth-first-load-balancing-method"></a>Genişlik-ilk yük dengeleme yöntemi

Genişlik ilk yük dengeleme yöntemi, bu senaryo için optimize etmek için kullanıcı bağlantılarını dağıtmak için izin verir. Bu yöntem, havuza konan sanal masaüstü ortamlarına bağlanan kullanıcılar için en iyi deneyimi sağlamak isteyen kuruluşlar için idealdir.

Genişlik ilk yöntem yeni bağlantılar ayarı ilk sorguları oturumu. Yöntem daha sonra en az oturum sayısına sahip oturum ana bilgisayarını seçer. Bir kravat varsa, yöntem sorgudaki ilk oturum ana bilgisayarını seçer.

## <a name="depth-first-load-balancing-method"></a>Derinlik-ilk yük dengeleme yöntemi

Derinlik-ilk yük dengeleme yöntemi, bu senaryo için optimize etmek için bir seferde bir oturum ana bilgisayar doygunluk sağlar. Bu yöntem, ana bilgisayar havuzu için ayırdıkları sanal makine sayısı üzerinde daha ayrıntılı denetim isteyen maliyet bilincine sahip kuruluşlar için idealdir.

Derinlik ilk yöntem yeni bağlantılar ayarı ve maksimum oturum sınırını geçmedi oturum hosts. Yöntem daha sonra en yüksek oturum sayısına sahip oturum ana bilgisayarını seçer. Bir kravat varsa, yöntem sorgudaki ilk oturum ana bilgisayarını seçer.