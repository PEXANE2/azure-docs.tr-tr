---
title: CloudSimple tarafından Azure VMware Çözümlerini yönetmek için temel kavramlar
titleSuffix: Azure VMware Solution by CloudSimple
description: CloudSimple tarafından Azure VMware Çözümlerini yönetmek için temel kavramları açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 410215550f728d6b11b02b3ef108d429481e42a0
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869022"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solutions-by-cloudsimple"></a>Azure VMware Çözümlerinin CloudSimple tarafından yönetilmesi için temel kavramlar

Azure VMware Çözümlerini CloudSimple tarafından yönetmek, aşağıdaki kavramların anlaşılmasını gerektirir:

* CloudSimple tarafından Azure VMware Çözümleri olarak görüntülenen CloudSimple hizmeti - Service
* CloudSimple tarafından Azure VMware Çözümleri olarak görüntülenen CloudSimple düğümü - Düğüm
* CloudBasit özel bulut
* Servis ağı
* CloudSimple tarafından Azure VMware Çözümleri olarak görüntülenen CloudSimple sanal makine - Sanal makine

## <a name="cloudsimple-service"></a>CloudSimple hizmeti

CloudSimple hizmeti ile Azure portalından CloudSimple tarafından VMware Çözümleri ile ilişkili tüm kaynakları oluşturabilir ve yönetebilirsiniz. Hizmeti kullanmak istediğiniz her bölgede bir hizmet kaynağı oluşturun.

[CloudSimple hizmeti](cloudsimple-service.md)hakkında daha fazla bilgi edinin.

## <a name="cloudsimple-node"></a>CloudBasit düğüm

CloudSimple düğümü, VMware ESXi hipervizörünün dağıtıldığı özel, çıplak metal, hyperconverged bir işlem ve depolama ana bilgisayarıdır. Bu düğüm daha sonra VMware vSphere, vCenter, vSAN ve NSX platformlarına dahil edilir. CloudSimple ağ hizmetleri ve kenar ağ hizmetleri de etkinleştirilir. Her [düğüm, CloudSimple özel bulutlar](cloudsimple-private-cloud.md)oluşturmak için sağlayabileceğiniz bir işlem ve depolama kapasitesi birimi olarak hizmet vermektedir. CloudSimple hizmetinin kullanılabildiği bir bölgede düğüm ler sağlamaveya ayırması.

[CloudSimple düğümleri](cloudsimple-node.md)hakkında daha fazla bilgi edinin.

## <a name="cloudsimple-private-cloud"></a>CloudBasit özel bulut

CloudSimple özel bulutu, kendi yönetim etki alanında bir vCenter sunucusu tarafından yönetilen yalıtılmış bir VMware yığını ortamıdır. VMware yığını ESXi ana bilgisayarları, vSphere, vCenter, vSAN ve NSX içerir. Yığın özel düğümler (özel ve izole çıplak metal donanım) üzerinde çalışır ve vCenter ve NSX Manager içeren yerel VMware araçları aracılığıyla kullanıcılar tarafından tüketilen. Özel düğümler Azure konumlarında dağıtılır ve Azure tarafından yönetilir. Her özel bulut, VLAN'lar, alt ağlar ve güvenlik duvarı tabloları gibi ağ hizmetleri kullanılarak segmente edilebilir ve güvenli hale alınabilir. Şirket içi ortamınıza ve Azure ağına bağlantılar güvenli, özel VPN ve Azure ExpressRoute bağlantıları kullanılarak oluşturulur.

[CloudSimple özel buluthakkında](cloudsimple-private-cloud.md)daha fazla bilgi edinin.

## <a name="service-networking"></a>Servis ağı

CloudSimple hizmeti, CloudSimple hizmetinizin dağıtıldığı bölge başına bir ağ sağlar. Ağ, varsayılan olarak etkin olan tek bir TCP Katmanı 3 adres alanıdır. Bu bölgede oluşturulan tüm özel bulutlar ve alt ağlar, ek yapılandırma olmadan birbirleriyle iletişim kurar. VLAN'ları kullanarak vCenter'da dağıtılmış bağlantı noktası grupları oluşturursunuz. İş yükü kaynaklarınızı özel bulutunuzda yapılandırmak ve güvence altına almak için aşağıdaki ağ özelliklerini kullanabilirsiniz:

* [VLAN ve alt ağlar](cloudsimple-vlans-subnets.md)
* [Güvenlik duvarı tabloları](cloudsimple-firewall-tables.md)
* [VPN ağ geçitleri](cloudsimple-vpn-gateways.md)
* [Genel IP](cloudsimple-public-ip-address.md)
* [Azure ağ bağlantısı](cloudsimple-azure-network-connection.md)

## <a name="cloudsimple-virtual-machine"></a>CloudBasit sanal makine

CloudSimple hizmeti ile Azure portalından VMware sanal makinelerini yönetebilirsiniz. vSphere ortamınızdan bir veya daha fazla küme veya kaynak havuzu, hizmetin oluşturulduğu abonelmeye eşlenebilir.

Aşağıdakiler hakkında daha fazla bilgi edinin:

* [CloudBasit sanal makineler](cloudsimple-virtual-machines.md)
* [Azure abonelik eşleme](https://docs.microsoft.com/azure/vmware-cloudsimple/azure-subscription-mapping/)
