---
title: Havuzlar için VM boyutlarını seçin-Azure Batch | Microsoft Docs
description: Azure Batch havuzlarda işlem düğümleri için kullanılabilir VM boyutları arasından seçim yapma
services: batch
documentationcenter: ''
author: ju-shim
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/12/2019
ms.author: jushiman
ms.custom: seodec18
ms.openlocfilehash: 34ab09f7d8d47804992b8ef6864bfea60d1c9b4d
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76026615"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Bir Azure Batch havuzundaki işlem düğümleri için VM boyutu seçme

Bir Azure Batch havuzu için bir düğüm boyutu seçtiğinizde, Azure 'da kullanılabilir olan neredeyse tüm VM boyutlarının arasından seçim yapabilirsiniz. Azure, farklı iş yükleri için Linux ve Windows VM 'Leri için bir dizi boyut sunar.

VM boyutu seçmenin bazı özel durumları ve sınırlamaları vardır:

* Batch 'de bazı VM serileri veya VM boyutları desteklenmez.
* Bazı VM boyutları kısıtlıdır ve ayrılabilmeleri için özel olarak etkinleştirilmesi gerekir.

## <a name="supported-vm-series-and-sizes"></a>Desteklenen VM Serisi ve boyutları

### <a name="pools-in-virtual-machine-configuration"></a>Sanal makine yapılandırmasındaki havuzlar

Sanal makine yapılandırmasındaki toplu iş havuzları neredeyse tüm VM boyutlarını destekler ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)). Desteklenen boyutlar ve kısıtlamalar hakkında daha fazla bilgi edinmek için aşağıdaki tabloya bakın.

Listelenen tüm promosyon veya önizleme VM boyutları destek için garanti edilmez.

| VM serisi  | Desteklenen boyutlar | Batch hesabı havuzu ayırma modu<sup>1</sup> |
|------------|---------|-----------------|
| Temel A serisi | Basic_A0 *dışındaki* tüm boyutlar (a0) | Herhangi biri |
| A Serisi | Standard_A0 *dışındaki* tüm boyutlar | Herhangi biri |
| Av2 Serisi | Tüm Boyutlar | Herhangi biri |
| B serisi | Hiçbiri | Kullanılamıyor |
| DC serisi | Hiçbiri | Kullanılamıyor |
| Dv2, DSv2 serisi | Tüm Boyutlar | Herhangi biri |
| Dv3, Dsv3 serisi | Tüm Boyutlar | Herhangi biri |
| Ev3, Esv3 serisi | Tüm Boyutlar | Herhangi biri |
| Fsv2-serisi | Tüm Boyutlar | Herhangi biri |
| H serisi | Tüm Boyutlar | Herhangi biri |
| HB Serisi<sup>2</sup> | Tüm Boyutlar | Herhangi biri |
| HC Serisi<sup>2</sup> | Tüm Boyutlar | Herhangi biri |
| Ls serisi | Tüm Boyutlar | Herhangi biri |
| Lsv2 serisi | Hiçbiri | Kullanılamıyor |
| M serisi | Standard_M64ms (yalnızca düşük öncelikli), Standard_M128s (yalnızca düşük öncelikli) | Herhangi biri |
| Mv2 serisi | Hiçbiri | Kullanılamıyor |
| NC serisi | Tüm Boyutlar | Herhangi biri |
| NCv2-serisi<sup>2</sup> | Tüm Boyutlar | Herhangi biri |
| NCv3-serisi<sup>2</sup> | Tüm Boyutlar | Herhangi biri |
| ND serisi<sup>2</sup> | Tüm Boyutlar | Herhangi biri |
| NDv2 serisi | Tüm Boyutlar | Kullanıcı aboneliği modu |
| NV serisi | Tüm Boyutlar | Herhangi biri |
| NVv3 serisi | Hiçbiri | Kullanılamıyor |
| SAP HANA | Hiçbiri | Kullanılamıyor |

<sup>1</sup> bazı yeni VM serileri başlangıçta kısmen desteklenir. Bu VM Serisi, **havuz ayırma modu** **Kullanıcı aboneliğine**ayarlanmış olan Batch hesapları tarafından tahsis edilebilir. Batch hesabı yapılandırması hakkında daha fazla bilgi için bkz. [Batch hesaplarını yönetme](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode) . **Kullanıcı aboneliği** Batch hesapları için kısmen desteklenen bu VM serisine nasıl kota isteneceğini öğrenmek için [Kotalar ve sınırlar](batch-quota-limit.md) bölümüne bakın.  

<sup>2</sup> bu VM boyutları, sanal makine yapılandırmasındaki Batch havuzlarında tahsis edilebilir, ancak belirli bir [Kota artışı](batch-quota-limit.md#increase-a-quota)istemeniz gerekir.

### <a name="pools-in-cloud-service-configuration"></a>Bulut hizmeti yapılandırmasındaki havuzlar

Bulut hizmeti yapılandırmasındaki toplu iş havuzları aşağıdakiler **dışında** [Cloud Services için tüm VM boyutlarını](../cloud-services/cloud-services-sizes-specs.md) destekler:

| VM serisi  | Desteklenmeyen Boyutlar |
|------------|-------------------|
| A Serisi   | Çok küçük       |
| Av2 Serisi | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="size-considerations"></a>Boyutla ilgili dikkat edilmesi gerekenler

* **Uygulama gereksinimleri** -düğümlerde çalıştıracağınız uygulamanın özelliklerini ve gereksinimlerini göz önünde bulundurun. Uygulamanın çok iş parçacıklı olup olmadığı ve ne kadar bellek kullandığı gibi konular en uygun ve ekonomik düğüm boyutunu belirlemeye yardımcı olabilir. Çok örnekli [MPI iş yükleri](batch-mpi.md) veya CUDA uygulamaları için sırasıyla özel [HPC](../virtual-machines/linux/sizes-hpc.md) veya [GPU özellikli](../virtual-machines/linux/sizes-gpu.md) VM boyutlarını göz önünde bulundurun. (Bkz. [Batch havuzlarında RDMA özellikli veya GPU özellikli örnekler kullanma](batch-pool-compute-intensive-sizes.md).)

* **Düğüm başına görevler** -tek seferde bir düğümde bir görevin çalıştığını varsayarak düğüm boyutu seçmek tipik bir davranıştır. Ancak, iş yürütme sırasında birden fazla görevin (ve dolayısıyla birden çok uygulama örneğinin) işlem düğümlerinde [paralel olarak çalıştırılması](batch-parallel-node-tasks.md) avantajlı olabilir. Bu durumda, paralel görev yürütmesinin arttığı talebe uyum sağlamak için çok düğümlü bir düğüm boyutu seçmek yaygındır.

* **Farklı görevler Için yük düzeyleri** -bir havuzdaki tüm düğümler aynı boyutta. Farklı sistem gereksinimlerine ve/veya yük düzeylerine sahip uygulamalar çalıştırmayı planlıyorsanız ayrı havuzlar oluşturmanız önerilir.

* **Bölge kullanılabilirliği** -Batch hesaplarınızı oluşturduğunuz bölgelerde bir VM serisi veya boyutu kullanılamayabilir. Bir boyutun kullanılabilir olduğunu denetlemek için bkz. [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).

* **Kotalar** -Batch hesabınızdaki [çekirdekler kotaları](batch-quota-limit.md#resource-quotas) , bir Batch havuzuna ekleyebileceğiniz belirli boyuttaki düğümlerin sayısını sınırlayabilir. Kota artışı istemek için [Bu makaleye](batch-quota-limit.md#increase-a-quota)bakın. 

* **Havuz yapılandırması** -genel olarak, sanal makine yapılandırmasında, bulut hizmeti yapılandırmasıyla karşılaştırıldığında bir havuz oluşturduğunuzda daha fazla VM boyutu seçeneğiniz vardır.

## <a name="next-steps"></a>Sonraki adımlar

* Toplu Işe yönelik ayrıntılı genel bakış için bkz. [Batch ile büyük ölçekli paralel işlem çözümleri geliştirme](batch-api-basics.md).
* İşlem yoğunluklu VM boyutlarını kullanma hakkında daha fazla bilgi için bkz. [Batch havuzlarında RDMA özellikli veya GPU özellikli örnekler kullanma](batch-pool-compute-intensive-sizes.md).
