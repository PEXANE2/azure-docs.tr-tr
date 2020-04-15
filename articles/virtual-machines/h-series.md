---
title: H serisi - Azure Sanal Makineler
description: H serisi VM'ler için teknik özellikler.
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 03/10/2020
ms.author: lahugh
ms.openlocfilehash: 6e0257f8a32d05f49dd67195f22d387319358ef2
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81314142"
---
# <a name="h-series"></a>H Serisi

H serisi VM'ler, çekirdek gereksinimleri başına yüksek CPU frekansları veya büyük bellek tarafından yönlendirilen uygulamalar için optimize edilebiyi optimize edin. H serisi VM'ler 8 veya 16 Intel Xeon E5 2667 v3 işlemci çekirdeğine, CPU çekirdeği başına 14 GB'a kadar RAM'e sahiptir ve hiper iş parçacığı yoktur. H serisi, tutarlı RDMA performansı için engelleyici olmayan yağ ağacı yapılandırmasında 56 Gb/sn Mellanox FDR InfiniBand'e sahiptir. H serisi VM'ler Intel MPI 5.x ve MS-MPI'yi destekler.

ACU: 290-300

Premium Depolama: Desteklenmiyor

Premium Depolama Önbelleğe Alma: Desteklenmiyor

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

| Boyut | Sanal işlemci | İşlemci | Bellek (GB) | Bellek bant genişliği GB/s | Temel CPU frekansı (GHz) | Tüm çekirdek frekansı (GHz, tepe) | Tek çekirdekli frekans (GHz, tepe) | RDMA performansı (Gb/s) | MPI desteği | Geçici depolama (GB) | Maksimum veri diskleri | Maksimum diski aktarım hızı: IOPS | Maksimum Ethernet NIC'leri |
| --- | --- |--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_H8   | 8  | Intel Xeon E5 2667 v3 | 56 | 40 | 3,2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 1000 | 32 | 32x500 | 2 |
| Standard_H16  | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3,2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 2000 | 64 | 64x500 | 4 |
| Standard_H8m  | 8  | Intel Xeon E5 2667 v3 | 112 | 40 | 3,2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 1000 | 32 | 32x500 | 2 |
| Standard_H16m | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3,2 | 3.3 | 3.6 | - | Intel 5.x, MS-MPI | 2000 | 64 | 64x500 | 4 |
| Standard_H16r <sup>1</sup>  | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3,2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 64x500 | 4 |
| Standard_H16mr <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3,2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 64x500 | 4 |

<sup>1</sup> MPI uygulamaları için, özel RDMA arka uç ağı FDR InfiniBand ağı tarafından etkinleştirilir.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]


## <a name="supported-os-images-linux"></a>Desteklenen işletim sistemi görüntüleri (Linux)
 
Azure Marketi'nde RDMA bağlantısını destekleyen birçok Linux dağıtımı vardır:
  
* **CentOS tabanlı HPC** - SR-IOV özellikli olmayan VM'ler için, CentOS tabanlı sürüm 6.5 HPC veya daha sonraki bir sürüm, 7.5'e kadar uygundur. H serisi VM'ler için 7.1 ile 7.5 sürümleri önerilir. RDMA sürücüleri ve Intel MPI 5.1 VM'ye yüklenir.
  SR-IOV VM'ler için CentOS-HPC 7.6, RDMA sürücüleri ve yüklü çeşitli MPI paketleri ile optimize edilmiş ve önceden yüklenmiş olarak gelir.
  Diğer RHEL/CentOS VM görüntüleri için InfiniBandLinux uzantısını ekleyerek InfiniBand'i etkinleştirin. Bu Linux VM uzantısı RDMA bağlantısı için Mellanox OFED sürücülerini (SR-IOV VM'lerde) yükler. Aşağıdaki PowerShell cmdlet, Mevcut RDMA özellikli vm'de InfiniBandDriverLinux uzantısının en son sürümünü (sürüm 1.0) yükler. RDMA özellikli VM *myVM* olarak adlandırılır ve *Batı ABD* bölgesindeki *myResourceGroup* adlı kaynak grubunda aşağıdaki gibi dağıtılır:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverLinux" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverLinux" -TypeHandlerVersion "1.0"
  ```
  Alternatif olarak, VM uzantıları aşağıdaki JSON öğesi ile kolay dağıtım için Azure Kaynak Yöneticisi şablonlarına eklenebilir:
  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverLinux",
  "typeHandlerVersion": "1.0",
  } 
  ```
  
  Aşağıdaki komut, *myResourceGroup*adlı kaynak grubunda dağıtılan *myVMSS* adlı mevcut bir sanal makine ölçeğinde tüm RDMA özellikli VM'lerde en son sürüm 1.0 InfiniBandDriverLinux uzantısını yükler:
  ```powershell
  $VMSS = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS"
  Add-AzVmssExtension -VirtualMachineScaleSet $VMSS -Name "InfiniBandDriverLinux" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverLinux" -TypeHandlerVersion "1.0"
  Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "MyVMSS" -VirtualMachineScaleSet $VMSS
  Update-AzVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS" -InstanceId "*"
  ```
  
  > [!NOTE]
  > CentOS tabanlı HPC görüntülerinde çekirdek güncelleştirmeleri **yum** yapılandırma dosyasında devre dışı bırakılır. Bunun nedeni, Linux RDMA sürücülerinin Bir RPM paketi olarak dağıtılması ve çekirdek güncelleştirilirse sürücü güncelleştirmelerinin çalışmayabilir olmasıdır.
  >
  

* **SUSE Linux Enterprise Server** - HPC için SLES 12 SP3, HPC için SLES 12 SP3 (Premium), HPC için SLES 12 SP1, HPC için SLES 12 SP1 (Premium), SLES 12 SP4 ve SLES 15. RDMA sürücüleri yüklenir ve Intel MPI paketleri VM'de dağıtılır. Aşağıdaki komutu çalıştırarak MPI yükleyin:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
  
* **Ubuntu** - Ubuntu Server 16.04 LTS, 18.04 LTS. VM'deki RDMA sürücülerini yapılandırın ve Intel MPI'yi indirmek için Intel'e kaydolun:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../includes/virtual-machines-common-ubuntu-rdma.md)]  

  InfiniBand'i etkinleştirme, MPI'yi ayarlama hakkında daha fazla bilgi [için](./workloads/hpc/enable-infiniband.md)bkz.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.
