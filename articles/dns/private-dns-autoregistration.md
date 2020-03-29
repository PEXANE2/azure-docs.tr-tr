---
title: Azure DNS özel bölgelerinin otomatik kayıt özelliği nedir
description: Azure DNS özel bölgelerinin otomatik kayıt özelliğine genel bakış
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 9/24/2019
ms.author: rohink
ms.openlocfilehash: 9d1854b459e799d5cbb401de9ac717dd7d0fde1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71961191"
---
# <a name="what-is-the-autoregistration-feature-of-azure-dns-private-zones"></a>Azure DNS özel bölgelerinin otomatik kayıt özelliği nedir?

Azure DNS özel bölgeler otomatik kayıt özelliği, sanal ağda dağıtılan sanal makineler için DNS kayıt yönetiminin acısını ortadan çıkarır. Bir sanal ağı özel bir DNS bölgesine [bağlayıp](./private-dns-virtual-network-links.md) tüm sanal makineler için otomatik kaydı etkinleştirdiğinizde, sanal ağda dağıtılan sanal makinelerin DNS kayıtları otomatik olarak özel DNS bölgesinde oluşturulur. İleriye dönük kayıtların (A kayıtları) ek olarak, sanal makineler için ters arama kayıtları (PTR kayıtları) da otomatik olarak oluşturulur.
Sanal ağa daha fazla sanal makine eklerseniz, bu sanal makinelere ait DNS kayıtları da otomatik olarak bağlı özel DNS bölgesinde oluşturulur.

Sanal bir makineyi sildiğinizde, sanal makinenin DNS kayıtları özel DNS bölgesinden otomatik olarak silinir.

Sanal ağ bağlantısı oluştururken "Otomatik kaydı etkinleştir" seçeneğini seçerek otomatik kaydı etkinleştirebilirsiniz.

![Otomatik Kaydı Etkinleştir](./media/privatedns-concepts/enable-autoregistration.png)

## <a name="restrictions"></a>Kısıtlamalar

* Otomatik kayıt yalnızca sanal makineler için çalışır. Dahili yük dengeleyicileri vb. gibi diğer tüm kaynaklar için, sanal ağa bağlı özel DNS bölgesinde el ile DNS kayıtları oluşturabilirsiniz.
* DNS kayıtları yalnızca birincil sanal makine NIC için otomatik olarak oluşturulur. Sanal makinelerinizin birden fazla NIC'si varsa, diğer ağ arabirimleri için DNS kayıtlarını el ile oluşturabilirsiniz.
* IPv6 (AAAA kayıtları) için otomatik kayıt desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI'yi](./private-dns-getstarted-cli.md)kullanarak Azure DNS'de nasıl özel bir bölge oluşturabilirsiniz öğrenin.

* Azure DNS'deki özel bölgelerle gerçekleştirilebilecek bazı yaygın [özel bölge senaryoları](./private-dns-scenarios.md) hakkında bilgi edinin.

* Belirli işlem türleri için bekleyebileceğiniz belirli davranışlar da dahil olmak üzere Azure DNS'deki özel bölgelerle ilgili sık sorulan sorular ve yanıtlar için [Bkz.](./dns-faq-private.md)
