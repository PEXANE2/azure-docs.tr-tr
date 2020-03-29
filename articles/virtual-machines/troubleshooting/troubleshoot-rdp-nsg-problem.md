---
title: NSG'de RDP bağlantı noktası etkinleştirilemediği için Azure VM'lerine bağlanamıyor | Microsoft Dokümanlar
description: Azure portalındaki NSG yapılandırması nedeniyle RDP'nin başarısız olduğu bir sorunu nasıl gidereceklerini öğrenin | Microsoft Dokümanlar
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: v-jesits
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: 6c877690cf27edd73f1d828b8a1dda6f4f34e780
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77918181"
---
#  <a name="cannot-connect-remotely-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>RdP bağlantı noktası NSG'de etkinleştirilemediği için VM'ye uzaktan bağlanamıyor

Bu makalede, uzak masaüstü protokolü (RDP) bağlantı noktası ağ güvenlik grubunda (NSG) etkinleştirilmediği için Bir Azure Windows sanal makineye (VM) bağlanamayacağınız bir sorunun nasıl çözüleceği açıklanmaktadır.


## <a name="symptom"></a>Belirti

AĞ güvenlik grubunda RDP bağlantı noktası açılmadığından Azure'daki bir VM'ye RDP bağlantısı yapamazsınız.

## <a name="solution"></a>Çözüm 

Yeni bir VM oluşturduğunuzda, Internet'ten gelen tüm trafik varsayılan olarak engellenir. 

Bir NSG'deki RDP bağlantı noktasını etkinleştirmek için aşağıdaki adımları izleyin:
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Sanal Makinelerde,** sorun olan VM'yi seçin. 
3. **Ayarlar'da** **Ağ'ı**seçin. 
4. **Gelen bağlantı noktası kurallarında,** RDP bağlantı noktasının doğru ayarlanıp ayarlanmadığını denetleyin. Aşağıdaki yapılandırmanın bir örneğidir: 

    **Öncelik**: 300 </br>
    **İsim**: Port_3389 </br>
    **Bağlantı Noktası:** 3389 </br>
    **Protokol**: TCP </br>
    **Kaynak**: Herhangi bir </br>
    **Yerler**: Herhangi bir </br>
    **Action**: İzin ver </br>

Kaynak IP adresini belirtirseniz, bu ayar VM'ye bağlanmak için yalnızca belirli bir IP adresinden veya IP adresi aralığından gelen trafiğe izin verir. RDP oturumunu başlatmak için kullandığınız bilgisayarın aralık içinde olduğundan emin olun.

NSG'ler hakkında daha fazla bilgi için [ağ güvenlik grubuna](../../virtual-network/security-overview.md)bakın.

> [!NOTE]
> RDP bağlantı noktası 3389 Internet'e maruz kalır. Bu nedenle, bu bağlantı noktasını yalnızca sınama için önerilenler için kullanmanızı öneririz. Üretim ortamları için VPN veya özel bağlantı kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

RdP bağlantı noktası NSG'de zaten etkinse, [Azure VM'deki sorun giderme genel hatasına](./troubleshoot-rdp-general-error.md)bakın.



