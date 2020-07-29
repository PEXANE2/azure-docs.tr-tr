---
title: CloudSimple-Service tarafından Azure VMware çözümü
description: CloudSimple hizmeti ve kavramlarına genel bir bakış sunar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d128a248c2e6e1e2e35e3b633975ba081e77f028
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77024970"
---
# <a name="cloudsimple-service-overview"></a>CloudSimple hizmetine genel bakış

CloudSimple hizmeti, CloudSimple ile Azure VMware çözümünü kullanmanıza olanak sağlar.  Hizmetin oluşturulması, düğüm satın almanızı, düğümleri ayırmanızı ve özel bulutlar oluşturmanızı sağlar.  Cloudsimple hizmetini, CloudSimple hizmetinin kullanılabildiği her bir Azure bölgesinde oluşturursunuz. Hizmet, CloudSimple tarafından Azure VMware çözümünün Edge ağını tanımlar. Edge ağı VPN, ExpressRoute ve özel Bulutlarınıza internet bağlantısı içeren hizmetleri destekler.

## <a name="gateway-subnet"></a>Ağ geçidi alt ağı

CloudSimple hizmeti başına bir ağ geçidi alt ağı gerekir ve oluşturulduğu bölge için benzersizdir. Ağ geçidi alt ağı, Edge ağı oluşturulurken kullanılır ve bir/28 CıDR bloğu gerektirir.  Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kuran herhangi bir ağla çakışmamalıdır. CloudSimple ile iletişim kuran ağlarda şirket içi ağlar ve Azure sanal ağı vardır.  Bir ağ geçidi alt ağı oluşturulduktan sonra silinemez.  Ağ geçidi alt ağı, hizmet silindiğinde kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'Da CloudSimple hizmeti oluşturmayı](quickstart-create-cloudsimple-service.md)öğrenin.
