---
title: Azure VMware çözümleri (AVS)-hizmet
description: AVS hizmeti ve kavramlarına genel bir bakış sağlar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d09c8c34093e7d33122f934138ff9fdf4842508e
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024970"
---
# <a name="avs-service-overview"></a>AVS hizmetine genel bakış

AVS hizmeti, AVS tarafından Azure VMware çözümünü kullanmanıza olanak sağlar. Hizmetin oluşturulması, düğümleri satın almanızı, düğümleri ayırmanızı ve AVS özel bulutları oluşturmanızı sağlar. AVS hizmetini, AVS hizmetinin kullanılabildiği her bir Azure bölgesinde oluşturursunuz. Hizmet, AVS tarafından Azure VMware çözümünün Edge ağını tanımlar. Edge ağı, AVS özel Bulutlarınıza VPN, ExpressRoute ve internet bağlantısı içeren hizmetleri destekler.

## <a name="gateway-subnet"></a>Ağ geçidi alt ağı

Bir ağ geçidi alt ağı her bir AVS hizmeti için gereklidir ve oluşturulduğu bölgeye özeldir. Ağ geçidi alt ağı, Edge ağı oluşturulurken kullanılır ve bir/28 CıDR bloğu gerektirir. Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. Bu, AVS ortamıyla iletişim kuran herhangi bir ağla çakışmamalıdır. AVS ile iletişim kuran ağlarda şirket içi ağlar ve Azure sanal ağı vardır. Bir ağ geçidi alt ağı oluşturulduktan sonra silinemez. Ağ geçidi alt ağı, hizmet silindiğinde kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da BIR AVS hizmeti oluşturmayı](quickstart-create-cloudsimple-service.md)öğrenin.
