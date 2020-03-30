---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 07/10/2019
ms.author: danlep
ms.openlocfilehash: 8106b1a77f11874b1b13f41bfc9e62ea800afc98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68361537"
---
### <a name="database-tier"></a>Veritabanı katmanı

Veritabanı katmanı, uygulama için veritabanı örneklerini içerir. Veritabanı bir Oracle DB, Oracle RAC veya Oracle Exadata Veritabanı sistemi olabilir. 

Seçim Oracle DB'yi kullanmaksa, veritabanı örneği Azure Marketi'nde bulunan Oracle DB görüntüleri aracılığıyla Azure'da dağıtılabilir. Alternatif olarak, Oracle DB'yi OCI'daki bir PaaS modelinde dağıtmak için Azure ve OCI arasındaki ara bağlantıdan yararlanabilirsiniz.

Oracle RAC için, Oracle RAC'yi Azure CloudSimple'da IaaS modelinde veya OCI'da PaaS modelinde dağıtabilirsiniz. İki düğümlü RAC sistemi kullanmanız önerilir. 

Son olarak, Exadata sistemleri için OCI ara bağlantısını kullanın ve Exadata sistemini OCI'de dağıtın. Yukarıdaki mimari diyagram, OCI'da iki alt ağ arasında dağıtılan bir Exadata sistemini gösterir.

Üretim senaryoları için, veritabanının birden çok örneğini iki kullanılabilirlik bölgesi (Azure'da dağıtılıyorsa) veya iki kullanılabilirlik etki alanı (OCI'de) dağıtın. Birincil ve bekleme veritabanlarını eşitlemek için Oracle Active Data Guard'ı kullanın.

Veritabanı katmanı yalnızca orta katmandan istek alır. Yalnızca yönetim nedenleriyle kale sunucusundan orta katmandan 1521 bağlantı noktasındaki isteklere ve 22 bağlantı noktasından gelen isteklere izin vermek için bir ağ güvenlik grubu (Veritabanını OCI'de dağıtıyorsanız güvenlik listesi) ayarlamanız önerilir.

OCI'da dağıtılan veritabanları için, FastConnect devrenize bağlı dinamik bir yönlendirme ağ geçidi (DRG) ile ayrı bir sanal bulut ağı kurulmalıdır.