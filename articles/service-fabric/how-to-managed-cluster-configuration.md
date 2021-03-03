---
title: Service Fabric yönetilen kümenizi yapılandırma (Önizleme)
description: Service Fabric yönetilen kümenizi otomatik işletim sistemi yükseltmeleri, NSG kuralları ve daha fazlası için nasıl yapılandıracağınızı öğrenin.
ms.topic: how-to
ms.date: 02/15/2021
ms.openlocfilehash: 44b1b949fe314231cb44f190c31b53903e47a904
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101732641"
---
# <a name="service-fabric-managed-cluster-preview-configuration-options"></a>Service Fabric yönetilen küme (Önizleme) yapılandırma seçenekleri

Kümenizi oluştururken [Service Fabric yönetilen küme SKU 'su](overview-managed-cluster.md#service-fabric-managed-cluster-skus) seçmenin yanı sıra, yapılandırmak için birkaç farklı yol vardır. Geçerli önizlemede şunları yapabilirsiniz:

* Kümeniz için [ağ seçeneklerini](how-to-managed-cluster-networking.md) yapılandırma
* Düğüm türüne bir [sanal makine ölçek kümesi uzantısı](how-to-managed-cluster-vmss-extension.md) ekleyin
* Düğüm türlerinizin [yönetilen kimliğini](how-to-managed-identity-managed-cluster-virtual-machine-scale-sets.md) yapılandırma
* Düğümleriniz için [Otomatik işletim sistemi yükseltmelerini](how-to-managed-cluster-configuration.md#enable-automatic-os-image-upgrades) etkinleştirin
* Düğümleriniz üzerinde [Işletim sistemi ve veri diski şifrelemesini](how-to-enable-managed-cluster-disk-encryption.md) etkinleştirme

## <a name="enable-automatic-os-image-upgrades"></a>Otomatik işletim sistemi görüntüsü yükseltmelerini etkinleştir

Yönetilen küme düğümlerinizi çalıştıran sanal makinelerde otomatik işletim sistemi görüntüsü yükseltmelerini etkinleştirmeyi seçebilirsiniz. Sanal makine ölçek kümesi kaynakları Service Fabric yönetilen kümeler ile sizin adınıza yönetiliyor olsa da, Küme düğümleriniz için otomatik işletim sistemi görüntüsü yükseltmelerini etkinleştirme seçeneğiniz vardır. [Klasik Service Fabric](service-fabric-best-practices-infrastructure-as-code.md#azure-virtual-machine-operating-system-automatic-upgrade-configuration) kümelerinde olduğu gibi, kümenizde istenmeyen kesintiler oluşmasını engellemek için yönetilen küme düğümleri varsayılan olarak yükseltilmez.

Otomatik işletim sistemi yükseltmelerini etkinleştirmek için:

* `2021-01-01-preview` *Microsoft. servicefabric/managedkümeler* ve *Microsoft. servicefabric/managedkümeler/nodetypes* kaynaklarının (veya üzeri) sürümünü kullanın
* Kümenin özelliğini `enableAutoOSUpgrade` *true* olarak ayarlayın
* Küme nodeTypes ' kaynak özelliğini `vmImageVersion` *en son* olarak ayarla

Örnek:

```json
    {
      "apiVersion": "2021-01-01-preview",
      "type": "Microsoft.ServiceFabric/managedclusters",
      ...
      "properties": {
        ...
        "enableAutoOSUpgrade": true
      },
    },
    {
      "apiVersion": "2021-01-01-preview",
      "type": "Microsoft.ServiceFabric/managedclusters/nodetypes",
       ...
      "properties": {
        ...
        "vmImageVersion": "latest",
        ...
      }
    }
}

```

Etkinleştirildikten sonra, Service Fabric yönetilen kümede işletim sistemi görüntü sürümlerini sorgulamaya ve izlemeye başlar. Yeni bir işletim sistemi sürümü varsa, küme düğüm türleri (sanal makine ölçek kümeleri) birer birer yükseltilir. Service Fabric çalışma zamanı yükseltmeleri, yalnızca bir küme düğümü işletim sistemi görüntüsü yükseltmelerinden onaylandıktan sonra gerçekleştirilir.

Yükseltme başarısız olursa, en fazla üç yeniden deneme için Service Fabric 24 saat sonra yeniden denenir. Klasik (yönetilmeyen) Service Fabric yükseltmelere benzer şekilde, sağlıksız uygulamalar veya düğümler işletim sistemi görüntüsü yükseltmesini engelleyebilir.

Görüntü yükseltmeleri hakkında daha fazla bilgi için bkz. [Azure sanal makine ölçek kümeleri Ile otomatik işletim sistemi görüntüsü yükseltmeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md).

## <a name="next-steps"></a>Sonraki adımlar

[Service Fabric yönetilen kümelere genel bakış](overview-managed-cluster.md)

[Service Fabric küme şablonları](https://github.com/Azure-Samples/service-fabric-cluster-templates)
