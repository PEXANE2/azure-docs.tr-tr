---
title: Azure işleme başvuru mimarileri
description: Azure Batch ve diğer Azure hizmetlerini kullanarak, buluta gömülmüş şirket içi bir işleme grubunu genişletmek için mimariler
ms.date: 02/07/2019
ms.topic: how-to
ms.custom: seodec18
ms.openlocfilehash: 784fd89c3dea88e25a2058713897c7a655c8a3af
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83726528"
---
# <a name="reference-architectures-for-azure-rendering"></a>Azure işleme için başvuru mimarileri

Bu makalede, Azure 'da şirket içi bir işleme grubu olarak genişleyen senaryolar veya "veri bloğu" için üst düzey mimari diyagramları gösterilmektedir. Örneklerde Azure işlem, ağ ve depolama hizmetleri için farklı seçenekler gösterilmektedir.

## <a name="hybrid-with-nfs-or-cfs"></a>NFS veya CFS ile karma

Aşağıdaki diyagramda aşağıdaki Azure hizmetlerini içeren bir karma senaryo gösterilmektedir:

* **İşlem** -Azure Batch havuzu veya sanal makine ölçek kümesi.

* Şirket içi **ağ** : Azure ExpressRoute veya VPN. Azure: Azure VNet.

* **Depolama** -giriş ve çıkış dosyaları: Azure VM 'LERINI kullanarak NFS veya cfs, Azure dosya eşitleme veya rsync aracılığıyla şirket içi depolamayla eşitlenir. Alternatif olarak: NFS kullanarak şirket içi NAS cihazlarındaki dosyaları girmek veya çıkış yapmak için avere vFXT.

  ![Bulut patlaması-NFS veya CFS ile karma](./media/batch-rendering-architectures/hybrid-nfs-cfs-avere.png)

## <a name="hybrid-with-blobfuse"></a>Blobsigortası ile karma

Aşağıdaki diyagramda aşağıdaki Azure hizmetlerini içeren bir karma senaryo gösterilmektedir:

* **İşlem** -Azure Batch havuzu veya sanal makine ölçek kümesi.

* Şirket içi **ağ** : Azure ExpressRoute veya VPN. Azure: Azure VNet.

* **Depolama** -giriş ve çıkış dosyaları: Azure blobsigortası aracılığıyla işlem kaynaklarına bağlanmış BLOB depolama.

  ![Blobsigortası ile bulut patlaması-karma](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Karma işlem ve depolama

Aşağıdaki diyagramda hem işlem hem de depolama için tam bağlantılı bir karma senaryo gösterilmektedir ve aşağıdaki Azure hizmetleri bulunur:

* **İşlem** -Azure Batch havuzu veya sanal makine ölçek kümesi.

* Şirket içi **ağ** : Azure ExpressRoute veya VPN. Azure: Azure VNet.

* **Depolama** -şirket Içi: avere vFXT. Azure Data Box BLOB depolama alanına veya şirket içi avere FXT 'ye yönelik şirket içi dosyaların, NAS hızlandırma için isteğe bağlı olarak arşivlenmesi.

  ![Bulut patlaması-karma işlem ve depolama](./media/batch-rendering-architectures/hybrid-compute-storage-avere.png)


## <a name="next-steps"></a>Sonraki adımlar

* Azure Batch ile [işleme yöneticilerini](batch-rendering-render-managers.md) kullanma hakkında daha fazla bilgi edinin.

* [Azure 'Da işleme](batch-rendering-service.md)seçenekleri hakkında daha fazla bilgi edinin.