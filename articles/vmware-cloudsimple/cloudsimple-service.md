---
title: CloudSimple-Service tarafından Azure VMware çözümü
description: Genel bakış ile CloudSimple hizmeti hakkında bilgi edinin. Hizmetin oluşturulması, düğüm satın almanızı, düğümleri ayırmanızı ve özel bulutlar oluşturmanızı sağlar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ec77209c0995bf5aec0957c8d4d2269f27d743bc
ms.sourcegitcommit: a2a7746c858eec0f7e93b50a1758a6278504977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88142137"
---
# <a name="cloudsimple-service-overview"></a>CloudSimple hizmetine genel bakış

CloudSimple hizmeti, CloudSimple ile Azure VMware çözümünü kullanmanıza olanak sağlar.  Hizmetin oluşturulması, düğüm satın almanızı, düğümleri ayırmanızı ve özel bulutlar oluşturmanızı sağlar.  Cloudsimple hizmetini, CloudSimple hizmetinin kullanılabildiği her bir Azure bölgesinde oluşturursunuz. Hizmet, CloudSimple tarafından Azure VMware çözümünün Edge ağını tanımlar. Edge ağı VPN, ExpressRoute ve özel Bulutlarınıza internet bağlantısı içeren hizmetleri destekler.

## <a name="gateway-subnet"></a>Ağ geçidi alt ağı

CloudSimple hizmeti başına bir ağ geçidi alt ağı gerekir ve oluşturulduğu bölge için benzersizdir. Ağ geçidi alt ağı, Edge ağı oluşturulurken kullanılır ve bir/28 CıDR bloğu gerektirir.  Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kuran herhangi bir ağla çakışmamalıdır. CloudSimple ile iletişim kuran ağlarda şirket içi ağlar ve Azure sanal ağı vardır.  Bir ağ geçidi alt ağı oluşturulduktan sonra silinemez.  Ağ geçidi alt ağı, hizmet silindiğinde kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'Da CloudSimple hizmeti oluşturmayı](quickstart-create-cloudsimple-service.md)öğrenin.
