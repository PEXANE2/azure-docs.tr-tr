---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 2689e81b089147dfc913fb119e0a499d60574b60
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050247"
---
Bu adımda, aynı ağda çalışan bir istemci uygulamasını kullanarak kullanılabilirlik grubu dinleyicisini test edersiniz.

İstemci bağlantısı aşağıdaki gereksinimlere sahiptir:

* Dinleyiciye yönelik istemci bağlantıları, her zaman açık kullanılabilirlik çoğaltmalarını barındırıp 'den farklı bir bulut hizmetinde bulunan makinelerden gelmelidir.
* Her zaman açık çoğaltmalar farklı alt ağlarda ise, istemcilerin bağlantı dizesinde *MultiSubnetFailover = true* belirtmesi gerekir. Bu durum, çeşitli alt ağlardaki çoğaltmalara yönelik paralel bağlantı denemelerine neden olur. Bu senaryo, bölgeler arası her zaman açık kullanılabilirlik grubu dağıtımı içerir.

Bir örnek, aynı Azure sanal ağındaki VM 'lerden birinden (bir çoğaltma barındıran bir tane değil) bir dinleyiciye bağlanmasıdır. Bu testi tamamlamaya yönelik kolay bir yol, SQL Server Management Studio kullanılabilirlik grubu dinleyicisine bağlamayı denemenize olanak sağlar. Başka bir basit yöntem [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx)aşağıdaki gibi çalıştırılır:

```console
sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15
```

> [!NOTE]
> EndpointPort değeri *1433*ise, çağrısında belirtmeniz gerekmez. Önceki çağrı Ayrıca, istemci makinenin aynı etki alanına katıldığını ve çağıranın, Windows kimlik doğrulaması kullanılarak veritabanı için izin verildiğini varsayar.
> 
> 

Dinleyiciyi test ettiğinizde, istemcilerin yük devretme genelinde dinleyiciye bağlanabilmelerini sağlamak için kullanılabilirlik grubunun yükünü devretmek için emin olun.

