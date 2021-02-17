---
title: Batch havuzu yapılandırmasını Cloud Services sanal makinelere geçir
description: Havuz yapılandırmanızı en son ve önerilen yapılandırmaya güncelleştirme hakkında bilgi edinin
ms.topic: how-to
ms.date: 2/16/2021
ms.openlocfilehash: 9cbcf3864526bd8f8132f3b0f729e2d728e07bb8
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100546049"
---
# <a name="migrate-batch-pool-configuration-from-cloud-services-to-virtual-machines"></a>Batch havuzu yapılandırmasını Cloud Services sanal makinelere geçir

Batch havuzları, [Cloudserviceconfiguration](/rest/api/batchservice/pool/add#cloudserviceconfiguration) ya da [virtualmachineconfiguration](/rest/api/batchservice/pool/add#virtualmachineconfiguration)kullanılarak oluşturulabilir. ' virtualMachineConfiguration ', tüm Batch yeteneklerini desteklediği için önerilen yapılandırmadır. ' cloudServiceConfiguration ' havuzları tüm özellikleri desteklemiyor ve yeni özellik planlanmıyor.

' CloudServiceConfiguration ' havuzlarını kullanırsanız, ' virtualMachineConfiguration ' havuzlarının kullanılması kesinlikle önerilir. Bu, genişletilmiş [VM Serisi](batch-pool-vm-sizes.md), Linux VM 'ler, [kapsayıcılar](batch-docker-container-workloads.md), [Azure Resource Manager sanal ağlar](batch-virtual-network.md)ve [düğüm disk şifrelemesi](disk-encryption.md)gibi tüm Batch olanaklarından faydalanabilmeniz için size olanak sağlar.

Bu makalede ' virtualMachineConfiguration ' öğesine nasıl geçiş yapılacağı açıklanır.

## <a name="new-pools-are-required"></a>Yeni havuzlar gereklidir

Mevcut etkin havuzlar ' cloudServiceConfiguration ' iken ' virtualMachineConfiguration ' olarak güncelleştirilemez, yeni havuzlar oluşturulmalıdır. ' VirtualMachineConfiguration ' kullanarak havuz oluşturma, tüm Batch API 'Leri, komut satırı araçları, Azure portal ve Batch Explorer kullanıcı arabirimi tarafından desteklenir.

**[.Net](tutorial-parallel-dotnet.md) ve [Python](tutorial-parallel-python.md) öğreticileri, ' virtualmachineconfiguration ' kullanarak havuz oluşturma örneklerini sağlar.**

## <a name="pool-configuration-differences"></a>Havuz yapılandırma farkları

Havuz yapılandırması güncelleştirilirken aşağıdakiler göz önünde bulundurulmalıdır:

- ' cloudServiceConfiguration ' havuz düğümleri her zaman Windows işletim sistemi, ' virtualMachineConfiguration ' havuzları ise Linux veya Windows işletim sistemi olabilir.
- ' CloudServiceConfiguration ' havuzlarıyla karşılaştırıldığında, ' virtualMachineConfiguration ' havuzlarının kapsayıcı desteği, veri diskleri ve disk şifrelemesi gibi daha zengin bir özellik kümesi vardır.
- ' virtualMachineConfiguration ' havuz düğümleri yönetilen işletim sistemi disklerini kullanır. Her düğüm için kullanılan [yönetilen disk türü](../virtual-machines/disks-types.md) , havuz IÇIN seçilen VM boyutuna bağlıdır. Havuz için bir ' VM boyutu belirtilmişse (örneğin, ' Standard_D2s_v3 '), Premium SSD kullanılır. '-S ' olmayan bir VM boyutu belirtilmemişse, örneğin ' Standard_D2_v3 ' için standart HDD kullanılır.

   > [!IMPORTANT]
   > Sanal makinelerde ve sanal makine ölçek kümelerinde olduğu gibi, her düğüm için kullanılan işletim sistemi tarafından yönetilen disk, VM maliyetine ek olarak bir maliyet doğurur. Yerel SSD düğümlerinde işletim sistemi diski oluşturulduğundan, ' cloudServiceConfiguration ' düğümleri için işletim sistemi diski maliyeti yoktur.

- Havuz ve düğüm başlatma ve silme süreleri, ' cloudServiceConfiguration ' havuzları ve ' virtualMachineConfiguration ' havuzları arasında biraz farklı olabilir.

## <a name="azure-data-factory-custom-activity-pools"></a>Özel etkinlik havuzlarını Azure Data Factory

Azure Batch havuzları, Data Factory özel etkinlikleri çalıştırmak için kullanılabilir. Özel etkinlikleri çalıştırmak için kullanılan ' cloudServiceConfiguration ' havuzlarının silinmesi ve yeni ' virtualMachineConfiguration ' havuzlarının oluşturulması gerekir.

- Yürütmelerin kesintiye uğratılmayacak emin olmak için, silme/yeniden oluşturma işleminden önce işlem hatları duraklamalıdır.
- Bağlı hizmet yapılandırma değişikliklerinden kaçınmak için aynı havuz kimliği kullanılabilir.
- Yeni havuzlar oluşturulduğunda işlem hatlarını sürdürür.

Data Factory özel etkinlikleri çalıştırmak için Azure Batch kullanma hakkında daha fazla bilgi için:

- [Bağlı hizmet Azure Batch](../data-factory/compute-linked-services.md#azure-batch-linked-service)
- [Data Factory işlem hattının özel etkinlikleri](../data-factory/transform-data-using-dotnet-custom-activity.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Havuz yapılandırması](nodes-and-pools.md#configurations)hakkında daha fazla bilgi edinin.
- [Havuz en iyi uygulamaları](best-practices.md#pools)hakkında daha fazla bilgi edinin.
- [Havuz ekleme](/rest/api/batchservice/pool/add) ve [virtualmachineconfiguration](/rest/api/batchservice/pool/add#virtualmachineconfiguration)için REST API başvuru.