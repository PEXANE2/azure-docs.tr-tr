---
title: Azure Application Gateway ile VMware sanal makinelerini kullanma
description: Azure Application Gateway 'in, VMware sanal makinelerinde çalışan Web sunucuları için gelen Web trafiğini yönetmek üzere nasıl kullanılacağını açıklar. AVS özel bulut ortamı
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 94cc6e40b88fe631d525f41001034f5dada05397
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77015465"
---
# <a name="use-azure-application-gateway-with-vmware-virtual-machines-in-the-avs-private-cloud-environment"></a>Azure Application Gateway, AVS özel bulut ortamında VMware sanal makineleri ile kullanma

AVS özel bulut ortamınızdaki VMware sanal makinelerinde çalışan web sunucularınız için gelen Web trafiğini yönetmek üzere Azure Application Gateway kullanabilirsiniz.

Ortak özel karma dağıtımda Azure Application Gateway yararlanarak, uygulamalarınıza yönelik Web trafiğini yönetebilir, VMware ortamında çalışan hizmetler için güvenli bir ön uç ve yük boşaltma SSL işlemi sağlayabilirsiniz. Azure Application Gateway, gelen Web trafiğini, yapılandırılmış kurallara ve sistem durumu araştırmalara göre VMware ortamlarında bulunan arka uç havuzu örneklerine yönlendirir.

Bu Azure Application Gateway çözümü şunları yapmanızı gerektirir:

* Bir Azure aboneliğine sahip olmanız.
* Bir Azure sanal ağı ve sanal ağ içinde bir alt ağ oluşturun ve yapılandırın.
* AVS özel bulutunuzun ExpressRoute kullanarak NSG kuralları ve vNet 'iniz oluşturun ve yapılandırın.
* AVS özel bulutunuzu oluşturun & yapılandırın.
* Azure Application Gateway oluşturun & yapılandırın.

## <a name="azure-application-gateway-deployment-scenario"></a>Azure Application Gateway dağıtım senaryosu

Bu senaryoda Azure Application Gateway, Azure sanal ağınızda çalışır. Sanal ağ, bir ExpressRoute devresi üzerinden AVS özel bulutuna bağlanır. AVS özel bulutundaki tüm alt ağların sanal ağ alt ağlarından erişilebilen IP 'si vardır.

![Azure sanal ağ 'da Azure yük dengeleyici](media/load-balancer-use-case.png)

## <a name="how-to-deploy-the-solution"></a>Çözümü dağıtma

Dağıtım işlemi aşağıdaki görevlerden oluşur:

1. [Önkoşulların karşılandığından emin olun](#1-verify-prerequisites)
2. [Azure sanal bağlantınızı AVS özel bulutuna bağlama](#2-connect-your-azure-virtual-network-to-your-avs-private-cloud)
3. [Azure Application Gateway dağıtma](#3-deploy-an-azure-application-gateway)
4. [Web sunucusu VM havuzunu, AVS özel bulutunuzda oluşturma ve yapılandırma](#4-create-and-configure-a-web-server-vm-pool-in-your-avs-private-cloud)

## <a name="1-verify-prerequisites"></a>1. önkoşulları doğrulama

Bu önkoşulların karşılandığından emin olun:

* Bir Azure Resource Manager ve sanal ağ zaten oluşturulmuş.
* Azure sanal ağınız içindeki adanmış bir alt ağ (Application Gateway için) zaten oluşturuldu.
* Bir AVS özel bulutu zaten oluşturulmuş.
* Sanal ağ ve alt ağlar arasında özel buluttaki IP alt ağları arasında bir IP çakışması yoktur.

## <a name="2-connect-your-azure-virtual-network-to-your-avs-private-cloud"></a>2. Azure Sanal ağınızı AVS özel bulutuna bağlayın

Azure Sanal ağınızı AVS özel bulutuna bağlamak için bu işlemi izleyin.

1. [AVS portalında ExpressRoute eşleme bilgilerini kopyalayın](virtual-network-connection.md).

2. [Azure sanal ağınız için bir sanal ağ geçidi yapılandırın](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md).

3. [Sanal AĞıNıZı AVS ExpressRoute devresine bağlayın](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md#connect-a-vnet-to-a-circuit---different-subscription).

4. [Sanal ağınızı ExpressRoute devresine bağlamak için kopyaladığınız eşleme bilgilerini kullanın](virtual-network-connection.md).

## <a name="3-deploy-an-azure-application-gateway"></a>3. bir Azure Application Gateway dağıtma

Bu için ayrıntılı yönergeler, [Azure Portal kullanarak yol tabanlı yönlendirme kurallarına sahip bir uygulama ağ geçidi oluşturma](../application-gateway/create-url-route-portal.md)bölümünde bulunur. Gerekli adımların özeti aşağıda verilmiştir:

1. Aboneliğinizde ve kaynak grubunuzda bir sanal ağ oluşturun.
2. Sanal ağınızda (adanmış alt ağ olarak kullanılmak üzere) bir alt ağ oluşturun.
3. Standart Application Gateway oluştur (isteğe bağlı olarak WAF 'yi etkinleştir): Azure portal giriş sayfasından, sayfanın sol üst tarafında bulunan > **ağ** **Application Gateway** > **kaynak** ' na tıklayın. Standart SKU, boyut ' u seçin ve Azure aboneliği, kaynak grubu ve konum bilgilerini sağlayın. Gerekirse, bu uygulama ağ geçidi için yeni bir genel IP oluşturun ve sanal ağ ile uygulama ağ geçidi için ayrılmış alt ağ hakkında ayrıntılı bilgi sağlayın.
4. Sanal makineler içeren bir arka uç havuzu ekleyin ve uygulama ağ geçidinize ekleyin.

## <a name="4-create-and-configure-a-web-server-vm-pool-in-your-avs-private-cloud"></a>4. AVS özel bulutunuzda bir Web sunucusu VM havuzu oluşturun ve yapılandırın

VCenter 'da, tercih ettiğiniz işletim sistemi ve Web sunucusu (Windows/IIS veya Linux/Apache gibi) ile VM 'Ler oluşturun. AVS özel bulutunuzda Web katmanı için tasarlanmış bir alt ağ/VLAN seçin. Web sunucusu VM 'lerinin en az bir vNIC 'in Web katmanı alt ağında olduğunu doğrulayın.
