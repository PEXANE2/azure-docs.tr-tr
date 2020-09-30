---
title: Azure VMware çözümünü şirket içi ortamınıza bağlama
description: Azure VMware çözümünü şirket içi ortamınıza bağlamayı öğrenin.
ms.topic: tutorial
ms.date: 10/02/2020
ms.openlocfilehash: 2a0cb641df00f3e580e87e38aff382d8e8101fc7
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91583422"
---
# <a name="connect-azure-vmware-solution-to-your-on-premises-environment"></a>Azure VMware çözümünü şirket içi ortamınıza bağlama

Bu makalede, Azure VMware çözümünü şirket içi ortamınıza bağlamayı [planlama sırasında toplanan bilgileri](production-ready-deployment-steps.md) kullanmaya devam edersiniz.

Başlamadan önce, Azure VMware çözümünü şirket içi ortamınıza bağlamak için iki önkoşul vardır:

- Şirket içi ortamınızdan Azure 'a bir ExpressRoute devresi.
- [Planlama aşamasının](production-ready-deployment-steps.md)bir parçası olarak tanımladığınız expressroute Global Reach eşlemesi için, çakışmayan bir/29 çakışmayan ağ adresi bloğu.

>[!NOTE]
> VPN aracılığıyla bağlanabilirsiniz, ancak bu hızlı başlangıç belgesi için kapsam dışındadır.

## <a name="establish-an-expressroute-global-reach-connection"></a>ExpressRoute Global Reach bağlantısı kurma

ExpressRoute Global Reach kullanarak Azure VMware Çözüm özel bulutunuz ile şirket içi bağlantı kurmak için, Şirket [içi ortamları özel bir bulut](tutorial-expressroute-global-reach-private-cloud.md) öğreticisine izleyin.



## <a name="verify-on-premises-network-connectivity"></a>Şirket içi ağ bağlantısını doğrulama

Artık ExpressRoute 'un NSX-T ağ kesimlerini ve Azure VMware Çözüm Yönetimi segmentlerini bağlandığı **Şirket içi kenar yönlendiricinizde** görmeniz gerekir.

>[!NOTE]
>Herkes farklı bir ortama sahiptir ve bu yolların şirket içi ağa geri yayılmasına izin vermeniz gerekir.  

Bazı ortamlarda ExpressRoute devresini koruyan güvenlik duvarları olacaktır.  Hiçbir güvenlik duvarı yoksa ve yol ayıklama işlemi gerçekleşmezseniz, Azure VMware Çözüm vCenter Server veya şirket içi ortamınızdan NSX-T segmentindeki bir [VM](deploy-azure-vmware-solution.md#add-a-vm-on-the-nsx-t-network-segment) 'ye ping gönderebilirsiniz.

Ayrıca, NSX-T segmentindeki VM 'den şirket içi ortamınızdaki kaynaklara ulaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

VMware HCX 'i dağıtmak ve yapılandırmak için sonraki bölüme geçin

> [!div class="nextstepaction"]
> [VMware HCX 'i dağıtma ve yapılandırma](tutorial-deploy-vmware-hcx.md)