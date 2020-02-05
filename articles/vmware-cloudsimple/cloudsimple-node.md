---
title: Azure VMware çözümleri (AVS)-düğümlere genel bakış
description: AVS düğümleri ve kavramları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 08d8fc3e6f1f2f83cf3c4fee3fdafb0bd07e336c
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024934"
---
# <a name="avs-nodes-overview"></a>AVS düğümlerine genel bakış

Düğümler, bir AVS özel bulutunun yapı taşlarıdır. Düğüm:

* VMware ESXi hiper yöneticinin yüklü olduğu adanmış çıplak işlem Konağı  
* AVS özel bulutları oluşturmak için satın alabileceğiniz veya ayırabilmeniz için bir bilgi işlem birimi
* AVS hizmetinin kullanılabildiği bir bölgede satın alınabilir veya rezerve edilir

Satın alınan düğümlerden bir AVS özel bulutu oluşturursunuz. Bir AVS özel bulutu oluşturmak için aynı SKU 'ya ait en az üç düğüm gerekir. Bir AVS özel bulutu genişletmek için ek düğümler ekleyin. Mevcut bir kümeye düğüm ekleyebilir veya Azure portal düğümleri satın alarak ve bunları AVS hizmetiyle ilişkilendirerek yeni bir küme oluşturabilirsiniz. Satın alınan tüm düğümler, AVS hizmeti altında görünür. 

## <a name="provisioned-nodes"></a>Sağlanan düğümler

Sağlanan düğümler, Kullandıkça Öde kapasitesi sağlar. Sağlama düğümleri, VMware kümenizi isteğe bağlı olarak hızlıca ölçeklendirmenize yardımcı olur. Gerekli olan düğümleri ekleyebilir veya VMware kümenizi ölçeklendirmek için sağlanan bir düğümü silebilirsiniz. Sağlanan düğümler aylık olarak faturalandırılır ve sağlandıkları aboneliğe ücretlendirilir.

* Azure aboneliğiniz için kredi kartı ile ödeme yaparsanız, kart hemen faturalandırılır.
* Faturaya göre faturalandırıldıysanız ücretler bir sonraki faturanızda görünür.

## <a name="vmware-solution-by-avs-nodes-sku"></a>AVS düğümlerine göre VMware çözümü SKU 'SU

Aşağıdaki düğüm türleri, sağlama veya ayırma için kullanılabilir.

| SKU           | CS28-Node                 | CS36-Node                 | CS36m-Node                |
|---------------|-----------------------------|-----------------------------|-----------------------------|
| Bölge        | Doğu ABD, Batı ABD            | Doğu ABD, Batı ABD            | Batı Avrupa                 |
| CPU           | 2x 2.2 GHz, 28 çekirdek (56 HT) | 2x 2.3 GHz, 36 çekirdek (72 HT) | 2x 2.3 GHz, 36 çekirdek (72 HT) |
| RAM           | 256 GB                      | 512 GB                      | 576 GB                      |
| Önbellek Diski    | 1,6-TB NVMe                 | 3,2-TB NVMe                 | 3,2-TB NVMe                 |
| Kapasite diski | 5,625 TB RAW                | 11,25 TB RAW                | 15,36 TB RAW                |
| Depolama türü  | Tüm Flash                   | Tüm Flash                   | Tüm Flash                   |

## <a name="limits"></a>Sınırlar

Aşağıdaki düğüm limitleri, AVS özel bulutları için geçerlidir.

| Kaynak | Sınır |
|----------|-------|
| Bir AVS özel bulutu oluşturmak için gereken en az düğüm sayısı | 3 |
| Bir AVS özel bulutu 'ndaki kümede bulunan en fazla düğüm sayısı | 16 |
| Bir AVS özel bulutundaki en fazla düğüm sayısı | 64 |
| Yeni kümedeki düğüm sayısı alt sınırı | 3 |

## <a name="next-steps"></a>Sonraki adımlar

* [Düğümleri satın almayı](create-nodes.md) öğrenin
* [AVS özel bulutları](cloudsimple-private-cloud.md) hakkında bilgi edinin
* [Düğüm sağlamayı](create-nodes.md) öğrenin
* [Özel bulutlar](cloudsimple-private-cloud.md) hakkında bilgi edinin
