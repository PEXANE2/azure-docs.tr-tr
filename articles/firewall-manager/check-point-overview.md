---
title: Check Point CloudGuard Connect kullanarak Azure sanal hub 'ları güvenli hale getirme
description: Azure sanal hub 'larını güvenli hale getirmek için Check Point CloudGuard Connect hakkında bilgi edinin
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 10/30/2020
ms.author: victorh
ms.openlocfilehash: 759b8211aa8790e81c273f3d90d6679e70012ea4
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93133761"
---
# <a name="secure-virtual-hubs-using-check-point-cloudguard-connect"></a>Check Point CloudGuard Connect kullanarak sanal hub 'ları güvenli hale getirme

Check Point CloudGuard Connect, Azure Güvenlik Duvarı Yöneticisi 'nde güvenilir bir güvenlik Iş ortağıdır. Küresel olarak dağıtılan şube ağını, Gelişmiş tehdit önleme ile Internet (B2I) veya sanal ağ ile Internet (V2I) bağlantılarına karşı korur. 

Azure Güvenlik Duvarı Yöneticisi 'nde basit bir yapılandırmayla, bir hizmet olarak CloudGuard Connect Security (SECaaS) aracılığıyla şube hub 'ını ve sanal ağ bağlantılarını Internet 'e yönlendirebilirsiniz. Trafik, hub 'ınızdan, IPSec VPN tünellerinin denetim noktası bulut hizmetine geçişte korunur.

Check Point portalında otomatik eşitlemeyi etkinleştirdiğinizde, Azure portal *güvenli* olarak işaretlenen herhangi bir kaynak otomatik olarak güvenlidir. Varlıklarınızı iki kez yönetmeniz gerekmez. Azure portal bir kez güvenli hale getirmeniz yeterlidir.

Check Point bir şemsiye altında birden çok güvenlik hizmetini birleştirir. Tümleşik güvenlik trafiği bir kez çözülür ve tek bir geçişte denetlenir. Uygulama denetimi, URL filtrelemesi ve Içerik tanıma (DLP), güvenli Web kullanımını ve verilerinizi korumayı zorunlu tutar. IP 'leri ve virüsten koruma, kullanıcıları bilinen ağ kötüye açıklarına karşı korur. Bot, komut ve denetim sunucularıyla bağlantıları engeller ve bir konağa bulaşmışsa sizi uyarır.

Tehdit öykünmesi (korumalı alana alma), kullanıcıların bilinmeyen ve sıfır günlük tehditlere karşı korunmasını sağlar. Check Point SandBlast Zero-Day koruması, dosyaların hızlı bir şekilde karantinaya alındığı ve incelenebildiği bulutta barındırılan bir çok yönlü bir teknolojidir. Ağınıza girmeden önce kötü amaçlı davranışı saptamak için bir sanal korumalı alanda çalışır. Çalışanların tehditlere yanıt vermesini sağlamak için hasar yapılmadan önce tehditleri önler. 

## <a name="deployment-example"></a>Dağıtım örneği

Check Point CloudGuard Connect 'in güvenilir bir Azure Güvenlik iş ortağı olarak nasıl dağıtılacağını görmek için aşağıdaki videoyu izleyin.

> [!VIDEO https://youtu.be/C8AuN76DEmU]

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik iş ortağı sağlayıcısı dağıtma](deploy-trusted-security-partner.md)