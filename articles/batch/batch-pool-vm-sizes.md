---
title: Havuzlar için VM boyutlarını seçin
description: Azure Batch havuzlarda işlem düğümleri için kullanılabilir VM boyutları arasından seçim yapma
ms.topic: conceptual
ms.date: 06/10/2020
ms.custom: seodec18
ms.openlocfilehash: c1621bbbe6676144176636346f3f5c34db169b7a
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84693167"
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
| Dav4<sup>1</sup> | Hiçbiri-henüz kullanılamıyor |
| Dasv4<sup>1</sup> | Standard_D48as_v4, Standard_D64as_v4, Standard_D96as_v4 hariç tüm boyutlar |
| Ddv4, Ddsv4 |  Hiçbiri-henüz kullanılamıyor |
| Ev3, Esv3 | E64is_v3 ve E64i_v3 hariç tüm boyutlar |
| Eav4<sup>1</sup> | Standard_E48a_v4, Standard_E64a_v4, Standard_E96a_v4 hariç tüm boyutlar |
| Easv4<sup>1</sup> | Standard_E48as_v4, Standard_E64as_v4, Standard_E96as_v4 hariç tüm boyutlar |
| Edv4, Edsv4 |  Hiçbiri-henüz kullanılamıyor |
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

* [Batch hizmeti iş akışı ve](batch-service-workflow-features.md) havuzlar, düğümler, işler ve görevler gibi birincil kaynaklar hakkında bilgi edinin.
* İşlem yoğunluklu VM boyutlarını kullanma hakkında daha fazla bilgi için bkz. [Batch havuzlarında RDMA özellikli veya GPU özellikli örnekler kullanma](batch-pool-compute-intensive-sizes.md).
