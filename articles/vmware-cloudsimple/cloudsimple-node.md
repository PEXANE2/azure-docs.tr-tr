---
title: CloudSimple'a göre Azure VMware Çözümü - Düğümlere genel bakış
description: CloudSimple düğümleri ve kavramları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 65afe26a98a53b00b72a1ea2b49799db2049b727
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024934"
---
# <a name="cloudsimple-nodes-overview"></a>CloudSimple düğümlerine genel bakış

Düğümler Özel Bulut'un yapı taşlarıdır. Düğüm:

* VMware ESXi hipervizörü takıldığı özel bir çıplak metal işlem ana bilgisayar  
* Özel Bulutlar oluşturmak için sağlanabilir veya rezerve edebileceğiniz bir bilgi işlem birimi
* CloudSimple hizmetinin kullanılabildiği bir bölgede kullanılabilir veya rezerve edilebilir

Sağlanan düğümlerden bir Özel Bulut oluşturursunuz. Özel Bulut oluşturmak için, aynı SKU'nun en az üç düğümüne ihtiyacınız vardır. Özel Bulut'u genişletmek için ek düğümler ekleyin.  Varolan bir kümeye düğüm ekleyebilir veya Azure portalında düğümler sağlayarak ve bunları CloudSimple hizmetiyle ilişkilendirerek yeni bir küme oluşturabilirsiniz.  Tüm sağlanan düğümler CloudSimple hizmeti altında görünür.  

## <a name="provisioned-nodes"></a>Verilen düğümler

Verilen düğümler, istediğiniz kadar öde kapasitesi sağlar. Düğümleri sağlama, VMware kümenizi isteğe bağlı olarak hızla ölçeklendirmenize yardımcı olur. Gerektiğinde düğümekleyebilir veya VMware kümenizi küçültmek için sağlanan bir düğümü silebilirsiniz. Sağlanan düğümler aylık olarak faturalandırılır ve sağlandığı aboneye ücretlendirilir.

* Azure aboneliğiniz için kredi kartıyla ödeme yaptığınızda, kart hemen faturalandırılır.
* Faturayla faturalandırılırsanız, ücretler bir sonraki faturanızda görünür.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>CloudSimple düğümleri SKU tarafından VMware Çözüm

Aşağıdaki düğüm türleri tedarik veya rezervasyon için kullanılabilir.

| SKU           | CS28 - Düğüm                 | CS36 - Düğüm                 | CS36m - Düğüm                |
|---------------|-----------------------------|-----------------------------|-----------------------------|
| Bölge        | Doğu ABD, Batı ABD            | Doğu ABD, Batı ABD            | Batı Avrupa                 |
| CPU           | 2x2.2 GHz, 28 Çekirdek (56 HT) | 2x2.3 GHz, 36 Çekirdek (72 HT) | 2x2.3 GHz, 36 Çekirdek (72 HT) |
| RAM           | 256 GB                      | 512 GB                      | 576 GB                      |
| Önbellek Diski    | 1.6-TB NVMe                 | 3.2-TB NVMe                 | 3.2-TB NVMe                 |
| Kapasite Diski | 5.625 TB Ham                | 11.25 TB Ham                | 15.36 TB Ham                |
| Depolama Türü  | Tüm Flash                   | Tüm Flash                   | Tüm Flash                   |

## <a name="limits"></a>Sınırlar

Aşağıdaki düğüm sınırları Özel Bulutlar için geçerlidir.

| Kaynak | Sınır |
|----------|-------|
| Özel Bulut oluşturmak için minimum düğüm sayısı | 3 |
| Özel Bulut'ta kümedeki maksimum düğüm sayısı | 16 |
| Özel Bulut'ta maksimum düğüm sayısı | 64 |
| Yeni kümedeki en az düğüm sayısı | 3 |

## <a name="next-steps"></a>Sonraki adımlar

* Düğümleri nasıl [sağlarz öğreneceksin](create-nodes.md)
* Özel [Bulutlar](cloudsimple-private-cloud.md) hakkında bilgi edinin
