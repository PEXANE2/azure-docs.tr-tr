---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: d579e7a4fd83c1a0ce335e0b2357dcbafb217398
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188316"
---
Bu adımda, aynı ağda çalışan bir istemci uygulamasını kullanarak kullanılabilirlik grubu dinleyicisini test edersiniz.

İstemci bağlantısı aşağıdaki gereksinimlere sahiptir:

* Dinleyiciye yönelik istemci bağlantıları, her zaman açık kullanılabilirlik çoğaltmalarını barındırıp 'den farklı bir bulut hizmetinde bulunan makinelerden gelmelidir.
* Her zaman açık çoğaltmalar farklı alt ağlarda ise, istemcilerin bağlantı dizesinde *MultiSubnetFailover = true* belirtmesi gerekir. Bu durum, çeşitli alt ağlardaki çoğaltmalara yönelik paralel bağlantı denemelerine neden olur. Bu senaryo, bölgeler arası her zaman açık kullanılabilirlik grubu dağıtımı içerir.

Bir örnek, aynı Azure sanal ağındaki VM 'lerden birinden (bir çoğaltma barındıran bir tane değil) bir dinleyiciye bağlanmasıdır. Bu testi tamamlamaya yönelik kolay bir yol, SQL Server Management Studio kullanılabilirlik grubu dinleyicisine bağlamayı denemenize olanak sağlar. Başka bir basit yöntem de [sqlcmd. exe](https://technet.microsoft.com/library/ms162773.aspx)' yi aşağıdaki gibi çalıştırmalıdır:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> EndpointPort değeri *1433*ise, çağrısında belirtmeniz gerekmez. Önceki çağrı Ayrıca, istemci makinenin aynı etki alanına katıldığını ve çağıranın, Windows kimlik doğrulaması kullanılarak veritabanı için izin verildiğini varsayar.
> 
> 

Dinleyiciyi test ettiğinizde, istemcilerin yük devretme genelinde dinleyiciye bağlanabilmelerini sağlamak için kullanılabilirlik grubunun yükünü devretmek için emin olun.

