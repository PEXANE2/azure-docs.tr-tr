---
title: Azure VM boyutları - GPU | Microsoft Dokümanlar
description: Azure'daki sanal makineler için kullanılabilen farklı GPU optimize edilmiş boyutları listeler. VCPUs, veri diskleri ve NIC'lerin sayısı nın yanı sıra bu serideki boyutlar için depolama verime ve ağ bant genişliği hakkındaki bilgileri listeler.
services: virtual-machines
documentationcenter: ''
author: vikancha
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 7e1e0d488844a94bd0be2b91398678e620295729
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77913591"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU optimize edilmiş sanal makine boyutları

GPU optimize vm boyutları tek veya birden fazla NVIDIA GPU ile kullanılabilir özel sanal makinelerdir. Bu boyutlar bilgi işlem yoğun, grafik yoğun ve görselleştirme iş yükleri için tasarlanmıştır. Bu makalede, GPU sayısı ve türü hakkında bilgi sağlar, vCPUs, veri diskleri, ve NIC'ler. Depolama iş ortası ve ağ bant genişliği de bu gruplandırmada her boyut için dahildir.

- [NC serisi,](nc-series.md) [NCv2 serisi, NCv3](ncv2-series.md) [serisi](ncv3-series.md) boyutları bilgi işlem yoğun ve ağ yoğun uygulamalar ve algoritmalar için optimize edilebiyi. Bazı örnekler CUDA ve OpenCL tabanlı uygulamalar ve simülasyonlar, AI ve Derin Öğrenme vardır. NCv3 serisi, NVIDIA'nın Tesla V100 GPU'suyla yüksek performanslı bilgi işlem iş yüklerine odaklanmıştır. NC serisi Intel Xeon E5-2690 v3 2.60GHz v3 (Haswell) işlemci kullanır ve NCv2 serisi ve NCv3 serisi VM'ler Intel Xeon E5-2690 v4 (Broadwell) işlemci kullanır.

- [ND-serisi](nd-series.md)ve [NDv2 serisi](ndv2-series.md) boyutları derin öğrenme için eğitim ve çıkarım senaryoları üzerinde durulmaktadır. Onlar NVIDIA Tesla P40 GPU ve Intel Xeon E5-2690 v4 (Broadwell) işlemci kullanın. NDv2 serisi Intel Xeon Platinum 8168 (Skylake) işlemcikullanır.

- [NV serisi](nv-series.md) ve [NVv3 serisi](nvv3-series.md) boyutlar, OpenGL ve DirectX gibi çerçeveler kullanılarak uzaktan görselleştirme, akış, oyun, kodlama ve VDI senaryoları için optimize edilmiş ve tasarlanmıştır. Bu VM'ler NVIDIA Tesla M60 GPU tarafından desteklenen.

- [NVv4 serisi](nvv4-series.md) VDI boyutları optimize edilmiş ve VDI ve uzaktan görselleştirme için tasarlanmıştır. Bölümlenmiş GPU'larla NVv4, daha küçük GPU kaynakları gerektiren iş yükleri için doğru boyutu sunar. Bu VM'ler AMD Radeon Instinct MI25 GPU tarafından desteklenen. NVv4 VM'ler şu anda yalnızca Windows konuk işletim sistemini desteklemede dir.

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Azure N serisi VM'lerin GPU özelliklerinden yararlanmak için NVIDIA GPU sürücülerinin yüklenmesi gerekir.

[NVIDIA GPU Sürücü Uzantısı,](/azure/virtual-machines/extensions/hpccompute-gpu-windows) n serisi VM'ye uygun NVIDIA CUDA veya GRID sürücülerini yükler. Azure portalını veya Azure PowerShell veya Azure Kaynak Yöneticisi şablonları gibi araçları kullanarak uzantıyı yükleyin veya yönetin. Desteklenen işletim sistemleri ve dağıtım adımları için [NVIDIA GPU Sürücü Uzantısı belgelerine](/azure/virtual-machines/extensions/hpccompute-gpu-windows) bakın. VM uzantıları hakkında genel bilgi için [Azure sanal makine uzantıları ve özelliklerine](/azure/virtual-machines/extensions/overview)bakın.

NVIDIA GPU sürücülerini el ile yüklemeyi seçerseniz, desteklenen işletim sistemleri, sürücüler, kurulum ve doğrulama adımları için Linux için Windows veya N serisi GPU sürücü [kurulumu için N serisi GPU sürücü kurulumuna](/azure/virtual-machines/windows/n-series-driver-setup) bakın. [N-series GPU driver setup for Linux](/azure/virtual-machines/linux/n-series-driver-setup)

## <a name="deployment-considerations"></a>Dağıtma konuları

- N serisi VM'lerin kullanılabilirliği için [bölgeye göre sunulan Ürünler](https://azure.microsoft.com/regions/services/)bölümüne bakın.

- N serisi VM'ler yalnızca Kaynak Yöneticisi dağıtım modelinde dağıtılabilir.

- N serisi VM'ler, diskleri için destekladıkları Azure Depolama türünde farklılık gösterir. NC ve NV VM'ler yalnızca Standart Disk Depolama (HDD) tarafından desteklenen VM diskleri destekler. NCv2, NCv3, ND, NDv2 ve NVv2 VM'ler yalnızca Premium Disk Depolama (SSD) tarafından desteklenen VM diskleri destekler.

- Birkaç N serisinden fazla VM dağıtmak istiyorsanız, istediğiniz kadar öde aboneliği veya diğer satın alma seçeneklerini düşünün. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

- Azure aboneliğinizdeki çekirdek kotasını (bölge başına) artırmanız ve NC, NCv2, NCv3, ND, NDv2, NV veya NVv2 çekirdekleri için ayrı kotayı artırmanız gerekebilir. Kota artışı talep etmek için ücretsiz [olarak çevrimiçi müşteri destek isteği açın.](../azure-portal/supportability/how-to-create-azure-support-request.md) Varsayılan sınırlar abonelik kategorinize bağlı olarak değişebilir.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaç](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.
