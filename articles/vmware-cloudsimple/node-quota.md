---
title: Azure VMware çözümleri (AVS)-AVS düğüm kotası
description: AVS düğümlerinin kota sınırlarını ve kota artışı için nasıl istek yapılacağını açıklar
author: sharaths-cs
ms.author: dikamath
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: fa1b056c8c96fb09def63ca1cd696fc2da5e9bed
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77019647"
---
# <a name="avs-node-quota-limits"></a>AVS düğüm kotası limitleri

Dört düğüm, aboneliğiniz AVS hizmeti için etkinleştirildiğinde satın alma için kullanılabilen varsayılan miktardır. Azure portal herhangi bir [düğüm türü](cloudsimple-node.md) satın alabilirsiniz. Bir AVS özel bulutu oluşturmak için aynı SKU 'nun en az üç düğümü gereklidir. Düğümleri satın aldıysanız, ek düğümler satın almaya çalıştığınızda bir hata görebilirsiniz.

## <a name="quota-increase"></a>Kota artışı

Bir destek isteği göndererek düğüm kotasının artmasını sağlayabilirsiniz. Hizmet işlemleri ekibi, isteği değerlendirir ve düğüm kotasını artırmak için sizinle birlikte çalışır. Yeni bir bilet açtığınızda aşağıdaki seçenekleri belirleyin:

* Sorun türü: **Teknik**
* Abonelik: **ABONELIK kimliğiniz**
* Hizmet türü: **AVS tarafından VMware çözümü**
* Sorun türü: **adanmış düğümler kotası**
* Sorun alt türü: **adanmış düğümlerin kotasını artırma**
* Konu: **Kota artışı**

Destek bileti ayrıntılarında gereken düğüm ve düğüm SKU sayısını sağlayın.

* Düğüm SKU 'SU
* Kota artışı istediğiniz ek düğümlerin sayısı

## <a name="next-steps"></a>Sonraki adımlar

* [Satın alma düğümleri](create-nodes.md)
* [AVS düğümlerine genel bakış](cloudsimple-node.md)
