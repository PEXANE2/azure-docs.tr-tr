---
title: CloudSimple-Nodes 'a göre Azure VMware çözümü genel bakış
description: CloudSimple düğümleri ve kavramları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 729b832c068dcd401fa6e9d2f4af9193d3859a83
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71845540"
---
# <a name="cloudsimple-nodes-overview"></a>CloudSimple düğümlerine genel bakış

Düğümler, özel bulutun yapı taşlarıdır. Düğüm:

* VMware ESXi hiper yöneticinin yüklü olduğu adanmış çıplak işlem Konağı  
* Özel bulutlar oluşturmak için satın alabileceğiniz veya ayırabilmeniz için bir bilgi işlem birimi
* CloudSimple hizmetinin kullanılabildiği bir bölgede satın alınabilir veya rezerve edilir

Satın alınan düğümlerden özel bir bulut oluşturursunuz. Özel bir bulut oluşturmak için aynı SKU 'nun en az üç düğümü gerekir. Özel bir bulutu genişletmek için ek düğümler ekleyin.  Mevcut bir kümeye düğüm ekleyebilir veya Azure portal düğümleri satın alarak ve CloudSimple hizmetiyle ilişkilendirerek yeni bir küme oluşturabilirsiniz.  Satın alınan tüm düğümler CloudSimple hizmeti altında görünür.  

## <a name="purchased-nodes"></a>Satın alınan düğümler

Satın alınan düğümler, Kullandıkça Öde kapasitesi sağlar. Düğüm satın alma, VMware kümenizi isteğe bağlı olarak hızlıca ölçeklendirmenize yardımcı olur. Gerekli olan düğümleri ekleyebilir veya VMware kümenizi ölçeklendirmek için satın alınan bir düğümü silebilirsiniz. Satın alınan düğümler aylık olarak faturalandırılır ve satın alındığı abonelik üzerinden ücretlendirilir.

* Azure aboneliğiniz için kredi kartı ile ödeme yaparsanız, kart hemen faturalandırılır.
* Faturaya göre faturalandırıldıysanız ücretler bir sonraki faturanızda görünür.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>CloudSimple düğümleri SKU 'SU tarafından VMware çözümü

Satın alma veya rezervasyon için aşağıdaki düğüm türleri kullanılabilir.

| SKU           | CS28-Node                 | CS36-Node                 | CS36m-Node                |
|---------------|-----------------------------|-----------------------------|-----------------------------|
| Bölge        | Doğu ABD, Batı ABD            | Doğu ABD, Batı ABD            | Batı Avrupa                 |
| CPU           | 2x 2.2 GHz, 28 çekirdek (56 HT) | 2x 2.3 GHz, 36 çekirdek (72 HT) | 2x 2.3 GHz, 36 çekirdek (72 HT) |
| RAM           | 256 GB                      | 512 GB                      | 576 GB                      |
| Önbellek Diski    | 1,6-TB NVMe                 | 3,2-TB NVMe                 | 3,2-TB NVMe                 |
| Kapasite diski | 5,625 TB RAW                | 11,25 TB RAW                | 15,36 TB RAW                |
| Depolama türü  | Tüm Flash                   | Tüm Flash                   | Tüm Flash                   |

## <a name="limits"></a>Sınırlar

Aşağıdaki düğüm limitleri özel bulutlar için geçerlidir.

| Kaynak | Sınır |
|----------|-------|
| Özel bir bulut oluşturmak için düğüm sayısı alt sınırı | 3 |
| Özel buluttaki bir kümede bulunan en fazla düğüm sayısı | 16 |
| Özel buluttaki en fazla düğüm sayısı | 64 |
| Yeni kümedeki düğüm sayısı alt sınırı | 3 |

## <a name="next-steps"></a>Sonraki adımlar

* [Düğümleri satın almayı](create-nodes.md) öğrenin
* [Özel bulutlar](cloudsimple-private-cloud.md) hakkında bilgi edinin
