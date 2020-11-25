---
title: Havuzlar için VM boyutlarını ve görüntülerini seçin
description: Azure Batch havuzlarda işlem düğümleri için kullanılabilir VM boyutları ve işletim sistemi sürümleri arasından seçim yapma
ms.topic: conceptual
ms.date: 11/24/2020
ms.custom: seodec18
ms.openlocfilehash: 8bb54a4db62f56f442f7cec81e6768241a05ffee
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95895239"
---
# <a name="choose-a-vm-size-and-image-for-compute-nodes-in-an-azure-batch-pool"></a>Bir Azure Batch havuzundaki işlem düğümleri için VM boyutu ve görüntüsü seçme

Bir Azure Batch havuzu için bir düğüm boyutu seçtiğinizde, Azure 'da kullanılabilir olan neredeyse tüm VM boyutlarının arasından seçim yapabilirsiniz. Azure, farklı iş yükleri için Linux ve Windows VM 'Leri için bir dizi boyut sunar.

## <a name="supported-vm-series-and-sizes"></a>Desteklenen VM Serisi ve boyutları

Batch havuzunuz için bir VM boyutu seçmeye yönelik birkaç özel durum ve sınırlama vardır:

- Batch 'de bazı VM serileri veya VM boyutları desteklenmez.
- Bazı VM boyutları kısıtlıdır ve ayrılabilmeleri için özel olarak etkinleştirilmesi gerekir.

### <a name="pools-in-virtual-machine-configuration"></a>Sanal makine yapılandırmasındaki havuzlar

Sanal makine yapılandırmasındaki toplu iş havuzları neredeyse tüm [VM boyutlarını](../virtual-machines/sizes.md)destekler. Desteklenen boyutlar ve kısıtlamalar hakkında daha fazla bilgi edinmek için aşağıdaki tabloya bakın.

| VM serisi  | Desteklenen boyutlar |
|------------|---------|
| Temel A | Basic_A0 *dışındaki* tüm boyutlar (a0) |
| A | Standard_A0 *dışındaki* tüm boyutlar |
| AV2 | Tüm Boyutlar |
| B | Desteklenmez |
| DC | Desteklenmez |
| Dv2, DSv2 | Tüm Boyutlar |
| Dv3, Dsv3 | Tüm Boyutlar |
| Dav4<sup>1</sup> | Tüm Boyutlar |
| Dasv4<sup>1</sup> | Tüm Boyutlar |
| Ddv4, Ddsv4 |  Tüm Boyutlar |
| Dv4, Dsv4 | Desteklenmez |
| Ev3, Esv3 | E64is_v3 dışındaki tüm boyutlar |
| Eav4<sup>1</sup> | Tüm Boyutlar |
| Easv4<sup>1</sup> | Tüm Boyutlar |
| Edv4, Edsv4 |  Tüm Boyutlar |
| Ev4, Esv4 | Desteklenmez |
| F, FS | Tüm Boyutlar |
| Fsv2 | Tüm Boyutlar |
| G, GS | Tüm Boyutlar |
| H | Tüm Boyutlar |
| HB<sup>1</sup> | Tüm Boyutlar |
| HBv2<sup>1</sup> | Tüm Boyutlar |
| HC<sup>1</sup> | Tüm Boyutlar |
| Ls | Tüm Boyutlar |
| Lsv2<sup>1</sup> | Tüm Boyutlar |
| A<sup>1</sup> | Tüm Boyutlar |
| Mv2<sup>1, 2</sup> | Tüm Boyutlar |
| NC | Tüm Boyutlar |
| NCv2<sup>1</sup> | Tüm Boyutlar |
| NCv3<sup>1</sup> | Tüm Boyutlar |
| NCasT4_v3 | Hiçbiri-henüz kullanılamıyor |
| ND<sup>1</sup> | Tüm Boyutlar |
| NDv2<sup>1</sup> | Hiçbiri-henüz kullanılamıyor |
| NV | Tüm Boyutlar |
| NVv3<sup>1</sup> | Tüm Boyutlar |
| NVv4<sup>1</sup> | Tüm Boyutlar |
| SAP HANA | Desteklenmez |

<sup>1</sup> bu VM Serisi, sanal makine yapılandırmasındaki Batch havuzlarında ayrılabilir, ancak yeni bir Batch hesabı oluşturmanız ve belirli bir [Kota artışı](batch-quota-limit.md#increase-a-quota)istemeniz gerekir. Bu sınırlama, VM Serisi başına vCPU kotası Batch hesapları için tam olarak desteklendikten sonra kaldırılacaktır.

<sup>2</sup> bu VM Serisi yalnızca 2. nesil VM görüntüleriyle kullanılabilir.

### <a name="using-generation-2-vm-images"></a>2. nesil VM görüntülerini kullanma

[Mv2](../virtual-machines/mv2-series.md)gıbı bazı VM serileri yalnızca [2. nesil VM görüntüleriyle](../virtual-machines/generation-2.md)kullanılabilir. 2. nesil VM görüntüleri, [' ImageReference '](/rest/api/batchservice/pool/add#imagereference) yapılandırmasının ' SKU ' özelliği kullanılarak HERHANGI bir VM görüntüsü gibi belirtilir; ' SKU ' dizelerinin "-G2" veya "-Gen2" gibi bir soneki vardır. 2. nesil görüntüler dahil toplu Işlem tarafından desteklenen VM görüntülerinin listesini almak için [' desteklenen görüntüleri Listele '](/rest/api/batchservice/account/listsupportedimages) API, [POWERSHELL](/powershell/module/az.batch/get-azbatchsupportedimage)veya [Azure CLI](/cli/azure/batch/pool/supported-images)kullanın.

### <a name="pools-in-cloud-service-configuration"></a>Bulut hizmeti yapılandırmasındaki havuzlar

Bulut hizmeti yapılandırmasındaki toplu iş havuzları aşağıdakiler **dışında** [Cloud Services için tüm VM boyutlarını](../cloud-services/cloud-services-sizes-specs.md) destekler:

| VM serisi  | Desteklenmeyen Boyutlar |
|------------|-------------------|
| A Serisi   | Çok küçük       |
| Av2 Serisi | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="size-considerations"></a>Boyutla ilgili dikkat edilmesi gerekenler

- **Uygulama gereksinimleri** -düğümlerde çalıştıracağınız uygulamanın özelliklerini ve gereksinimlerini göz önünde bulundurun. Uygulamanın çok iş parçacıklı olup olmadığı ve ne kadar bellek kullandığı gibi konular en uygun ve ekonomik düğüm boyutunu belirlemeye yardımcı olabilir. Çok örnekli [MPI iş yükleri](batch-mpi.md) veya CUDA uygulamaları için sırasıyla özel [HPC](../virtual-machines/sizes-hpc.md) veya [GPU özellikli](../virtual-machines/sizes-gpu.md) VM boyutlarını göz önünde bulundurun. Daha fazla bilgi için bkz. [Batch havuzlarında RDMA özellikli veya GPU özellikli örnekler kullanma](batch-pool-compute-intensive-sizes.md).

- **Düğüm başına görevler** -tek seferde bir düğümde bir görevin çalıştığını varsayarak düğüm boyutu seçmek tipik bir davranıştır. Ancak, iş yürütme sırasında birden fazla görevin (ve dolayısıyla birden çok uygulama örneğinin) işlem düğümlerinde [paralel olarak çalıştırılması](batch-parallel-node-tasks.md) avantajlı olabilir. Bu durumda, paralel görev yürütmesinin arttığı talebe uyum sağlamak için çok düğümlü bir düğüm boyutu seçmek yaygındır.

- **Farklı görevler Için yük düzeyleri** -bir havuzdaki tüm düğümler aynı boyutta. Farklı sistem gereksinimlerine ve/veya yük düzeylerine sahip uygulamalar çalıştırmayı planlıyorsanız ayrı havuzlar oluşturmanız önerilir.

- **Bölge kullanılabilirliği** -Batch hesaplarınızı oluşturduğunuz bölgelerde bir VM serisi veya boyutu kullanılamayabilir. Bir boyutun kullanılabilir olduğunu denetlemek için bkz. [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).

- **Kotalar** -Batch hesabınızdaki [çekirdekler kotaları](batch-quota-limit.md#resource-quotas) , bir Batch havuzuna ekleyebileceğiniz belirli boyuttaki düğümlerin sayısını sınırlayabilir. Gerektiğinde, [bir kota artışı isteyebilirsiniz](batch-quota-limit.md#increase-a-quota).

- **Havuz yapılandırması** -genel olarak, sanal makine yapılandırmasında, bulut hizmeti yapılandırmasıyla karşılaştırıldığında bir havuz oluşturduğunuzda daha fazla VM boyutu seçeneğiniz vardır.

## <a name="supported-vm-images"></a>Desteklenen VM görüntüleri

Her bir görüntü için düğüm Aracısı SKU kimlikleri dahil olmak üzere Batch tarafından şu anda desteklenen Windows ve Linux VM görüntülerinin listesini döndürmek için aşağıdaki API 'lerden birini kullanın:

- Batch hizmeti REST API: [desteklenen görüntüleri listeleme](/rest/api/batchservice/account/listsupportedimages)
- PowerShell: [Get-AzBatchSupportedImage](/powershell/module/az.batch/get-azbatchsupportedimage)
- Azure CLı: [az Batch Pool destekleniyor-Images](/cli/azure/batch/pool/supported-images)

## <a name="next-steps"></a>Sonraki adımlar

- [Batch hizmeti iş akışı ve](batch-service-workflow-features.md) havuzlar, düğümler, işler ve görevler gibi birincil kaynaklar hakkında bilgi edinin.
- İşlem yoğunluklu VM boyutlarını kullanma hakkında daha fazla bilgi için bkz. [Batch havuzlarında RDMA özellikli veya GPU özellikli örnekler kullanma](batch-pool-compute-intensive-sizes.md).
