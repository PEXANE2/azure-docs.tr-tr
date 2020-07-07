---
title: Kavramlar-özel bulut yükseltmeleri
description: Azure VMware çözümünde (AVS) anahtar yükseltme işlemi ve özellikleri hakkında bilgi edinin
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: c5c79625ef661f220a0ba88d6d24ab59e7228195
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82740375"
---
# <a name="azure-vmware-solution-avs-upgrade-concepts"></a>Azure VMware çözümü (AVS) yükseltme kavramları

Azure VMware çözümü (AVS) özel bulutlarının önemli avantajlarından biri, platformun sizin için korunmasını sağlar. Platform bakımı, VMware tarafından doğrulanan bir yazılım paketine otomatik yükseltmeler içerir. Yükseltmeler düzenli olarak gerçekleştirilir ve bu da her zaman yazılımın en son doğrulanan sürümlerine sahip olmanızı sağlar.

## <a name="avs-private-cloud-software-upgrades"></a>AVS özel bulut yazılımı yükseltmeleri

AVS özel bulut platformu VMware vSphere, ESXi, vSAN ve NSX-T yazılımının belirli sürümlerini içerir. Özel bulut yazılımı paketi, yeni özel bulut yüklemelerinde kullanılmak üzere ve var olan özel bulutların yükseltmeleri için onaylanır.

Yaşam döngüsü yönetimi yükseltme işlemi özel bulutlarınız için kapalı kalma süresi gerektirmez. Yükseltme işlemi, doğrulanan AVS özel bulut yazılımının en son sürümünü otomatik olarak kullanmanızı sağlar. Özel bulutların doğrulanan yazılım paketi 'nin en son sürümünün bir sürümünden daha fazla olması için, yükseltmeler normal temposunda uygulanır. Özel bulutunuzun planlı yükseltmeleri hakkında bilgilendirilirsiniz. Özel bulutunuz en son sürümün bir sürümü içindeyse yükseltmeyi erteleyebilirsiniz.

Kritik düzeltme ekleri ve güncelleştirmeler doğrulandığında uygulanır. Gerekli kritik yükseltmelerden önce bildirim alacaksınız. Bu ilke, özel bulutunuzun kritik düzeltme eklerine ve güncelleştirmelerin hemen uygulanmasını sağlar.

VMware yazılım sürümleri, [özel bulutlar ve kümeler kavramı makalesinde](concepts-private-clouds-clusters.md) ve [SSS](faq.md)'de verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım [özel bir bulut oluşturmaktır](tutorial-create-private-cloud.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
