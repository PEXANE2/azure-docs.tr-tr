---
title: CloudSimple-Nodes 'a göre Azure VMware çözümü genel bakış
description: Düğümler, sağlanan düğümler, özel bir bulut ve CloudSimple düğümleri SKU 'Ları tarafından VMware çözümü dahil olmak üzere CloudSimple kavramları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 7de4b5faa997d909089daedab7e48e5d5a6de2e7
ms.sourcegitcommit: a2a7746c858eec0f7e93b50a1758a6278504977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88140845"
---
# <a name="cloudsimple-nodes-overview"></a>CloudSimple düğümlerine genel bakış

Düğümler, özel bulutun yapı taşlarıdır. Düğüm:

* VMware ESXi hiper yöneticinin yüklü olduğu adanmış çıplak işlem Konağı  
* Özel bulutlar oluşturmak için sağlayabileceğiniz veya ayırabileceğiniz bir bilgi işlem birimi
* CloudSimple hizmetinin kullanılabildiği bir bölgede sağlama veya ayırma için kullanılabilir

Sağlanan düğümlerden özel bir bulut oluşturabilirsiniz. Özel bir bulut oluşturmak için aynı SKU 'nun en az üç düğümü gerekir. Özel bir bulutu genişletmek için ek düğümler ekleyin.  Mevcut bir kümeye düğüm ekleyebilir veya Azure portal düğümleri sağlayıp CloudSimple hizmetiyle ilişkilendirerek yeni bir küme oluşturabilirsiniz.  Sağlanan tüm düğümler CloudSimple hizmeti altında görünür.  

## <a name="provisioned-nodes"></a>Sağlanan düğümler

Sağlanan düğümler, Kullandıkça Öde kapasitesi sağlar. Sağlama düğümleri, VMware kümenizi isteğe bağlı olarak hızlıca ölçeklendirmenize yardımcı olur. Gerekli olan düğümleri ekleyebilir veya VMware kümenizi ölçeklendirmek için sağlanan bir düğümü silebilirsiniz. Sağlanan düğümler aylık olarak faturalandırılır ve sağlandıkları aboneliğe ücretlendirilir.

* Azure aboneliğiniz için kredi kartı ile ödeme yaparsanız, kart hemen faturalandırılır.
* Faturaya göre faturalandırıldıysanız ücretler bir sonraki faturanızda görünür.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>CloudSimple düğümleri SKU 'SU tarafından VMware çözümü

Aşağıdaki düğüm türleri, sağlama veya ayırma için kullanılabilir.

| SKU           | CS28-Node                 | CS36-Node                 | CS36m-Node                |
|---------------|-----------------------------|-----------------------------|-----------------------------|
| Bölge        | Doğu ABD, Batı ABD            | Doğu ABD, Batı ABD            | West Europe                 |
| CPU           | 2x 2.2 GHz, 28 çekirdek (56 HT) | 2x 2.3 GHz, 36 çekirdek (72 HT) | 2x 2.3 GHz, 36 çekirdek (72 HT) |
| RAM           | 256 GB                      | 512 GB                      | 576 GB                      |
| Önbellek diski    | 1,6-TB NVMe                 | 3,2-TB NVMe                 | 3,2-TB NVMe                 |
| Kapasite diski | 5,625 TB RAW                | 11,25 TB RAW                | 15,36 TB RAW                |
| Depolama Türü  | Tüm Flash                   | Tüm Flash                   | Tüm Flash                   |

## <a name="limits"></a>Sınırlar

Aşağıdaki düğüm limitleri özel bulutlar için geçerlidir.

| Kaynak | Sınır |
|----------|-------|
| Özel bir bulut oluşturmak için düğüm sayısı alt sınırı | 3 |
| Özel buluttaki bir kümede bulunan en fazla düğüm sayısı | 16 |
| Özel buluttaki en fazla düğüm sayısı | 64 |
| Yeni kümedeki düğüm sayısı alt sınırı | 3 |

## <a name="next-steps"></a>Sonraki adımlar

* [Düğüm sağlamayı](create-nodes.md) öğrenin
* [Özel bulutlar](cloudsimple-private-cloud.md) hakkında bilgi edinin
