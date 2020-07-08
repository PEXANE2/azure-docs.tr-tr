---
title: CloudSimple-genel IP adresi tarafından Azure VMware çözümü
description: CloudSimple tarafından Azure VMware çözümünde genel IP adresleri ve bunların avantajları hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 147ab6b5ace4493e1a0b303c320cb2e81d829b29
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77024985"
---
# <a name="cloudsimple-public-ip-address-overview"></a>CloudSimple genel IP adresine genel bakış

Genel bir IP adresi, internet kaynaklarının özel bir IP adresindeki özel bulut kaynaklarıyla iletişim kurmasına olanak tanır. Özel IP adresi, özel bulut vCenter 'unuzda bir sanal makine veya yazılım yük dengeleyicidir. Genel IP adresi, özel bulutunuzda çalışan hizmetleri Internet 'e sunmanıza olanak tanır.

Genel IP adresi, atamasını kaldırana kadar özel IP adresine ayrılmıştır. Genel IP adresi yalnızca bir özel IP adresine atanabilir.

Genel IP adresiyle ilişkili bir kaynak, her zaman internet erişimi için genel IP adresini kullanır. Varsayılan olarak, genel IP adresinde yalnızca giden internet erişimine izin verilir.  Genel IP adresindeki gelen trafik reddedildi.  Gelen trafiğe izin vermek için, genel IP adresi için belirli bir bağlantı noktasına bir güvenlik duvarı kuralı oluşturun.

## <a name="benefits"></a>Yararları

Gelen iletişim için genel bir IP adresi kullanmak şunları sağlar:

* Dağıtılmış hizmet reddi (DDoS) saldırısı engellemesi. Bu koruma, genel IP adresi için otomatik olarak etkinleştirilir.
* Her zaman açık trafik izleme ve genel ağ düzeyinde saldırıları gerçek zamanlı olarak azaltma. Bu savunmaları, Microsoft çevrimiçi hizmetler tarafından kullanılan savunlardır.
* Azure genel ağının tüm ölçeği. Ağ, bölgeler arasında saldırı trafiğini dağıtmak ve azaltmak için kullanılabilir.  

## <a name="next-steps"></a>Sonraki adımlar

* [Genel IP adresi ayırmayı](public-ips.md) öğrenin