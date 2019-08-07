---
title: CloudSimple-Azure tarafından VMware çözümüne yönelik düğümlere genel bakış
description: CloudSimple düğümleri ve kavramları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 346bd046810ebae5142bc23400419857000d0c8e
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812582"
---
# <a name="cloudsimple-nodes-overview"></a>CloudSimple düğümlerine genel bakış

Düğüm:

* VMware ESXi hiper yöneticinin yüklü olduğu adanmış bir çıplak işlem ana bilgisayarı  
* Özel bulutlar oluşturmak için sağlayabileceğiniz veya ayırabileceğiniz bir bilgi işlem birimi  
* CloudSimple hizmetinin kullanılabildiği bir bölgede sağlama veya ayırma için kullanılabilir

Düğümler, özel bulutun yapı taşları oluşturuyor.  Özel bir bulut oluşturmak için aynı SKU 'nun en az üç düğümü gerekir.  Özel bir bulutu genişletmek için ek düğümler ekleyin.  Mevcut bir kümeye düğüm ekleyebilirsiniz. Ya da Azure portal düğümleri sağlayarak ve CloudSimple hizmetiyle ilişkilendirerek yeni bir küme oluşturabilirsiniz.  Sağlanan tüm düğümler CloudSimple hizmeti altında görünür.  CloudSimple Portal 'da sağlanan düğümlerden özel bir bulut oluşturabilirsiniz.

## <a name="provisioned-nodes"></a>Sağlanan düğümler

Sağlanan düğümler, Kullandıkça Öde kapasitesi sağlar. Sağlama düğümleri, VMware kümenizi isteğe bağlı olarak hızlıca ölçeklendirmenize yardımcı olur. Gerekli olan düğümleri ekleyebilir veya VMware kümenizi ölçeklendirmek için sağlanan bir düğümü silebilirsiniz. sağlanan düğümler aylık olarak faturalandırılır ve sağlandıkları aboneliğe ücretlendirilir:

* Azure aboneliğiniz için kredi kartı ile ödeme yaparsanız, kart hemen faturalandırılır.
* Faturaya göre faturalandırıldıysanız ücretler bir sonraki faturanızda görünür.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>CloudSimple düğümleri SKU 'SU tarafından VMware çözümü

Aşağıdaki tür düğümleri sağlama veya ayırma için kullanılabilir.

| SKU | CS28-Node | CS36-Node |
|-----|-------------|-------------|
| CPU | 2x 2.2 GHz, 28 çekirdek (56 HT) | 2x 2.3 GHz, 36 çekirdek (72 HT) |
| RAM | 256 GB | 512 GB |
| Önbellek diski |  1,6-TB NVMe | 3,2-TB NVMe |
| Kapasite diski | 5,625 TB RAW | 11,25 TB RAW |
| Depolama türü | Tüm Flash | Tüm Flash |

## <a name="limits"></a>Sınırlar

Aşağıdaki düğüm limitleri özel bulutlar için geçerlidir.

| Resource | Sınır |
|----------|-------|
| Özel bir bulut oluşturmak için düğüm sayısı alt sınırı | 3 |
| Özel buluttaki bir kümede bulunan en fazla düğüm sayısı | 16 |
| Özel buluttaki en fazla düğüm sayısı | 64 |
| Yeni kümedeki düğüm sayısı alt sınırı | 3 |

## <a name="next-steps"></a>Sonraki adımlar

* [Düğüm sağlamayı](create-nodes.md) öğrenin
* [Özel bulut](cloudsimple-private-cloud.md) hakkında bilgi edinin