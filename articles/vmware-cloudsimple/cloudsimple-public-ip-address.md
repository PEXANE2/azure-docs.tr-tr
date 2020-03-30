---
title: CloudSimple tarafından Azure VMware Çözümü - Genel IP adresi
description: CloudSimple tarafından Azure VMware Solution'da genel IP adresleri ve avantajları hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 147ab6b5ace4493e1a0b303c320cb2e81d829b29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024985"
---
# <a name="cloudsimple-public-ip-address-overview"></a>CloudSimple genel IP adresine genel bakış

Genel BIR IP adresi, internet kaynaklarının özel bir IP adresinden Özel Bulut kaynaklarına gelen iletişim kurmasına olanak tanır. Özel IP adresi, Özel Bulut vCenter'ınızda sanal bir makine veya yazılım yük dengeleyicisidir. Genel IP adresi, Özel Bulut'unuzda çalışan hizmetleri internete maruz bırakmanızı sağlar.

Herkese açık IP adresi, siz atayana kadar özel IP adresine adanmıştır. Ortak bir IP adresi yalnızca bir özel IP adresine atanabilir.

Genel bir IP adresiyle ilişkili bir kaynak, internet erişimi için her zaman genel IP adresini kullanır. Varsayılan olarak, herkese açık bir IP adresinde yalnızca giden internet erişimine izin verilir.  Genel IP adresine gelen trafik reddedildi.  Gelen trafiğe izin vermek için, belirli bağlantı noktasına açılan genel IP adresi için bir güvenlik duvarı kuralı oluşturun.

## <a name="benefits"></a>Avantajlar

Gelen iletişim kurmak için ortak bir IP adresi kullanmak şunları sağlar:

* Dağıtılmış hizmet reddi (DDoS) saldırı önleme. Bu koruma, genel IP adresi için otomatik olarak etkinleştirilir.
* Her zaman trafik izleme ve ortak ağ düzeyinde saldırıların gerçek zamanlı azaltma. Bu savunma, Microsoft çevrimiçi hizmetleri tarafından kullanılan aynı savunmadır.
* Azure global ağının tüm ölçeği. Ağ, bölgeler arasında saldırı trafiğini dağıtmak ve azaltmak için kullanılabilir.  

## <a name="next-steps"></a>Sonraki adımlar

* [Genel BIR IP adresini](public-ips.md) nasıl tahsis edemeyen