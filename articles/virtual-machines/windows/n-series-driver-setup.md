---
title: Windows için Azure N serisi GPU sürücü kurulumu
description: Azure'da Windows Server veya Windows çalıştıran N serisi VM'ler için NVIDIA GPU sürücüleri nasıl ayarlanır?
author: vikancha
manager: jkabat
ms.service: virtual-machines-windows
ms.topic: how-to
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: vikancha
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bc11937410bf0307a00895e0ebd1f01a58bd1b1b
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81865772"
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-windows"></a>Windows çalıştıran N serisi VM'lere NVIDIA GPU sürücülerini yükleme 

Windows çalıştıran Azure N serisi VM'lerin GPU özelliklerinden yararlanmak için NVIDIA GPU sürücülerinin yüklenmesi gerekir. [NVIDIA GPU Sürücü Uzantısı,](../extensions/hpccompute-gpu-windows.md) n serisi VM'ye uygun NVIDIA CUDA veya GRID sürücülerini yükler. Azure portalını veya Azure PowerShell veya Azure Kaynak Yöneticisi şablonları gibi araçları kullanarak uzantıyı yükleyin veya yönetin. Desteklenen işletim sistemleri ve dağıtım adımları için [NVIDIA GPU Sürücü Uzantısı belgelerine](../extensions/hpccompute-gpu-windows.md) bakın.

GPU sürücülerini el ile yüklemeyi seçerseniz, bu makalede desteklenen işletim sistemleri, sürücüler ve yükleme ve doğrulama adımları sağlanır. Manuel sürücü kurulum bilgileri [Linux VM'ler](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)için de kullanılabilir.

Temel özellikler, depolama kapasiteleri ve disk ayrıntıları için [GPU Windows VM boyutlarına](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın. 

[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]

## <a name="driver-installation"></a>Sürücü yükleme

1. Uzak Masaüstü'ne bağlı olarak her N serisi VM'ye bağlanın.

2. Windows işletim sisteminiz için desteklenen sürücüyü indirin, ayıklayın ve yükleyin.

GRID sürücüsüvm'e yüklendikten sonra yeniden başlatma gerekir. CUDA sürücü yüklemesi nden sonra yeniden başlatma gerekmez.

## <a name="verify-driver-installation"></a>Sürücü yüklemeyi doğrulama

Nvidia Control paneline sadece GRID sürücü yüklemesi ile erişilebildiğine dikkat ediniz. CUDA sürücüleri yüklediyseniz, Nvidia kontrol paneli görünmez.

Aygıt Yöneticisi'nde sürücü yüklemeyi doğrulayabilirsiniz. Aşağıdaki örnek, Tesla K80 kartının Azure NC VM üzerinde başarılı bir şekilde yapılandırışını gösterir.

![GPU sürücü özellikleri](./media/n-series-driver-setup/GPU_driver_properties.png)

GPU aygıt durumunu sorgulamak için sürücüyle birlikte yüklenen [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) komut satırı yardımcı programını çalıştırın.

1. Komut istemini açın ve **C:\Program Files\NVIDIA Corporation\NVSMI** dizinini değiştirin.

2. `nvidia-smi` öğesini çalıştırın. Sürücü yüklüyse, aşağıdakine benzer çıktı görürsünüz. VM'de şu anda bir GPU iş yükü çalıştırıyorsanız **GPU-Util** **%0** gösterir. Sürücü sürümünüz ve GPU ayrıntılarınız gösterilenlerden farklı olabilir.

![NVIDIA cihaz durumu](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>RDMA ağ bağlantısı

RDMA ağ bağlantısı, aynı kullanılabilirlik kümesinde dağıtılan NC24r gibi RDMA özellikli N serisi VM'lerde veya sanal makine ölçeği nde tek bir yerleşim grubunda etkinleştirilebilir. RDMA bağlantısını etkinleştiren Windows ağ aygıtı sürücülerini yüklemek için HpcVmDrivers uzantısı eklenmelidir. RDMA özellikli N serisi VM'ye VM uzantısını eklemek için Azure Kaynak Yöneticisi için [Azure PowerShell](/powershell/azure/overview) cmdlets kullanın.

Batı ABD bölgesinde myVM adlı mevcut BIR RDMA özellikli VM en son sürümü 1.1 HpcVMDrivers uzantısı yüklemek için:
  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Daha fazla bilgi için [Windows için Sanal makine uzantıları ve özellikleri ne](extensions-features.md)bakın.

RDMA ağı, [Microsoft MPI](https://docs.microsoft.com/message-passing-interface/microsoft-mpi) veya Intel MPI 5.x ile çalışan uygulamalar için İleti Geçme Arabirimi (MPI) trafiğini destekler. 


## <a name="next-steps"></a>Sonraki adımlar

* Geliştiriciler NVIDIA Tesla GPU için GPU hızlandırılmış uygulamalar bina da indirebilir ve en son [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads)yükleyebilirsiniz. Daha fazla bilgi için [CUDA Yükleme Kılavuzu'na](https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi)bakın.


