---
title: Azure Route Server (Önizleme) nedir?
description: Azure Route Server 'ın ağ sanal gereci (NVA) ve sanal ağınız arasında yönlendirmeyi nasıl basitleştireceğinizi öğrenin.
services: route-server
author: duongau
ms.service: route-server
ms.topic: overview
ms.date: 03/02/2021
ms.author: duau
ms.openlocfilehash: d868c064b96f58ab3febc1fd3b7f20b74d507cb0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102437170"
---
# <a name="what-is-azure-route-server-preview"></a>Azure Route Server (Önizleme) nedir? 

Azure Route sunucusu, ağ sanal gereci (NVA) ve sanal ağınız arasında dinamik yönlendirmeyi basitleştirir. Yol tablolarını el ile yapılandırma veya sürdürme gereksinimi olmadan BGP yönlendirme protokolünü ve Azure sanal ağı (VNET) Azure yazılım tanımlı ağını (SDN) destekleyen herhangi bir NVA arasında doğrudan Sınır Ağ Geçidi Protokolü (BGP) yönlendirme protokolü üzerinden yönlendirme bilgileri alışverişi yapmanıza olanak sağlar. Azure yol sunucusu, tam olarak yönetilen bir hizmettir ve yüksek kullanılabilirlik ile yapılandırılır.

> [!IMPORTANT]
> Azure yol sunucusu (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-does-it-work"></a>Nasıl çalışır?

Aşağıdaki diyagramda Azure Route sunucusunun bir SDWAN NVA ve bir sanal ağdaki güvenlik NVA ile nasıl çalıştığı gösterilmektedir. BGP eşlemesini kurduktan sonra Azure Route sunucusu, SDWAN gerecinden bir şirket içi yol (10.250.0.0/16) ve güvenlik duvarından bir varsayılan yol (0.0.0.0/0) alır. Bu yollar daha sonra sanal ağdaki VM 'lerde otomatik olarak yapılandırılır. Sonuç olarak, şirket içi ağa giden tüm trafik SDWAN gerecine gönderilir. Internet 'e yönelik tüm trafik güvenlik duvarına gönderilir. Ters yönde, Azure Route sunucusu sanal ağ adresini (10.1.0.0/16) hem NVA 'lar hem de gönderecek. SDWAN gereci, şirket içi ağa daha fazla yayabilir.

![Bir sanal ağda yapılandırılmış Azure yol sunucusunu gösteren diyagram.](./media/overview/route-server-overview.png)

## <a name="key-benefits"></a>Önemli avantajlar 

Azure Route Server, sanal ağınızdaki NVA 'nın yapılandırma, yönetim ve dağıtımını basitleştirir.  

* Artık sanal ağ adresleriniz güncelleştirildiğinde NVA 'daki yönlendirme tablosunu el ile güncelleştirmeniz gerekmez. 

* NVA yeni yollar duyurur veya eskileri geri çekdiğinde artık [Kullanıcı tanımlı yolları](../virtual-network/virtual-networks-udr-overview.md) el ile güncelleştirmeniz gerekmez. 

* Azure Route sunucusu ile NVA 'nın birden çok örneğini kullanabilirsiniz. , Tasarımınıza bağlı olarak, NVA 'ınızda BGP özniteliklerini yapılandırabilir (örn. performans için etkin-etkin veya dayanıklılık için etkin-Pasif), Azure Route Server 'ın hangi NVA örneğinin etkin olduğunu veya hangisinin pasif olduğunu bilmesini sağlayabilirsiniz. 

* NVA ve Azure yol sunucusu arasındaki arabirim, ortak bir standart protokolü temel alır. NVA BGP 'yi desteklediği sürece, bunu Azure Route sunucusu ile de aktarabilirsiniz. Daha fazla bilgi için bkz. [Route Server desteklenen yönlendirme protokolleri](route-server-faq.md#protocol).

* Azure Route sunucusu ' nu yeni veya var olan sanal ağınızdan dağıtabilirsiniz. 

## <a name="faq"></a>SSS

Azure Route sunucusu hakkında sık sorulan sorular için bkz. [Azure yol sunucusu SSS](route-server-faq.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Route Server 'ı yapılandırmayı öğrenin](quickstart-configure-route-server-powershell.md)
- [Azure yönlendirme sunucusu 'nun Azure ExpressRoute ve Azure VPN ile nasıl çalıştığını öğrenin](expressroute-vpn-support.md)
