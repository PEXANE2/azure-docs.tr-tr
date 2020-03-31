---
title: Havuzlar için VM boyutlarını seçin - Azure Toplu İş | Microsoft Dokümanlar
description: Azure Toplu İşlem havuzlarında işlem düğümleri için kullanılabilir VM boyutları arasından seçim yapma
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/12/2019
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: c18190ec5e5d079d51630a976681717a78a46e00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77087053"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Azure Toplu İşlem havuzundaki işlem düğümleri için VM boyutu seçin

Bir Azure Toplu İş havuzu için düğüm boyutu seçtiğinizde, Azure'da bulunan hemen hemen tüm VM boyutları arasından seçim yapabilirsiniz. Azure, linux ve Windows VM'leri için farklı iş yükleri için çeşitli boyutlar sunar.

VM boyutu seçmenin birkaç istisnası ve sınırlaması vardır:

* Toplu olarak bazı VM serileri veya VM boyutları desteklenmez.
* Bazı VM boyutları kısıtlanır ve tahsis edilmeden önce özel olarak etkinleştirilmesi gerekir.

## <a name="supported-vm-series-and-sizes"></a>Desteklenen VM serisi ve boyutları

### <a name="pools-in-virtual-machine-configuration"></a>Sanal Makine yapılandırmasındaki havuzlar

Sanal Makine yapılandırmasındaki toplu havuzlar hemen hemen tüm VM boyutlarını destekler ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)). Desteklenen boyutlar ve kısıtlamalar hakkında daha fazla bilgi edinmek için aşağıdaki tabloya bakın.

| VM serisi  | Desteklenen boyutlar |
|------------|---------|
| Temel A | Basic_A0 (A0) *hariç* tüm boyutlar |
| A | Standard_A0 *hariç* tüm boyutlar |
| Av2 | Tüm boyutlar |
| B | None |
| DC | None |
| Dv2, DSv2 | Tüm boyutlar |
| Dv3, Dsv3 | Tüm boyutlar |
| Dav4, Dasv4 | Yok - henüz mevcut değil |
| Ev3, Esv3 | E64is_v3 ve E64i_v3 hariç tüm boyutlar |
| Eav4, Easv4 | Yok - henüz mevcut değil |
| F, Fs | Tüm boyutlar |
| Fsv2 | Tüm boyutlar |
| G, Gs | Tüm boyutlar |
| H | Tüm boyutlar |
| HB<sup>1</sup> | Tüm boyutlar |
| HBv2<sup>1</sup> | Tüm boyutlar |
| HC<sup>1</sup> | Tüm boyutlar |
| Ls | Tüm boyutlar |
| Lsv2 | Yok - henüz mevcut değil |
| M<sup>1</sup> | M64, M64m, M128, M128 hariç tüm boyutlar |
| Mv2 | Yok - henüz mevcut değil |
| NC | Tüm boyutlar |
| NCv2<sup>1</sup> | Tüm boyutlar |
| NCv3<sup>1</sup> | Tüm boyutlar |
| ND<sup>1</sup> | Tüm boyutlar |
| NDv2<sup>1</sup> | Yok - henüz mevcut değil |
| NV | Tüm boyutlar |
| NVv3<sup>1</sup> | Tüm boyutlar |
| NVv4 | None |
| SAP HANA | None |

<sup>1</sup> Bu VM boyutları Sanal Makine yapılandırmasında Toplu havuzlarda ayrılabilir, ancak yeni bir Toplu iş hesabı oluşturmalı ve belirli bir [kota artışı](batch-quota-limit.md#increase-a-quota)istemeniz gerekir. Toplu İşlem hesapları için VM serisi başına vCPU kotası tam olarak desteklendikten sonra bu sınırlama kaldırılır.

### <a name="pools-in-cloud-service-configuration"></a>Bulut Hizmeti yapılandırmasındaki havuzlar

Bulut Hizmeti yapılandırmasındaki toplu iş havuzları, aşağıdakiler **dışında** Bulut Hizmetleri için tüm [VM boyutlarını](../cloud-services/cloud-services-sizes-specs.md) destekler:

| VM serisi  | Desteklenmeyen boyutlar |
|------------|-------------------|
| A Serisi   | Ekstra küçük       |
| Av2 Serisi | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="size-considerations"></a>Boyutla ilgili dikkat edilmesi gerekenler

* **Uygulama gereksinimleri** - Düğümlerde çalıştıracağınız uygulamanın özelliklerini ve gereksinimlerini göz önünde bulundurun. Uygulamanın çok iş parçacıklı olup olmadığı ve ne kadar bellek kullandığı gibi konular en uygun ve ekonomik düğüm boyutunu belirlemeye yardımcı olabilir. Çok örnekli [MPI iş yükleri](batch-mpi.md) veya CUDA uygulamaları için, sırasıyla özel [HPC](../virtual-machines/linux/sizes-hpc.md) veya [GPU özellikli](../virtual-machines/linux/sizes-gpu.md) VM boyutlarını göz önünde bulundurun. [(Bkz. Toplu Iş havuzlarında RDMA özellikli veya GPU özellikli örnekleri kullanın.)](batch-pool-compute-intensive-sizes.md)

* **Düğüm başına görevler** - Bir görevin bir seferde düğüm üzerinde çalıştığını varsayarak bir düğüm boyutu seçmek tipiktir. Ancak, birden çok görev (ve bu nedenle birden çok uygulama örnekleri) iş yürütme sırasında bilgi işlem düğümleri üzerinde [paralel olarak çalıştırmak](batch-parallel-node-tasks.md) için avantajlı olabilir. Bu durumda, paralel görev yürütme artan talebi karşılamak için çok çekirdekli bir düğüm boyutu seçmek yaygındır.

* **Farklı görevler için yük düzeyleri** - Havuzdaki düğümlerin tümü aynı boyuttadır. Farklı sistem gereksinimlerine ve/veya yük düzeylerine sahip uygulamalar çalıştırmayı planlıyorsanız ayrı havuzlar oluşturmanız önerilir.

* **Bölge kullanılabilirliği** - Toplu Iş hesaplarını oluşturduğunuz bölgelerde VM serisi veya boyutu kullanılamayabilir. Bir boyutun kullanılabilir olup olmadığını kontrol etmek için, [bölgeye göre kullanılabilen Ürünler'e](https://azure.microsoft.com/regions/services/)bakın.

* **Kotalar** - Toplu Iş hesabınızdaki [çekirdek kotaları,](batch-quota-limit.md#resource-quotas) Toplu Iş havuzuna ekleyebileceğiniz belirli bir boyutun düğüm sayısını sınırlayabilir. Kota artışı istemek için [bu makaleye](batch-quota-limit.md#increase-a-quota)bakın. 

* **Havuz yapılandırması** - Genel olarak, Sanal Makine yapılandırmasında Bulut Hizmeti yapılandırmasıyla karşılaştırıldığında daha fazla VM boyutu seçeneğiniz var.

## <a name="next-steps"></a>Sonraki adımlar

* Toplu İşleme'ye derinlemesine bir bakış için [bkz.](batch-api-basics.md)
* İşlem yoğun VM boyutlarını kullanma hakkında bilgi için toplu [iş havuzlarında RDMA özellikli veya GPU özellikli örnekleri kullanın'a](batch-pool-compute-intensive-sizes.md)bakın.
