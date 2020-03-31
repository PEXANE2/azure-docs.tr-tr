---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: d579e7a4fd83c1a0ce335e0b2357dcbafb217398
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188316"
---
Bu adımda, aynı ağda çalışan bir istemci uygulaması kullanarak kullanılabilirlik grubu dinleyicisini sınarsınız.

İstemci bağlantısı nın aşağıdaki gereksinimleri vardır:

* Dinleyiciye istemci bağlantıları, Her Zaman Kullanılabilirlik yinelemelerini barındıran olandan farklı bir bulut hizmetinde bulunan makinelerden gelmelidir.
* Always On kopyaları farklı alt ağlardaysa, istemciler bağlantı dizesinde *MultisubnetFailover=True* belirtmelidir. Bu durum, çeşitli alt ağlarda yinelemelere paralel bağlantı girişimleri ile sonuçlanır. Bu senaryo, her zaman kullanılabilirlik grubu dağıtımında bir çapraz bölge içerir.

Bir örnek, dinleyiciye aynı Azure sanal ağındaki VM'lerden (ancak yinelemebarındıran bir ağda olmayan) bağlanmaktır. Bu testi tamamlamanın kolay bir yolu, SQL Server Management Studio'yu kullanılabilirlik grubu dinleyicisine bağlamayı denemektir. Başka bir basit yöntem [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx)çalıştırmak için , aşağıdaki gibi:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> EndpointPort değeri *1433*ise, çağrıda belirtmeniz gerekmez. Önceki aramada, istemci makinenin aynı etki alanına birleştiği ve arayana Windows kimlik doğrulaması kullanılarak veritabanında izin verildiği de varsayar.
> 
> 

Dinleyiciyi sınarken, istemcilerin başarısız olduğunda dinleyiciye bağlanabilmesini sağlamak için kullanılabilirlik grubu üzerinde başarısız olduğunuzdan emin olun.

