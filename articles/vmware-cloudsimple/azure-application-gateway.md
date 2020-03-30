---
title: VMware sanal makineleriyle Azure Uygulama Ağ Geçidi'ni kullanma
description: VMware sanal makinelerinde çalışan web sunucuları için gelen web trafiğini yönetmek için Azure uygulama ağ geçidinin nasıl kullanılacağını açıklar CloudSimple Private Cloud ortamını kazanır
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2cbfdd358fdfd5403c677c067376142169cdc6bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77015465"
---
# <a name="use-azure-application-gateway-with-vmware-virtual-machines-in-the-cloudsimple-private-cloud-environment"></a>CloudSimple Private Cloud ortamında VMware sanal makineleriyle Azure Uygulama Ağ Geçidi'ni kullanın

CloudSimple Private Cloud ortamınızdaki VMware sanal makinelerinde çalışan web sunucularınızın gelen web trafiğini yönetmek için Azure Uygulama Ağ Geçidi'ni kullanabilirsiniz.

Azure Uygulama Ağ Geçidi'ni genel-özel karma dağıtımda yararlanarak, uygulamalarınız için web trafiğini yönetebilir, güvenli bir ön uç sağlayabilir ve VMware ortamında çalışan hizmetleri için SSL işlemeyi boşaltabilirsiniz. Azure Application Gateway, vmware ortamlarında bulunan arka uç havuz örneklerini yapılandırılmış kurallara ve sistem durumu sondalarına göre arka plana alan web trafiğini yönlendirir.

Bu Azure Uygulama Ağ Geçidi çözümü şunları şunları nızı gerektirir:

* Bir Azure aboneliğine sahip olmanız.
* Bir Azure sanal ağı ve sanal ağ içinde bir alt ağ oluşturun ve yapılandırın.
* ExpressRoute'u kullanarak NSG kurallarını oluşturun ve yapılandırdıve vNet'inize göre CloudSimple Private Cloud'unuza yönlendirin.
* Özel Bulutunuzu Yapılandırma& oluşturun.
* Azure Uygulama Ağ Geçidinizi Yapılandırma& Oluşturun.

## <a name="azure-application-gateway-deployment-scenario"></a>Azure Uygulama Ağ Geçidi dağıtım senaryosu

Bu senaryoda, Azure Uygulama Ağ Geçidi Azure sanal ağınızda çalışır. Sanal ağ, Bir ExpressRoute devresi üzerinden Özel Bulut'unuza bağlanır. Özel Bulut'taki tüm alt ağlarsanal ağ alt ağlarından IP'ye erişilebiliyor.

![Azure sanal ağında Azure yük dengeleyicisi](media/load-balancer-use-case.png)

## <a name="how-to-deploy-the-solution"></a>Çözüm nasıl dağıtılır?

Dağıtım işlemi aşağıdaki görevlerden oluşur:

1. [Ön koşulların karşılandığını doğrulayın](#1-verify-prerequisites)
2. [Azure sanal bağlantınızı Özel Bulut'a bağlayın](#2-connect-your-azure-virtual-network-to-your-private-cloud)
3. [Azure uygulama ağ geçidi dağıtma](#3-deploy-an-azure-application-gateway)
4. [Özel Bulutunuzda Web Sunucusu VM havuzu oluşturma ve yapılandırma](#4-create-and-configure-a-web-server-vm-pool-in-your-private-cloud)

## <a name="1-verify-prerequisites"></a>1. Ön koşulları doğrulayın

Bu ön koşulların karşılandığını doğrulayın:

* Azure Kaynak Yöneticisi ve sanal ağ zaten oluşturuldu.
* Azure sanal ağınızda özel bir alt ağ (Uygulama Ağ Geçidi için) zaten oluşturulmuştur.
* CloudSimple Private Cloud zaten oluşturuldu.
* Sanal ağdaki IP alt ağları ile Özel Bulut'taki alt ağlar arasında IP çakışması yoktur.

## <a name="2-connect-your-azure-virtual-network-to-your-private-cloud"></a>2. Azure sanal ağınızı Özel Bulut'unuza bağlayın

Azure sanal ağınızı Özel Bulut'unuza bağlamak için bu işlemi izleyin.

1. [CloudSimple portalında, ExpressRoute bakan bilgilerini kopyalayın.](virtual-network-connection.md)

2. [Azure sanal ağınız için bir sanal ağ ağ geçidi yapılandırın.](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md)

3. [Sanal ağınızı CloudSimple ExpressRoute devresine bağla.](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md#connect-a-vnet-to-a-circuit---different-subscription)

4. [Sanal ağınızı ExpressRoute devresine bağlamak için kopyaladığınız eş bilgilerini kullanın.](virtual-network-connection.md)

## <a name="3-deploy-an-azure-application-gateway"></a>3. Azure uygulama ağ geçidi dağıtma

Bunun ayrıntılı yönergeleri, [Azure portalını kullanarak yol tabanlı yönlendirme kuralları içeren bir uygulama ağ geçidi oluştur'da](../application-gateway/create-url-route-portal.md)kullanılabilir. Gerekli adımların bir özeti aşağıda veda edebilirsiniz:

1. Abonelik ve kaynak grubunuzda sanal ağ oluşturun.
2. Sanal ağınızda bir alt ağ (özel alt ağ olarak kullanılmak üzere) oluşturun.
3. Standart bir Uygulama Ağ Geçidi oluşturun (isteğe bağlı olarak WAF'ı etkinleştirin: Azure portal ana sayfasından, sayfanın sol üst tarafından **Kaynak** > **Ağı** > **Uygulama Ağ Geçidi'ni** tıklatın. Standart SKU'yu ve boyutunu seçin ve Azure aboneliği, kaynak grubu ve konum bilgilerini sağlayın. Gerekirse, bu uygulama ağ geçidi için yeni bir genel IP oluşturun ve sanal ağ ve uygulama ağ geçidi için özel alt ağ hakkında ayrıntılar sağlayın.
4. Sanal makinelerle bir arka uç havuzu ekleyin ve uygulama ağ geçidinize ekleyin.

## <a name="4-create-and-configure-a-web-server-vm-pool-in-your-private-cloud"></a>4. Özel Bulut'unuzda bir web sunucusu VM havuzu oluşturun ve yapılandırır

vCenter'da, seçtiğiniz işletim sistemi ve web sunucusuyla (Windows/IIS veya Linux/Apache gibi) VM'ler oluşturun. Özel Bulut'unuzdaki web katmanı için belirlenmiş bir alt ağ/VLAN seçin. Web sunucusu VM'lerin en az bir vNIC'sinin web katmanı alt netinde olduğunu doğrulayın.
