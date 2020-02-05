---
title: Azure VMware çözümleri (AVS)-genel IP adresi
description: Azure VMware çözümlerinde (AVS) genel IP adresleri ve bunların avantajları hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2cb9d0e33da4447760ae0be216c1dd9868c498bd
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024985"
---
# <a name="avs-public-ip-address-overview"></a>AVS genel IP adresine genel bakış

Genel bir IP adresi, internet kaynaklarının özel bir IP adresinde gelen AVS özel bulut kaynaklarıyla iletişim kurmasına olanak tanır. Özel IP adresi, AVS özel bulut vCenter 'unuzda bir sanal makine veya yazılım yük dengeleyicidir. Genel IP adresi, AVS özel bulutunuzda çalışan hizmetleri Internet 'e sunmanıza olanak tanır.

Genel IP adresi, atamasını kaldırana kadar özel IP adresine ayrılmıştır. Genel IP adresi yalnızca bir özel IP adresine atanabilir.

Genel IP adresiyle ilişkili bir kaynak, her zaman internet erişimi için genel IP adresini kullanır. Varsayılan olarak, genel IP adresinde yalnızca giden internet erişimine izin verilir.  Genel IP adresindeki gelen trafik reddedildi.  Gelen trafiğe izin vermek için, genel IP adresi için belirli bir bağlantı noktasına bir güvenlik duvarı kuralı oluşturun.

## <a name="benefits"></a>Avantajlar

Gelen iletişim için genel bir IP adresi kullanmak şunları sağlar:

* Dağıtılmış hizmet reddi (DDoS) saldırısı engellemesi. Bu koruma, genel IP adresi için otomatik olarak etkinleştirilir.
* Her zaman açık trafik izleme ve genel ağ düzeyinde saldırıları gerçek zamanlı olarak azaltma. Bu savunmaları, Microsoft çevrimiçi hizmetler tarafından kullanılan savunlardır.
* Azure genel ağının tüm ölçeği. Ağ, bölgeler arasında saldırı trafiğini dağıtmak ve azaltmak için kullanılabilir.  

## <a name="next-steps"></a>Sonraki adımlar

* [Genel IP adresi ayırmayı](public-ips.md) öğrenin