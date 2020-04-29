---
title: Azure VM boyutları-GPU | Microsoft Docs
description: Azure 'daki sanal makineler için kullanılabilen farklı GPU iyileştirilmiş boyutları listeler. Bu serideki boyutlarda vCPU sayısı, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkındaki bilgileri listeler.
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
ms.openlocfilehash: 73a878d791997169b0823bb949e78bdced77cae6
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82509695"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU iyileştirilmiş sanal makine boyutları

GPU iyileştirilmiş VM boyutları, tek veya birden çok NVıDıA GPU ile kullanılabilen özelleştirilmiş sanal makinelerdir. Bu boyutlar, işlem yoğunluğu, grafik yoğun ve görselleştirme iş yükleri için tasarlanmıştır. Bu makalede GPU 'Lar, vCPU 'Lar, veri diskleri ve NIC 'lerin sayısı ve türleri hakkında bilgi sağlanır. Bu gruplandırmadaki her boyut için depolama verimlilik ve ağ bant genişliği de mevcuttur.

- [NC serisi](nc-series.md), [NCv2 serisi](ncv2-series.md), [NCv3 serisi](ncv3-series.md) boyutları, yoğun işlem yoğunluğu ve ağ kullanımı yoğun uygulamalar ve algoritmalar için iyileştirilmiştir. Bazı örnekler CUDA ve OpenCL tabanlı uygulamalar ve benzetimler, AI ve derin öğrenime sahiptir. NCv3-Series, NVıDıA 'nin Tesla V100 GPU 'SU olan yüksek performanslı bilgi işlem iş yükleri üzerine odaklanır. NC serisi, Intel Xeon E5-2690 v3 2.60 GHz v3 (Haswell) işlemcisini kullanır ve NCv2-serisi ve NCv3 serisi VM 'Ler Intel Xeon E5-2690 v4 (geniş Iyi) işlemcisini kullanır.

- [ND serisi](nd-series.md)ve [NDv2 serisi](ndv2-series.md) boyutları, derinlemesine öğrenime yönelik eğitim ve çıkarım senaryolarına odaklanılmıştır. NVıDıA Tesla P40 GPU ve Intel Xeon E5-2690 v4 (çok Iyi) işlemcisini kullanırlar. NDv2-Series, Intel Xeon Platinum 8168 (ufuk Gölü) işlemcisini kullanır.

- [NV serisi](nv-series.md) ve [NVv3 serisi](nvv3-series.md) boyutları, OpenGL ve DirectX gibi çerçeveleri kullanarak uzaktan görselleştirme, akış, oyun, kodlama ve VDI senaryoları için iyileştirilmiştir ve tasarlanmıştır. Bu VM 'Ler NVıDıA Tesla M60 GPU tarafından desteklenir.

- [NVv4 serisi](nvv4-series.md) VDı ve uzaktan görselleştirme için optimize edilmiş ve tasarlanan VM boyutları. Bölümlenmiş GPU 'Lar ile NVv4, daha küçük GPU kaynakları gerektiren iş yükleri için doğru boyut sunar. Bu VM 'Ler AMD Radeon Instinct MI25 GPU tarafından desteklenir. NVv4 VM 'Leri Şu anda yalnızca Windows Konuk işletim sistemini desteklemektedir.

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Azure N serisi VM 'lerin GPU yeteneklerini avantajlarından yararlanmak için NVıDıA veya AMD GPU sürücüleri yüklenmelidir.

[NVıDıA GPU sürücü uzantısı](/azure/virtual-machines/extensions/hpccompute-gpu-windows) , bir N serisi VM 'ye uygun NVIDIA CUDA veya kılavuz sürücülerini yükleme. Azure portal veya Azure PowerShell veya Azure Resource Manager şablonları gibi araçları kullanarak uzantıyı yükler veya yönetir. Desteklenen işletim sistemleri ve dağıtım adımları için [NVıDıA GPU sürücü uzantısı belgelerine](/azure/virtual-machines/extensions/hpccompute-gpu-windows) bakın. VM uzantıları hakkında genel bilgi için bkz. [Azure sanal makine uzantıları ve özellikleri](/azure/virtual-machines/extensions/overview).

NVıDıA GPU sürücülerini el ile yüklemeyi tercih ederseniz desteklenen işletim sistemleri, sürücüler, yükleme ve doğrulama adımları için bkz. [Windows Için n SERISI GPU sürücü kurulumu](/azure/virtual-machines/windows/n-series-driver-setup) veya [Linux IÇIN n serisi GPU sürücü kurulumu](/azure/virtual-machines/linux/n-series-driver-setup) .

AMD GPU sürücülerini el ile yüklemek için, desteklenen işletim sistemleri, sürücüler, yükleme ve doğrulama adımları için bkz. [Windows Için N SERISI AMD GPU sürücü kurulumu](/azure/virtual-machines/windows/n-series-amd-driver-setup) .

## <a name="deployment-considerations"></a>Dağıtma konuları

- N serisi VM 'lerin kullanılabilirliği için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).

- N serisi VM 'Ler yalnızca Kaynak Yöneticisi dağıtım modelinde dağıtılabilir.

- N serisi VM 'Ler, diskleri için destekledikleri Azure Storage türünde farklılık gösterir. NC ve NV VM 'Ler yalnızca standart Disk Depolama (HDD) tarafından desteklenen VM disklerini destekler. NCv2, NCv3, ND, NDv2 ve NVv2 VM 'Ler yalnızca Premium Disk Depolama (SSD) tarafından desteklenen VM disklerini destekler.

- Birkaç tane N serisi VM dağıtmak istiyorsanız, Kullandıkça Öde aboneliğine veya diğer satın alma seçeneklerine göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

- Azure aboneliğinizdeki çekirdek kotasını (bölge başına) artırmanız ve NC, NCv2, NCv3, ND, NDv2, NV veya NVv2 çekirdekleri için ayrı kotayı artırmanız gerekebilir. Kota artışı istemek için, ücretsiz [bir çevrimiçi müşteri destek isteği açın](../azure-portal/supportability/how-to-create-azure-support-request.md) . Varsayılan sınırlar, abonelik kategorime bağlı olarak değişebilir.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
