---
title: CloudSimple tarafından Azure VMware Çözümü - Hizmet
description: CloudSimple hizmetine ve kavramlarına genel bakış sağlar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d128a248c2e6e1e2e35e3b633975ba081e77f028
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024970"
---
# <a name="cloudsimple-service-overview"></a>CloudSimple hizmetine genel bakış

CloudSimple hizmeti, CloudSimple tarafından Azure VMware Solution'ı tüketmenizi sağlar.  Hizmeti oluşturmak düğümleri satın almanızı, düğümleri rezerve etmenizi ve Özel Bulutlar oluşturmanıza olanak tanır.  CloudSimple hizmetinin kullanılabildiği her Azure bölgesinde CloudSimple hizmetini oluşturursunuz. Hizmet, CloudSimple tarafından Azure VMware Solution'ın kenar ağını tanımlar. Kenar ağı, Özel Bulutları'nıza VPN, ExpressRoute ve internet bağlantısı içeren hizmetleri destekler.

## <a name="gateway-subnet"></a>Ağ geçidi alt ağı

CloudSimple hizmeti başına bir ağ geçidi alt ağı gereklidir ve oluşturulduğu bölgeye özgüdür. Ağ geçidi alt ağı oluşturulurken kullanılır ve /28 CIDR bloğu gerektirir.  Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kuranabilecek herhangi bir ağla örtüşmemelidir. CloudSimple ile iletişim sağlayan ağlar şirket içi ağları ve Azure sanal ağı içerir.  Bir ağ geçidi alt ağı oluşturulduktan sonra silinemez.  Hizmet silindiğinde ağ geçidi alt ağı kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da CloudSimple hizmetini](quickstart-create-cloudsimple-service.md)nasıl oluşturabilirsiniz öğrenin.
