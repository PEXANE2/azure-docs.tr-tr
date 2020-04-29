---
title: Azure için Mobility hizmeti proxy ayarlarını Azure olağanüstü durum kurtarma ile yapılandırma | Microsoft Docs
description: Müşteriler kaynak ortamlarında bir ara sunucu kullandıklarında Mobility hizmetinin nasıl yapılandırılacağı hakkında ayrıntılı bilgi sağlar.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/18/2020
ms.author: sideeksh
ms.openlocfilehash: 3d33b5a89a718a41e5c547551f6e7eb4f7033a63
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79503133"
---
# <a name="configure-mobility-service-proxy-settings-for-azure-to-azure-disaster-recovery"></a>Azure için Mobility hizmeti proxy ayarlarını Azure olağanüstü durum kurtarma ile yapılandırma

Bu makalede [Azure Site Recovery](site-recovery-overview.md), Azure VM 'lerini bir bölgeden diğerine çoğaltırken ve kurtarırken hedef Azure sanal MAKINESINDE (VM) ağ yapılandırmalarının özelleştirilmesi hakkında rehberlik sağlanır.

Bu belgenin amacı, Azure 'dan Azure 'a olağanüstü durum kurtarma senaryosunda Azure Site Recovery Mobility hizmeti için proxy ayarlarını yapılandırma adımlarını sağlamaktır. 

Proxy 'ler, uç noktalara ağ bağlantılarına izin veren/izin vermeyen Ağ geçitlerdir. Genellikle proxy, ağ uç noktalarına erişmeye çalışan istemci makine dışındaki bir makinedir. Atlama listesi, istemcinin proxy 'ye geçmeden doğrudan uç noktalara bağlantı yapmasına olanak sağlar. Bir Kullanıcı adı ve parola, ağ yöneticileri tarafından yalnızca kimliği doğrulanmış istemcilerin ara sunucu kullanabilmesi için isteğe bağlı olarak bir proxy için ayarlanabilir. 

## <a name="before-you-start"></a>Başlamadan önce

Site Recovery [Bu senaryo](azure-to-azure-architecture.md)için olağanüstü durum kurtarma nasıl sağladığını öğrenin.
[Azure Site Recovery](site-recovery-overview.md)kullanarak, Azure VM 'lerini bir bölgeden diğerine çoğaltırken ve kurtarırken [ağ kılavuzunu](azure-to-azure-about-networking.md) anlayın.
Kuruluşunuzun gereksinimlerine bağlı olarak proxy 'nizin uygun şekilde ayarlandığından emin olun.

## <a name="configure-the-mobility-service"></a>Mobility hizmetini yapılandırma

Mobility hizmeti yalnızca kimliği doğrulanmamış proxy 'leri destekler. Site Recovery uç noktalarıyla iletişim için proxy ayrıntılarını girmek üzere iki yol sağlar. 

### <a name="method-1-auto-detection"></a>Yöntem 1: otomatik algılama

Mobility hizmeti, çoğaltmayı etkinleştirme sırasında ortam ayarlarından veya IE ayarlarından (yalnızca Windows) ara sunucu ayarlarını otomatik olarak algılar. 

- Windows işletim sistemi: çoğaltmayı etkinleştirme sırasında Mobility hizmeti, yerel sistem kullanıcısı için Internet Explorer 'da yapılandırıldığı şekilde proxy ayarlarını algılar. Yerel sistem hesabı için proxy ayarlamak için, bir yönetici PsExec kullanarak bir komut istemi başlatabilir ve ardından Internet Explorer kullanabilir. 
- Windows işletim sistemi: ara sunucu ayarları http_proxy ve no_proxy ortam değişkenleri olarak yapılandırılır. 
- Linux işletim sistemi: ara sunucu ayarları,/etc/profile veya/etc/Environment ' da http_proxy no_proxy ortam değişkenleri olarak yapılandırılır. 
- Otomatik algılanan ara sunucu ayarları, Mobility hizmeti proxy yapılandırma dosyası ProxyInfo. conf dosyasına kaydedilir 
- ProxyInfo. conf öğesinin varsayılan konumu 
    - Windows: C:\ProgramData\Microsoft Azure Site Recovery\proxyfo.exe 
    - Linux:/usr/local/InMage/config/proxınfo.exe


### <a name="method-2-provide-custom-application-proxy-settings"></a>Yöntem 2: özel uygulama proxy 'si ayarları sağlama

Bu durumda, müşteri Mobility hizmeti yapılandırma dosyası ProxyInfo. conf ' de özel uygulama proxy 'si ayarları sağlar. Bu yöntem, müşterilerin yalnızca Mobility hizmeti için ara sunucu veya makinedeki geri kalan uygulamalar için bir proxy (veya proxy olmadan) Azure Site Recovery Mobility hizmeti için farklı bir ara sunucu sağlamasına olanak tanır.

## <a name="proxy-template"></a>Ara sunucu şablonu
ProxyInfo. conf şu şablonu içerir [proxy] Adres =http://1.2.3.4 Port = 5678 BypassList = hypervrecoverymanager. windowsazure. com, LOGIN. microsoftonline. com, blob. Core. Windows. net. BypassList, ' *. windows.net ' gibi joker karakterleri desteklemez, ancak windows.net verme için yeterince iyidir. 

## <a name="next-steps"></a>Sonraki adımlar:
- Azure VM 'lerini çoğaltmak için [ağ kılavuzunu](site-recovery-azure-to-azure-networking-guidance.md) okuyun.
- [Azure VM 'lerini çoğaltarak](site-recovery-azure-to-azure.md)olağanüstü durum kurtarma dağıtın.