---
title: CloudSimple-Azure genel IP adresi tarafından VMware çözümü
description: CloudSimple 'a göre VMware çözümünde genel IP adresleri ve bunların avantajları hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 29d2bdb75a7f4409e6b3ffee2608cfe7adde6b1f
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812531"
---
# <a name="cloudsimple-public-ip-address-overview"></a>CloudSimple genel IP adresine genel bakış

Genel bir IP adresi, internet kaynaklarının gelen ve özel bulut kaynaklarına özel bir IP adresinde iletişim kurmasına olanak tanır. Özel IP adresi bir sanal makine ya da bir yazılım yük dengeleyicidir. Özel IP adresi özel bulut vCenter 'unuzda. Genel IP adresi, özel bulutunuzda çalışan hizmetleri Internet 'e sunmanıza olanak tanır.

Genel IP adresi, atamasını kaldırana kadar özel IP adresine ayrılmıştır. Genel IP adresi yalnızca bir özel IP adresine atanabilir.

Genel IP adresiyle ilişkili bir kaynak, her zaman internet erişimi için genel IP adresini kullanır. Varsayılan olarak, genel IP adresinde yalnızca giden internet erişimine izin verilir.  Genel IP adresindeki gelen trafik reddedildi.  Gelen trafiğe izin vermek için, genel IP adresi için belirli bir bağlantı noktasına bir güvenlik duvarı kuralı oluşturun.

## <a name="benefits"></a>Avantajlar

Gelen iletişim için genel bir IP adresi kullanmak şunları sağlar:

* Dağıtılmış hizmet reddi (DDoS) saldırısı engellemesi. Bu koruma, genel IP adresi için otomatik olarak etkinleştirilir.
* Her zaman açık trafik izleme ve genel ağ düzeyinde saldırıları gerçek zamanlı olarak azaltma. Bu savunmaları, Microsoft çevrimiçi hizmetler tarafından kullanılan savunlardır.
* Azure genel ağının tüm ölçeği. Ağ, bölgeler arasında saldırı trafiğini dağıtmak ve azaltmak için kullanılabilir.  

## <a name="next-steps"></a>Sonraki adımlar

* [Genel IP adresi ayırmayı](https://docs.azure.cloudsimple.com/public-ips/) öğrenin
