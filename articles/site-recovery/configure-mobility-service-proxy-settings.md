---
title: Azure için Mobilite Hizmeti Proxy Ayarlarını Azure Olağanüstü Durum Kurtarma 'ya yapılandırın | Microsoft Dokümanlar
description: Müşteriler kaynak ortamlarında proxy kullandıklarında mobilite hizmetinin nasıl yapılandırılacaklarına ilişkin ayrıntılar sağlar.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/18/2020
ms.author: sideeksh
ms.openlocfilehash: 3d33b5a89a718a41e5c547551f6e7eb4f7033a63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79503133"
---
# <a name="configure-mobility-service-proxy-settings-for-azure-to-azure-disaster-recovery"></a>Azure için Mobilite Hizmeti Proxy Ayarlarını Azure Olağanüstü Durum Kurtarma'ya yapılandırma

Bu makalede, [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak Azure VM'lerini bir bölgeden diğerine çoğaltırken ve kurtarırken hedef Azure sanal makinesinde (VM) ağ yapılandırmalarını özelleştirme konusunda kılavuz lar verilmektedir.

Bu belgenin amacı, Azure'daki Azure Site Kurtarma Mobilite Hizmeti için Proxy Ayarlarını Azure'dan Azure Olağanüstü Durum Kurtarma senaryosuna yapılandırmak için adımlar sağlamaktır. 

Ekseksitler, ağ bağlantılarının uç noktalara bağlanmasına izin veren/izin veren ağ ağ geçitleridir. Genellikle proxy, istemci makinesinin dışında ağ uç noktalarına erişmeye çalışan bir makinedir. Baypas listesi, istemcinin proxy'den geçmeden doğrudan uç noktalara bağlantı kurmasını sağlar. Yalnızca kimlik doğrulaması yapılan istemcilerin proxy kullanabilmesi için ağ yöneticileri tarafından isteğe bağlı olarak bir proxy için kullanıcı adı ve parola ayarlanabilir. 

## <a name="before-you-start"></a>Başlamadan önce

Site [Kurtarma'nın bu senaryo](azure-to-azure-architecture.md)için olağanüstü durum kurtarmayı nasıl sağladığını öğrenin.
[Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak Azure VM'lerini bir bölgeden diğerine çoğaltırken ve kurtarırken [ağ kılavuzunu](azure-to-azure-about-networking.md) anlayın.
Proxy'nizin kuruluşunuzun ihtiyaçlarına göre uygun şekilde ayarlandığından emin olun.

## <a name="configure-the-mobility-service"></a>Mobilite Hizmetini Yapılandır

Mobilite Hizmeti yalnızca kimlik doğrulamamış vekilleri destekler. Site Kurtarma uç noktalarıyla iletişim için proxy ayrıntılarını girmenin iki yolunu sağlar. 

### <a name="method-1-auto-detection"></a>Yöntem 1: Otomatik algılama

Mobilite Hizmeti otomatik etkinleştirme sırasında ortam ayarlarından veya IE Ayarlarından (Yalnızca Windows) proxy ayarlarını algılar. 

- Windows Os: Çoğaltmayı Etkinleştir sırasında Mobilite Hizmeti, Yerel Sistem kullanıcısı için Internet Explorer'da yapılandırılan proxy ayarlarını algılar. Yerel Sistem hesabı için proxy ayarlamak için, bir yönetici bir komut istemi başlatmak için psexec ve sonra Internet Explorer kullanabilirsiniz. 
- Windows İşletim Sistemi: Proxy ayarları ortam değişkenleri http_proxy ve no_proxy olarak yapılandırılır. 
- Linux OS: Proxy ayarları /etc/profile veya /etc/environment ortam değişkenleri http_proxy, no_proxy olarak yapılandırılır. 
- Otomatik algılanan proxy ayarları Mobility Service proxy config dosyası ProxyInfo.conf kaydedilir 
- ProxyInfo.conf varsayılan konumu 
    - Windows: C:\ProgramData\Microsoft Azure Site Kurtarma\Config\ProxyInfo.conf 
    - Linux: /usr/local/InMage/config/ProxyInfo.conf


### <a name="method-2-provide-custom-application-proxy-settings"></a>Yöntem 2: Özel uygulama proxy ayarları sağlayın

Bu durumda, müşteri Mobility Service config dosyası ProxyInfo.conf özel uygulama proxy ayarları sağlar. Bu yöntem, müşterilerin yalnızca Mobilite Hizmeti için proxy veya Azure Site Kurtarma Mobilite Hizmeti için makinedeki uygulamaların geri kalanı için bir proxy (veya proxy olmadan) farklı bir proxy sağlamasına olanak tanır.

## <a name="proxy-template"></a>Proxy şablonu
ProxyInfo.conf aşağıdaki şablonu içerir [proxy] Adres =http://1.2.3.4 Port=5678 BypassList=hypervrecoverymanager.windowsazure.com,login.microsoftonline.com,blob.core.windows.net. BypassList '*.windows.net' gibi joker karakterleri desteklemez, ancak windows.net vermek atlayıp geçmek için yeterlidir. 

## <a name="next-steps"></a>Sonraki adımlar:
- Azure VM'lerini çoğaltmak için [ağ kılavuzunu](site-recovery-azure-to-azure-networking-guidance.md) okuyun.
- [Azure VM'leri çoğaltarak](site-recovery-azure-to-azure.md)olağanüstü durum kurtarma dağıtma.