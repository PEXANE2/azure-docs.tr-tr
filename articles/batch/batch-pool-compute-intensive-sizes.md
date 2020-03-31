---
title: İşlem yoğun Azure VM'lerini Toplu İşlem'le kullanma
description: Azure Toplu Toplu Havuzlarda HPC ve GPU sanal makine boyutlarından nasıl yararlanabilirsiniz? İşletim sistemi bağımlılıkları hakkında bilgi edinin ve çeşitli senaryo örneklerine bakın.
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 12/17/2018
ms.author: labrenne
ms.openlocfilehash: d0238e0b5b882748218835d7f06a147d435a9f90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80245064"
---
# <a name="use-rdma-or-gpu-instances-in-batch-pools"></a>Toplu iş havuzlarında RDMA veya GPU örneklerini kullanma

Belirli Toplu işlerden çalıştırmak için, büyük ölçekli hesaplama için tasarlanmış Azure VM boyutlarından yararlanabilirsiniz. Örnek:

* Çok örnekli [MPI iş yüklerini](batch-mpi.md)çalıştırmak için, Uzaktan Doğrudan Bellek Erişimi (RDMA) için ağ arabirimine sahip H serisi veya diğer boyutları seçin. Bu boyutlar, MPI uygulamalarını hızlandıran düğümler arası iletişim için bir InfiniBand ağına bağlanır. 

* CUDA uygulamaları için, NVIDIA Tesla grafik işleme birimi (GPU) kartlarını içeren N serisi boyutlar seçin.

Bu makalede, Toplu Toplu havuzlarda Azure'un özel boyutlarından bazılarını kullanmak için kılavuz lar ve örnekler verilmektedir. Özellikler ve arka plan için bkz:

* Yüksek performanslı işlem VM boyutları ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* GPU özellikli VM boyutları ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 

> [!NOTE]
> Toplu Iş hesaplarınızı oluşturduğunuz bölgelerde belirli VM boyutları kullanılamayabilir. Bir boyutun kullanılabilir olup olmadığını kontrol etmek için, [bölgeye göre kullanılabilen Ürünler'e](https://azure.microsoft.com/regions/services/) bakın ve [Toplu Iş havuzu için bir VM boyutu seçin.](batch-pool-vm-sizes.md)

## <a name="dependencies"></a>Bağımlılıklar

Toplu İşlem ebatlarında işlem yoğun boyutların RDMA veya GPU yetenekleri yalnızca belirli işletim sistemlerinde desteklenir. (Desteklenen işletim sistemleri listesi, bu boyutlarda oluşturulan sanal makineler için desteklenenlerin bir alt kümesidir.) Toplu İşlem havuzunuzu nasıl oluşturduğunuza bağlı olarak, düğümlere ek sürücü veya başka bir yazılım yüklemeniz veya yapılandırmanız gerekebilir. Aşağıdaki tablolar bu bağımlılıkları özetleyin. Ayrıntılar için bağlantılı makalelere bakın. Toplu Iş havuzlarını yapılandırma seçenekleri için bu makalenin ilerleyen günlerine bakın.

### <a name="linux-pools---virtual-machine-configuration"></a>Linux havuzları - Sanal makine yapılandırması

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılımlar | Havuz ayarları |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/linux/sizes-hpc.md)<br/>[NC24r, NC24rs_v2, NC24rs_v3, ND24rs<sup>*</sup>](../virtual-machines/linux/n-series-driver-setup.md#rdma-network-connectivity) | RDMA | Ubuntu 16.04 LTS veya<br/>CentOS tabanlı HPC<br/>(Azure Marketi) | Intel MPI 5<br/><br/>Linux RDMA sürücüleri | Düğümler arası iletişimi etkinleştirin, eşzamanlı görev yürütmesini devre dışı |
| [NC, NCv2, NCv3, NDv2 serisi](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla GPU (seriye göre değişir) | Ubuntu 16.04 LTS veya<br/>CentOS 7.3 veya 7.4<br/>(Azure Marketi) | NVIDIA CUDA veya CUDA Toolkit sürücüleri | Yok | 
| [NV, NVv2 serisi](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS veya<br/>CentOS 7.3<br/>(Azure Marketi) | NVIDIA GRID sürücüleri | Yok |

<sup>*</sup>RDMA özellikli N serisi boyutları da NVIDIA Tesla GPU'ları içerir

### <a name="windows-pools---virtual-machine-configuration"></a>Windows havuzları - Sanal makine yapılandırması

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılımlar | Havuz ayarları |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/sizes-hpc.md)<br/>[NC24r, NC24rs_v2, NC24rs_v3, ND24rs<sup>*</sup>](../virtual-machines/windows/n-series-driver-setup.md#rdma-network-connectivity) | RDMA | Windows Server 2016, 2012 R2 veya<br/>2012 (Azure Marketi) | Microsoft MPI 2012 R2 veya sonrası veya<br/> Intel MPI 5<br/><br/>Windows RDMA sürücüleri | Düğümler arası iletişimi etkinleştirin, eşzamanlı görev yürütmesini devre dışı |
| [NC, NCv2, NCv3, ND, NDv2 serisi](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla GPU (seriye göre değişir) | Windows Server 2016 veya <br/>2012 R2 (Azure Marketi) | NVIDIA CUDA veya CUDA Toolkit sürücüleri| Yok | 
| [NV, NVv2 serisi](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Windows Server 2016 veya<br/>2012 R2 (Azure Marketi) | NVIDIA GRID sürücüleri | Yok |

<sup>*</sup>RDMA özellikli N serisi boyutları da NVIDIA Tesla GPU'ları içerir

### <a name="windows-pools---cloud-services-configuration"></a>Windows havuzları - Bulut hizmetleri yapılandırması

> [!NOTE]
> Bulut Hizmeti yapılandırması ile Toplu Iş havuzlarında N serisi boyutları desteklenmez.
>

| Boyut | Özellik | İşletim sistemleri | Gerekli yazılımlar | Havuz ayarları |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/sizes-hpc.md) | RDMA | Windows Server 2016, 2012 R2, 2012 veya<br/>2008 R2 (Konuk OS ailesi) | Microsoft MPI 2012 R2 veya sonrası veya<br/>Intel MPI 5<br/><br/>Windows RDMA sürücüleri | Düğümler arası iletişimi etkinleştirin,<br/> eşzamanlı görev yürütmedevre dışı |

## <a name="pool-configuration-options"></a>Havuz yapılandırma seçenekleri

Toplu iş havuzunuz için özel leştirilmiş bir VM boyutunu yapılandırmak için gerekli yazılımı veya sürücüleri yüklemek için birkaç seçeneğiniz vardır:

* Sanal makine yapılandırmasındaki havuzlar için, sürücüleri ve yazılımı önceden yüklemiş olan önceden yapılandırılmış bir [Azure MarketI](https://azuremarketplace.microsoft.com/marketplace/) VM görüntüsünü seçin. Örnekler: 

  * [CentOS tabanlı 7.4 HPC](https://azuremarketplace.microsoft.com/marketplace/apps/openlogic.centos-hpc?tab=Overview) - RDMA sürücüleri ve Intel MPI 5.1 içerir

  * Linux veya Windows için [Veri Bilimi Sanal Makine](../machine-learning/data-science-virtual-machine/overview.md) - NVIDIA CUDA sürücüleri içerir

  * GPU ve RDMA sürücülerini de içeren Toplu taşıma kapsayıcısı iş yükleri için Linux görüntüleri:

    * [Azure Toplu toplu araç havuzları için CentOS (GPU ve RDMA sürücüleri ile)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-batch.centos-container-rdma?tab=Overview)

    * [Azure Toplu araç havuzları için Ubuntu Server (GPU ve RDMA sürücüleri ile)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-batch.ubuntu-server-container-rdma?tab=Overview)

* VM boyutu için gerekli sürücüleri, yazılımı veya diğer ayarları yüklediğiniz özel bir Windows veya [Linux VM görüntüsü](batch-sig-images.md) oluşturun. 

* Sıkıştırılmış bir sürücü veya uygulama yükleyicisinden toplu [iş uygulama paketi](batch-application-packages.md) oluşturun ve paketi havuz düğümlerine dağıtmak ve her düğüm oluşturulduğunda bir kez yüklemek için Toplu İşlem'i yapılandırın. Örneğin, uygulama paketi bir yükleyiciyse, uygulamayı tüm havuz düğümlerine sessizce yüklemek için bir [başlangıç görev](batch-api-basics.md#start-task) komut satırı oluşturun. İş yükünüz belirli bir sürücü sürümüne bağlıysa, bir uygulama paketi ve havuz başlangıç görevi kullanmayı düşünün.

  > [!NOTE] 
  > Başlangıç görevi yükseltilmiş (yönetici) izinlerle çalışmalı ve başarıyı beklemelidir. Uzun süren görevler, Toplu Iş havuzu sağlama süresini artırır.
  >

* [Toplu Tersane,](https://github.com/Azure/batch-shipyard) GPU ve RDMA sürücülerini Azure Toplu İş'te kapsayıcılaştırılmış iş yükleriyle şeffaf bir şekilde çalışacak şekilde otomatik olarak yapılandırır. Toplu Tersane tamamen yapılandırma dosyaları ile tahrik edilir. N serisi VM'lerde GPU sürücülerini önceden yapılandıran ve Microsoft Cognitive Toolkit yazılımını Docker görüntüsü olarak yükleyen [CNTK GPU Recipe](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) gibi GPU ve RDMA iş yüklerini etkinleştiren birçok örnek tarif yapılandırması mevcuttur.


## <a name="example-nvidia-gpu-drivers-on-windows-nc-vm-pool"></a>Örnek: Windows NC VM havuzunda NVIDIA GPU sürücüleri

CUDA uygulamalarını Windows NC düğümleri havuzunda çalıştırmak için NVDIA GPU sürücülerini yüklemeniz gerekir. Aşağıdaki örnek adımlar, NVIDIA GPU sürücülerini yüklemek için bir uygulama paketi kullanır. İş yükünüz belirli bir GPU sürücüsü sürümüne bağlıysa bu seçeneği seçebilirsiniz.

1. Windows Server 2016'daki GPU sürücüleri için bir kurulum [paketini NVIDIA web sitesinden](https://www.nvidia.com/Download/index.aspx) indirin - örneğin, [sürüm 411.82](https://us.download.nvidia.com/Windows/Quadro_Certified/411.82/411.82-tesla-desktop-winserver2016-international.exe). *Dosyayı GPUDriverSetup.exe*gibi kısa bir ad kullanarak yerel olarak kaydedin.
2. Paketin zip dosyasını oluşturun.
3. Paketi Toplu Hesabınıza yükleyin. Adımlar için [uygulama paketleri](batch-application-packages.md) kılavuzuna bakın. *GPUDriver*gibi bir uygulama kimliği ve *411.82*gibi bir sürüm belirtin.
1. Toplu İş API'lerini veya Azure portalını kullanarak, sanal makine yapılandırmasında istenilen sayıda düğüm ve ölçekiçeren bir havuz oluşturun. Aşağıdaki tablo, başlangıç görevi kullanarak Sessizce NVIDIA GPU sürücülerini yüklemek için örnek ayarları gösterir:

| Ayar | Değer |
| ---- | ----- | 
| **Görüntü Türü** | Market (Linux/Windows) |
| **Yayımcı** | MicrosoftWindowsServer |
| **Teklif** | WindowsServer |
| **Sku** | 2016-Datacenter |
| **Düğüm boyutu** | NC6 Standardı |
| **Uygulama paketi referansları** | GPUDriver, sürüm 411.82 |
| **Görevi başlat** | True<br>**Komut satırı** - `cmd /c "%AZ_BATCH_APP_PACKAGE_GPUDriver#411.82%\\GPUDriverSetup.exe /s"`<br/>**Kullanıcı kimliği** - Havuz otomatik kullanıcı, yönetici<br/>**Başarıyı bekleyin** - Doğru

## <a name="example-nvidia-gpu-drivers-on-a-linux-nc-vm-pool"></a>Örnek: Linux NC VM havuzundaki NVIDIA GPU sürücüleri

CUDA uygulamalarını Linux NC düğümleri havuzunda çalıştırmak için CUDA Toolkit'ten gerekli NVIDIA Tesla GPU sürücülerini yüklemeniz gerekir. Aşağıdaki örnek adımlar, GPU sürücüleri ile özel bir Ubuntu 16.04 LTS görüntü oluşturmak ve dağıtmak:

1. Ubuntu 16.04 LTS çalıştıran bir Azure NC serisi VM dağıtın. Örneğin, ABD Güney Orta bölgesinde VM oluşturun. 
2. Azure aboneliğine bağlanan bir istemci bilgisayar olan Azure portalını veya Azure Bulut BulutU'nu kullanarak [NVIDIA GPU Sürücüleri uzantısını](../virtual-machines/extensions/hpccompute-gpu-linux.md
) VM'ye ekleyin. Alternatif olarak, VM'ye bağlanmak ve [CUDA sürücülerini](../virtual-machines/linux/n-series-driver-setup.md) el ile yüklemek için aşağıdaki adımları izleyin.
3. Toplu İşlem için [Paylaşılan Resim Galerisi resmi](batch-sig-images.md) oluşturmak için adımları izleyin.
4. NC VM'leri destekleyen bir bölgede Toplu Iş hesabı oluşturun.
5. Toplu İş API'lerini veya Azure portalını [kullanarak, özel görüntüyü kullanarak](batch-sig-images.md) ve istenen sayıda düğüm ve ölçekiçeren bir havuz oluşturun. Aşağıdaki tabloda görüntü için örnek havuz ayarları gösterilmektedir:

| Ayar | Değer |
| ---- | ---- |
| **Görüntü Türü** | Özel Görüntü |
| **Özel Resim** | *Resmin adı* |
| **Düğüm ajanı SKU** | batch.node.ubuntu 16.04 |
| **Düğüm boyutu** | NC6 Standardı |

## <a name="example-microsoft-mpi-on-a-windows-h16r-vm-pool"></a>Örnek: Windows H16r VM havuzunda Microsoft MPI

Windows MPI uygulamalarını Azure H16r VM düğümleri havuzunda çalıştırmak için HpcVmDrivers uzantısını yapılandırmanız ve [Microsoft MPI'yi](https://docs.microsoft.com/message-passing-interface/microsoft-mpi)yüklemeniz gerekir. Gerekli sürücüler ve yazılımlarla özel bir Windows Server 2016 görüntüsünü dağıtmak için örnek adımlar aşağıda veda edebilirsiniz:

1. Windows Server 2016 çalıştıran bir Azure H16r VM dağıtın. Örneğin, ABD Batı bölgesinde VM oluşturun. 
2. Azure aboneliğinize bağlanan bir istemci bilgisayardan [Azure PowerShell komutu çalıştırarak](../virtual-machines/sizes-hpc.md) veya Azure Bulut Shell'i kullanarak VM'ye HpcVmDrivers uzantısını ekleyin. 
1. VM'ye Uzak Masaüstü bağlantısı yapın.
1. Microsoft MPI'nin en son sürümü için [kurulum paketini](https://www.microsoft.com/download/details.aspx?id=57467) (MSMpiSetup.exe) indirin ve Microsoft MPI'yi yükleyin.
1. Toplu İşlem için [Paylaşılan Resim Galerisi resmi](batch-sig-images.md) oluşturmak için adımları izleyin.
1. Toplu İş API'lerini veya Azure portalını [kullanarak, Paylaşılan Resim Galerisi'ni kullanarak](batch-sig-images.md) ve istenen sayıda düğüm ve ölçekiçeren bir havuz oluşturun. Aşağıdaki tabloda görüntü için örnek havuz ayarları gösterilmektedir:

| Ayar | Değer |
| ---- | ---- |
| **Görüntü Türü** | Özel Görüntü |
| **Özel Resim** | *Resmin adı* |
| **Düğüm ajanı SKU** | batch.node.windows amd64 |
| **Düğüm boyutu** | H16r Standardı |
| **Internode iletişim etkin** | True |
| **Düğüm başına maksimum görevler** | 1 |

## <a name="example-intel-mpi-on-a-linux-h16r-vm-pool"></a>Örnek: Linux H16r VM havuzunda Intel MPI

MPI uygulamalarını Linux H serisi düğümleri havuzunda çalıştırmak için, Azure Marketi'nden [CentOS tabanlı 7,4 HPC](https://azuremarketplace.microsoft.com/marketplace/apps/openlogic.centos-hpc?tab=Overview) görüntüsünü kullanmak seçeneğinden biri. Linux RDMA sürücüleri ve Intel MPI önceden yüklenmiştir. Bu resim, Docker kapsayıcı iş yüklerini de destekler.

Toplu İş API'lerini veya Azure portalını kullanarak, bu görüntüyü kullanarak ve istenilen sayıda düğüm ve ölçekle bir havuz oluşturun. Aşağıdaki tablo örnek havuz ayarlarını gösterir:

| Ayar | Değer |
| ---- | ---- |
| **Görüntü Türü** | Market (Linux/Windows) |
| **Yayımcı** | OpenLogic |
| **Teklif** | CentOS-HPC |
| **Sku** | 7.4 |
| **Düğüm boyutu** | H16r Standardı |
| **Internode iletişim etkin** | True |
| **Düğüm başına maksimum görevler** | 1 |

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure Toplu İş havuzunda MPI işleri çalıştırmak için [Windows](batch-mpi.md) veya [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) örneklerine bakın.

* Toplu İşlem'deki GPU iş yüklerine örnekler için [Toplu Tersane](https://github.com/Azure/batch-shipyard/) tariflerine bakın.
