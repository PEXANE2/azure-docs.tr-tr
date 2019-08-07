---
title: CloudSimple hizmetine göre VMware çözümü
description: CloudSimple hizmeti ve kavramlarının genel bakışını açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a43fbebb21be82fd751778d6fec95e0ee9c346ad
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812515"
---
# <a name="cloudsimple-service-overview"></a>CloudSimple hizmetine genel bakış

CloudSimple hizmeti sayesinde CloudSimple ile Azure VMware çözümünü kullanabilirsiniz. Hizmeti oluşturduktan sonra düğümleri sağlayabilir, düğümleri ayırabilir ve özel bulutlar oluşturabilirsiniz. Cloudsimple hizmetini, CloudSimple hizmetinin kullanılabildiği her bir Azure bölgesinde oluşturursunuz. Hizmet, CloudSimple tarafından Azure VMware çözümünün Edge ağını tanımlar. Edge ağı VPN, Azure ExpressRoute ve özel bulutlarınıza internet bağlantısı içeren hizmetleri destekler.

## <a name="gateway-subnet"></a>Ağ geçidi alt ağı

CloudSimple hizmeti başına bir ağ geçidi alt ağı gerekir ve oluşturulduğu bölge için benzersizdir. Ağ geçidi alt ağı, Edge ağını oluşturduğunuzda ve/28 CıDR bloğunu gerektirdiğinde kullanılır. Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kuran herhangi bir ağla çakışmamalıdır. CloudSimple ile iletişim kuran ağlarda şirket içi ağlar ve Azure sanal ağları bulunur. Bir ağ geçidi alt ağı oluşturulduktan sonra silinemez. Ağ geçidi alt ağı, hizmet silindiğinde kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'Da CloudSimple hizmeti oluşturmayı](quickstart-create-cloudsimple-service.md)öğrenin.
