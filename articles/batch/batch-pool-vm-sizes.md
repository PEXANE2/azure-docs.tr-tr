---
title: Havuzlar için VM boyutlarını seçin
description: Azure Batch havuzlarda işlem düğümleri için kullanılabilir VM boyutları arasından seçim yapma
ms.topic: how-to
ms.date: 09/12/2019
ms.custom: seodec18
ms.openlocfilehash: a299ce05caa02432f96dfbb5eb675bc2d6f82ab5
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83723723"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Bir Azure Batch havuzundaki işlem düğümleri için VM boyutu seçme

Bir Azure Batch havuzu için bir düğüm boyutu seçtiğinizde, Azure 'da kullanılabilir olan neredeyse tüm VM boyutlarının arasından seçim yapabilirsiniz. Azure, farklı iş yükleri için Linux ve Windows VM 'Leri için bir dizi boyut sunar.

VM boyutu seçmenin bazı özel durumları ve sınırlamaları vardır:

* Batch 'de bazı VM serileri veya VM boyutları desteklenmez.
* Bazı VM boyutları kısıtlıdır ve ayrılabilmeleri için özel olarak etkinleştirilmesi gerekir.

## <a name="supported-vm-series-and-sizes"></a>Desteklenen VM Serisi ve boyutları

### <a name="pools-in-virtual-machine-configuration"></a>Sanal makine yapılandırmasındaki havuzlar

Sanal makine yapılandırmasındaki toplu iş havuzları neredeyse tüm VM boyutlarını destekler ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)). Desteklenen boyutlar ve kısıtlamalar hakkında daha fazla bilgi edinmek için aşağıdaki tabloya bakın.

| VM serisi  | Desteklenen boyutlar |
|------------|---------|
| Temel A | Basic_A0 *dışındaki* tüm boyutlar (a0) |
| A | Standard_A0 *dışındaki* tüm boyutlar |
| AV2 | Tüm Boyutlar |
| B | Yok |
| DC | Yok |
| Dv2, DSv2 | Tüm Boyutlar |
| Dv3, Dsv3 | Tüm Boyutlar |
| Dav4, Dasv4 | Hiçbiri-henüz kullanılamıyor |
| Ev3, Esv3 | E64is_v3 ve E64i_v3 hariç tüm boyutlar |
| Eav4, Easv4 | Hiçbiri-henüz kullanılamıyor |
| F, FS | Tüm Boyutlar |
| Fsv2 | Tüm Boyutlar |
| G, GS | Tüm Boyutlar |
| H | Tüm Boyutlar |
| HB<sup>1</sup> | Tüm Boyutlar |
| HBv2<sup>1</sup> | Tüm Boyutlar |
| HC<sup>1</sup> | Tüm Boyutlar |
| Ls | Tüm Boyutlar |
| Lsv2 | Hiçbiri-henüz kullanılamıyor |
| A<sup>1</sup> | M64, M64m, M128, M128m hariç tüm boyutlar |
| Mv2 | Hiçbiri-henüz kullanılamıyor |
| NC | Tüm Boyutlar |
| NCv2<sup>1</sup> | Tüm Boyutlar |
| NCv3<sup>1</sup> | Tüm Boyutlar |
| ND<sup>1</sup> | Tüm Boyutlar |
| NDv2<sup>1</sup> | Hiçbiri-henüz kullanılamıyor |
| NV | Tüm Boyutlar |
| NVv3<sup>1</sup> | Tüm Boyutlar |
| NVv4 | Yok |
| SAP HANA | Yok |

<sup>1</sup> bu VM boyutları, sanal makine yapılandırmasındaki Batch havuzlarında tahsis edilebilir, ancak yeni bir Batch hesabı oluşturmanız ve belirli bir [Kota artışı](batch-quota-limit.md#increase-a-quota)istemeniz gerekir. Bu sınırlama, VM Serisi başına vCPU kotası Batch hesapları için tam olarak desteklendikten sonra kaldırılacaktır.

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
